public class Kmp {
    //next表的构造
    private int[]next;
    private void buildNext(String s){
        next=new int[s.length()];
        int t=next[0]=-1;
        int j=0;
        while(j<s.length()-1){
            if(t<0||s.charAt(j)==s.charAt(t)){
                j++;t++;
                next[j]=t;
            }
            else t=next[t];
        }
    }
    //KMP算法
    private int match(String p,String s){
        buildNext(p);
        int m=s.length();
        int n=p.length();
        int i=0,j=0;
        while(j<n&&i<m){
            if(j<0||s.charAt(i)==p.charAt(j)){
                i++;
                j++;
            }
            else j=next[j];
        }
        //如果找到子串了，j就会等于p.length()
        if(j>=n)return i-j;//i-j就是匹配的子串的开始位置
        else return -1;

    }
}
