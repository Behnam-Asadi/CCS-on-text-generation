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
