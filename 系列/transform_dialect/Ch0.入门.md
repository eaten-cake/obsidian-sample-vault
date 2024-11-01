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

```






