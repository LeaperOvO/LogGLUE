# LogGLUE: A Log Analysis Benchmark for Log Understanding Evaluation

In recent years, deep learning-based automated log analysis works have attracted much attention in the field of software engineering, and these automated log analysis works aim to exploit deep learning methods to analyze logs to improve the efficiency of operations and maintenance, such as anomaly detection, root cause analysis, and so on.

While these deep learning methods have achieved promising performance, these methods are often experimented on different tasks and datasets, making it difficult to uniformly and fairly evaluate the abilities of these models. To fairly evaluate the ability of these models of log understanding, similar to [GLUE](https://arxiv.org/pdf/1804.07461.pdf), we collect publicly available datasets to build a comprehensive benchmark to evaluate the model's ability. At the same time, we organize and make these data publicly available to facilitate subsequent research. In addition to collecting public datasets on specific tasks, such as anomaly detection, we also manually annotate new datasets, including: dimension inference, fine-grained token classification, in order to evaluate the model's abilities of numerical understanding and domain term understanding.

We are currently optimizing and refining this benchmark, and a paper of this benchmark will be released in the future.
We will release more comprehensive evaluation datasets and results in the future, including more tasks, more datasets, and more widely used models. We are also in the process of evaluating the ability of large language models to understand logs, and will soon publish our results.


***Our work is in continuous update...***

### Tasks and datasets

| Tasks               | DataSize | Task Type                             |Metrics|
|:--------------------|:---------|:--------------------------------------|:----------|
| Anomaly detection*  | -        | Binary Classification                 | Acc./Precision/Recall/F1 |
| Dimension Inference | 0.3K     | Multi-class Classification (14-class) |   Acc./Precision/Recall/F1 |
| Fine-grained Token Classification | 0.3K     | Multi-class Classification (9-class)  | Acc./Precision/Recall/F1 |
| Log Summary | 116K     | Binary Classification                 | Acc./Precision/Recall/F1 |
| Logging Inference | 2.4K     | Binary Classification                 | Acc./Precision/Recall/F1 |
| Semantic Inference | 3K       | Multi-class Classification (3-class)  | Acc./Precision/Recall/F1 |

*The data for anomaly detection is supported by [Loghub](https://github.com/logpai/loghub), and due to the huge size of the dataset, we will publish it later.

#### Dimension Inference
Dimension Inference is a new constructed task designed to evaluate the numerical understanding of models. 
Logs contain a large amount of numerical information, and previous works ignore the ability of numerical understanding in logs.
We argue that the ability of numerical understanding in logs is important. 
To evaluate whether the model can understand the meaning of the numerical values, we collected logs containing numerical data from LogHub and then labeled the data manually. 
Specifically, we categorize the unit of dimension into three main categories, namely time (e.g: ms), space (e.g: gb), and rate (e.g: gb/s). 
The complete categories include: KB, MB, GB, BYTE, MS, S, US, MIN, DAY, H, B/S, KB/S, MB/S, GB/S.
Then the logs containing these keywords are matched by a regular rule, where the label is the matched keyword, and the keywords in the logs are replaced with [MASK] as input. 
Finally the dataset is manually checked to avoid noise.

```
## Space example
Raw Log:
       MemoryStore started with capacity 17.7 GB.
Input:
       MemoryStore started with capacity 17.7 [MASK].
Output:
       GB

## Time example
Raw Log:
       Reading broadcast variable 9 took 160 ms 
Input:
       Reading broadcast variable 9 took 160 [MASK] 
Output:
       MS
```
#### Fine-grained Token Classification
Fine-grained Token Classification is also a new constructed task to evaluate whether the model can understand the domain term. Specificlly, 
fine-grained token classification task is to identify a single specific token in the log. The log content is highly brief, and many tokens in the log have different meanings.
For example, the numerical token 22 in "error: Bind to port 22 on 0.0.0.0 failed: Address already in use." represents the port, and 30914 in "full wake request (reason 2) 30914 ms" 
represents a quantitative relationship. We define the tokens in the log can be divided into the following categories, FUNCTION, IP, URL, PATH, VALUE and OTHER. 
Numerical tokens are further divided into STATE, COUNT and PORT. STATE represents the status code in the log, COUNT represents the quantity relationship, and PORT represents the port number in the network. OTHER represent tokens that do not belong to these classes, so there are 8 classes in total.
We select the log data from Loghub, and then identify the url, ip and other information in logs through regular rules. Due to the noise that may exist in the result of matching results, we invite two graduate students of software engineering to confirm the result, and the unanimously passed result is retained.
We keep the results confirmed by both.

```
## Example1: 
Input:
     ["Established session 0x14ed93111f20027 with negotiated timeout 10000 for client /10.10.34.13:37177", /10.10.34.13:37177]
Output:
     URL
     
## Example2: 
Input:
     ["error: Bind to port 22 on 0.0.0.0 failed: Address already in use.", 22]
Output:
     PORT

## Example3: 
Input:
     ["full wake request (reason 2) 30914 ms", 30914]
Output:
     COUNT
```

#### Log Summary
Log Summary is a log fragment and natural language description matched task, proposed by [Logsummary](https://github.com/WeibinMeng/LogSummary) 
and the corresponding dataset is publicly available. We collect this task to evaluate the model's semantic understanding of logs. We construct the task as a matched task, where the input is a pair consisting of log fragment and natural language description, and the output is True(represents Match) /False (represents UnMatch).

```
## Example1:
Input:
     Log:
     INFO dfs.FSNamesystem : BLOCK* NameSystem.allocateBlock : /user/root/rand/_temporary/_task_200811092030_0001_m_000786_0/part-00786. blk_5065822136883359230
     INFO dfs.FSNamesystem : BLOCK* NameSystem.addStoredBlock : blockMap updated : 10.251.199.86 : 50010 is added to blk_6754497465540127826 size 67108864
     INFO dfs.FSNamesystem : BLOCK* NameSystem.addStoredBlock : blockMap updated : 10.251.65.203 : 50010 is added to blk_-4741373429281641397 size 67108864
     Summary:
     blockMap updated;
Output:
     True
```
#### Logging Inference
The goal of logging inference is to evaluate whether the model can understand the source of the logs. 
Logs are generated by code, and the logic of log generation is embedded in the code. The dataset is supported by [LoggingDescriptions](https://github.com/logpai/LoggingDescriptions).
We build  logging inference as a matched task, where the positive samples are codes and their corresponding logs, and the negative samples are codes and other logs.

```
## Example1:
Input:
     Code:
     public static String dateToString(Date date, String dateFormat) {
           if (date == null || dateFormat == null || dateFormat.isEmpty()) {
                  return ""; 
           }  
           try {  
                 SimpleDateFormat formatter = new SimpleDateFormat(dateFormat); 
   	return formatter.format(date).toString();  
   	} 
   	catch (Exception e) { 

     Logging:
     error in coverting datetostring format
Output:
     True
```



#### Semantic Inference
The semantic inference task is a new constructed task designed to recognize semantics between logs. 
Logs often contain keywords with opposite states, e.g. start and stop. Although the overall content of the logs is similar, the semantics are completely opposite. We argue that the ability to make accurate semantic inferences is a key capability for understanding logs. 
The input of this task is two logs and the output is the semantic relation of three categories, which is True for semantic similarity, False for semantic opposite and Other for semantic irrelevance.
We collect the logs from loghub and then get the enhanced samples by replacing the words in the logs with WordNet.
The samples with synonym replacement are positive samples and the samples with antonym replacement are negative samples. In addition, we randomly select two logs that are not the same to construct the Other relationship. 
Finally the dataset is also manually checked to avoid noise.

```
## Example1:
Input:
      Log1: session opened for user root by (uid=0)
      Log2: session closed for user root
Output:
      False

## Example2:
Input:
      Log1: rts: kernel is down for reason 1001rts: bad message header: invalid cpu…
      Log2: rts: kernel terminated for reason 1001rts: bad message header: invalid cpu…
Output:
      True

## Example3:
Input:
      Log1: session opened for user root by (uid=0)
      Log2: rts: kernel terminated for reason 1001rts: bad message header: invalid cpu…
Output:
      Other
```

### Acknowledgements and existing Datasets

Our tasks and datasets are supported by the following works.

| Description                                                                                                    | Paper                                                                                                                 | Resource Link                     |
|:---------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------|:----------------------------------|
| Loghub:  Loghub maintains a collection of system logs, which are widely used for anomaly detection.            | [Loghub: A Large Collection of System Log Datasets towards Automated Log Analytics](https://arxiv.org/abs/2008.06448) | https://github.com/logpai/loghub  |
 | Logparser provides a toolkit and benchmarks for automated log parsing.                                         | [Tools and Benchmarks for Automated Log Parsing](https://ieeexplore.ieee.org/abstract/document/8804456/)                                                                    | https://github.com/logpai/logparser|
| LoggingDescriptions maintains the <code,log> pairs to support the logging description generation research.     |              [Characterizing the natural language descriptions in software logging statements](https://dl.acm.org/doi/abs/10.1145/3238147.3238193)               |https://github.com/logpai/LoggingDescriptions|
| LogSummary: an automatic, unsupervised end-to-end log summarization framework for software system maintenance. |                 [LogSummary: Unstructured Log Summarization for Software Systems](https://ieeexplore.ieee.org/abstract/document/10017337)                 |https://github.com/WeibinMeng/LogSummary|


### License
The datasets are freely available for research or academic work, subject to the following conditions: Any usage refer to the repository https://github.com/LeaperOvO/LogGLUE.