Proof of kazuyukitanimura's MurmurHash.js modifications
-------------------------------------------------------

```
// Start with the old version
k1 = (((k1 & 0xffff) * c1) + ((((k1 >>> 16) * c1) & 0xffff) << 16)) & 0xffffffff
k1 = (((k1 & 0xffff) * c1) + ((((k1 >>> 16) * (c1 & 0xffff)) & 0xffff) << 16)) & 0xffffffff
k1 = (((k1 & 0xffff) * c1) + (((k1 & 0xffff0000) * (c1 & 0xffff)) & 0xffff0000)) & 0xffffffff
k1 = (((k1 & 0xffff) * c1) + ((k1 & 0xffff0000) * (c1 & 0xffff))) & 0xffffffff
k1 = (((k1 & 0xffff) * ((c1 & 0xffff) + (c1 & 0xffff0000))) + ((k1 & 0xffff0000) * (c1 & 0xffff))) & 0xffffffff
k1 = ((((k1 & 0xffff) * (c1 & 0xffff)) + ((k1 & 0xffff) * (c1 & 0xffff0000))) + ((k1 & 0xffff0000) * (c1 & 0xffff))) & 0xffffffff
[k1 = (a + b + c) & 0xffffffff]
[a + c = (k1 & 0xffff) * (c1 & 0xffff) + (k1 & 0xffff0000) * (c1 & 0xffff) = k1 * (c1 & 0xffff)]
k1 = (k1 * (c1 & 0xffff) + (k1 & 0xffff) * (c1 & 0xffff0000)) & 0xffffffff
// End the simplified version
```

See [Gary Court's original version](https://github.com/garycourt/murmurhash-js)
See [kazuyukitanimura's modified version](https://github.com/kazuyukitanimura/murmurhash-js)
