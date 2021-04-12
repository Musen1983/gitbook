# Fix table-size long math on very large tables.@66e6cbc

|  | @@ -91,7 +91,7 @@ |  |
| :--- | :--- | :--- |
|  |  |  |
|  |  |  public class NonBlockingHashMapLong |
|  |  |  extends AbstractMap&lt;Long,TypeV&gt; |
|  |  |  implements ConcurrentMap&lt;Long,TypeV&gt;, Serializable { |
|  |  |  implements ConcurrentMap&lt;Long,TypeV&gt;, Cloneable, Serializable { |
|  |  |  |
|  |  |  private static final long serialVersionUID = 1234123412341234124L; |
|  |  |  |
|  | @@ -195,6 +195,16 @@ private static void print2\_impl\(final int i, final long K, final Object V\) { |  |
|  |  |  print\_impl\(i,K,V\); |
|  |  |  } |
|  |  |  |
|  |  |  // Count of reprobes |
|  |  |  private transient ConcurrentAutoTable \_reprobes = new ConcurrentAutoTable\(\); |
|  |  |  /\*\* Get and clear the current count of reprobes. Reprobes happen on key |
|  |  |  \* collisions, and a high reprobe rate may indicate a poor hash function or |
|  |  |  \* weaknesses in the table resizing function. |
|  |  |  \* @return the count of reprobes since the last call to {@link \#reprobes} |
|  |  |  \* or since the table was created. \*/ |
|  |  |  public long reprobes\(\) { long r = \_reprobes.get\(\); \_reprobes = new ConcurrentAutoTable\(\); return r; } |
|  |  |  |
|  |  |  |
|  |  |  // --- reprobe\_limit ----------------------------------------------------- |
|  |  |  // Heuristic to decide if we have reprobed toooo many times. Running over |
|  |  |  // the reprobe limit on a 'get' call acts as a 'miss'; on a 'put' call it |
|  | @@ -301,6 +311,7 @@ public boolean replace \( long key, TypeV oldValue, TypeV newValue \) { |  |
|  |  |  return putIfMatch\( key, newValue, oldValue \) == oldValue; |
|  |  |  } |
|  |  |  |
|  |  |  @SuppressWarnings\("unchecked"\) |
|  |  |  private TypeV putIfMatch\( long key, Object newVal, Object oldVal \) { |
|  |  |  if \(oldVal == null \|\| newVal == null\) throw new NullPointerException\(\); |
|  |  |  if\( key == NO\_KEY \) { |
|  | @@ -326,6 +337,11 @@ public void clear\(\) { // Smack a new empty table down |  |
|  |  |  while\( !CAS\(\_chm\_offset,\_chm,newchm\) \) { /\*Spin until the clear works\*/} |
|  |  |  CAS\(\_val\_1\_offset,\_val\_1,TOMBSTONE\); |
|  |  |  } |
|  |  |  // Non-atomic clear, preserving existing large arrays |
|  |  |  public void clear\(boolean large\) { // Smack a new empty table down |
|  |  |  \_chm.clear\(\); |
|  |  |  CAS\(\_val\_1\_offset,\_val\_1,TOMBSTONE\); |
|  |  |  } |
|  |  |  |
|  |  |  /\*\* Returns true if this Map maps one or more keys to the specified |
|  |  |  \* value. _Note_: This method requires a full internal traversal of the |
|  | @@ -351,6 +367,7 @@ public boolean containsValue\( Object val \) { |  |
|  |  |  \* most one such mapping.\) |
|  |  |  \* @throws NullPointerException if the specified key is null \*/ |
|  |  |  // Never returns a Prime nor a Tombstone. |
|  |  |  @SuppressWarnings\("unchecked"\) |
|  |  |  public final TypeV get\( long key \) { |
|  |  |  if\( key == NO\_KEY \) { |
|  |  |  final Object V = \_val\_1; |
|  | @@ -395,6 +412,17 @@ private void help\_copy\( \) { |  |
|  |  |  topchm.help\_copy\_impl\(false\); |
|  |  |  } |
|  |  |  |
|  |  |  // --- hash ---------------------------------------------------------------- |
|  |  |  // Helper function to spread lousy hashCodes Throws NPE for null Key, on |
|  |  |  // purpose - as the first place to conveniently toss the required NPE for a |
|  |  |  // null Key. |
|  |  |  private static final int hash\(long h\) { |
|  |  |  h ^= \(h&gt;&gt;&gt;20\) ^ \(h&gt;&gt;&gt;12\); |
|  |  |  h ^= \(h&gt;&gt;&gt; 7\) ^ \(h&gt;&gt;&gt; 4\); |
|  |  |  h += h&lt;&lt;7; // smear low bits up high, for hashcodes that only differ by 1 |
|  |  |  return \(int\)h; |
|  |  |  } |
|  |  |  |
|  |  |  |
|  |  |  // --- CHM ----------------------------------------------------------------- |
|  |  |  // The control structure for the NonBlockingHashMapLong |
|  | @@ -403,7 +431,7 @@ private void help\_copy\( \) { |  |
|  |  |  final NonBlockingHashMapLong \_nbhml; |
|  |  |  |
|  |  |  // Size in active K,V pairs |
|  |  |  private final ConcurrentAutoTable \_size; |
|  |  |  private ConcurrentAutoTable \_size; |
|  |  |  public int size \(\) { return \(int\)\_size.get\(\); } |
|  |  |  |
|  |  |  // --- |
|  | @@ -416,7 +444,7 @@ private void help\_copy\( \) { |  |
|  |  |  // keys\) then we need a larger table to cut down on the churn. |
|  |  |  |
|  |  |  // Count of used slots, to tell when table is full of dead unusable slots |
|  |  |  private final ConcurrentAutoTable \_slots; |
|  |  |  private ConcurrentAutoTable \_slots; |
|  |  |  public int slots\(\) { return \(int\)\_slots.get\(\); } |
|  |  |  |
|  |  |  // --- |
|  | @@ -469,6 +497,13 @@ private boolean CAS\_val\( int idx, Object old, Object val \) { |  |
|  |  |  \_keys = new long \[1&lt;&lt;logsize\]; |
|  |  |  \_vals = new Object\[1&lt;&lt;logsize\]; |
|  |  |  } |
|  |  |  // Non-atomic clear |
|  |  |  void clear\(\) { |
|  |  |  \_size = new ConcurrentAutoTable\(\); |
|  |  |  \_slots= new ConcurrentAutoTable\(\); |
|  |  |  Arrays.fill\(\_keys,0\); |
|  |  |  Arrays.fill\(\_vals,null\); |
|  |  |  } |
|  |  |  |
|  |  |  // --- print innards |
|  |  |  private void print\(\) { |
|  | @@ -501,8 +536,9 @@ private void print2\( \) { |  |
|  |  |  // --- get\_impl ---------------------------------------------------------- |
|  |  |  // Never returns a Prime nor a Tombstone. |
|  |  |  private Object get\_impl \( final long key \) { |
|  |  |  final int hash = hash\(key\); |
|  |  |  final int len = \_keys.length; |
|  |  |  int idx = \(int\)\(key & \(len-1\)\); // First key hash |
|  |  |  int idx = \(hash & \(len-1\)\); // First key hash |
|  |  |  |
|  |  |  // Main spin/reprobe loop, looking for a Key hit |
|  |  |  int reprobe\_cnt=0; |
|  | @@ -545,11 +581,12 @@ private Object get\_impl \( final long key \) { |  |
|  |  |  // Only the path through copy\_slot passes in an expected value of null, |
|  |  |  // and putIfMatch only returns a null if passed in an expected null. |
|  |  |  private Object putIfMatch\( final long key, final Object putval, final Object expVal \) { |
|  |  |  final int hash = hash\(key\); |
|  |  |  assert putval != null; |
|  |  |  assert !\(putval instanceof Prime\); |
|  |  |  assert !\(expVal instanceof Prime\); |
|  |  |  final int len = \_keys.length; |
|  |  |  int idx = \(int\)\(key & \(len-1\)\); // The first key |
|  |  |  int idx = \(hash & \(len-1\)\); // The first key |
|  |  |  |
|  |  |  // --- |
|  |  |  // Key-Claim stanza: spin till we can claim a Key \(or force a resizing\). |
|  | @@ -688,7 +725,7 @@ private Object putIfMatch\( final long key, final Object putval, final Object exp |  |
|  |  |  // current table, while a 'get' has decided the same key cannot be in this |
|  |  |  // table because of too many reprobes. The invariant is: |
|  |  |  // slots.estimate\_sum &gt;= max\_reprobe\_cnt &gt;= reprobe\_limit\(len\) |
|  |  |  private final boolean tableFull\( int reprobe\_cnt, int len \) { |
|  |  |  private boolean tableFull\( int reprobe\_cnt, int len \) { |
|  |  |  return |
|  |  |  // Do the cheap check first: we allow some number of reprobes always |
|  |  |  reprobe\_cnt &gt;= REPROBE\_LIMIT && |
|  | @@ -703,7 +740,7 @@ private final boolean tableFull\( int reprobe\_cnt, int len \) { |  |
|  |  |  // Since this routine has a fast cutout for copy-already-started, callers |
|  |  |  // MUST 'help\_copy' lest we have a path which forever runs through |
|  |  |  // 'resize' only to discover a copy-in-progress which never progresses. |
|  |  |  private final CHM resize\(\) { |
|  |  |  private CHM resize\(\) { |
|  |  |  // Check for resize already in progress, probably triggered by another thread |
|  |  |  CHM newchm = \_newchm; // VOLATILE READ |
|  |  |  if\( newchm != null \) // See if resize is already in progress |
|  | @@ -735,7 +772,7 @@ private final CHM resize\(\) { |  |
|  |  |  // the table needs a steady state of rare same-size resize |
|  |  |  // operations to clean out the dead keys. |
|  |  |  long tm = System.currentTimeMillis\(\); |
|  |  |  if\( newsz &lt;= oldlen && // New table would shrink or hold steady? |
|  |  |  if\( newsz &lt;= oldlen && // New table would shrink or hold steady? |
|  |  |  tm &lt;= \_nbhml.\_last\_resize\_milli+10000\) // Recent resize \(less than 10 sec ago\) |
|  |  |  newsz = oldlen&lt;&lt;1; // Double the existing size |
|  |  |  |
|  | @@ -823,7 +860,7 @@ private final CHM resize\(\) { |  |
|  |  |  // Help along an existing resize operation. We hope its the top-level |
|  |  |  // copy \(it was when we started\) but this CHM might have been promoted out |
|  |  |  // of the top position. |
|  |  |  private final void help\_copy\_impl\( final boolean copy\_all \) { |
|  |  |  private void help\_copy\_impl\( final boolean copy\_all \) { |
|  |  |  final CHM newchm = \_newchm; |
|  |  |  assert newchm != null; // Already checked by caller |
|  |  |  int oldlen = \_keys.length; // Total amount to copy |
|  | @@ -845,7 +882,8 @@ private final void help\_copy\_impl\( final boolean copy\_all \) { |  |
|  |  |  // thread counts trying to copy the table often 'panic'. |
|  |  |  if\( panic\_start == -1 \) { // No panic? |
|  |  |  copyidx = \(int\)\_copyIdx; |
|  |  |  while\( !\_copyIdxUpdater.compareAndSet\(this,copyidx,copyidx+MIN\_COPY\_WORK\) \) |
|  |  |  while\( copyidx &lt; \(oldlen&lt;&lt;1\) && // 'panic' check |
|  |  |  !\_copyIdxUpdater.compareAndSet\(this,copyidx,copyidx+MIN\_COPY\_WORK\) \) |
|  |  |  copyidx = \(int\)\_copyIdx; // Re-read |
|  |  |  if\( !\(copyidx &lt; \(oldlen&lt;&lt;1\)\) \) // Panic! |
|  |  |  panic\_start = copyidx; // Record where we started to panic-copy |
|  | @@ -897,23 +935,25 @@ private CHM copy\_slot\_and\_check\( int idx, Object should\_help \) { |  |
|  |  |  } |
|  |  |  |
|  |  |  // --- copy\_check\_and\_promote -------------------------------------------- |
|  |  |  private final void copy\_check\_and\_promote\( int workdone \) { |
|  |  |  private void copy\_check\_and\_promote\( int workdone \) { |
|  |  |  int oldlen = \_keys.length; |
|  |  |  // We made a slot unusable and so did some of the needed copy work |
|  |  |  long copyDone = \_copyDone; |
|  |  |  assert \(copyDone+workdone\) &lt;= oldlen; |
|  |  |  long nowDone = copyDone+workdone; |
|  |  |  assert nowDone &lt;= oldlen; |
|  |  |  if\( workdone &gt; 0 \) { |
|  |  |  while\( !\_copyDoneUpdater.compareAndSet\(this,copyDone,copyDone+workdone\) \) { |
|  |  |  copyDone = \_copyDone; // Reload, retry |
|  |  |  assert \(copyDone+workdone\) &lt;= oldlen; |
|  |  |  while\( !\_copyDoneUpdater.compareAndSet\(this,copyDone,nowDone\) \) { |
|  |  |  copyDone = \_copyDone; // Reload, retry |
|  |  |  nowDone = copyDone+workdone; |
|  |  |  assert nowDone &lt;= oldlen; |
|  |  |  } |
|  |  |  } |
|  |  |  |
|  |  |  // Check for copy being ALL done, and promote. Note that we might have |
|  |  |  // nested in-progress copies and manage to finish a nested copy before |
|  |  |  // finishing the top-level copy. We only promote top-level copies. |
|  |  |  if\( copyDone+workdone == oldlen && // Ready to promote this table? |
|  |  |  \_nbhml.\_chm == this && // Looking at the top-level table? |
|  |  |  if\( nowDone == oldlen && // Ready to promote this table? |
|  |  |  \_nbhml.\_chm == this && // Looking at the top-level table? |
|  |  |  // Attempt to promote |
|  |  |  \_nbhml.CAS\(\_chm\_offset,this,\_newchm\) \) { |
|  |  |  \_nbhml.\_last\_resize\_milli = System.currentTimeMillis\(\); // Record resize time for next check |
|  | @@ -970,7 +1010,7 @@ private boolean copy\_slot\( int idx \) { |  |
|  |  |  // appears in the old table. |
|  |  |  Object old\_unboxed = \(\(Prime\)oldval\).\_V; |
|  |  |  assert old\_unboxed != TOMBSTONE; |
|  |  |  \_newchm.putIfMatch\(key, old\_unboxed, null\); |
|  |  |  boolean copied\_into\_new = \(\_newchm.putIfMatch\(key, old\_unboxed, null\) == null\); |
|  |  |  |
|  |  |  // --- |
|  |  |  // Finally, now that any old value is exposed in the new table, we can |
|  | @@ -980,7 +1020,7 @@ private boolean copy\_slot\( int idx \) { |  |
|  |  |  while\( oldval != TOMBPRIME && !CAS\_val\(idx,oldval,TOMBPRIME\) \) |
|  |  |  oldval = \_vals\[idx\]; |
|  |  |  |
|  |  |  return oldval != TOMBPRIME; // True if we slammed the TOMBPRIME down |
|  |  |  return copied\_into\_new; |
|  |  |  } // end copy\_slot |
|  |  |  } // End of CHM |
|  |  |  |
|  | @@ -1048,8 +1088,9 @@ public void removeKey\(\) { |  |
|  |  |  |
|  |  |  @Override |
|  |  |  public void remove\(\) { |
|  |  |  // NOTE: it would seem logical that value removal will semantically mean removing the matching value for the |
|  |  |  // mapping, but the JDK always removes by key, even when the value has changed. |
|  |  |  // NOTE: it would seem logical that value removal will semantically mean |
|  |  |  // removing the matching value for the mapping, but the JDK always |
|  |  |  // removes by key, even when the value has changed. |
|  |  |  removeKey\(\); |
|  |  |  } |
|  |  |  |
|  | @@ -1137,6 +1178,7 @@ public void remove\(\) { |  |
|  |  |  } |
|  |  |  |
|  |  |  /\*\* Keys as a long array. Array may be zero-padded if keys are concurrently deleted. \*/ |
|  |  |  @SuppressWarnings\("unchecked"\) |
|  |  |  public long\[\] keySetLong\(\) { |
|  |  |  long\[\] dom = new long\[size\(\)\]; |
|  |  |  IteratorLong i=\(IteratorLong\)keySet\(\).iterator\(\); |
|  | @@ -1161,8 +1203,9 @@ public TypeV setValue\(final TypeV val\) { |  |
|  |  |  final SnapshotV \_ss; |
|  |  |  public SnapshotE\(\) { \_ss = new SnapshotV\(\); } |
|  |  |  public void remove\(\) { |
|  |  |  // NOTE: it would seem logical that entry removal will semantically mean removing the matching pair, but |
|  |  |  // the JDK always removes by key, even when the value has changed. |
|  |  |  // NOTE: it would seem logical that entry removal will semantically mean |
|  |  |  // removing the matching pair, but the JDK always removes by key, |
|  |  |  // even when the value has changed. |
|  |  |  \_ss.removeKey\(\); |
|  |  |  } |
|  |  |  public Map.Entry&lt;Long,TypeV&gt; next\(\) { \_ss.next\(\); return new NBHMLEntry\(\_ss.\_prevK,\_ss.\_prevV\); } |
|  | @@ -1225,6 +1268,7 @@ private void writeObject\(java.io.ObjectOutputStream s\) throws IOException { |  |
|  |  |  |
|  |  |  // --- readObject -------------------------------------------------------- |
|  |  |  // Read a CHM from a stream |
|  |  |  @SuppressWarnings\("unchecked"\) |
|  |  |  private void readObject\(java.io.ObjectInputStream s\) throws IOException, ClassNotFoundException { |
|  |  |  s.defaultReadObject\(\); // Read nothing |
|  |  |  initialize\(MIN\_SIZE\); |
|  | @@ -1236,4 +1280,34 @@ private void readObject\(java.io.ObjectInputStream s\) throws IOException, ClassNo |  |
|  |  |  } |
|  |  |  } |
|  |  |  |
|  |  |  /\*\* |
|  |  |  \* Creates a shallow copy of this hashtable. All the structure of the |
|  |  |  \* hashtable itself is copied, but the keys and values are not cloned. |
|  |  |  \* This is a relatively expensive operation. |
|  |  |  \* |
|  |  |  \* @return a clone of the hashtable. |
|  |  |  \*/ |
|  |  |  @SuppressWarnings\("unchecked"\) |
|  |  |  @Override |
|  |  |  public NonBlockingHashMapLong&lt;TypeV&gt; clone\(\) { |
|  |  |  try { |
|  |  |  // Must clone, to get the class right; NBHML might have been |
|  |  |  // extended so it would be wrong to just make a new NBHML. |
|  |  |  NonBlockingHashMapLong&lt;TypeV&gt; t = \(NonBlockingHashMapLong&lt;TypeV&gt;\) super.clone\(\); |
|  |  |  // But I don't have an atomic clone operation - the underlying \_kvs |
|  |  |  // structure is undergoing rapid change. If I just clone the \_kvs |
|  |  |  // field, the CHM in \_kvs\[0\] won't be in sync. |
|  |  |  // |
|  |  |  // Wipe out the cloned array \(it was shallow anyways\). |
|  |  |  t.clear\(\); |
|  |  |  // Now copy sanely |
|  |  |  for\( long K : keySetLong\(\) \) |
|  |  |  t.put\(K,get\(K\)\); |
|  |  |  return t; |
|  |  |  } catch \(CloneNotSupportedException e\) { |
|  |  |  // this shouldn't happen, since we are Cloneable |
|  |  |  throw new InternalError\(\); |
|  |  |  } |
|  |  |  } |
|  |  |  |
|  |  |  } // End NonBlockingHashMapLong class |

