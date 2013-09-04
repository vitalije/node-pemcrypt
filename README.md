# Pemcrypt

Install with `npm`.

```bash
$ npm i pemcrypt --save
```

Require. Show me some CommonJS/Modules love!

```js
var pemcrypt = require('pemcrypt');
```

# Purpose

The goal of `pemcrypt` is to allow you to commit to source control sensible environment configuration values in an encrypted manner. All you need to do then, is get the private key originally used to encrypt a file, and you're good to go.

Ideally, this would only be used for development configuration values. You would distribute a `.pemjson` file with your module, and give the private key to decrypt the file to your contributors. The upside is that you won't need to give them a new `.json` file every time a value needs to change, but rather just encrypt it again, and push the new `.pemjson`.

Make sure you add `*.pem`, and whatever the decrypted JSON filename is to your `.gitignore`. Commiting either of those would defeat the entire purpose of this module.

# #pemcrypt.generateKey

Generates a `.pem` file the first time around. You can save it wherever you won't, but **don't ever commit it to source control**.

```js
pemcrypt.generateKey(pemfile);
```

# #pemcrypt(options)

Creates a pemcrypt `store` object. This will be used to `encrypt` and `decrypt` our files. This function will look for a `.pem` file and load it immediately, throwing if one isn't found.

```js
var store = pemcrypt({
    pem: pemfile,   // same one used to generate the key
    cwd: __dirname  // defaults to process.cwd()
});
```

# #store.encrypt(storeName, persist)

Encrypts a raw `.json` file. This method will take a file path relative to `cwd`, _without the `.json` extension_. The `persist` parameter will determine whether the results are dumped to an encrypted `.pemjson` file next to the `.json` one.

This method is _synchronous_ and returns the encrypted data, too.

```js
var pemjson = store.encrypt('env/defaults');

console.log(pemjson); // garbage
```

# #store.decrypt(storeName, persist)

Decrypts an encrypted `.pemjson` file. This method will take a file path relative to `cwd`, _without the `.pemjson` extension_. The `persist` parameter will determine whether the results are dumped to a decrypted `.json` file next to the `.pemjson` one.

This method is _synchronous_ and returns the decrypted data, too.

```js
var json = store.decrypt('env/defaults');

console.log(json); // data!
```