# Java一些常见操作的写法
## 数组
## 字符串String
1. 把char[] 转化为String: char[] a; String b = new String(a);
## HashMap
Map<Integer, Integer> map = new HashMap<>();
1. 提取HashMap所有的值，转化为一个数组：__Integer[] arr = map.values.toArray(new Integer[map.size()]);__ 注意这里必须用Integer<br/>
2. 
