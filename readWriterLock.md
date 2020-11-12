```
//C++
class lock{
    semaphore mutex=1;
    semaphore db=1;
    int readers=0;

    void rlock(){
        down(&mutex);//锁住readers
        readers++;//操作readers
        if(readers==1)down(&db);//获取数据锁
        up(&mutex);
    }
    void runlock(){
        down(&mutex);//锁住readers
        readers--;
        if(readers==0)up(&db);
        up(&mutex);
    }
    void wlock(){
        down(&db);
    }
    void wunlock(){
        up(&db);
    }
}
```
class lock{
    //修改readers的锁
    semaphore mutex=1;
    //修改共享资源的锁
    semaphore db=1;
    //“队列”锁
    semaphore queue=1;
    int readers=0;

    void rlock(){
        down(&queue);
        down(&mutex);
        readers++;
        if(readers==1)down(&db);
        up(&mutex);
        up(&queue);
    }
    void runlock(){
        down(&mutex);
        readers--;
        if(readers==0)up(&db);
        up(&mutex);
    }
    void wlock(){
        down(&queue);
        down(&db);
        up(&queue);
    }
    void wunlock(){
        up(&db);
    }
}
```
```
class lock{
    semaphore rmutex=1;//修改读者数量的互斥锁
    semaphore wmutex=1;//修改者数量的互斥锁
    semaphore queue=1;//队列锁，写锁获取队列锁防止后续读者
    semaphore db=1;//数据互斥锁
    int writers=0;//写者数量
    int readers=0;//读者数量

    void rlock(){
        down(&queue);//先获取队列锁
        down(&rmutex);//获取修改读者数量变量的锁
        readers++;
        if（readrs==1)down(&db);
        up(&rmutex);
        up(&queue);
    }
    void runlock(){
        down(&rmutex);
        readers--;
        if(readers==0)up(&db);
        up(&rmutex);
    }
    void wlock(){
        down(&wmutex);
        writers++;
        if(writers==1)down(&queue);//如果是第一个写者了，获取队列锁，屏蔽掉读者
        up(&wmutex);
        down(&db);//等待独占锁
    }
    void wunlock(){
        //感觉释放db的锁放在最后也行？
        up(&db);
        down(&wmutex);
        writers--;
        if(writers==0)up(&queue);
        up(&wmutex);
    }
}
```
