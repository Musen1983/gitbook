# JCTools/JCTools

## Commits on Mar 30, 2021

1.  [Fix table-size long math on very large tables.](../commit/fix-table-size-long-math-on-very-large-tables.-66e6cbc.md)

   ```text
   Fix reprobe rate for pathological keys.
   Fix rare mis-count during table-promotion counting
   Allow a fast not-atomic size-preserving clear, for rapid recycling of large tables.
   Remove some IDE warnings.
   ```

## Commits on Nov 25, 2020

1.  [Fix](https://github.com/JCTools/JCTools/commit/deb1a6fa0e4aed3427b83081c877425c510e22aa) [\#330](https://github.com/JCTools/JCTools/issues/330) [replace issue in NBHM Identity map](https://github.com/JCTools/JCTools/commit/deb1a6fa0e4aed3427b83081c877425c510e22aa)

   ```text
   Also bring the NonBlockingIdentityHashMap implementation closer to the original NonBlockingHashMap resolving some issues in the process:
   * remove(Object key, Object val) should not use identity comparison
   * replace(TypeK  key, TypeV  oldValue, TypeV newValue) should not use identity comparison
   * missing assert in get
   * get_impl : comparing `key` instead of `K` (given instead of read)
   * putIfMatch0 : comparing `key` instead of `K` (given instead of read)
   The result is a much cleaner diff between NonBlockingIdentityHashMap and NonBlockingHashMap.
   ```

2. 3. 4. 5. 6. 7. 8. 9. 10. 
## Commits on Oct 7, 2020

1.  [Fixed versions in pom](https://github.com/JCTools/JCTools/commit/d5ead9fc3e7c78db76d2ac531ef1046ac5bded28)

   ```text
   The version of jctools is 3.1.1-SNAPSHOT.

   But some some of the poms were pointing to 3.0.1-SNAPSHOT.

   I was not able to build to project without getting this fixed.
   ```

