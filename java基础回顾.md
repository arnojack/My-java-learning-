
# Myjavalearning
## java基础回顾
- 类与构造器
- 封装的回顾
### 类与构造器
 1. 类名首字母应该大写，"驼峰写法"。
 2. 一个java代码文件中可以定义多个类，但是只能有一个类用public修饰
 3. 类的五大成分（1成员变量，2成员方法，3构造器，4代码块，5内部类）
### 封装的回顾
- 封装的作用
  1. 提高安全性
  2. 实现代码的组件化
- 封装的核心思想: 合理隐藏，合理暴露
### java Number & Math
- 方法
 1. parseInt()  将字符串解析为int类型
 2. xxxValue()  将Number对象转换为xxx数据类型的值并返回
 ### java Character
 - 方法
   1. isLetter()   是否是一个字母
   2. isDigit()    是否是一个数字字符
   3. isUpperCase()是否是大写字母
   4. isLowerCase()是否是小写字母
   5.	toUpperCase()指定字母的大写形式
   6.	toLowerCase()指定字母的小写形式
### java String
-格式化数字
输出格式化数字可以使用 printf() 和 format() 方法
如：printf("浮点型变量的值为 " +
                  "%f, 整型变量的值为 " +
                  " %d, 字符串变量的值为 " +
                  "is %s", floatVar, intVar, stringVar);
### java StringBuffer
- stringbuffer 线程安全的
- stringbuilder 非线程安全
- 方法
 1. append（String s）
 2. reverse()
 3. delete(int start,int end)
 4. insert(int offset,int i)
 5. replace(int start,int end,String str)
 ### java Arrays
 - 方法
 1.	 int binarySearch(Object[] a, Object key)
用二分查找算法。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(插入点) - 1)。
2.	 boolean equals(type[] a, type[] a2)
如果两个指定的 type 型数组彼此相等，则返回 true。
3.	 void fill(type[] a, type val)
将指定的 type 值分配给指定 type 型数组指定范围中的每个元素。
4.	 void sort(Object[] a)
对指定对象数组根据其元素的自然顺序进行升序排列。
