### 28. False Positives:

*False positives* are findings which indicate the presence of vulnerabilities but which in fact are **not** vulnerabilities.
- could be due to incorrect assumptions or simplifications
- require further manual analysis on findings to investigate if they are indeed false or true positives
- many false positives increases manual effort in verification and lowers the confidence in the accuracy of the earlier automated/manual analysis
- true positives might sometimes be classified as false positives which leads to vulnerabilities being exploited instead of being fixed

### 29. False Negatives:

*False negatives* are **missed** findings that should have indicated the presence of vulnerabilities but which are in fact are not reported at all.
- could be due to incorrect assumptions or inaccuracies in analysis
- false negatives, per definition, are not reported or even realized unless a different analysis reveals their presence or the vulnerabilities are exploited
- many false negatives lowers the confidence in the effectiveness of the earlier manual/automated analysis.
