# ThreadLocal源码解析

`ThreadLocalMap`

```java
static class ThreadLocalMap {

        /**
         * 此哈希映射中的条目扩展了 WeakReference，使用其主 ref 字段作为键（始终是 					ThreadLocal 对象）。请注意，空键（即 entry.get() == null）意味着不再引用该			键，因此可以从表中删除该条目。在下面的代码中，此类条目称为“陈旧条目”
         */
        static class Entry extends WeakReference<ThreadLocal<?>> {
            /** The value associated with this ThreadLocal. */
            Object value;

            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }

        /**
         * 初始容量 - 必须是 2 的幂
         */
        private static final int INITIAL_CAPACITY = 16;

        /**
         * 表，根据需要调整大小。 table.length 必须始终是 2 的幂
         */
        private Entry[] table;

        /**
         * 表中的条目数
         */
        private int size = 0;

        /**
         * 要调整大小的下一个大小值
         */
        private int threshold; // Default to 0

        /**
         * 设置调整大小阈值以保持最坏的 2/3 负载因子
         */
        private void setThreshold(int len) {
            threshold = len * 2 / 3;
        }

        /**
         * 增量 i 模 len.
         */
        private static int nextIndex(int i, int len) {
            return ((i + 1 < len) ? i + 1 : 0);
        }

        /**
         * 递减i 模 len.
         */
        private static int prevIndex(int i, int len) {
            return ((i - 1 >= 0) ? i - 1 : len - 1);
        }

        /**
         * 构造一个最初包含 (firstKey, firstValue) 的新映射。 ThreadLocalMaps 是惰性构			建的，所以我们只有在至少有一个条目可以放入时才创建一个
         */
        ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
            table = new Entry[INITIAL_CAPACITY];
            int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);
            table[i] = new Entry(firstKey, firstValue);
            size = 1;
            setThreshold(INITIAL_CAPACITY);
        }

        /**
         * 从给定的父映射构造一个包含所有可继承 ThreadLocals 的新映射。仅由 							createInheritedMap 调用
         * @param parentMap the map associated with parent thread.
         */
        private ThreadLocalMap(ThreadLocalMap parentMap) {
            Entry[] parentTable = parentMap.table;
            int len = parentTable.length;
            setThreshold(len);
            table = new Entry[len];

            for (int j = 0; j < len; j++) {
                Entry e = parentTable[j];
                if (e != null) {
                    @SuppressWarnings("unchecked")
                    ThreadLocal<Object> key = (ThreadLocal<Object>) e.get();
                    if (key != null) {
                        Object value = key.childValue(e.value);
                        Entry c = new Entry(key, value);
                        int h = key.threadLocalHashCode & (len - 1);
                        while (table[h] != null)
                            h = nextIndex(h, len);
                        table[h] = c;
                        size++;
                    }
                }
            }
        }

        /**
         * 获取与键关联的条目。此方法本身仅处理快速路径：直接点击现有键。否则它会中继到 					getEntryAfterMiss。这旨在最大限度地提高直接命中的性能，部分原因是使该方法易于内			联
         *
         * @param  key the thread local object
         * @return the entry associated with key, or null if no such
         */
        private Entry getEntry(ThreadLocal<?> key) {
            int i = key.threadLocalHashCode & (table.length - 1);
            Entry e = table[i];
            if (e != null && e.get() == key)
                return e;
            else
                return getEntryAfterMiss(key, i, e);
        }

        /**
         * 当在其直接哈希槽中找不到键时使用的 getEntry 方法版本
         *
         * @param  key the thread local object
         * @param  i the table index for key's hash code
         * @param  e the entry at table[i]
         * @return the entry associated with key, or null if no such
         */
        private Entry getEntryAfterMiss(ThreadLocal<?> key, int i, Entry e) {
            Entry[] tab = table;
            int len = tab.length;

            while (e != null) {
                ThreadLocal<?> k = e.get();
                if (k == key)
                    return e;
                if (k == null)
                    expungeStaleEntry(i);
                else
                    i = nextIndex(i, len);
                e = tab[i];
            }
            return null;
        }

        /**
         * 设置与键关联的值
         *
         * @param key the thread local object
         * @param value the value to be set
         */
        private void set(ThreadLocal<?> key, Object value) {

          // 我们不像 get() 那样使用快速路径，因为使用 set() 创建新条目与替换现有条目一样普				//遍，在这种情况下，快速路径往往会失败.

            Entry[] tab = table;
            int len = tab.length;
            int i = key.threadLocalHashCode & (len-1);

            for (Entry e = tab[i];
                 e != null;
                 e = tab[i = nextIndex(i, len)]) {
                ThreadLocal<?> k = e.get();

                if (k == key) {
                    e.value = value;
                    return;
                }

                if (k == null) {
                    replaceStaleEntry(key, value, i);
                    return;
                }
            }

            tab[i] = new Entry(key, value);
            int sz = ++size;
            if (!cleanSomeSlots(i, sz) && sz >= threshold)
                rehash();
        }

        /**
         * Remove the entry for key.
         */
        private void remove(ThreadLocal<?> key) {
            Entry[] tab = table;
            int len = tab.length;
            int i = key.threadLocalHashCode & (len-1);
            for (Entry e = tab[i];
                 e != null;
                 e = tab[i = nextIndex(i, len)]) {
                if (e.get() == key) {
                    e.clear();
                    expungeStaleEntry(i);
                    return;
                }
            }
        }

        /**
             用指定键的条目替换在设置操作期间遇到的陈旧条目。在 value 参数中传递的值					存储在条目中，无论指定键是否已经存在条目。作为副作用，此方法会删除包含陈旧条目的“运					行”中的所有陈旧条目。 （运行是两个空槽之间的一系列条目。）
         *
         * @param  key the key
         * @param  value the value to be associated with key
         * @param  staleSlot index of the first stale entry encountered while
         *         searching for key.
         */
        private void replaceStaleEntry(ThreadLocal<?> key, Object value,
                                       int staleSlot) {
            Entry[] tab = table;
            int len = tab.length;
            Entry e;

           //备份以检查当前运行中的先前陈旧条目。我们一次清理整个运行，以避免由于垃圾收集器释放			//成束的引用（即，每当收集器运行时）而导致的持续增量重新散列
            int slotToExpunge = staleSlot;
            for (int i = prevIndex(staleSlot, len);
                 (e = tab[i]) != null;
                 i = prevIndex(i, len))
                if (e.get() == null)
                    slotToExpunge = i;

            // 查找运行的键或尾随空槽，以先发生者为准
            for (int i = nextIndex(staleSlot, len);
                 (e = tab[i]) != null;
                 i = nextIndex(i, len)) {
                ThreadLocal<?> k = e.get();

               //如果我们找到密钥，那么我们需要将它与陈旧的条目交换以维护哈希表的顺序。然后
                //可以将新的陈旧槽或在其上方遇到的任何其他陈旧槽发送到 expungeStaleEntry 					//以删除或重新散列运行中的所有其他条目。
                if (k == key) {
                    e.value = value;

                    tab[i] = tab[staleSlot];
                    tab[staleSlot] = e;

                    // 如果存在，则从先前的陈旧条目开始删除
                    if (slotToExpunge == staleSlot)
                        slotToExpunge = i;
                    cleanSomeSlots(expungeStaleEntry(slotToExpunge), len);
                    return;
                }

                // 如果我们在反向扫描中没有找到过时的条目，那么在扫描密钥时看到的第一个过时的				//条目是第一个仍然存在于运行中的条目。
                if (k == null && slotToExpunge == staleSlot)
                    slotToExpunge = i;
            }

            //如果找不到密钥，则将新条目放入陈旧的插槽中
            tab[staleSlot].value = null;
            tab[staleSlot] = new Entry(key, value);

            // 如果运行中有任何其他陈旧条目，请删除它们
            if (slotToExpunge != staleSlot)
                cleanSomeSlots(expungeStaleEntry(slotToExpunge), len);
        }

        /**
         * 通过重新散列位于 staleSlot 和下一个空槽之间的任何可能冲突的条目来删除过时的条目。			这也会删除在尾随 null 之前遇到的任何其他陈旧条目
         .  See
         * Knuth, Section 6.4
         *
         * @param staleSlot index of slot known to have null key
         * @return the index of the next null slot after staleSlot
         * (all between staleSlot and this slot will have been checked
         * for expunging).
         */
        private int expungeStaleEntry(int staleSlot) {
            Entry[] tab = table;
            int len = tab.length;

            // expunge entry at staleSlot
            tab[staleSlot].value = null;
            tab[staleSlot] = null;
            size--;

            // Rehash until we encounter null
            Entry e;
            int i;
            for (i = nextIndex(staleSlot, len);
                 (e = tab[i]) != null;
                 i = nextIndex(i, len)) {
                ThreadLocal<?> k = e.get();
                if (k == null) {
                    e.value = null;
                    tab[i] = null;
                    size--;
                } else {
                    int h = k.threadLocalHashCode & (len - 1);
                    if (h != i) {
                        tab[i] = null;

                        // Unlike Knuth 6.4 Algorithm R, we must scan until
                        // null because multiple entries could have been stale.
                        while (tab[h] != null)
                            h = nextIndex(h, len);
                        tab[h] = e;
                    }
                }
            }
            return i;
        }

        /**
         * 启发式地扫描一些单元格以查找过时的条目。当添加新元素或删除另一个陈旧元素时调用此方			法。它执行对数扫描，作为不扫描（快速但保留垃圾）和与元素数量成比例的扫描数量之间的				平衡，这将找到所有垃圾但会导致一些插入花费 O(n) 时间。
         *
         * @param i a position known NOT to hold a stale entry. The
         * scan starts at the element after i.
         *
         * @param n scan control: {@code log2(n)} cells are scanned,
         * unless a stale entry is found, in which case
         * {@code log2(table.length)-1} additional cells are scanned.
         * When called from insertions, this parameter is the number
         * of elements, but when from replaceStaleEntry, it is the
         * table length. (Note: all this could be changed to be either
         * more or less aggressive by weighting n instead of just
         * using straight log n. But this version is simple, fast, and
         * seems to work well.)
         *
         * @return true if any stale entries have been removed.
         */
        private boolean cleanSomeSlots(int i, int n) {
            boolean removed = false;
            Entry[] tab = table;
            int len = tab.length;
            do {
                i = nextIndex(i, len);
                Entry e = tab[i];
                if (e != null && e.get() == null) {
                    n = len;
                    removed = true;
                    i = expungeStaleEntry(i);
                }
            } while ( (n >>>= 1) != 0);
            return removed;
        }

        /**
         * 重新打包和/或重新调整表格大小。首先扫描整个表删除过时的条目。如果这不能充分缩小表格			的大小，请将表格大小加倍
         */
        private void rehash() {
            expungeStaleEntries();

            // Use lower threshold for doubling to avoid hysteresis
            if (size >= threshold - threshold / 4)
                resize();
        }

        /**
         * table的容量翻倍
         */
        private void resize() {
            Entry[] oldTab = table;
            int oldLen = oldTab.length;
            int newLen = oldLen * 2;
            Entry[] newTab = new Entry[newLen];
            int count = 0;

            for (int j = 0; j < oldLen; ++j) {
                Entry e = oldTab[j];
                if (e != null) {
                    ThreadLocal<?> k = e.get();
                    if (k == null) {
                        e.value = null; // Help the GC
                    } else {
                        int h = k.threadLocalHashCode & (newLen - 1);
                        while (newTab[h] != null)
                            h = nextIndex(h, newLen);
                        newTab[h] = e;
                        count++;
                    }
                }
            }

            setThreshold(newLen);
            size = count;
            table = newTab;
        }

        /**
         *清除表中所有过时的条目
         */
        private void expungeStaleEntries() {
            Entry[] tab = table;
            int len = tab.length;
            for (int j = 0; j < len; j++) {
                Entry e = tab[j];
                if (e != null && e.get() == null)
                    expungeStaleEntry(j);
            }
        }
    }
```

