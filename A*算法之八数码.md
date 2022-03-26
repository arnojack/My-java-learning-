# A*算法之八数码
[本文链接](https://github.com/arnojack/My-java-learning-/blob/main/A*%E7%AE%97%E6%B3%95%E4%B9%8B%E5%85%AB%E6%95%B0%E7%A0%81.md)
- open采用红黑树自动排序，closed采用set存储和查找。

-  : Nilsson's sequence score 尼尔森序列分数 t = h+3*s
    1. h是当前节点与目标节点的曼哈顿距离
    2. 假如当前棋盘中心和目标状态的中心不匹配，s加1分
    3. 假如当前方块顺时针方向上的第一个方块不是目标状态中它顺时针方向上第一个方块，那么s加2分

- 关于java要注意对象的引用和赋值，equals和hashcode的重写
![this is a picture](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jb3Vyc2VyYS5jcy5wcmluY2V0b24uZWR1L2FsZ3M0L2Fzc2lnbm1lbnRzLzhwdXp6bGUvZ2FtZS10cmVlLnBuZw?x-oss-process=image/format,png)
## 数据结构
```
public static int[] start={0,1,4,2,7,6,3,8,5};
public static int[] end=  {1,2,3,4,5,6,7,8,0};
public static int num_end =Eight_digit.Inver_end(end);
public static int index_z =Eight_digit.findnum(end,0);
TreeMap<Integer,Node> open=new TreeMap<>();
Set<Node> closed=new HashSet<>();
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
        int s=0;
        if(findnum(temp,0)!=index_z)
            s=1;
        for(int i=0;i<9;i++){
            int n=Math.abs(findnum(temp,end[i])-i);
            int next=i;
            {          //计算棋盘顺时针时下一个元素位置
                if((next+1)%3==0){
                    next+=3;
                    if(next>=9){
                        next-=4;
                    }
                }else if(next%3==0){
                    next-=3;
                }else {
                    next+=1;
                }if(temp[i]!=end[i]){
                s+=2;
                }
            }
            if(n>3){
                n=n/3;
                num+=(n+n%3);
            }else {
                num+=n;
            }
            num+=3*s;
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
            if(temp.getG()==index.getG()&&!temp.equals(index)){
                temp=temp.getParent();
            }
            open.remove(in,index);

            temp.setNext(index);
            index.setParent(temp);
            temp=index;
            if(getH(temp.getState())==0)break;

            closed.add(index);
            Node[] a=new Node[4];
            a[0]=right(index);
            a[1]=left(index);
            a[2]=down(index);
            a[3]=up(index);
            Boolean isanswer=false;
            for(int i=0;i<4;i++){
                if(closed.contains(a[i]))
                    continue;
                if( InverseNumber(a[i].getState(),num_end) && a[i].isMove()){
                    isanswer=true;
                    open.put(a[i].getF(),a[i]);
                }
            }
        }
```
