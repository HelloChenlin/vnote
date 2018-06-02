```
void resize(int newCapacity) {   //传入新的容量  
    Entry[] oldTable = table;    //引用扩容前的Entry数组  
    int oldCapacity = oldTable.length;  
    if (oldCapacity == MAXIMUM_CAPACITY) {  //扩容前的数组大小如果已经达到最大(2^30)了  
        threshold = Integer.MAX_VALUE; //修改阈值为int的最大值(2^31-1)，这样以后就不会扩容了  
        return;  
    }  
  
    Entry[] newTable = new Entry[newCapacity];  //初始化一个新的Entry数组  
    transfer(newTable);                         //！！将数据转移到新的Entry数组里  
    table = newTable;                           //HashMap的table属性引用新的Entry数组  
    threshold = (int) (newCapacity * loadFactor);//修改阈值  



void transfer(Entry[] newTable) {  
    Entry[] src = table;                   //src引用了旧的Entry数组  
    int newCapacity = newTable.length;  
    for (int j = 0; j < src.length; j++) { //遍历旧的Entry数组  
        Entry<K, V> e = src[j];             //取得旧Entry数组的每个元素  
        if (e != null) {  
            src[j] = null;//释放旧Entry数组的对象引用（for循环后，旧的Entry数组不再引用任何对象）  
            do {  
                Entry<K, V> next = e.next;  
                int i = indexFor(e.hash, newCapacity); //！！重新计算每个元素在数组中的位置  
                e.next = newTable[i]; //标记[1]  
                newTable[i] = e;      //将元素放在数组上  
                e = next;             //访问下一个Entry链上的元素  
            } while (e != null);  
        }  
    }  
}  

static int indexFor(int h, int length) {  
    return h & (length - 1);  
}  
```

问题1    `HashMap`扩容为什么是2倍扩容而不是`HashTable`的2倍-1？  
答：在2倍-1 的情况下，假设存在`key`的`hash`值为8和9，得到数组索引的下标都为8，增加了冲突的概率。  
&nbsp;  &nbsp; &nbsp; &nbsp;      如果是2倍，转换成2进制必定是n个1，与hash值进行位运算发生冲突的概率会大大降低。  
问题2   `HashMap`扩容的时候为什么用这种`hash`函数而不是对数组取模？  
答：位运算效率更高。  

##### HashMap扩容和死循环
```
void transfer(Entry[] newTable, boolean rehash) {  
        int newCapacity = newTable.length;  
        for (Entry<K,V> e : table) {  
  
            while(null != e) {  
                Entry<K,V> next = e.next;            ---------------------(1)  
                if (rehash) {  
                    e.hash = null == e.key ? 0 : hash(e.key);  
                }  
                int i = indexFor(e.hash, newCapacity);   
                e.next = newTable[i];  
                newTable[i] = e;  
                e = next;  
            } // while  
  
        }  
    }
```
