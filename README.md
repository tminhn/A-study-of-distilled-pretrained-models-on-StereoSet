<h3 align="center">
<p>IF6289_project_BERT
</h3>


### Reproduce the results in StereoSet with codes from https://github.com/moinnadeem/StereoSet/tree/master/code. 

The results for StereoSet scrores are in `stereoset/code/test_predictions.json`

1. Since they only kept the results for BERT-base-cased on dev set, we reproduced the results for BERT-base-cased and BERT-base-uncased on test set. The BERT-base-cased results will be compared to the ones on StereoSet paper. The uncased version will be compared to the ones in bias-bench paper and also to the cased version. Note that bias-bench does not have results for NSP.

2. For StereoSet, there was an error in `eval_discriminate_models.py` line 222 as the first argument should be a tensor. To fix this, we replace "outputs" with "outputs.logits". This error may have happened because the code on StereoSet github is not maintained and "outputs", an instance of "NextSentencePredictorOutput", is updated with new attributes. 
    Note: this is from chatGPT: "In the context of the BERTNextSentence model, the NextSentencePredictorOutput class was introduced in version 4.8.0 of the Hugging Face Transformers library, which was released on October 8th, 2021. This class includes a logits attribute that contains the output logits from the model for the next sentence prediction task. Prior to version 4.8.0, the output for this task was included as part of the overall model output without a separate logits attribute."

3. Potential problem: In the StereoSet paper, the evaluation metrics for intrasentence and interesentence are the log probabilities, but they only used softmax in the code. Using this provided code, we are able to reproduce the scores in the paper.


Next we can finetune Bert-base-uncased with different dropout configuration using dataset Wiki-10 from bias-bench paper so we can compare later. At this point, we only borrow the idea from Webster et al. (2020), and reproduce the setup for bias-bench.

### Finetune with dropout configurations with codes from https://github.com/McGill-NLP/bias-bench

1. To finetune models, we use `run_mlm.py` (this file is similar to the finetuning file `run_mlm.py` from huggingface https://github.com/huggingface/transformers/tree/main/examples/pytorch/language-modeling) with added blocks of code for debiasing (new argument for dropout is one of the added code). Because the bias-bench paper has a fixed configuration for dropout parameters (the dropout_debias option is boolean), we modified the argument to specify `hidden_dropout_prob`, `attention_probs_dropout_prob`.

2. Current problems: running out of CUDA memory. We need to change the batch_size and gradient_accumulation_steps or using other techniques to finetune model. This may lead to worse performance. Also, we consider using DistilBERT for faster training and it can be a good comparision to the BERT perfomance in bias-bench.
