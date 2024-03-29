# encrypt-myrepo

Keep secret files in github repository could be safe as long as it is encrypted. `crypt-in-repo` is a helper for developer who need to save secret files with their code in a safe way.

## Install

```shell
$ npm i encrypt-myrepo --save-dev
```

## Usage

Create config file `encrypt-myrepo.json` in project root folder.

Here is an example:
```json
{
    "files": [
        "README.md",
        "key.cert",
        "cert/password.json"
    ],
    "ext": ".crypt"
}
```

Add script in package.json
```json
{
    "scripts": {
        "encrypt": "encrypt-myrepo encrypt",
        "decrypt": "encrypt-myrepo decrypt"
    }
}
```

Run the script:
```shell
# encrypt
CIR_PASS=mypassword npm run encrypt

# decrypt
CIR_PASS=mypassword npm run decrypt
```

### Example using command line

Encrypt files:

```shell
npm run encrypt -- --pass mypassword --file secret.cert ios.p12

npm run encrypt -- --config ./encrypt-myrepo.json
```

Decrypt files:

```shell
npm run decrypt -- --pass mypassword --file secret.cert ios.p12

npm run decrypt -- --config ./encrypt-myrepo.json
```

### Example using environment variables

Encrypt files:

```shell
CIR_CONFIG=./encrypt-myrepo.json npm run encrypt

CIR_PASS=mypassword npm run encrypt -- --file secret.cert ios.p12
```

Decrypt files:
```shell
CIR_CONFIG=./encrypt-myrepo.json npm run decrypt

CIR_PASS=mypassword npm run decrypt -- --file secret.cert ios.p12
```

## Documents

Options can set in config file, command line or environment variables:

| Config file | Command line options | Env variable | Explain |
|---|---|---|---|
| pass  | --pass, -p              | CIR_PASS=passphase         | Passphrase for enrypt/decrypt file. |
| files | --file file1 [file2...]<sup>1</sup> | CIR_FILES=file1[;file2...]<sup>2</sup> | Array of origin files. |
| ext   | --ext                   | CIR_EXT=.crypt             | Extension of encrypted files. <br/>Default value: .aes256 |
| limit | --limit                 | CIR_SIZELIMIT=1048576          | Limit size of origin file. <br/> Default value: 1048576 (1MB) |

Notes:
1. Assign file list in command line follow the [yargs array(key)](https://yargs.js.org/docs/#api-reference-arraykey) standards:

    - `--file file1 --file file2` will be parsed as `['file1','file2']`
    - `--file file1 file2` will also be parsed as `['file1','file2]`

2. Assign file list in env variable, the filename should seperated by `;`.

`encrypt-myrepo` can assign config file other than default `encrypt-myrepo.json`. With command line options `--config config_file`

`crypt-in-repo`  `--config config_file` to get config file. System environment variable `CIR_CONFIG` has the same functionality.
