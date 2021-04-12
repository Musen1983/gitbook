# JCTools/JCTools

 [Fix](https://github.com/JCTools/JCTools/commit/deb1a6fa0e4aed3427b83081c877425c510e22aa) [\#330](https://github.com/JCTools/JCTools/issues/330) [replace issue in NBHM Identity map](https://github.com/JCTools/JCTools/commit/deb1a6fa0e4aed3427b83081c877425c510e22aa)

```text
Also bring the NonBlockingIdentityHashMap implementation closer to the original NonBlockingHashMap resolving some issues in the process:
* remove(Object key, Object val) should not use identity comparison
* replace(TypeK  key, TypeV  oldValue, TypeV newValue) should not use identity comparison
* missing assert in get
* get_impl : comparing `key` instead of `K` (given instead of read)
* putIfMatch0 : comparing `key` instead of `K` (given instead of read)
The result is a much cleaner diff between NonBlockingIdentityHashMap and NonBlockingHashMap.
```

