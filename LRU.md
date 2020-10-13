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
