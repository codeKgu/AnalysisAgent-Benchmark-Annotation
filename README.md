# Anaysis Benchmark Annotation Protocol

## Intro

## Part 1 Performing your own analysis

## Part 2 Inputting your analysis in the UI

After the analysis, we need to input our analysis into a specific format. We describe this process here. We include a video tutorial as well, which inputs the analysis (very basic toy example) specified in this [notebook](tutorial/soccer_tutorial.ipynb) into the UI.
[![](images/youtube.png)](https://www.youtube.com/watch?v=XkaSKPQbSZ8)

### Step 1 Add Data Transformations

Transformations are individual unit data transformations we perform to the data to operationalize it before our model. We have defined a set of **transformation verbs** that represents each transformation. Each transformation should also have only one output column. Therefore, if your involve multiple of these unit transformations, we ask that you break this down. These are described in the [here](TransformVerbs.md).

![](images/transforms1.png)

For each transform we need to specify the following (e.g., [0:07 in the tutorial](https://youtu.be/XkaSKPQbSZ8?si=TcW-ZYASWMTuNYuX&t=7)):

#### 1.1 Basic Info

- **Specification Name**: A short unique name in snake case (using "\_" as spaces) to identify this transformation (makes it easy to identify)
- **Specification**: a more descriptive sentence detailing the transformation. Another analyst should be able to code the exact transformation based on this description
- **Rationale**: Explain why this transformation was chosen. This is not needed for every transformation

#### 1.2 Branching and Dependencies

If the transform relies on another transform to occur before for it to be valid or even run, we
specify that here. A _branch_ represents an alternative code path. Each branch depends on a different set of parent transformations (each branch can depend on 1 or more transformations).

✨✨ Note, all parent transforms for each branch should be specified already.

**Branches Field**

In the **Branches** field we can add a branch by first specifying which transformation(s) are this current specification's immediate parents. (e.g., [4:28 in the tutorial](https://youtu.be/XkaSKPQbSZ8?si=u0P8XukYIHPCBR6j&t=268)).

In this `groupby_player` transform spec example below, we specify four different alternative branches. Each of these branches depends on two previously specified transforms. The branches are:

1. `calculate_club_win_rate`, `calc_rater_min`
2. `calculate_club_win_rate`, `calc_rater_max`
3. `calculate_club_win_rate`, `calc_rater_mean`
4. `calculate_club_win_rate`, `filter_rater_combined`

![](images/branches.png)

**Branch Info**

For each branch, if there are multiple branches in upstream parent specifications, you will the alternatives paths here as well.

For example, for the `agg_columns_after_groupby` transform which depends on the `groupby_player` transform, we see the four alternative transform paths and the graph of all parent specifications. Specifically, because `groupby_player` is a parent of the current specification, and has more than alternative branch, we can that here.
![](images/branch_info.png)

For the graph, each _node_ in this graph (with the exception of ROOT) represents a specified unit transformation.
Each _directed edge_ A -> B represents a specified dependency of transform B on transform A.

**This may not be a comprehensive set so if your transform does not fit let us know**

**Conditions**

Sometimes, we may only want a current specification to run based on some condition or constraints on the choice of upstream specification. We can write a condition to specify this (see [7:36](https://youtu.be/XkaSKPQbSZ8?si=--UcC2-hBykYCLyL&t=455) and [11:38](https://youtu.be/XkaSKPQbSZ8?si=R_HvpADef9kJLFUv) in the tutorial video).

A condition is a python boolean expression that is evalutated on upstream branching. Each transformation on an upstream branch is treated as a list. We can use the keywords `and`, `or`, `in` and `not` to write our expression. For example, if our current spec has the `groupby_player` in its ancestors (i.e., there is a path from `groupby_player` to the current spec) and we want to say that this specification should only occur on the branch containing "filter_rater_combined" we can write the expression below:

```python
# only run ind 3 will occcur
"filter_rater_combined" in groupby_player
```

Alternatively, if we want branches that do not have "calc_rater_min" then we write:

```python
# run inds 1,2,and 3 will occur
not "calc_rater_min" in groupby_player
```

Likewise, if we want paths 1 and 3 to occur, we can write:

```python
"calc_rater_max" in groupby_player or "filter_rater_combined" in groupby_player
```

#### 1.3 Transform Code and Transform Detials

This part is super important to be correct because this is what we compare benchmark participants against.

**Transform Code**

![alt text](images/transform_code1.png)
Based on your specified branches and dependencies, you should see all the parent code paths.
Write the code for the transformation here. It will be the same for each path and if the dependencies are specified correctly there should be no issue.

After writing the code, run all paths to ensure that the code runs without execution errors and that the result is what you intended to specify. This part is super important because we will be comparing the output of each transformation with those of benchmark participant's.

**Transform Details**

Based on your code, add the transform details:

- **Transform verb**: Select one of the transform verbs that fit this transformation. Note we seperate groupby and post groupby operations even though they are often written together as one expression. These verbs could not be comprehensive so if you find that a verb does not fit your transformation please put "other" for now and let us know!
- **Input to output column mapping**: Based on the selected transform we add a mapping of the input columns to every output column for this transformation. Input columns are columns of the dataframe that are used in the transformation. The output column is any column that is created as a result of the transformation (this column name must match the columns created in the data from running the code)
- **Tags**: [Optional] These are used just for you to keep track of your specified transforms

### Step 2 Add Conceptual Variables

This is where we specify the high-level variables that are related to the research question and dataset (see [13:09 in the tutorial](https://youtu.be/XkaSKPQbSZ8?si=5eGWNf30f5iuCRoB&t=789)). Please inlcude all conceptual variables that you think are relevant and **justified**.

> A conceptual variable is an abstract idea or concept that researchers are interested in studying. It represents a broad concept or construct that cannot be directly observed or measured. Instead, researchers create operational definitions or measurable indicators that represent the conceptual variable. For example, "human intelligence" is a conceptual variable that researchers might operationalize using measures such as IQ tests, academic achievement, or problem-solving tasks. Conceptual variables are fundamental to the research process as they guide the development of hypotheses and research designs.

![](images/cvar.png)

For each conceptual variable please include:

- **Variable type**: IV, DV, or Control
- **Text specification**: A description fo the conceptual variable/construct
- **Rationale**: Why is this variable included
- **Final Output Column(s) for the Concept**: Specify all column(s) either originally in the data or derived from your transformations that you use to operationalize this conceptual variable

### Step 3 Add Statistical Model

Here, we specify all alternative statistical models (see [14:46 in the tutorial](https://youtu.be/XkaSKPQbSZ8?si=k2Yh6sFC2S6_XhV4&t=886)).

![](images/model.png)

For each model please specify:

- **Model specification**: The name of the model. This should be specific enough that another analyst can write the code for your model based on this specification.
- **Associated columns**: Which derived columns you had specified in your conceptual variables are included in this model. The associated columns should include at least an DV and IV.
- **Rationale**: Why is this model included?
- **Code**: Copy paste the code you used to write this model
- **Tags**: [Optional] These are used just for you to keep track of your specified models
- **Associated transforms**: Sometimes after deriving a column, we may make additional transformations to the data before our model such as additional filters or droping missing values etc. Here we can specify the final transformation(s) associated with this model.

### Step 4 Review all specifications

After inputting our specifications, we perform a final review to ensure there are no mistakes or inconsistencies (see [16:07 in the tutorial](https://youtu.be/XkaSKPQbSZ8?si=ZM1vMwMPiB_pgoGw&t=968)).

You can view your specifications (transforms, conceptual variables, and statistical models) in the `Summary` page.

In the `Review` page, ensure that there are no warning messages. If there are warning messages, please go back to the specifications and make any neccessary fixes.

Finally, run all code paths again to perform a final check that there are no execution errors.
