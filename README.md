<h3 align="center">
<p>IF6289_project_BERT
</h3>


## Reproduce the results in StereoSet 

codes from https://github.com/moinnadeem/StereoSet/tree/master/code. 

The results for StereoSet scrores are in `stereoset/code/test_predictions.json`

1. Since they only kept the results for BERT-base-cased on dev set, we reproduced the results for BERT-base-cased and BERT-base-uncased on test set. The BERT-base-cased results will be compared to the ones on StereoSet paper. The uncased version will be compared to the ones in bias-bench paper and also to the cased version. Note that bias-bench does not have results for NSP.

2. For StereoSet, there was an error in `eval_discriminate_models.py` line 222 as the first argument should be a tensor. To fix this, we replace "outputs" with "outputs.logits". This error may have happened because the code on StereoSet github is not maintained and "outputs", an instance of "NextSentencePredictorOutput", is updated with new attributes. 
    Note: this is from chatGPT: "In the context of the BERTNextSentence model, the NextSentencePredictorOutput class was introduced in version 4.8.0 of the Hugging Face Transformers library, which was released on October 8th, 2021. This class includes a logits attribute that contains the output logits from the model for the next sentence prediction task. Prior to version 4.8.0, the output for this task was included as part of the overall model output without a separate logits attribute."

3. Potential problem: In the StereoSet paper, the evaluation metrics for intrasentence and interesentence are the log probabilities, but they only used softmax in the code. Using this provided code, we are able to reproduce the scores in the paper.


Next we can finetune Bert-base-uncased with different dropout configuration using dataset Wiki-10 from bias-bench paper so we can compare later. At this point, we only borrow the idea from Webster et al. (2020), and reproduce the setup for bias-bench.

## Finetune with dropout configurations

codes from https://github.com/McGill-NLP/bias-bench

1. To finetune models, we use `run_mlm.py` (this file is similar to the finetuning file `run_mlm.py` from huggingface https://github.com/huggingface/transformers/tree/main/examples/pytorch/language-modeling) with added blocks of code for debiasing (new argument for dropout is one of the added code). Because the bias-bench paper has a fixed configuration for dropout parameters (the dropout_debias option is boolean), we modified the argument to specify `hidden_dropout_prob`, `attention_probs_dropout_prob`.

2. Current problems: running out of CUDA memory. We need to change the batch_size and gradient_accumulation_steps or using other techniques to finetune model. We also added --fp16 argument to reduce memory usage. This may lead to worse performance. Also, we consider using DistilBERT for faster training and it can be a good comparision to the BERT perfomance in bias-bench.

3. We decide to add DistilBERT because of the above reason. Because DistilBERT was not trained on NextSentencePrediction and finetune it on that task is not feasible, we only measure its intrasentence performance. We added the code for DistilBERT in bias-bench (adapted the code because DistilBERT does not use token_type_ids).

4. New problems for finetuning: There is a common trend that lms decreases while ss improves. The problem is we don't know the decrease in bias is because of the debiasing method or just the model performing worse (this is also mentioned in bias-bench). The second problem is how we finetune the model. To finetune on our computers, we have to lower the batch_size and use a smaller dataset (wikipedia 2.5 instead of wikipedia 10). This may decrease the performance no matter dropout configurations. How can we find a stable configuration to finetune?

5. Quick observations: DistilBERT's lms is better than BERT's (why? could it be because DistilBERT was mainly pretrained on Fill-Mask?). icat for finetuned DistilBERT seems to improve slightly with worse lms and better ss. While for finetuned BERT, it seems that lms and ss are getting worse in bias-bench. However, bias-bench does not use "profession" bias and doesnt include icat. It is likely that finetuned BERT's icat is also getting worse (this has not been verified with our setup). If this is true, it could be interesting to explain why there is an opposite trend for DistilBERT and BERT (could it be because DistilBERT is smaller and reacts better to training?)

6. We redo all the finetuning with dropout to make sure that we compare results on the same dataset (wikitext) and eliminate the possibility that wikitext has less bias than 10% wiki dump.

7. We finetune all the distilled models with and without dropout.
