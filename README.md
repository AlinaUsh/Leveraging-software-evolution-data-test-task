## Test task for leveraging software evolution data with LLMs

In accordance with the advice of the authors, this work uses the latest version of the repository, and not the one that was at the time of writing the paper.

### EDA

To begin with, it was interesting to look at the size of the texts on which the model was trained and how much the length of the text varies between 2 given versions of the code.

Then I looked at what parameters were changing at all. For example, for simplicity, changes within a single file are always taken (as warned in the description of the dataset). 
Since additional information about the code (including commit message) can be useful during training, I looked at their distribution too. I also noticed that some of the features are very similar to each other and looked at them a little more closely.  

I looked to see if there were examples in the dataset where all the code was written or deleted on the contrary. Later, I generalized this to a situation where the code was just being added or just deleted (then one of the lines would be a subsequence of the other).

All relevant graphs and numbers can be found in file `task1.ipynb`.

### Evaluation

As in the article, I use the pass@k metric, which can be defined as the probability that at least one of k generated code samples will pass given tests.

In addition to model Refact-1_6B-fim, I also run the evaluation on the WizardCoder-1B-V1.0 model, which is close in size to ours.

To run the Refact-1_6B-fim model, I had to make small changes to file `bigcode-evaluation-harness/bigcode_eval/tasks/humanevalpack.py`. Namely, I added prompt generation for this model in accordance with the Chat scheme presented in Hugging Face. 

Also, while working, I did not run on the entire dataset all the time, but only on a subset of it. However, I have not yet made a separate argument for this when running evaluation.

### Results analysis

Due to the strange failures in colab, I did not have time to conduct enough experiments. In the next section, I described in more detail what I would like to do more. Currently, WizardCoder-1B-V1.0 shows better results than Refact-1_6B-fim. I guess this is caused by not the best prompt for this model. For my experiments, I used rather small values of n_samples to make computations faster. Increasing this parameter also leads to an increase in the metric. 

### If only I had more time...: 
- Make different prompts for tests and docs, as it is done in `bigcode-evaluation-harness/bigcode_eval/tasks/humanevalpack_openai.py`
- More experiments with hyperparameters and optimizations (when I tried to add them, colab behaved strangely (not from optimizations, but in general), I had to abandon this in order to make it)
- Try the fill-in-the-middle prompt option
- EDA for humaneval dataset
- EDA for commitpackft:
 
  - Distribution of commit types. Not just checking that one of the versions of the code is a subsequence of the other, but classification according to the meaning of the change. But this requires an additional model, it would be too difficult to do this now in a test task
- More different model options and possibly metrics (it would be interesting to look at the uncertainty, for example, to understand which tasks are the most difficult and why)