# vLLM

## LLM & SamplingParams

### LLM 类

这段代码定义了一个名为[`LLM`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22LLM%22%5D "vllm/vllm/entrypoints/llm.py")的类，该类用于根据给定的提示和采样参数生成文本。

这个类包含一个分词器，一个语言模型（可能分布在多个GPU上），以及为中间状态（也称为KV缓存）分配的GPU内存空间。给定一批提示和采样参数，这个类使用智能批处理机制和高效的内存管理从模型生成文本。

注意，这个类主要用于离线推理。对于在线服务，应使用`AsyncLLMEngine`类。要查看参数的完整列表，请参阅`EngineArgs`。

这个类的参数包括：

- [`model`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22model%22%5D "vllm/vllm/entrypoints/llm.py")：HuggingFace Transformers模型的名称或路径。
- [`tokenizer`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22tokenizer%22%5D "vllm/vllm/entrypoints/llm.py")：HuggingFace Transformers分词器的名称或路径。
- [`tokenizer_mode`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22tokenizer_mode%22%5D "vllm/vllm/entrypoints/llm.py")：分词器模式。"auto"将使用快速分词器（如果可用），"slow"将始终使用慢速分词器。
- [`trust_remote_code`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22trust_remote_code%22%5D "vllm/vllm/entrypoints/llm.py")：下载模型和分词器时是否信任远程代码（例如，来自HuggingFace）。
- [`tensor_parallel_size`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22tensor_parallel_size%22%5D "vllm/vllm/entrypoints/llm.py")：用于分布式执行的GPU数量。
- [`dtype`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22dtype%22%5D "vllm/vllm/entrypoints/llm.py")：模型权重和激活的数据类型。目前，我们支持`float32`，`float16`，和`bfloat16`。如果为`auto`，我们将使用模型配置文件中指定的`torch_dtype`属性。但是，如果配置中的`torch_dtype`是`float32`，我们将使用`float16`。
- [`quantization`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22quantization%22%5D "vllm/vllm/entrypoints/llm.py")：用于量化模型权重的方法。目前，我们支持"awq"，"gptq"和"squeezellm"。如果为None，我们首先检查模型配置文件中的`quantization_config`属性。如果该属性为None，我们假定模型权重未经量化，并使用[`dtype`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22dtype%22%5D "vllm/vllm/entrypoints/llm.py")来确定权重的数据类型。
- [`revision`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22revision%22%5D "vllm/vllm/entrypoints/llm.py")：要使用的特定模型版本。它可以是分支名，标签名，或提交id。
- [`tokenizer_revision`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22tokenizer_revision%22%5D "vllm/vllm/entrypoints/llm.py")：要使用的特定分词器版本。它可以是分支名，标签名，或提交id。
- [`seed`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22seed%22%5D "vllm/vllm/entrypoints/llm.py")：用于初始化采样的随机数生成器的种子。
- [`gpu_memory_utilization`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22gpu_memory_utilization%22%5D "vllm/vllm/entrypoints/llm.py")：为模型权重，激活和KV缓存预留的GPU内存的比例（在0和1之间）。较高的值将增加KV缓存大小，从而提高模型的吞吐量。然而，如果值过高，可能会导致内存溢出（OOM）错误。
- [`swap_space`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22swap_space%22%5D "vllm/vllm/entrypoints/llm.py")：每个GPU使用的CPU内存大小（以GiB为单位）作为交换空间。这可以用于临时存储请求的状态，当它们的`best_of`采样参数大于1时。如果所有请求的`best_of`都为1，你可以安全地将此设置为0。否则，过小的值可能会导致内存溢出（OOM）错误。
- [`enforce_eager`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22enforce_eager%22%5D "vllm/vllm/entrypoints/llm.py")：是否强制执行。如果为True，我们将禁用CUDA图并始终在急切模式下执行模型。如果为False，我们将使用CUDA图和急切执行的混合模式。
- [`max_context_len_to_capture`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fentrypoints%2Fllm.py%22%2C%22max_context_len_to_capture%22%5D "vllm/vllm/entrypoints/llm.py")：CUDA图覆盖的最大上下文长度。当一个序列的上下文长度大于这个值时，我们将回退到急切模式。
- `disable_custom_all_reduce`：参见ParallelConfig。

### SamplingParams 类

