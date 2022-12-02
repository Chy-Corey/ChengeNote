## 向量

#### 一、引用向量元素

1. 引用单个元素

   ```matlab
   vector = [1 2 3];
   a = vector(2);
   ```

2. 引用所有元素

   ```matlab
   vector = [1 2 3];
   a = vector(:);
   ```

3. 引用部分元素

   ```matlab
   vector = [1 2 3];
   a = vector([2,3])
   ```

   