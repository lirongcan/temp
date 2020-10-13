```
//合并排序
class mergesort{
    private int[]a;
    private int[]aux;//辅助数组
    mergesort(int[]a){
        this.a=a;
        aux=new int[a.length];
    }
    private void merge_help(int low, int high){
        if(low>=high)return;
        int mid=(low+high)/2;
        merge_help(low,mid);
        merge_help(mid+1,high);
        int l=low,h=mid+1;
        int i=l;
        while(h<=high){
            while (l<=mid&&a[l]<=a[h])aux[i++]=a[l++];
            aux[i++]=a[h++];
        }
        while(l<=mid)aux[i++]=a[l++];
        System.arraycopy(aux,low,a,low,high-low+1);
    }
    void sort(){
        merge_help(0,a.length-1);
    }
}

//冒泡排序
class bubblesort{
    private int[]a;
    bubblesort(int[]a){this.a=a;}
    private void swap(int i,int j){
        int temp=a[i];
        a[i]=a[j];
        a[j]=temp;
    }
    void sort(){
        for (int i = 0; i < a.length; i++) {
            for (int j = a.length-1; j > i; j--) {
                if(a[j]<a[j-1])swap(j,j-1);
            }
        }
    }
}

//插入排序
class insertsort{
    private int[]a;
    insertsort(int[]a){this.a=a;}
    private void swap(int i,int j){
        int temp=a[i];
        a[i]=a[j];
        a[j]=temp;
    }
    void sort(){
        for (int i = 1; i < a.length; i++) {
            for (int j = i; j > 0&&a[j] < a[j-1]; j--) {
                swap(j,j-1);
            }
        }
    }
}

//选择排序
class selectsort{
    private int[]a;
    selectsort(int[]a){this.a=a;}
    private void swap(int i,int j){
        int temp=a[i];
        a[i]=a[j];
        a[j]=temp;
    }
    void sort(){
        for (int i = 0; i < a.length - 1; i++) {
            int m=i;
            for (int j = i+1; j < a.length; j++) {
                if(a[j]<a[m])m=j;
            }
            swap(m,i);
        }
    }
}
```
