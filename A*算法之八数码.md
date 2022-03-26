# A*算法之八数码
[本文链接](https://github.com/arnojack/My-java-learning-/edit/main/A*%E7%AE%97%E6%B3%95%E4%B9%8B%E5%85%AB%E6%95%B0%E7%A0%81)
-open采用红黑树自动排序，closed采用hashmap存储和查找。
-关于java要注意对象的引用和赋值，equals和hashcode的重写
## 数据结构
 public static int[] start={2,8,3,1,6,4,7,0,5};
    public static int[] end=  {1,2,3,8,0,4,7,6,5};
    public static int num_end =Eight_digit.Inver_end(end);
    TreeMap<Integer,Node> open=new TreeMap<>();
    HashMap<Integer,Node> closed=new HashMap<>();
