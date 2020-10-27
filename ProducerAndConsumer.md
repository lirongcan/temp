```
class Good {
    static final int MAX_CONSUMER=5;
    static final int MAX_PRODUCER=10;
    static ReentrantLock lock=new ReentrantLock();
    static Condition condition = lock.newCondition();
    static LinkedList<Good> p=new LinkedList<>();
    static private volatile int i=0;
    String getId(){
        i++;
        return "good_"+String.valueOf(i);
    }
}

class producer implements Runnable{
    @Override
    public void run(){
        while(true){
            try{
                Good.lock.lock();
                Thread.sleep(1000);
                if(Good.p.size()< Good.MAX_CONSUMER){
                    Good temp=new Good();
                    Good.p.add(temp);
                    System.out.println(Thread.currentThread().getName()+" is producing "+temp.getId());
                    if(Good.p.size()==1) Good.condition.signal();
                }
                else Good.condition.await();
            }
            catch(InterruptedException e){
                e.printStackTrace();
            }
            finally {
                Good.lock.unlock();
            }
        }
    }
}

class consumer implements Runnable{
    @Override
    public void run(){
        while(true){
            try{
                Good.lock.lock();
                Thread.sleep(1000);
                if(Good.p.size()>0){
                    System.out.println(Thread.currentThread().getName()+" is consuming "+ Good.p.remove(0).getId());
                    if(Good.p.size()==0) Good.condition.signal();
                }
                else Good.condition.await();
            }
            catch(InterruptedException e){
                e.printStackTrace();
            }
            finally {
                Good.lock.unlock();
            }
        }
    }
}

public class ReentranLockProAndCon {
    public void test(){
        for (int i = 0; i < Good.MAX_CONSUMER; i++) {
            new Thread(new consumer(), "Consumer"+String.valueOf(i)).start();
        }
        for (int i = 0; i < Good.MAX_PRODUCER; i++) {
            new Thread(new producer(),"Producor"+String.valueOf(i)).start();
        }
    }
}
```
