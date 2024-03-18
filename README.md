# Code Review For Analysts

## Contributing

These are my rough thoughts. I expect this document to change over time. I also greatly value community feedback on these thoughts. This includes both content and organization. Providing feedback via issues, pull requests, or discussions is welcomed and encouraged.

## About

There is a lot written about the benefits of code review. There is also a lot written about code review processes for software engineers or people who work with code in production and can take advantage of CI/CD pipelines or testing environments. I think there is less written about code review for analysts. If you know of any existing resources along these lines, please share.

Even though this repository is named `Code Review For Analysts`, your job title does not need to be `analyst` to benefit from this document; this document is for teams who write code for the purposes of data analysis and employ code review. This document does not apply to 
code review of R packages, as that follows as a separate process. Other job titles that may find this document useful include data scientist, (bio)statistician, and analytics engineer.

## Related Resources

* _Code Review in the Lab, or … How do you review code that accompanies a research project or paper?_, ROpenSci 2018, [recording](https://ropensci.org/commcalls/2018-10-16/), [blog post](https://ropensci.org/blog/2018/11/29/codereview/)
* _Opionated Analysis Development_, Hillary Parker 2017, [paper](https://peerj.com/preprints/3210/), [slides](https://www.slideshare.net/hilaryparker/opinionated-analysis-development), [recording](https://posit.co/resources/videos/opinionated-analysis-development/)
* _R Code Review Checklist_, Travis Gerke 2021, [repository](https://github.com/tgerke/r-code-review-checklist)
* _Code smells and feels_, Jenny Bryan 2018, [slides](https://speakerdeck.com/jennybc/code-smells-and-feels), [recording](https://www.youtube.com/watch?v=7oyiPBjLAWY)

## Scope

My team works with clinical trials data and programs in R using tidyverse syntax. The specifications written here are tailored to that context; I am happy to consider making it more generalizable in the future.

My team also works in a specific project template. For now, some of those details may go in here as well.

## Aims

The aims of this document are to:

1. Provide a team with a clear set of expectations and guidelines to follow before they submit code for review.
2. Empower a team to learn how to review code.


## Before any code is written

* Make sure you (the person submitting the code) have a clear understanding of the end user's request. A reliable way to do this is to write the specifications as you understand them and send your written description to the requester to confirm you are on the same page.
* Check the associated project repository for existing code you can build upon.
* Check the associated project repository for any open pull requests. If there are open PRs, assess if they contain either related work or work that could cause a downstream merge conflict. If so, discuss a plan on how to handle.
* Similarly, check the project repository for open issues that may relate to the request and/or be resolved through completion of the request.

## Getting started

### Is a review required? 

Code review is **not required** if:
* you are an experienced employee and you are re-executing existing work with minimal changes (i.e., updated data).
* you are an experienced employee and you are creating new work, but it has been agreed that there will be an alternative review process such as the clinical operations or data management team reviewing the results.

Code review is **required** if you are a newer employee (<4 months) and for all other circumstances.

### Am I ready to request a review?


Depending on the scope of the code in progress, it may be beneficial to **have some initial conversation and feedback prior to officially requesting a review**. This is especially important for newer employees or newer bodies of work, 
and can be very beneficial to ensure that data context, areas of high code-complexity or risk, and request nuances are appropriately accounted for. Having this initial conversation can help ensure that both the code submitter and reviewer are on the same page and greatly streamline the review process. 

After that:

1. Confirm that your code follows the guidelines below.
2. Restart R and run `renv::status()`. Correct any problems (e.g. updates to renv.lock or package version changes via `renv::restore()`).
3. Restart R and re-execute your script in a fresh environment. Alternatively, re-render your quarto or rmarkdown document. The results should render warning and error free prior to submission.
4. In the PR, inform the reviewer when the deliverable is due or a suggested timeline for review that it can be prioritized accordingly.

## Code review guidelines

### Note

We started writing code before we had a team with code review. We have a lot of older code that does not conform to our new standards. This means:

1. If you use any existing code as a starting point note that the existing code might not have gone through this code review process. If this a new body of work, you are still responsible for updating it according to these guidelines.
2. The point of this is **not** to update our entire code base according to these standards. The point is to raise the bar for new work, or for existing high impact high friction work. 

### Type of review request

* **Standard**
* **Scoped:** This might be appropriate for pre-existing, legacy code that requires some updates. This body of code may not satisfy the guidelines below; however, the gains in re-factoring the code is low impact and therefore not worthwhile.
* **Expedited:** This is a review for work that requires a quick turnaround. The reviewer may be lenient on review specifications below; however, note that shortcuts taken in specifications will likely lengthen the review process.
  
### Documentation

High level documentation should be added in the top of the script / quarto document regarding the aims of this body of work.
This documentation should also include links to relevant internal documents when applicable (i.e. protocols, data request forms, etc.).
If conversations occurred to clarify or confirm any portion of that request, then that should be added to the high level description as well 
(e.g., "Confirmed with Susan that the dates are for the 2023 calendar year."). If the scope of the request evolves, then the documentation
should be aligned with the updated scope.

Comments are also expected throughout the script.
* New data sets should have a brief comment about expected form and content
(i.e., 1 row per subject containing demographic information).
* Joins should have comments explaining type of join used.
* New variables should have comments about what it is and notes about nuances in derivation.
* Grouping operations should have comments about what level the grouping is at and why.
* If during the review process an inquiry is raised and resolved via a comment, please document that as a comment in the script (and not just leave in the review conversation on GitHub).
* Probably more types of comments expected.


### Code style

We follow the [tidyverse style guide](https://style.tidyverse.org/index.html).

### Naming

#### Object names

Objects in R such as data frames should be preceded by the object type, e.g. `lst_medidata_data_raw`, `df_main` and `vec_subjects`.

#### Variable names

Sometimes variable names must be defined according to a pre-specified data model (e.g. CDISC/SDTM/ADaM). 

When we do have control over naming conventions, employ the [Column Names as Contracts](https://www.emilyriederer.com/post/column-name-contracts/) framework concept. 
The general hierarchy is `type_contents_detail` (where `detail` is optional), for example, `ID_PATIENT`, `DT_PSA_BASELINE`.

Binary variables (i.e., coded as `TRUE`, `FALSE`) should have the name and value in alignment. For example, a variable such as
`flag_psa_invalid` should have a value of `TRUE` when the record is invalid; alternative `confirm_psa_valid` would
have a value of `TRUE` when the record is valid.

### Data sets and steps

Ideally we would like to create the fewest possible relevant data sets and also retain data sets
that allow one to fully check new variable derivations.

* Data sets should be created in terms of unit of observation (1 row per subject, 1 row per imaging scan, etc.) and scope
(demographics, endpoints, etc).
* If the data sets have the same unit of observation (i.e., 1 row per subject) consider if they should be merged together
or if they should be separate.
* Operations/mutations on data sets should be done in a single data step when possible (i.e., don't create age
category in one data step and then bmi category in separate data sets).
* Left joins are generally preferred for readability, but make sure rows from the right-hand data frames that you wish to keep aren't silently dropped.
The [tidylog](https://github.com/elbersb/tidylog) package can be helpful to provide verbose feedback about the implications of various joins.
* Data steps should generally be organized with all joins followed by all filters followed by mutating statements.
```
df_subjects <- df_1 |>
  left_join(df_2, ...) |>
  left_join(df_3, ...) |>
  filter(...) |>
  mutate(
     ...
  )
```

This is as opposed to patterns like this, which is harder for a reviwer to follow.
```
# please don't do this
df_subjects <- df_1 |>
  filter(...) |>
  left_join(df_2, ...) |>
  mutate(...) |>
  filter(...) |>
  left_join(df_3, ...) |>
  mutate(...)
```

* **Use `filter` statements sparingly/carefully.** Sometimes they are appropriate, for example, to remove
screen fail subjects. More often than not, you would be better served to create new derived
variables indicating subjects of interest as we are often interested in not only observations that
meet a certain criteria but also the full context of how many subjects did not meet that criteria.
Flags are encouraged for this purpose, and this practice aligns with SDTM standards, e.g. `ENRLFL`, `SAFFL`, and `ITTFL`.
* When grouping or reducing data, you will likely need to retain one data step to inspect derivations and
a separate data step for reducing / refining the observations of interest.
* Always confirm that there is 1 row per expected unit of observation (i.e., 1 row per subject, or 1 row per scan per date).
  This can be done via `janitor::get_dupes()`.

### New variable creation  

In general, it is best practice to create new variables with new names and not overwrite existing variables. 
This is because in order to inspect the new variable creation, you need to cross check the previous values against
the new values. When you overwrite variables, you lose the ability to compare new values against the old values to
verify the derivation.

The only exception that can be made for this is when converting a character variable to a factor when all original
character values are retained.

This rule applies to objects / data frames in memory as well: best not to overwrite `df_main` with a new `df_main`, rather, rename or create intermediate data frames if necessary. When creating or renaming intermediate objects, it can be helpful to keep naming conventions consistent, descriptive, and related to the original object. (i.e, `df_main` -> `df_main_long` -> `df_main_final` For a dataframe that was pivoted to long format in an intermediate step before prodcuing the final object.)

### Binary variables

When creating binary variables, there is a preference to code value as the logical `TRUE`/`FALSE`. If there
is a strong rationale behind character coding (i.e., `yes`, `no`) the go forth with that; however, be
consistent in coded values (i.e., `Yes` vs `yes`).

It is also very important to consider if `NA` is a valid coded option. In general, the difference between
coding an observation as `NA` vs `FALSE` is whether or not that observation should count in the denominator 
when tabulating the variable (values of `NA` by default generally do not contribute towards denominator, values 
of `FALSE` do).

### Case-when logic

Avoid shoehorning complex logic into a `case_when` statement as this makes it very hard to inspect results.
Instead, derive simpler variables that can be easily inspected and then built upon for more complex logic.

For example, consider date variables imported from excel that need to be converted to proper date formats
with dates marked as year 1900 set to missing.

```
df_raw <- tribble(
  ~id, ~dt_excel,
  1,  42467,
  2,  NA,
  3,  2   
)
```
In this first data step, computational statements are employed on both the left hand side
of the evaluation and the right and side of the assignment.

```
df_1 <- df_raw |> 
  mutate(
    dt_clean = case_when(
      year(dt_excel) <= 1900 ~ NA_Date_,
      .default = janitor::convert_to_date(dt_excel)
    )
  )
```
If results are unexpected here, it is hard to determine if it is because 
1. `year(dt_excel)` failed,
2. `year(dt_excel) <= 1900` failed, or
3. `janitor::convert_to_date(dt_excel)` failed

Instead of embedding computational assignments inside of a `case_when` statement, create simpler
variables that build up to this logic in a piece wise way.

```
df_2 <- df_raw |> 
  mutate(
    dt_proper = janitor::convert_to_date(dt_excel),
    dt_year = year(dt_proper),
    dt_year_valid = dt_year > 1900,
    dt_clean = case_when(
      dt_year_valid ~ dt_proper,
      .default = NA_Date_
    )
  )
```

All new variables should have labels describing their meaning. In addition to creating meaningful [variable names], the [`labelled` package](https://larmarange.github.io/labelled/) can assist with the creation and modification of appropriate metadata labels.

### Checking derived variables

In general, the best way to inspect derived variables is to tabulate the newly derived variable against the original variable, as in

`data |> count(variable_new, variable_old)`.

Often, these span more than the default print method. To see all results,
you can use `View()` or `print(n = Inf)`.

```
# does not work well in Rmarkdown/quarto, can be disruptive when submitting entire scripts
# but also has easy sorting and filtering features to examine results
data |> count(variable_new, variable_old) |> View()

# prints all results to console, no easy sorting/filtering features
data |> count(variable_new, variable_old) |> print(n = Inf)
```

**Each newly created variable must be tabulated against original variables used in the derivation to
confirm the new variable is coded as intended.**

While inserting code like this that will prompt the reviewer to examine key aspects of the workflow suffices, as stated, it can be disruptive when submitting/sourcing entire scripts or executing Rmarkdown/Quarto files. One alternative to this may be to create small test chunks that will allow for the script to execute and only stop or provide warnings when an error, or unexpected condition, is met:

```
# Create a logic/boolean object that checks the derivation
string_id_check <- !is.character(df_4$subject_id)

# Create a conditional statement based on the logic check
if(string_id_check){

# The data frame and relevant variables that need to be reviewed will open in the IDE...
view(df_4 |> count(subject_id, flag_string_id))

# And an error message will display in the console alerting the reviewer
stop("Subject ID in df_4 is NOT a string. Please Review!")
}

```

Creating tests can be as simple or complex as you desire, but care should be taken to assess the overall impact of not only the work/output, but the tests as well. Creating tests can be beneficial with complex or legacy code where the workflow is well-defined and the effort is deemed appropriate. However, with smaller operations, work that requires an expedited review, or work with low-impact (like one-off data requests), tests may not be appropriate or required.

#### NAs

Missing values are always of special interest. Verify the presence, if any, of missing values
in original variables and confirm that those observations are coded correctly in 
newly derived variables. If you haven't done so, also confirm if missing values hold a special or nuanced meaning within the context of the data.

#### Example

In `df_1` above, results can be inspected but it would be hard to diagnose where code went wrong if results were unexpected.

```
df_1 |> count(dt_excel, dt_clean)
```

In this case, the 1900 condition failed to result in a missing value.

In `df_2`, each individual variable derived can be inspected to pinpoint where logic fails.

```
df_2 |> count(dt_proper, dt_excel)
df_2 |> count(dt_year, dt_proper)
df_2 |> count(dt_year_valid, dt_year)
df_2 |> count(dt_clean, dt_year_valid, dt_proper)
```

### Commented code

Commented out code in your submission is rarely valuable and best omitted. It most often reflects older work that is no longer relevant. If you
do really want to leave in commented out code, be sure to add a comment describing why it is important to retain. 

If you are removing commented out code and you are concerned about losing work, use of regular GitHub releases to mark merged work
for specific deliverables or data changes is recommended to allow you to quickly return to this point in time.

### Warnings

Code should execute without warnings. If you have encountered a warning you are not sure how to resolve,
ask about it in a pre-code review discussion. In some cases, warnings may occur due to nuances in the data or changes in the packages that are used within the workflow. If warnings of this nature are not altering the workflow and output being reviewed, you should provide comments summarizing why the warnings are expected/are occurring.

### Deprecated Functions

There may be cases where legacy code uses functions that are currently deprecated or superseded. In most cases, the use of deprecated functions will return a warning:

```
df_3 <-
data_frame(
           id = 1:3,
           offstudy = c(TRUE,FALSE,FALSE)
           )

#> Warning: `data_frame()` was deprecated in tibble 1.1.0.
#> ℹ Please use `tibble()` instead.
```

In cases where warnings or errors are produced from deprecated or superseded functions, you should update the code accordingly. If updating the function introduces significant change into the existing workflow, this should be documented on a high level in the top of the script and within the workflow where the changes occurred.

## TODO

Explore what of this can be detected automatically via `lintr`:

* Commented code linter <https://lintr.r-lib.org/reference/commented_code_linter.html>.
* Join/filter/mutate patterns in data steps??

Explore husky for linting <https://typicode.github.io/husky/>.





