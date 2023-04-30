<h3 align="center">
<p>IF6289_project_BERT
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
