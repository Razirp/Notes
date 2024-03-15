# 大模型推理优化

> 优化大模型推理时的速度、显存消耗等。

相关技术：

- 大模型调度引擎
  - vLLM
  - Orca
  - TensorRT-LLM (FasterTransformer)
  - llama.cpp
  - MLC-LLM
- 神经网络执行引擎
  - TensorRT
  - Tensorflow Serving
  - TVM
  - SNPE
- 线性代数计算库
  - cuBLAS
  - Eigen
  - Intel MKL
  - ARM Compute Library

## 推理服务框架

1. **vLLM[1]**：适用于大批量Prompt输入，并对推理速度要求高的场景；
2. **Text generation inference[2]**：依赖HuggingFace模型，并且不需要为核心模型增加多个adapter的场景；
3. **CTranslate2[3]**：可在CPU上进行推理；
4. **OpenLLM[4]**：为核心模型添加adapter并使用HuggingFace Agents，尤其是不完全依赖PyTorch；
5. **Ray Serve[5]**：稳定的Pipeline和灵活的部署，它最适合更成熟的项目；
6. **MLC LLM[6]**：可在客户端（边缘计算）（例如，在Android或iPhone平台上）本地部署LLM；
7. **DeepSpeed-MII[7]**：使用DeepSpeed库来部署LLM；

## 优化策略

### 大模型调度引擎

#### KV Cache

#### Iteration-level Scheduling

#### PagedAttention

> From vLLM.

- 解决KV Cache中的GPU显存占用大的问题

#### GPTQ量化

#### 手工优化

- Fused kernels
- …

### 神经网络执行引擎

#### 算子融合 (Operator Fusion)

> 将多个相邻的算子合并进行计算。

#### 量化 (Quantization)

> 将连续的浮点类型参数离散化为整数类型，以降低存储空间并提高计算速度。

#### 分布式 (Distribution)

> 多卡推理和通信加速。

#### 批量化 (Batching)

> 将多个请求合并进行处理。

- 合并请求可以增大代数运算的矩阵规模，更好地利用下层代数库；
- 合并请求可以减少对静态的模型参数矩阵的扫描次数，降低内存带宽的消耗。

#### 线性代数加速

- GPU并行计算
  - CUDA
  - OpenCL
- CPU - SIMD，多核技术
  - SSEx
  - AVX
  - NEON
  - SVE
- Tiling分块技术
  - 通过巧妙分块，显著减少内存带宽需求，提高计算速度
  - 矩阵乘法等可应用
- Autotuning 自动调优
  - 通过参数空间搜索，自动选择最适合当前硬件环境的优化策略，使线性代数运算更为高效

