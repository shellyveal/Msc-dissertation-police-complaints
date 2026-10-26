# Quantifying Civilian Oversight
### Free-Text Analysis of Civilian Complaints against Albuquerque Police

This dissertation examines the use of optical character recognition (OCR), natural language processing (NLP), and statistical modeling to analyze civilian police oversight. The project uses 1,902 finding letters published by Albuquerque's Civilian Police Oversight Agency (CPOA) concerning complaints against the Albuquerque Police Department (APD).

The research develops a pipeline for extracting information from agency finding letters, evaluates machine learning approaches for extrapolating classifications from a hand-coded sample to the full corpus, and investigates relationships between complaint characteristics and recommended disciplinary outcomes.

## Main Inquiries
1. Can OCR be used to reliably extract relevant information for evaluating the output of Albuquerque's Civilian Police Oversight Agency?
2. How do varied NLP/statistical modeling pipelines compare when extrapolating case classifications from a hand-coded sample in the context of civilian oversight agency finding letters?
   - How do BERT word embeddings compare to SBERT sentence vectors? How does a simple TF-IDF, Naive Bayes pipeline compare to both?
3. What relationships, if any, exist between different classifications of complaints and the resulting recommended disciplinary outcomes?

## Key Results

|  Purpose  |  Metric  | Result | N (if applicable) |
|----|----|----|----|
| OCR Validation | Corpus CER | 3.1 % | 158,655 characters |
| Classifier Evaluation | Cross-validated Macro F1 range | 0.64-0.78 | Varies by classifier |
| Classifier Evaluation | Test-set Accuracy range | 0.68-0.97 | Varies by classifier |
| Odds of "Sustained" Findings traffic vs. non-traffic scenarios | Odds ratio | +45% | 1,902 observations|

*Note: Classifier metrics vary by complaint classification and modeling pipeline. The traffic-related result describes an association, not a causal effect.*

## Research Motivation

Natural language processing (NLP) is increasingly used in police science to study and predict crime, but its application to evaluating institutional mechanisms of police accountability remains comparatively underexplored. Civilian oversight agencies produce substantial records that can provide insight into complaint patterns, investigative findings, and accountability processes, yet these records are often difficult to analyze systematically at scale.

This project evaluates whether open-source OCR and text-classification methods can make such records more accessible to quantitative analysis. Although specialized OCR adaptations have been developed for government documents, the results suggest that baseline open-source OCR software, specifically Tesseract, can perform reasonably well on CPOA finding letters with relatively limited manual intervention.

By combining document extraction, supervised text classification, and statistical analysis, the project demonstrates an approach to studying civilian oversight using existing administrative records while examining the methodological challenges and limitations involved.

## Workflow

Collect the corpus — Mass-download publicly available CPOA finding letters.

1. **Extract and validate text** — Apply OCR to the letters and compare results against a hand-transcribed subset to evaluate extraction accuracy.

2. **Build a hand-coded sample** — Draw a random sample stratified over time and manually code complaint characteristics.

3. **Train and compare models** — Evaluate alternative NLP classification pipelines against the hand-coded labels.

4. **Extrapolate classifications** — Apply the fine-tuned SBERT + LinearSVC model to estimate binary complaint classifications across the full corpus.

5. **Analyze findings** — Use logistic regression models (GLMs with a logit link) to examine associations between complaint characteristics and CPOA findings.

**Additional analysis:** Track trends in Albuquerque Police Department (APD) nonconcurrence with CPOA findings over time, examining patterns in a key aspect of civilian oversight and accountability.


## Core Findings

This dissertation demonstrates the feasibility of using open-source OCR and natural language processing (NLP) to transform civilian police oversight records into structured data for quantitative analysis. Key findings are as follows:

- **OCR can support large-scale analysis of oversight records.** Tesseract OCR achieved a corpus character error rate (CER) of 3.1% on a manually transcribed validation sample, demonstrating that open-source document extraction can be effective with targeted cleaning and verification.

- **Sentence embeddings outperformed more traditional text-classification approaches.** Sentence-BERT (SBERT) paired with a linear support vector classifier generally outperformed TF-IDF pipelines and the more computationally expensive BERT model. Cross-validated macro F1 scores ranged from 0.64 to 0.78 across the final classification tasks, indicating moderate but useful predictive performance.

- **Complaint context was more informative of sustained findings than most allegation categories.** Traffic-related scenarios and complaints involving regulatory issues, such as reporting errors, were significant predictors of sustained CPOA findings. By contrast, complaints concerning officer demeanor showed no significant relationship with sustained outcomes.

- **Potential disparities in complaint outcomes warrant further investigation.** Allegations of bias-based policing showed a negative association with sustained findings, raising questions about how the agency evaluates discrimination claims. References to complainants' mental health also exhibited a significant negative association in the regression models, although the small number of relevant cases makes this result particularly uncertain. These patterns are preliminary and worthy of further exploration.

- **The public record leaves important questions about accountability unresolved.** Approximately 20.7% of observations included sustained findings, while 3.7% contained a recorded instance of APD nonconcurrence with recommended discipline. Nonconcurrence was concentrated in particular periods, reaching 54% of sustained cases in 2021. Sparse and inconsistent reporting of nonconcurrence limited the ability to assess how often disciplinary recommendations were ultimately modified.

Overall, this research offers a methodological foundation for evaluating civilian oversight through free-text analysis. It also identifies potential weaknesses in the transparency, consistency, and impartiality of complaint review that merit further investigation.

## Data 
Most finding letters used in this study can be downloaded from the City of Albuquerque's public records portal: https://onbase.cabq.gov/publicaccess/

Accessing the letters: > Albuquerque Boards and Commissions > Board or Commission: "POLICE OVERSIGHT ADVISORY BOARD" > record type: "OTHER"

The most recent finding letters published can be found at the CPOA's page on the City of Albuquerque website: https://www.cabq.gov/cpoa/case-outcomes/cpoa-finding-letters

*Note: Due to the occasional inclusion of personally identifiable information, parts of the corpus were redacted in the textual preprocessing stages of this dissertation, code for which is excluded*

## Limitations
- Associations moderated by performance of Tesseract's OCR, classifier accuracy, and regular expression extraction reliability, which was challenged by the changes in documentation format across years
- The choice to hand-code 10% of the sample, while defensible, brought limitations to the classifiers due to case imbalance
- Generalizability of the study's observations is limited greatly by the specificity of the CPOA's context
