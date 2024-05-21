## Updates

<span style="color: green;">5/21 Updated SciPy library to version 1.10.</span>

<span style="color: green;">5/20 You can add more than 1 IVs for a model.</span>


## Q&A

**Q1: What are the expectations for statistical modelling?**

A: As experts, your extensive experience in conducting thorough and iterative model comparison, feature selection, and other analysis steps for your research projects is invaluable. While we encourage you to apply the knowledge and experience gained from your comprehensive analyses, for the purpose of our data collection, we aim for the quality of an "excellent class report." This means that the models should reasonably capture the relationships between variables and effectively address the research question. Please note that the focus should be on creating models that are reasonable and justifiable, rather than striving for the level of rigor and thoroughness typically required in practice. We trust your judgment in determining which models are appropriate for inclusion in the ground truth dataset.

**Q1.1: Should I record all the models I try or only the final models?**

A: Only include models that you would present to another analyst as reasonable. If a naive model shows critical issues (e.g. non-normal residuals), it is not reasonable and does not need to be recorded.

**Q1.2: Do I need to include ML models?**

A: No, ML models (e.g. SVM, Random Forest) should not be included because we don't have prediction tasks.

**Q2: What do you mean by alternative analysis decisions?**

A: For a given analysis problem, there is a decision space of reasonable analysis decisions surrounding the conceptual model and data transformations. Alternative decisions are equally justifiable choices within this decision space. The justifiability of decisions involves rationales tied to the data, your analysis method, prior experience, some domain knowledge.

**Q2.1: How should I specify alternatives if there are many combinations of reasonable transformations?**

A: Specify 2 main branches:
  - Branch 1: Apply all N transformations to the root, then apply subsequent transformations to the result.
  - Branch 2: No transformations needed. Apply subsequent transformations directly to the root.

**Q3: How do I handle a conceptual variable that maps to multiple columns in the data?**

A: Create separate models for each column that operationalizes the conceptual variable. 
Example: The conceptual variable "Referee Bias" is represented by "meanIAT" (implicit bias) and "meanExp" (explicit bias). Fit 2 separate models:
  - Model 1: redCards ~ playerSkinTone + meanIAT
  - Model 2: redCards ~ playerSkinTone + meanExp

**Q4: What are the guidelines for naming conceptual variables?**

A: Names should be clear and specific, but not overly detailed. 

Good examples:
- "Player Skin Tone", "Number of Red Cards", "Average Referee Implicit Bias"
- "Player Age", "Referee Years of Experience", "Game Location"

Too vague: 
- "Skin Tone", "Cards", "Bias"
- "Age", "Experience", "Location"  

Too specific:
- "Player's Skin Tone on 5-point Scale from Very Light to Very Dark"
- "Count of Red Cards Received by Player from Referee in Dyad" 
- "Referee's Mean Implicit Association Test D-score for Black vs White Faces"
- "Player's Age in Years at Start of 2012-2013 Season"
