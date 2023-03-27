# IF6289_project_BERT

First, we want to reproduce the results in StereoSet:

1. Since they only kept the results for BERT-base-cased on dev set, we reproduced the results for BERT-base-cased and BERT-base-uncased on test set. The BERT-base-cased results will be compared to the ones on StereoSet paper. The uncased version will be compared to the ones in bias-bench paper and also to the cased version. Note that bias-bench does not have results for NSP.

2. For StereoSet, there was an error in eval_discriminate_models.py line 222 as the first argument should be a tensor. To fix this, we replace "outputs" with "outputs.logits". This error may have happened because the code on StereoSet github is not maintained and "outputs", an instance of "NextSentencePredictorOutput", is updated with new attributes. 
