# LogGLUE: A Log Analysis Benchmark for Log Understanding Evaluation

In recent years, deep learning-based automated log analysis works have attracted much attention in the field of software engineering, and these automated log analysis works aim to exploit deep learning methods to analyze logs to improve the efficiency of operations and maintenance, such as anomaly detection, root cause analysis, and so on.

While these deep learning methods have achieved promising performance, these methods are often experimented on different tasks and datasets, making it difficult to uniformly and fairly evaluate the abilities of these models. To fairly evaluate the ability of these models of log understanding, similar to [GLUE](https://arxiv.org/pdf/1804.07461.pdf), we collect publicly available datasets to build a comprehensive benchmark to evaluate the model's ability. At the same time, we organize and make these data publicly available to facilitate subsequent research. In addition to collecting public datasets on specific tasks, such as anomaly detection, we also manually annotate new datasets, including: dimension inference, fine-grained token classification, in order to evaluate the model's abilities of numerical understanding and domain term understanding.

We are currently optimizing and refining this benchmark, and a paper of this benchmark will be released in the future.
We will release more comprehensive evaluation datasets and results in the future, including more tasks, more datasets, and more widely used models. We are also in the process of evaluating the ability of large language models to understand logs, and will soon publish our results.


Our work is in continuous update...

### Tasks and datasets




### Acknowledgements and existing Datasets

Our tasks and datasets are supported by the following works.

|                                                  Description                                                   | Paper                                                                                                                 | Resource Link                     |
|:--------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------|:----------------------------------|
|      Loghub:  Loghub maintains a collection of system logs, which are widely used for anomaly detection.       | [Loghub: A Large Collection of System Log Datasets towards Automated Log Analytics](https://arxiv.org/abs/2008.06448) | https://github.com/logpai/loghub  |
 |                     Logparser provides a toolkit and benchmarks for automated log parsing.                     | [Tools and Benchmarks for Automated Log Parsing](https://ieeexplore.ieee.org/abstract/document/8804456/)                                                                    | https://github.com/logpai/logparser|
|   LoggingDescriptions maintains the <code,log> pairs to support the logging description generation research.   |              [Characterizing the natural language descriptions in software logging statements](https://dl.acm.org/doi/abs/10.1145/3238147.3238193)               |https://github.com/logpai/LoggingDescriptions|
| LogSummary: an automatic, unsupervised end-to-end log summarization framework for software system maintenance. |                 [LogSummary: Unstructured Log Summarization for Software Systems](https://ieeexplore.ieee.org/abstract/document/10017337)                 |https://github.com/WeibinMeng/LogSummary|


### License
The datasets are freely available for research or academic work, subject to the following conditions: Any usage refer to the repository https://github.com/LeaperOvO/LogGLUE.