这段代码定义了一个名为[`SamplingParams`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22SamplingParams%22%5D "vllm/vllm/sampling_params.py")的类，该类用于设置文本生成的采样参数。这些参数主要参考了OpenAI文本补全API的设置，同时还增加了对束搜索（beam search）的支持，这是OpenAI不支持的。

类的参数包括：

- [`n`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22n%22%5D "vllm/vllm/sampling_params.py")：对于给定的提示，返回的输出序列的数量。
- [`best_of`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22best_of%22%5D "vllm/vllm/sampling_params.py")：从提示生成的输出序列的数量。从这些[`best_of`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22best_of%22%5D "vllm/vllm/sampling_params.py")序列中，返回前[`n`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22n%22%5D "vllm/vllm/sampling_params.py")个序列。[`best_of`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22best_of%22%5D "vllm/vllm/sampling_params.py")必须大于或等于[`n`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22n%22%5D "vllm/vllm/sampling_params.py")。当[`use_beam_search`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22use_beam_search%22%5D "vllm/vllm/sampling_params.py")为True时，[`best_of`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22best_of%22%5D "vllm/vllm/sampling_params.py")被视为束宽。默认情况下，[`best_of`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22best_of%22%5D "vllm/vllm/sampling_params.py")设置为[`n`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22n%22%5D "vllm/vllm/sampling_params.py")。
- [`presence_penalty`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22presence_penalty%22%5D "vllm/vllm/sampling_params.py")：基于新令牌是否出现在迄今为止生成的文本中来对其进行惩罚的浮点数。值>0鼓励模型使用新令牌，而值<0鼓励模型重复令牌。
- [`frequency_penalty`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22frequency_penalty%22%5D "vllm/vllm/sampling_params.py")：基于新令牌在迄今为止生成的文本中的频率来对其进行惩罚的浮点数。值>0鼓励模型使用新令牌，而值<0鼓励模型重复令牌。
- [`repetition_penalty`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22repetition_penalty%22%5D "vllm/vllm/sampling_params.py")：基于新令牌是否出现在提示和迄今为止生成的文本中来对其进行惩罚的浮点数。值>1鼓励模型使用新令牌，而值<1鼓励模型重复令牌。
- [`temperature`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22temperature%22%5D "vllm/vllm/sampling_params.py")：控制采样随机性的浮点数。较低的值使模型更确定性，而较高的值使模型更随机。零表示贪婪采样。
- [`top_p`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22top_p%22%5D "vllm/vllm/sampling_params.py")：控制要考虑的顶部令牌的累积概率的浮点数。必须在(0, 1]内。设置为1以考虑所有令牌。
- [`top_k`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22top_k%22%5D "vllm/vllm/sampling_params.py")：控制要考虑的顶部令牌的数量的整数。设置为-1以考虑所有令牌。
- [`min_p`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22min_p%22%5D "vllm/vllm/sampling_params.py")：表示相对于最可能的令牌的概率，一个令牌被考虑的最小概率的浮点数。必须在[0, 1]内。设置为0以禁用此功能。
- [`seed`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22seed%22%5D "vllm/vllm/sampling_params.py")：用于生成的随机种子。
- [`use_beam_search`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22use_beam_search%22%5D "vllm/vllm/sampling_params.py")：是否使用束搜索而不是采样。
- [`length_penalty`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22length_penalty%22%5D "vllm/vllm/sampling_params.py")：基于其长度对序列进行惩罚的浮点数。在束搜索中使用。
- [`early_stopping`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22early_stopping%22%5D "vllm/vllm/sampling_params.py")：控制束搜索的停止条件。它接受以下值：`True`，生成停止只要有[`best_of`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22best_of%22%5D "vllm/vllm/sampling_params.py")个完整候选者；`False`，应用启发式方法，当找到更好的候选者非常不可能时，生成停止；`"never"`，束搜索过程只有在不能有更好的候选者时才停止（规范束搜索算法）。
- [`stop`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22stop%22%5D "vllm/vllm/sampling_params.py")：生成时停止的字符串列表。返回的输出不会包含停止字符串。
- [`stop_token_ids`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22stop_token_ids%22%5D "vllm/vllm/sampling_params.py")：生成时停止的令牌列表。返回的输出将包含停止令牌，除非停止令牌是特殊令牌。
- [`include_stop_str_in_output`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22include_stop_str_in_output%22%5D "vllm/vllm/sampling_params.py")：是否在输出文本中包含停止字符串。默认为False。
- [`ignore_eos`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22ignore_eos%22%5D "vllm/vllm/sampling_params.py")：是否忽略EOS令牌，并在生成EOS令牌后继续生成令牌。
- [`max_tokens`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22max_tokens%22%5D "vllm/vllm/sampling_params.py")：每个输出序列生成的最大令牌数。
- [`logprobs`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22logprobs%22%5D "vllm/vllm/sampling_params.py")：每个输出令牌返回的对数概率数。请注意，实现遵循OpenAI API：返回结果包括[`logprobs`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22logprobs%22%5D "vllm/vllm/sampling_params.py")最可能的令牌的对数概率，以及所选令牌。API将始终返回采样令牌的对数概率，因此响应中可能有多达[`logprobs+1`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22logprobs%2B1%22%5D "vllm/vllm/sampling_params.py")个元素。
- [`prompt_logprobs`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22prompt_logprobs%22%5D "vllm/vllm/sampling_params.py")：每个提示令牌返回的对数概率数。
- [`skip_special_tokens`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22skip_special_tokens%22%5D "vllm/vllm/sampling_params.py")：是否在输出中跳过特殊令牌。
- [`spaces_between_special_tokens`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22spaces_between_special_tokens%22%5D "vllm/vllm/sampling_params.py")：是否在输出中的特殊令牌之间添加空格。默认为True。
- [`logits_processors`](command:_github.copilot.openSymbolInFile?%5B%22vllm%2Fvllm%2Fsampling_params.py%22%2C%22logits_processors%22%5D "vllm/vllm/sampling_params.py")：基于先前生成的令牌修改logits的函数列表。

