---
title: HuggingFace
tags: 
keywords: rouge, pygments, prettify, color coding,
last_updated: Sept 5, 2022
summary: "Huggingface course from huggingface website."
sidebar: mydoc_sidebar
permalink: huggingface.html
folder: mydoc
---

[Course Link](https://huggingface.co/course/chapter1/1) |
[Modelhub Link](https://huggingface.co/models) |
[Edit here](https://github.com/bhbharat/bhbharat.github.io/edit/gh-pages/pages/mydoc/course-huggingface.md) |
[Hackathon](https://datahack.analyticsvidhya.com/contest/linguipedia-codefest-natural-language-processing-1/#ProblemStatement)



Some of the pipelines available in the Huggingface are:

 - **Feature-extraction (get the vector representation of a text)**
 - **Fill-mask**
 - **NER (named entity recognition)**
 - **Question-answering**
 - **Sentiment-analysis**
 - **Summarization**
 - **Text-generation**
 - **Translation**
 - **Zero-shot-classification**

    

## Transformers Model Architecture

The transformers models can be grouped into following categories:

1. **GPT-like** (also called auto-regressive Transformer models)
2. **BERT-like** (also called auto-encoding Transformer models)
3. **BART/T5-like** (also called sequence-to-sequence Transformer models)

All the Transformer models mentioned above (GPT, BERT, BART, T5, etc.) have been trained as language models. This means they have been trained on large amounts of raw text in a **self-supervised** fashion. Self-supervised learning is a type of training in which the objective is automatically computed from the inputs of the model.

![](https://raw.githubusercontent.com/bhbharat/bhbharat.github.io/master/images/transformers.PNG)

## There are 2 types of language modellings:

1. **Casual language modelling** - predict the next word in the sentence in supervised manner
2. **Masked language modelling** - predict the masked word in the sentence

## The model is primarily composed of two blocks:

1. **Encoder (left)**: The encoder receives an input and builds a representation of it (its features). This means that the model is optimized to acquire understanding from the input.
2. **Decoder (right):** The decoder uses the encoder’s representation (features) along with other inputs to generate a target sequence. This means that the model is optimized for generating outputs.
    
## Each of these parts can be used independently, depending on the task:

1. **Encoder-only models:** Good for tasks that require understanding of the input, such as sentence classification and named entity recognition. eg. *BERT*
2. **Decoder-only models:** Good for generative tasks such as text generation. eg. *GPT-2*
3. **Encoder-decoder models or sequence-to-sequence models:** Good for generative tasks that require an input, such as translation or summarization. eg. *T5, BART*


## Attention Layer

 A key feature of Transformer models is that they are built with special layers called attention layers. this layer will tell the model to pay specific attention to certain words in the sentence you passed it (and more or less ignore the others) when dealing with the representation of each word. **So the word vector not only represents word in question but other related words as well.**

## Transformer architecture

 The Transformer architecture was originally designed for translation. **The encoder takes the complete sentence but decoder takes only the words before the word to be predicted.** the first attention layer in a decoder block pays attention to all (past) inputs to the decoder, but the second attention layer uses the output of the encoder. eg. BERT is an architecture while bert-base-cased, a set of weights trained by the Google team for the first release of BERT, is a checkpoint.


## Encoder models:

Encoder models use only the encoder of a Transformer model. At each stage, the attention layers can access all the words in the initial sentence (bi-directional). The pre-training of these models is done by masking random words and tasking the model with reconstructing the initial sentence.

Encoder models are best suited for tasks requiring an understanding of the full sentence, such as 

1. **sentence classification,** 
2. **named entity recognition (and more generally word classification), and** 
3. **extractive question answering.**

Representatives of this family of models include:

 - ALBERT
 - BERT
 - DistilBERT
 - ELECTRA
 - RoBERTa
 - List item

## Decoder models:

Decoder models use only the decoder of a Transformer model. At each stage, for a given word the attention layers can only access the words positioned before it in the sentence (uni-directional). The pretraining of decoder models usually revolves around predicting the next word in the sentence. These models are best suited for tasks involving text generation.

Representatives of this family of models include:

 1. CTRL 
 2. GPT 
 3. GPT-2 
 4. Transformer XL

## Encoder-decoder models

Encoder-decoder models (also called sequence-to-sequence models) use both parts of the Transformer architecture. At each stage, the attention layers of the encoder can access all the words in the initial sentence, whereas the attention layers of the decoder can only access the words positioned before a given word in the input.

Sequence-to-sequence models are best suited for tasks revolving around generating new sentences depending on a given input, such as 

1. **summarization**, 
2. **translation, or** 
3. **generative question answering.**


Representatives of this family of models include:

 - BART 
 - mBART 
 - Marian 
 - T5

## Summary

| Model           | Examples                                   | Tasks                                                                            |
| --------------- | ------------------------------------------ | -------------------------------------------------------------------------------- |
| Encoder         | ALBERT, BERT, DistilBERT, ELECTRA, RoBERTa | Sentence classification, named entity recognition, extractive question answering |
| Decoder         | CTRL, GPT, GPT-2, Transformer XL           | Text generation                                                                  |
| Encoder-decoder | BART, T5, Marian, mBART                    | Summarization, translation, generative question answering                        |



## Under the pipeline

we first need to download that information from the Model Hub. To do this, we use the AutoTokenizer class and its `from_pretrained()` method. Using the checkpoint name of our model, it will automatically fetch the data associated with the model’s tokenizer and cache it.

```python
from transformers import AutoTokenizer
checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
```

To specify the type of tensors we want to get back (PyTorch, TensorFlow, or plain NumPy), we use the return_tensors argument:

```python
raw_inputs = [
    "I've been waiting for a HuggingFace course my whole life.",
    "I hate this so much!",
]
inputs = tokenizer(raw_inputs, padding=True, truncation=True, return_tensors="pt")
print(inputs)
```
We can download our pretrained model the same way we did with our tokenizer. 
```python
from transformers import AutoModel
checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
model = AutoModel.from_pretrained(checkpoint)
```

This architecture contains only the **base Transformer module**: given some inputs, it outputs what we’ll call hidden states, also known as features. While these hidden states can be useful on their own, they’re **usually inputs to another part of the model, known as the head**. The hidden size can be very large (768 is common for smaller models, and in larger models this can reach 3072 or more)

```python
outputs = model(**inputs)
print(outputs.last_hidden_state.shape)
## output will be torch.Size([2, 16, 768])
```
The outputs of 🤗 Transformers models behave like namedtuples or dictionaries.
we will need a model with a sequence classification head (to be able to classify the sentences as positive or negative). So, **we won’t actually use the AutoModel class, but AutoModelForSequenceClassification**:

```python
from transformers import AutoModelForSequenceClassification
checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)
outputs = model(**inputs)
```
Now if we look at the shape of our inputs, the dimensionality will be much lower: the model head takes as input the high-dimensional vectors we saw before, and outputs vectors containing two values (one per label).
The output would be in the form of logits not the probabilities

```python
import torch
predictions = torch.nn.functional.softmax(outputs.logits, dim=-1)
print(predictions)
model.config.id2label
```
## Creating a Transformer

We can load the model architecture (without loading the pretrained checkpoints) by loading model and the config file as shown below:

```python
from transformers import BertConfig, BertModel
config = BertConfig()
model = BertModel(config)
```
The Bert model is loaded but it doesn't contain weights so we'll have to load the weights.
```python
from transformers import BertModel
from transformers import AutoModel # used for automatically loading the model type
model = BertModel.from_pretrained("bert-base-cased")
model = AutoModel.from_pretrained("bert-base-cased")
model.save_pretrained("directory_on_my_computer")
```
The above code directly loads the model with the weights. The models are downloaded and cached in directory *~/.cache/huggingface/transformers* which can be changed by changing **HF_HOME** environment variable. When saved, 2 files i.e *config.json* and *pytorch_model.bin* files are saved in the directory. *config.json* file contains all the attributes to build the model and *pytorch_model.bin* contains the model's weights.

## Tokenizers

There are 3 types of tokenizers that are used:
1. Word based - it has a limitation of huge amounts of tokens (500,000 tokens in english language). Also "dog" and "dogs" are considered different tokens, and not seen as similar tokens.
2. Character based - The vocabulary is much smaller ( under 100) and very few unknown tokens are there. But the limitation is that, intuitively, it's less meaningful.
3. Subword tokenization - in this tokenization, the rare words are decomposed into meaningful subwords. i.e the word tokenization would be represented by 2 tokens, token & ##ization.

There are many other methods that are getting used like Byte-level BPE used in GPT-2, WordPiece used in BERT etc.

## loading of tokenizers:

```python
from transformers import BertTokenizer
from transformers import AutoTokenizer # automatically loads the tokenizer based on the checkpoints
tokenizer = BertTokenizer.from_pretrained("bert-base-cased")
tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
tokenizer.save_pretrained("directory_on_my_computer")
sequence = "Using a Transformer network is simple"
tokens = tokenizer.tokenize(sequence)
decoded_string = tokenizer.decode([7993, 170, 11303, 1200, 2443, 1110, 3014])
```

the tokenizer has a _vocabulary_, which is the part we download when we instantiate it with the `from_pretrained()` method. The tokenizer adds the special word `[CLS]` at the beginning and the special word `[SEP]` at the end. This is because the model is pretrained with those, so to get the same results for inference we need to add them as well.

## Attention masks

The key feature of Transformer models is attention layers that _contextualize_ each token. These will take into account the padding tokens since they attend to all of the tokens of a sequence. To get the same result when passing individual sentences of different lengths through the model, we need to tell those attention layers to ignore the padding tokens. _Attention masks_ are tensors with the exact same shape as the input IDs tensor, filled with 0s and 1s: 1s indicate the corresponding tokens should be attended to, and 0s indicate the corresponding tokens should not be attended to.


## Fine tuning a pretrained model

### Preprocessing the dataset:

When passing a pair of sentences for next sentence prediction, we need to handle the two sequences as a pair, and apply the appropriate preprocessing. the tokenizer can also take a pair of sequences and prepare it the way BERT model expects.

```python
inputs = tokenizer("This is the first sentence.", "This is the second one.")
# results - 
{ 
  'input_ids': [101, 2023, 2003, 1996, 2034, 6251, 1012, 102, 2023, 2003, 1996, 2117, 2028, 1012, 102],
  'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1],
  'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
}
tokenizer.convert_ids_to_tokens(inputs["input_ids"])
#results - 
['[CLS]', 'this', 'is', 'the', 'first', 'sentence', '.', '[SEP]', 'this', 'is', 'the', 'second', 'one', '.', '[SEP]']
```

the model expects the inputs to be of the form `[CLS] sentence1 [SEP] sentence2 [SEP]` when there are two sentences. The token_type_ids tells us, which token belongs to which sentence. Not all the checkpoints will have the token_type_ids as its not pretrained like that. The BERT was also trained for next sentence prediction so it returns the toekn_type_ids. With next sentence prediction, the model is provided pairs of sentences (with randomly masked tokens) and asked to predict whether the second sentence follows the first.

We can apply the tokenizer to whole dataset by using map() function with the following function.
```python
def tokenize_function(example):
    return tokenizer(example["sentence1"], example["sentence2"], truncation=True)
tokenized_datasets = raw_datasets.map(tokenize_function, batched=True)
```

using `batched=True` in our call to `map` so the function is applied to multiple elements of the dataset at once, and not on each element separately. This allows for faster preprocessing.

#### Dynamic padding

Since we're loading the datasets in batches, we have to define a collate function that will apply the correct amount of padding to the items of the dataset we want to batch together. It'll only apply it as necessary on each batch and avoid having over-long inputs with a lot of padding. This will speed up training by quite a bit, but note that if training on a TPU it can cause problems as TPUs prefer fixed shapes.

```python
from transformers import DataCollatorWithPadding
data_collator = DataCollatorWithPadding(tokenizer=tokenizer)
batch = data_collator(samples)
```
## Trainer API

🤗 Transformers provides a `Trainer` class to help you fine-tune any of the pretrained models it provides on your dataset. 1. The first step before we can define our `Trainer` is to define a `TrainingArguments` class that will contain all the hyperparameters the `Trainer` will use for training and evaluation. 
2. The second step is to define our model. 
3. Once we have our model, we can define a `Trainer` by passing it all the objects constructed up to now — the `model`, the `training_args`, the training and validation datasets, our `data_collator`, and our `tokenizer`

```python
from transformers import TrainingArguments
training_args = TrainingArguments("test-trainer")

from transformers import AutoModelForSequenceClassification
model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)

from datasets import load_metric
def compute_metrics(eval_preds):
    metric = load_metric("glue", "mrpc")
    logits, labels = eval_preds
    predictions = np.argmax(logits, axis=-1)
    return metric.compute(predictions=predictions, references=labels)
    
from transformers import Trainer
trainer = Trainer(
    model,
    training_args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator,
    tokenizer=tokenizer,
    compute_metrics=compute_metrics,
)

trainer.train()
predictions = trainer.predict(tokenized_datasets["validation"])
import numpy as np
preds = np.argmax(predictions.predictions, axis=-1)
```

## Datasets

Huggingface datasets library provides an easy way to load the data and use it for the models. 

Loading a dataset:

```python

from datasets import load_dataset
data_files = {"train": "drugsComTrain_raw.tsv", "test": "drugsComTest_raw.tsv"}
drug_dataset = load_dataset("csv", data_files=data_files, delimiter="\t")

drug_sample = drug_dataset["train"].shuffle(seed=42).select(range(1000))
drug_dataset = drug_dataset.rename_column(original_column_name="Unnamed: 0", new_column_name="patient_id")

#create new columns
def lowercase_condition(example):
    return {"condition": example["condition"].lower()}
drug_dataset.map(lowercase_condition)

drug_dataset = drug_dataset.filter(lambda x: x["condition"] is not None)
tokenized_dataset = drug_dataset.map(tokenize_function, batched=True)

drug_dataset.set_format("pandas")
from datasets import Dataset
freq_dataset = Dataset.from_pandas(frequencies)

drug_dataset_clean.save_to_disk("drug-reviews")
drug_dataset_reloaded = load_from_disk("drug-reviews")
drug_sample[:3]
```
