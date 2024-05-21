## Updates

5/21 Updated SciPy library to version 1.10.

5/20 You can add more than 1 IVs for a model.


## Q&A

**Q1: Should I record all the models I try or only the final models?**

A: Only include models that you consider reasonable and would present to another analyst. If an initial model shows issues, iterate to improve it, but only record the final model(s) you deem appropriate.

Examples:
- You fit a model predicting red cards from player skin tone, but the residuals are highly non-normal. After log-transforming the outcome variable, the residuals look good. Record only the final log-transformed model.
- You fit several models predicting red cards from player skin tone, with different combinations of control variables. After comparing their performance, you decide that controlling for number of games and player position is most appropriate. Record only this final model with your chosen controls.

**Q2: How should I specify alternatives if there are many combinations of reasonable transformations?**

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
