# Java一些常见操作的写法
## 数组
1. 克隆数组: int[] array = nums.clone();
2. [ [1,2], [2,3], [3,4], [1,3] ] <br/>
给这个数组排序:
```Java
Arrays.sort(intervals, new Comparator<int[]>(){
    public int compare(int[] line1, int[] line2){
        return line1[1] - line2[1];
    }
});
```
## 字符串String
1. 把char[] 转化为String: 已知给定了一个char[] a; String b = new String(a);
2. 相同长度字符串比较: str1.compareTo(str2);

## HashSet
1. 遍历的两种方法：<br/>
   1.1 for each<br/>
   2.1 迭代器<br/>
```Java
Iterator<Integer> it = set.iterator();
while(it.hasNext()){
    int num = it.next();
}
```    
## HashMap
Map<Integer, Integer> map = new HashMap<>();
1. 提取HashMap所有的值，转化为一个数组：__Integer[] arr = map.values.toArray(new Integer[map.size()]);__ 注意这里必须用Integer<br/>
2. 遍历HashMap
```Java
for (Map.Entry<Integer, Integer> entry : occurrences.entrySet()) {
    int num = entry.getKey(), count = entry.getValue();
    if (queue.size() == k) {
        if (queue.peek()[1] < count) {
            queue.poll();
            queue.offer(new int[]{num, count});
        }
    } else {
        queue.offer(new int[]{num, count});
    }
}
```
## StringBuilder
1. StringBuilder append int类型：sb.append(""+5);
2. StringBuilder清空 sb.delete(0,sb.length());
## 其他一些常见技巧
1. 向上取整: (n-1)/2+1;
2. PriorityQueue自定义排序方法
```Java
PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b) -> 
  a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]
);
```
## 优先队列
大根堆: 
```Java
PriorityQueue<Integer> l = new PriorityQueue<>((a,b)->b-a);
```
小根堆: 
```Java
PriorityQueue<Integer> l = new PriorityQueue<>((a,b)->a-b);
```

