<h3 align="center">
<p>Project for IFT6289 - Natural Language Processing with Deep Learning
</h3>
This is a final project for the course IFT6289 - Natural Language Processing with Deep Learning, offered at University of Montreal.
<h3 align="center">
<p>Abstract
</h3>

Machine learning models can develop biases
based on the patterns they see in the training
data. Popular pre-trained language models such
as BERT, RoBERTa, ALBERT, and GPT-2 also
learn bias patterns from their training corpus.
Since language models often reflect society’s
inequalities, we may find that the datasets for
language models contain these biases, especially between genders, races, professions, and
religions. Many recent papers have been trying
to debias these large language models. In this
project, we measure the bias and their language
modeling performance on a dataset called StereoSet and its metric, icat. Our goal is to find
methods that can debias models efficiently or
at least convey certain properties of models’
bias. Firstly, we have reproduced the experiments of BERT, RoBERTa, GPT-2, and ALBERT models on StereoSet dataset from our
baseline paper Meade et al. (2022). After reproducing the result, we found a mistake in the
language modeling scores of models, and we
address it through our experiments. Then we
have finetuned these models under our low settings using Wikitext with or without dropout
configuration to see its effect on bias. We found
that this configuration improves icat scores in
all models. Distillation is a good practice for
language model tasks to perform as well as
the teacher model but in a more efficient way,
so we experiment on the distilled models of
BERT, RoBERTa, GPT-2 to monitor the icat
behavior of these models before and after dropout configuration. We have found that using
distillation decreases the performance of BERT
family models (BERT and RoBERTa) while
improving the GPT-2 model’s icat in all target
terms. We have also proposed a hypothesis that
encoders-only and decoders-only transformers
react differently to distillation and finetuning
in terms of icat, which could be explored for
deeper understanding of language models.

The report, formatted in ACL style, is available as a PDF and can be found [here](./distilled_pretrained_models_StereoSet.pdf).

<h3 align="center">
<p>Adaptation
</h3>

codes from https://github.com/McGill-NLP/bias-bench. The folders in our repository include files that we have modified:

1. bias-bench/benchmark/stereoset.py
2. bias-bench/benchmark/model/models.py
3. experiments/run_clm.py
4. experiments/run_mlm.py
5. experiments/stereoset_debias.py
6. experiments/stereoset_evaluation.py
7. experiments/stereoset.py

To reproduce our results, please follow the instruction on https://github.com/McGill-NLP/bias-bench and replace their files with ours. The 3 seeds we use for finetuning are 42 (default), 43, 44.
