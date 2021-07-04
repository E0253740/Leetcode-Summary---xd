# Java一些常见操作的写法
## 数组
## 字符串String
1. 把char[] 转化为String: 已知给定了一个char[] a; String b = new String(a);
## HashMap
Map<Integer, Integer> map = new HashMap<>();
1. 提取HashMap所有的值，转化为一个数组：__Integer[] arr = map.values.toArray(new Integer[map.size()]);__ 注意这里必须用Integer<br/>
2. 
## 其他一些常见技巧
1. 向上取整: (n-1)/2+1;
2. 
