```
class Goods{
    private int id;
    private String name;

    Goods(int id,String name){
        this.id=id;
        this.name=name;
    }
}
class Producer implements Runnable{
    @Override
    public void run() {
        while(true){
            try{
                Thread.sleep(2000);
            }catch (InterruptedException e){
                e.printStackTrace();
            }
            synchronized (ProducerAndConsumer.queue){
                Goods goods = new Goods(1, "first");
                if(ProducerAndConsumer.queue.size()<ProducerAndConsumer.MAX_CONSUMER){
                    ProducerAndConsumer.queue.add(goods);
                    System.out.println(Thread.currentThread().getName()+" producing...");
                }
                else{
                    try{
                        ProducerAndConsumer.queue.wait();
                    }
                    catch (InterruptedException e){
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}

class Consumer implements Runnable{
    @Override
    public void run() {
        while(true){
            try{
                Thread.sleep(2000);
            }
            catch (InterruptedException e){
                e.printStackTrace();
            }
            synchronized (ProducerAndConsumer.queue){
                if(!ProducerAndConsumer.queue.isEmpty()){
                    ProducerAndConsumer.queue.poll();
                    System.out.println(Thread.currentThread().getName()+" consuming...");
                }
                else{
                    ProducerAndConsumer.queue.notify();
                }
            }
        }
    }
}
public class ProducerAndConsumer {
    static final int MAX_POOL=10;
    static final int MAX_PRODUCER=5;
    static final int MAX_CONSUMER=4;
    static final Queue<Goods> queue=new ArrayBlockingQueue<>(MAX_POOL);
    public static void test(){
        Producer producer=new Producer();
        Consumer consumer=new Consumer();
        for (int i = 0; i < MAX_PRODUCER; i++) {
            Thread threadA=new Thread(producer,"Producer"+i);
            threadA.start();
        }
        for (int i = 0; i < MAX_CONSUMER; i++) {
            Thread threadB=new Thread(consumer,"Consumer"+i);
            threadB.start();
        }
    }
}
```
