# Code Review For Analysts

## Contributing

These are my rough thoughts. I expect this document to change over time. I also greatly value community feedback on these thoughts. This includes both content and organization. Providing feedback via issues, pull requests, or discussions is welcomed and encouraged.

## About

There is a lot written about the benefits of code review. There is also a lot written about code review processes for software engineers or people who work with code in production and can take advantage of CI/CD pipelines or testing environments. I think there is less written about code review for analysts. If you know of any existing resources along these lines, please share.

Even though this repository is named `Code Review For Analysts`, your job title does not need to be `analyst` to benefit from this document; this document is for teams who write code for the purposes of data analysis and employ code review. This document does not apply to 
code review of R packages, as that follows as a separate process. Other job titles that may find this document useful include data scientist, ...

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

* Make sure you (the person submitting the code) have a clear understanding of the request. A reliable way to do this is to write the specifications as you understand them and send your written description to the requester to confirm you are on the same page.
* Check the associated project repository for existing code you can build upon.
* Check the associated project repository for any open pull requests. If there are open PRs, assess if they contain either related work or work that could cause a downstream merge conflict. If so, discuss a plan on how to handle.

## Getting started

### Is a review required? 

Code review is **not required** if:
* you are an experienced employee and you are re-executing existing work with minimal changes (i.e., updated data).
* you are an experienced employee and you are creating new work, but it has been agreed that there will be an alternative review process such as the clinical operations or data management team QCing the results.

Code review is **required** if you are a newer employee (<4 months) and for all other circumstances.

### Am I ready to request a review?


Depending on the scope of the code in progress, it may be beneficial to **have some initial conversation and feedback prior to officially requesting a review**. This is especially important for newer employees or newer bodies of work, 
and can be very beneficial to ensure that data context and nuances are appropriately accounted for. Having this initial conversation can help ensure that both the code submitter and reviewer are on the same page and greatly streamline the review process. 

After that:

1. Confirm that your code follows the guidelines below.
2. Restart R and re-execute your script in a fresh environment. Alternatively, re-render your quarto or rmarkdown document. The results should render warning and error free prior to submission.
3. Update the `renv.lock`, if applicable.
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
* Probably more types of comments expected.


### Code style

We follow the [tidyverse style guide](https://style.tidyverse.org/index.html).

### Data sets and steps

Ideally we would like to create the fewest possible relevant data sets and also retain data sets
that allow one to fully check new variable derivations.

* Data sets should be created in terms of unit of observation (1 row per subject, 1 row per imaging scan, etc.) and scope
(demographics, endpoints, etc).
* If the data sets have the same unit of observation (i.e., 1 row per subject) consider if they should be merged together
or if they should be separate.
* Operations/mutations on data sets should be done in a single data step when possible (i.e., don't create age
category in one data step and then bmi category in a separate data step).
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
* When grouping or reducing data, you will likely need to retain one data step to QC derivations and
a separate data step for reducing / refining the observations of interest.
* Always confirm that there is 1 row per expected unit of observation (i.e., 1 row per subject, or 1 row per scan per date).
  This can be done via `janitor::get_dupes()`.

### New variable creation  

In general, it is best practice to create new variables with new names and not overwrite existing variables. 
This is because in order to QC the new variable creation, you need to cross check the previous values against
the new values. When you overwrite variables, you lose the ability to QC them.

The only exception I make for this is when converting a character variable to a factor when all original
character values are retained.

### Binary variables

When creating binary variables, values should be coded as the logical `TRUE`/`FALSE` (not `yes`, `no`, etc.).
It is also very important to consider if `NA` is a valid coded option. In general, the difference between
coding an observation as `NA` vs `FALSE` is whether or not that observation should count in the denominator 
when tabulating the variable (values of `NA` by default generally do not contribute towards denominator, values 
of `FALSE` do).

### Case-when logic

Do not shoehorn complex logic into a `case_when` statement - this makes it very hard to QC results.
Instead, derive simpler variables that can be easily QC'd and then built upon for more complex logic.

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

Instead of embedding computational assignments inside of `case_when` statement, create simpler
variables that build up to this logic.

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

All new variables should have labels describing their meaning.

### Checking derived variables

In general, the best way to QC derived variables is to tabulate the newly derived variable against the original variable, as in

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

**Each newly created variable must be QC'd.**

#### NAs

Missing values are always of special interest. Verify the presence, if any, of missing values
in original variables and confirm that those observations are coded correctly in 
newly derived variables.

#### Example

In `df_1` above, results can be QCd but it would be hard to diagnose where code went wrong if results were unexpected.

```
df_1 |> count(dt_excel, dt_clean)
```

In this case, the 1900 condition failed to result in a missing value.

In `df_2`, each individual variable derived can be QCd to pinpoint where logic fails.

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
ask about it in a pre- code review discussion.





