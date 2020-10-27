```
public class LRU {
    static class Node{
        int key,value;
        Node(int key,int value){
            this.key=key;
            this.value=value;
        }
    }
    LinkedList<Node>cache;
    int size;
    LRU(int size){
        this.cache=new LinkedList<>();
        this.size=size;
    }
    //返回-1表示没找到
    int get(int key){
        Iterator<Node>iterator=cache.descendingIterator();
        int result=-1;
        while(iterator.hasNext()){
            Node node=iterator.next();
            if(node.key==key){
                result=node.value;
                iterator.remove();
                cache.add(new Node(key, result));
                break;
            }
        }
        return result;
    }
    void put(int key,int value){
        Iterator<Node>iterator=cache.iterator();
        while(iterator.hasNext()){
            Node node=iterator.next();
            if(node.key==key){
                iterator.remove();
                break;
            }
        }
        if(size==cache.size())cache.removeFirst();
        cache.add(new Node(key,value));
    }
}
```
```
public class SimpleLRUCache<K, V> {
    //缓冲区大小
    private final int MAX_CACHE_SIZE;
    //默认的负载因子
    private final float DEFAULT_LOAD_FACTORY = 0.75f;
 
    LinkedHashMap<K, V> map;
 
    public SimpleLRUCache(int cacheSize) {
        MAX_CACHE_SIZE = cacheSize;
        int capacity = (int)Math.ceil(MAX_CACHE_SIZE / DEFAULT_LOAD_FACTORY) + 1;
        //创建一个重写了removeEldestEntry方法的LinkedHashMap
        map = new LinkedHashMap<K, V>(capacity, DEFAULT_LOAD_FACTORY, true) {
			private static final long serialVersionUID = 1L;
			@Override
            protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
                //如果将结点插入map后map的大小大于缓冲区大小，返回true，删除头结点
                return size() > MAX_CACHE_SIZE;
            }
        };
    }
    //为了线程安全，给用到的方法全部加synchronized
    public synchronized void put(K key, V value) {
        map.put(key, value);
    }
    public synchronized V get(K key) {
        return map.get(key);
    }
    public synchronized void remove(K key) {
        map.remove(key);
    }
    public synchronized Set<Map.Entry<K, V>> entrySet() {
        return map.entrySet();
    }
    @Override
    public String toString() {
        StringBuilder stringBuilder = new StringBuilder();
        for (Map.Entry<K, V> entry : map.entrySet()) {
            stringBuilder.append(String.format("%s: %s  ", entry.getKey(), entry.getValue()));
        }
        return stringBuilder.toString();
    }
}
```
