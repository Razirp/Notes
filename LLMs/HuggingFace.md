# Hugging Face

## Hugging Face Transformers

```bash
pip install transformers
```

### pipeline()

`pipeline()`可以通过`model`参数指定模型或者通过`task`参数指定任务，来加载相应的模型。

> 仅指定`task`任务时，加载的是对应任务的默认模型和适用于所处理任务的预处理类。

通过`pipeline()`返回的对象，可以直接传入文本进行推理，得到该任务的推理结果。

如：

```python
>>> from transformers import pipeline
>>> generator = pipeline(task="automatic-speech-recognition")
>>> generator("https://huggingface.co/datasets/Narsil/asr_dummy/resolve/main/mlk.flac")
{'text': 'I HAVE A DREAM BUT ONE DAY THIS NATION WILL RISE UP LIVE UP THE TRUE MEANING OF ITS TREES'}
```

```python
>>> generator = pipeline(model="openai/whisper-large")
>>> generator("https://huggingface.co/datasets/Narsil/asr_dummy/resolve/main/mlk.flac")
{'text': ' I have a dream that one day this nation will rise up and live out the true meaning of its creed.'}
```

若有多个输入，也可以将输入作为列表传递给`pipeline`对象。

除了在对象初始化阶段可以指定参数外，在后续利用对象进行推理时也可以临时指定参数。

#### 重要参数

##### device

`device=n`可以指定模型存放的设备。

`device_map="auto"`允许`Hugging Face Accelerate`自动确定如何加载和存储模型权重。

> 可以适用于单个GPU无法完整存放模型的情况。
>
> 要求不要与`device`同时设置。

##### batch_size

在推理时使用批处理并不一定可以获得更好的性能，所以需慎重并保持观察。However，当想使用批处理时，可以通过`batch_size`参数设置批次大小。

> - 有实时性推理要求时，不要使用
> - 用CPU时，不要使用
> - 如果在GPU上对大量静态数据进行推理：
>   - 如果对`sequence_length`所知不多（如“自然”数据），默认不要使用
>   - 如果`sequence_length`比较规律，可以尝试
>   - GPU比较大时，可以尝试
>
> 在使用批处理时，注意处理好内存溢出（OOM）。
>
> - 可能被超长的序列输入触发

批处理只是改变了推理方式（一次可能传递多个数据给模型），但不会影响返回结果的形式。

##### 特定模型/任务的参数

#### 在数据集上进行推理

可以使用迭代器如`Hugging Face Datasets`对象来迭代整个数据集：

```python
# KeyDataset is a util that will just output the item we're interested in.
from transformers.pipelines.base import KeyDataset
from transformers import pipeline
from datasets import load_dataset

pipe = pipeline(model="hf-internal-testing/tiny-random-wav2vec2", device=0)
dataset = load_dataset("hf-internal-testing/librispeech_asr_dummy", "clean", split="validation[:10]")

for out in pipe(KeyDataset(dataset, "audio")):	# 这里的`pipe()`是调用了`pipeline`对象
    print(out)
```





