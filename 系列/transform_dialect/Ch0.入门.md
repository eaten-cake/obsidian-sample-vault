### 1. Reductin (归约)
```cpp
%0 = vector.reduction <add>, %0 : vector<8xf32> into f32
```
归约操作通常指的是通过某种二元操作（如加法、乘法、最大值等）来合并输入元素，从而生成单一的输出结果。
“归约”一词用于描述这一行为是因为它确实在“减少”或“简化”数据的维度或数量上。

### 2. Contraction (收缩)
#### 2.1 向量点积

```cpp
%result = vector.contract {
  indexing_maps = [affine_map<(i) -> (i)>, affine_map<(i) -> (i)>, affine_map<() -> ()>],
  iterator_types = ["reduction"]
} %vec1, %vec2, %zero : vector<8xf32>, vector<8xf32> into f32

```

`indexing_maps` 使得两个向量中每个元素对应相乘，然后通过 `reduction` 操作将结果加在一起。

#### 2.2 矩阵乘法
```cpp
%result = vector.contract {
  indexing_maps = [affine_map<(i, j, k) -> (i, k)>,
                   affine_map<(i, j, k) -> (k, j)>,
                   affine_map<(i, j, k) -> (i, j)>],
  iterator_types = ["parallel", "parallel", "reduction"]
} %lhs, %rhs, %init: vector<8x10xf32>, vector<10x16xf32> into vector<8x16xf32>
```

### 3. Generic Operation on Memory
```cpp
linalg.generic {
  indexing_maps = [affine_map<(i, j, k) -> (i, k)>,
                   affine_map<(i, j, k) -> (k, j)>,
                   affine_map<(i, j, k) -> (i, j)>],
  iterator_types = ["parallel", "parallel", "reduction"]
} ins(%lhs, %rhs : memref<8x10xf32>, memref<10x16xf32>)
  outs(%init : memref<8x16xf32>) {
^bb0(%lhs_one: f32, %rhs_one: f32, %init_one: f32):
  %0 = arith.mulf %lhs_one, %rhs_one : f32
  %1 = arith.addf %init_one, %0 : f32
  linalg.yield %1 : f32
}
```

### 4. loop fusion

```cpp
linalg.generic {
  indexing_maps [affine_map<(i) -> (i)>, affine_map<(i) -> (i)>],
  iterator_types = ["parallel"]
} ins(%in : memref<?xf32>) outs(%out : memref<?xf32>) {
^bb0(%in_one : f32, %out_one : f32):
  %c0 = arith.constant 0.0 : f32
  %0 = arith.cmpf ogt %in_one, %c0 : f32
  %1 = arith.select %0, %in_one, %c0 : f32
  linalg.yield %1 : f32 
}
```




