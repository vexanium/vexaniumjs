<!--[![Build Status](https://travis-ci.org/vexanium/vexjs-keygen.svg?branch=master)](https://travis-ci.org/vexanium/vexjs-keygen)-->
[![NPM](https://img.shields.io/npm/v/vexjs-keygen.svg)](https://www.npmjs.org/package/vexjs-keygen)

# Repository

The purpose of this library is for managing keys in local storage.  This is designed to derive and cache keys but also needs a password manager to store a "root" key. This library does not have secure or password protected storage. It does however figure out permission hierarchies and is configurable enough to only store keys you feel are safe to store.

General purpose cryptography is found in [vexjs-ecc](http://github.com/vexanium/vexjs-ecc) library.  Hierarchical
deterministic key generation uses PrivateKey.getChildKey in vexjs-ecc.

### Usage

```javascript
let {Keystore, Keygen} = require('vexjs-keygen')
Vex = require('vexjs')

sessionConfig = {
  timeoutInMin: 30,
  uriRules: {
    'owner' : '/account_recovery',
    'active': '/(transfer|contracts)',
    'active/**': '/producers'
  }
}

keystore = Keystore('myaccount', sessionConfig)
vex = Vex.Testnet({keyProvider: keystore.keyProvider})

Keygen.generateMasterKeys().then(keys => {
  // create blockchain account called 'myaccount'
  console.log(keys)

  vex.getAccount('myaccount').then(account => {
    keystore.deriveKeys({
      parent: keys.masterPrivateKey,
      accountPermissions: account.permissions
    })
  })

})
```

See [./API](./API.md)

# Development

```javascript
let {Keystore, Keygen} = require('./src')
```

Use Node v8+ (updates `package-lock.json`)

# Browser

```bash
git clone https://github.com/vexanium/vexjs-keygen.git
cd vexjs-keygen
npm install
npm run build
# builds: ./dist/vexjs-keygen.js
```

```html
<script src="vexjs-keygen.js"></script>
<script>
//kos.Keystore
//kos.Keygen
//...
</script>
```

# Runtime Environment

Node 6+ and browser (browserify, webpack, etc)

Built with React Native in mind, create an issue if you find a bug.
