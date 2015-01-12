#PublicKey.js

Demo: https://diafygi.github.io/publickeyjs/

This is a simple javascript library that allows you to search for PGP public
keys on [SKS keyservers](https://sks-keyservers.net/) and [Keybase](https://keybase.io/).
The API and results are similar to specifications described in
[HTTP Keyserver Protocol](http://tools.ietf.org/html/draft-shaw-openpgp-hkp-00).


##API

To use this library, just add `publickey.js` in your scripts.

```html
<script src="publickey.js"></script>
```

###PublicKey

Initialize the library by creating a new PublicKey object.

####Arguments

* `keyservers` - Array of keyserver domains (default is `["https://keys.fedoraproject.org/", "https://keybase.io/"]`).

####Examples

```javascript
//Initialize with the default keyservers
var pk = new PublicKey();

//Initialize only with a specific keyserver
var pk = new PublicKey(["https://key.ip6.li/"]);
```

###pk.search(query, callback)

Search for public key ids on the initialized keyservers.

####Arguments

* `query` - String to search for (usually an email, name, or username).
* `callback` - Function that is called when finished. Two arguments are passed
to the callback: `results` and `errorCode`. `results` is an Array of users that
were returned by the search. `errorCode` is the error code (either HTTP status
code or keybase error code) returned by the last keyserver that was tried. If
any results were found, `errorCode` is null. If no results are found, `results`
is null and `errorCode` is not null.

####Examples

```javascript
//Search for diafygi's key id
var pk = new PublicKey();
pk.search("diafygi", function(results, errorCode){
    errorCode !== null ? console.log(errorCode) : console.log(results);
});

//Search for a nonexistent key id
var pk = new PublicKey();
pk.search("doesntexist123", function(results, errorCode){
    errorCode !== null ? console.log(errorCode) : console.log(results);
});
```

###pk.get(keyId, callback)

Get a public key from any keyserver based on keyId.

####Arguments

* keyId - String key id of the public key (this is usually a fingerprint)
* callback - Function that is called when finished. Two arguments are
        passed to the callback: publicKey and errorCode. publicKey is
        an ASCII armored OpenPGP public key. errorCode is the error code
        (either HTTP status code or keybase error code) returned by the
        last keyserver that was tried. If a publicKey was found,
        errorCode is null. If no publicKey was found, publicKey is null
        and errorCode is not null.

####Examples

```javascript
//Get a valid public key
var pk = new PublicKey();
pk.get("F75BE4E6EF6E9DD203679E94E7F6FAD172EFEE3D", function(publicKey, errorCode){
    errorCode !== null ? console.log(errorCode) : console.log(publicKey);
});

//Try to get an invalid public key
var pk = new PublicKey();
pk.get("bogus_id", function(publicKey, errorCode){
    errorCode !== null ? console.log(errorCode) : console.log(publicKey);
});
```


