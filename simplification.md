Proof of modifications
----------------------

Gary Court's version contains the following line:

```javascript
k1 = (((k1 & 0xffff) * c1) + ((((k1 >>> 16) * c1) & 0xffff) << 16)) & 0xffffffff
```

which can be simplified to:

```javascript
k1 = (k1 * (c1 & 0xffff) + (k1 & 0xffff) * (c1 & 0xffff0000)) & 0xffffffff
```

Consider each half of the equation individually, letting `k1 = (a + b) & 0xffffffff`. Starting with `b`, we can simplify:

```javascript
b = (((k1 >>> 16) * c1) & 0xffff) << 16
b = (((k1 >>> 16) * (c1 & 0xffff)) & 0xffff) << 16
b = ((k1 & 0xffff0000) * (c1 & 0xffff)) & 0xffff0000
b = ((k1 & 0xffff0000) * (c1 & 0xffff)) & 0xffffffff
b = (k1 & 0xffff0000) * (c1 & 0xffff)
```

The last line is equal in this case because the entire expression `(a + b)` is ANDed with `0xffffffff`, so it can be factored out of `b`. Next, `a` can be expanded:

```javascript
a = (k1 & 0xffff) * c1
a = (k1 & 0xffff) * ((c1 & 0xffff) + (c1 & 0xffff0000))
a = (k1 & 0xffff) * (c1 & 0xffff) + (k1 & 0xffff) * (c1 & 0xffff0000)
```

Letting `a = e + f`, we get `k1 = (e + f + b) & 0xffffffff`. Combining `e + b`, we can find:

```javascript
e + b = (k1 & 0xffff) * (c1 & 0xffff) + (k1 & 0xffff0000) * (c1 & 0xffff)
e + b = ((k1 & 0xffff) + (k1 & 0xffff0000)) * (c1 & 0xffff)
e + b = k1 * (c1 & 0xffff)
```

Finally, putting it all together:

```javascript
k1 = ((e + b) + f) & 0xffffffff
k1 = (k1 * (c1 & 0xffff) + (k1 & 0xffff) * (c1 & 0xffff0000)) & 0xffffffff
````

Overall, all this does is multiply `k1` by `c1` and only keep the 32-bit result. Unfortunately, JavaScript can't handle the multiply directly since the result will be cast to floating point if it results in more than 53 bits of precision (and will lose the lower bits, exactly the ones we want to keep).

All other modifications to the algorithm are based on this change.

To see the original and modified versions this is based on, check out:
* [Gary Court's original version](https://github.com/garycourt/murmurhash-js)
* [kazuyukitanimura's modified version](https://github.com/kazuyukitanimura/murmurhash-js)
