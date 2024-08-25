

```
  Arrays.sort(numbers, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2; // 升序排列
            }
        });
        
        
Arrays.sort(numbers, (o1, o2) -> o1 - o2); // 升序排列


Collections.sort(results, new Comparator<Integer>() {
            public int compare(Integer num1, Integer num2) {
                return num2 - num1;
            }
});
```

