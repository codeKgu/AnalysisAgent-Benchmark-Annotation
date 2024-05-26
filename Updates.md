## Updates
+ 5/26
  
  **Update on the cross-validation phase:**

  Since we have collected more than 1 annotations for a few datasets, we are preparing materials for cross-annotator validation. We will let you know once this is ready in the system. You can then proceed to do some binary labeling for LLM-generated and annotator-generated inputs.
   
+ 5/25

  **Update on timeline:**

  Thanks for everyone's hard work and consistent engagement so far. We want to remind everyone about our timeline. **The deadline for annotations is Sunday (6/9). Starting today, everyone have roughly another 2 weeks to complete ~10 datasets.** We want to make sure we have enough time to merge and process everyone's submission. Let us know if you have change in plans.
  
  **Also, please let me know once you have completed all the assigned datasets so that I can assign new ones to you as soon as possible.**

+ 5/24

  **Update on important common issues:**
  
  We've been through everyone's first few submissions of completed annotations, and here are a few issues and things to look out for.
    1. Make sure that ALL the columns that you've assigned to conceptual variables are included in at least one of your models.
        When adding conceptual variables and assigning columns, check if you will include these columns in the same model:
        - If so, it should be under separate conceptual variables.
        - If not, it should be under the same variable and added when you add an alternative model.
    ONE EXCEPTION: one-hot encoding may result in multiple columns that are conceptually the same. In this case, you can add them under the same conceptual variable and use them in the same model.
    3. When do I add a dependency?
        1. If a transformation modify the row of the dataframe (e.g. filtering some values at the beginning), all your following transformations should depend on this > ✅ add dependency
        2. If there is a previous transformation that operates on the same column > ✅ add dependency
        3. If a transformation modify a column that is independent of the other columns (i.e., when to do this transformation doesn't matter) > 🙅 no need to add dependency
           
+ 5/21 Updated SciPy library to version 1.10.

+ 5/20 You can add more than 1 IVs for a model.


## Q&A for common issues

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

A: Names should be clear and specific, but not overly detailed. It should be clear enough for LLM or someone else to undersatnd but not going into details of how you've operationalized it.

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
