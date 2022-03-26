# A*算法之八数码
[本文链接](https://github.com/arnojack/My-java-learning-/edit/main/A*%E7%AE%97%E6%B3%95%E4%B9%8B%E5%85%AB%E6%95%B0%E7%A0%81)
- open采用红黑树自动排序，closed采用hashmap存储和查找。

- 估价函数采用每对不同两数之间间隔的步数之和

- 关于java要注意对象的引用和赋值，equals和hashcode的重写
![this is a picture](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jb3Vyc2VyYS5jcy5wcmluY2V0b24uZWR1L2FsZ3M0L2Fzc2lnbm1lbnRzLzhwdXp6bGUvZ2FtZS10cmVlLnBuZw?x-oss-process=image/format,png)
## 数据结构
```
public static int[] start={2,8,3,1,6,4,7,0,5};
public static int[] end=  {1,2,3,8,0,4,7,6,5};
public static int num_end =Eight_digit.Inver_end(end);
TreeMap<Integer,Node> open=new TreeMap<>();
HashMap<Integer,Node> closed=new HashMap<>();
```
## 主要函数
### 自定义Node
```
public class Node{
    private Node parent;
    private Node next;
    private int[] state;
    private int F;
    private int G;

    @Override
    public int hashCode() {
        if (state == null)
            return 0;

        int result = 1;
        //hashCode核心计算
        //前一对象hashCode*31 + 后一对象hashCode, 并依次累加
        //注意: 乘积系数31为系统选定的较优系数, 参见String的hashCode方法, 下面也有详细介绍
        for (Object element : state)
            result = 31 * result + (element == null ? 0 : element.hashCode());
        return result;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Node user = (Node) o;
        int []a=user.getState();
        int []b=state;
        for(int i=0;i<b.length;i++)
            if(a[i]!=b[i])return false;
        return true;
    }
```
### Node的初始化
```
public static Node init(Node parent,int[]a,Node next){
        int [] n=new int[9];// 这是由于java传参是对象的引用，必须新建一个对象，否则原值会随a的值改变而改变
        for(int i=0;i<9;i++)
            n[i]=a[i];
        Node node=new Node();
        node.setParent(parent);
        node.setState(n);
        node.setNext(next);
        return node;
    }
```
### 向左
```
public static Node left(Node no){
        Node node=init(null,no.getState(),null);
        node.setG(no.getG());
        int index=findnum(node.getState(),0);
        if(index%3==0){
            node.setMove(false);
            return node;
        }else {
            node.setMove(true);
            node.setState(swap(node.getState(),index,index-1));
        }
        node.setG(node.getG()+1);
        node.setF(node.getG()+getH(node.getState()));
        return node;
    }
```
### 向右
```
public static Node right(Node no){
        Node node=init(null,no.getState(),null);
        node.setG(no.getG());
        int index=findnum(node.getState(),0);
        if((index+1)%3==0){
            node.setMove(false);
            return node;
        }else {
            node.setMove(true);
            node.setState(swap(node.getState(),index,index+1));
        }
        node.setG(node.getG()+1);
        node.setF(node.getG()+getH(node.getState()));
        return node;
    }
```
### 向上
```
public static Node up(Node no){
        Node node=init(null,no.getState(),null);
        node.setG(no.getG());
        int index=findnum(node.getState(),0);
        if(index<3){
            node.setMove(false);
            return node;
        }else {
            node.setMove(true);
            node.setState(swap(node.getState(),index,index-3));
        }
        node.setG(node.getG()+1);
        node.setF(node.getG()+getH(node.getState()));

        return node;
    }
```
### 向下
```
public static Node down(Node no){
        Node node=init(null,no.getState(),null);
        node.setG(no.getG());
        int index=findnum(node.getState(),0);
        if(index>5){
            node.setMove(false);
            return node;
        }else {
            node.setMove(true);
            node.setState(swap(node.getState(),index,index+3));
        }
        node.setG(node.getG()+1);
        node.setF(node.getG()+getH(node.getState()));
        return node;
    }
```
### 估价函数
```
public static int getH(int []temp){
        int num=0;
        for(int i=0;i<9;i++){
            int n=Math.abs(findnum(temp,end[i])-i);
            if(n>3){
                n=n/3;
                num+=(n+n%3);
            }else {
                num+=n;
            }
        }
        return num;
    }
```
### A*算法实例
```
public void Digit(Integer key,Node head){
        open.put(key,head);
        Node temp=head;
        while (true){
            int in=open.firstKey();
            Node index=open.get(in);
            open.remove(in,index);

            temp.setNext(index);
            index.setParent(temp);
            temp=index;
            if(getH(temp.getState())==0)break;

            closed.put(in,index);
            Node[] a=new Node[4];
            a[0]=right(index);
            a[1]=left(index);
            a[2]=down(index);
            a[3]=up(index);
            Boolean isanswer=false;
            for(int i=0;i<4;i++){
                if(closed.containsValue(a[i]))
                    continue;
                if( InverseNumber(a[i].getState(),num_end) && a[i].isMove()){
                    isanswer=true;
                    open.put(a[i].getF(),a[i]);
                }
            }
            if(!isanswer){
                temp=temp.getParent();
            }
        }

    }
```