## 推理

1. 实例化`SamplingParams`，设置采样参数

   - 可以实例化多组参数，分别用于不同情境

2. 实例化`LLM`，载入`model`和`tokenizer`

3. `output=llm.generate(query_prompts, sampling_params)`返回一个 `RequestOutput`类型的对象，包含模型的输出

   - 以ChatGLM 3输入一个`你好`为例，返回的`output`打印出来为：

     ```
     [RequestOutput(request_id=0, prompt='你好', prompt_token_ids=[64790, 64792, 36474, 54591], prompt_logprobs=None, outputs=[CompletionOutput(index=0, text='，我是人工智能助手。很高兴为您服务！请问有什么问题我可以帮您解答？', token_ids=[31123, 33030, 34797, 42481, 31155, 48895, 38549, 31645, 31404, 42693, 33277, 31639, 40648, 55268, 55353, 36295, 31514, 2], cumulative_logprob=0.0, logprobs=None, finish_reason=stop)], finished=True, metrics=RequestMetrics(arrival_time=5994290.312362854, last_token_time=5994290.312362854, first_scheduled_time=1710433309.626002, first_token_time=1710433309.6582766, time_in_queue=1704439019.3136392, finished_time=1710433309.9074395), lora_request=None)]
     ```

     

## Tips

### 在CUDA 11.8下从源码安装vLLM

1. `clone`存储库后，删除`xxx.toml`文件
2. `setup.py`中设置`MAIN_CUDA_VERSION = 11.8`
3. 安装cu118的`xformers`
   - `pip install -U xformers --index-url https://download.pytorch.org/whl/cu118`
   - 若想使用国内源，可以将`–index-url`换到相应的国内源链接，如：
     - 阿里镜像源 `https://mirrors.aliyun.com/pytorch-wheels`
     - 上交大镜像源 `https://mirror.sjtu.edu.cn/pytorch-wheels`
   - 比如说 `pip install -U xformers --index-url https://mirror.sjtu.edu.cn/pytorch-wheels/cu118`
4. 在`requirements.txt`中注释掉与`cuda 12.1`相关的包（相关包需要自行安装`cuda 11.8`版的），如
   - `torch`
   - `xformers`
   - `cupy-cuda12x == 12.1.0` 改为 `cupy-cuda11x == …`
     - 如 `cupy-cuda11x == 12.1.0`
5. 如果报错`packaging`模块未找到，则`pip install packaging`安装
6. 运行`pip install -e .`从源码安装`vLLM`



