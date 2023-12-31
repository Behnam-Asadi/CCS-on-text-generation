# Abstract

The inconsistency of outputs presents a key challenge for deep learning-powered NLP systems. To address this, we've adopted the Contrast-Consistent Search (CCS), an unsupervised method that evaluates the truthfulness of a text sequence. Using CCS, we've enhanced a large language model for zero-shot sentiment analysis tasks, resulting in improved accuracy. Furthermore, we've devised seven CCS-based methods to classify model responses as reliable or unreliable. In our tests, these methods significantly improved the identification of unreliable outputs.

# Approach

We conducted an evaluation of seven candidate filters, with the goal of identifying superior options. Each filter was put through a series of tests using a multitude of questions from every dataset. We made the assumption that these filters classify CCS answers, due to better performance observed in this setting. Most filters came with an adjustable parameter t; for every filter category, we probed 100 uniformly distributed values of t. The filters we evaluated included:

1. Max CCS: An answer was considered reliable if the highest CCS output exceeded t.
2. CCS & model consensus: An answer was deemed reliable if the model's response coincided with the CCS response.
3. Max CCS and CCS & model consensus: An answer was considered reliable if it passed the criteria of both aforementioned filters (for a given value of t).
4. CCS difference: An answer was marked as reliable if the variance between CCS values was larger than parameter t.
5. CCS difference and CCS & model consensus: An answer was classified as reliable if it satisfied the conditions of both the "CCS difference filter" (for a certain t) and the "CCS & model consensus" filter.
6. Logits difference and CCS & model consensus: An answer was identified as reliable if the discrepancy in logits surpassed parameter t and it met the "CCS & model consensus" filter criteria.
7. CCS answer: Simply, every CCS answer was classified as reliable.

The filters mentioned earlier are all (either partially or entirely) dependent on CCS outputs. As a comparative measure, we test three baseline filters that operate independently of CCS outputs in the baseline scenario of pinpointing reliably answered questions by a model:
1. Max logit: An answer is designated as reliable if the maximum logit output surpasses a certain parameter t.
2. Logits difference: An answer is labeled as reliable if the disparity between logit values exceeds a specific parameter t.
3. Model answer: All model responses are straightforwardly classified as reliable.
In the context of these baselines, we operate under the presumption that CCS is not utilized, hence we regard the baseline filters as classifiers of model outputs.

Furthermore, we used RoBERTa Large MNLI, which is the RoBERTa large model fine-tuned on the Multi-Genre Natural Language Inference (MNLI) corpus, as our base model.

# Results

For each of our two testing datasets and three levels of accuracy rates (90%, 95%, and 99%, defined as the percentage of filtered examples that are correct, or in other words, the filter's true positive rate), the following tables showcase the maximum percentage of examples each filter could approve, while preserving the specified accuracy. This data can be seen as the regularity with which a filter can offer a certain degree of certainty. Additionally, the tables enumerate the accuracy levels of our two filters that accept all inputs, namely the "Model answer" and "CCS answer" filters.

<div align="center">
<img  src="src/img/IMDB_table.png"  align = 'center' width="700">
  <figcaption><br>The largest portion of examples that each filter can accept while maintaining a given accuracy level (IMDB dataset).</figcaption>
</div>

<p></p>

<div align="center">
<img  src="src/img/IMDB_plot.png"  align = 'center' width="700">
  <figcaption><br>The largest portion of examples that each filter can accept while maintaining a given accuracy level (IMDB dataset).</figcaption>
</div>

<p></p>

<div align="center">
<img  src="src/img/AMAZON-POLARITY_table.png"  align = 'center' width="700">
  <figcaption><br>The largest portion of examples that each filter can accept while maintaining a given accuracy level (AMAZON-POLARITY dataset).</figcaption>
</div>

<p></p>

<div align="center">
<img  src="src/img/AMAZON-POLARITY_plot.png"  align = 'center' width="700">
  <figcaption><br>The largest portion of examples that each filter can accept while maintaining a given accuracy level (AMAZON-POLARITY Dataset).</figcaption>
</div>

<p></p>

<p>
  Our top-performing filters considerably outshine our baseline methods in terms of coverage at the examined accuracy levels. In essence, they classify far fewer examples as "unreliable" while maintaining an equivalent accuracy level. These high-performing filters are the ones utilizing the CCS difference metric. We speculate that this is due to the CCS difference metric's superior ability to encapsulate the "confidence" of CCS as it directly contrasts the CCS outputs for the negative and positive prompts within a contrastive pair.

When compared to baseline methods, the techniques we employ considerably broaden the "accuracy-coverage frontier"—that is, the segment of responses a filter can approve while sustaining a particular accuracy level among the samples it admits. For instance, in the AMAZON_POLARITY dataset, the baseline filters need to reject ≥ 50% of examples for the remainder to achieve 90% accuracy. However, a CCS-based metric can reach the same accuracy level while only designating 3% of model outputs as unreliable. Similarly, in the IMDB dataset, the top baseline filter needs to admit only 8% of answers to achieve 99% accuracy among accepted answers, while the top-performing CCS-based filter can accept 29% of answers while still maintaining 99% accuracy.
</p>
