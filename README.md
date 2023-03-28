# IF6289_project_BERT

First, we want to reproduce the results in StereoSet. The results for StereoSet scrores are in '''stereoset/code/test_predictions.json.'''

1. Since they only kept the results for BERT-base-cased on dev set, we reproduced the results for BERT-base-cased and BERT-base-uncased on test set. The BERT-base-cased results will be compared to the ones on StereoSet paper. The uncased version will be compared to the ones in bias-bench paper and also to the cased version. Note that bias-bench does not have results for NSP.

2. For StereoSet, there was an error in eval_discriminate_models.py line 222 as the first argument should be a tensor. To fix this, we replace "outputs" with "outputs.logits". This error may have happened because the code on StereoSet github is not maintained and "outputs", an instance of "NextSentencePredictorOutput", is updated with new attributes. 

  Note: this is from chatGPT: "In the context of the BERTNextSentence model, the NextSentencePredictorOutput class was introduced in version 4.8.0 of the Hugging Face Transformers library, which was released on October 8th, 2021. This class includes a logits attribute that contains the output logits from the model for the next sentence prediction task. Prior to version 4.8.0, the output for this task was included as part of the overall model output without a separate logits attribute."

3. Potential problem: In the StereoSet paper, the evaluation metrics for intrasentence and interesentence are the log probabilities, but they only used softmax in the code. Using this provided code, we are able to reproduce the scores in the paper.
