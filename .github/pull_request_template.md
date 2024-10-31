# How to use this

The initial code submitter should follow and fill in this checklist. The code reviewer is then responsible for confirming
that the code adheres to the checklist.

If the PR is a "Refresh", everything after `Pre-work` may be deleted or ignored.

# Review type

Please select one or provide additional details.

- [ ] **Refresh**
- [ ] **Standard**
- [ ] **Scoped** This might be appropriate for pre-existing, legacy code that requires some updates. This body of code may not satisfy the guidelines below; however, the gains in re-factoring the code is low impact and therefore not worthwhile.

# Dates

* **Data cut date:**

* **Target date for review completion:**

* **Target date to deliver output:**

# Request details

* **Link to original request and/or copy and paste description here.**

* **Any additional links for supporting documents?**

* **Which specific files need to be reviewed?** 

* **Link to output deliverable for inspection.**

# Pre-work

- [ ] Make sure you  have a clear understanding of the request.
- [ ] Consider if you should have an initial conversation and prior to writing any code to align on expectations and approach.
- [ ] Check for any existing code you can build upon.
- [ ] Check for any open pull requests. If there are open PRs, assess if they contain either related work or work that could cause a downstream merge conflict. If so, discuss a plan on how to handle.
- [ ] Check for any open issues that may relate to the request and/or be resolved through completion of the request.
- [ ] Execute `renv::status()`. Correct any problems (e.g. updates to `renv.lock` or package version changes via `renv::restore()`).

# Programming

## General

- [ ] Sufficient comments are in place.
- [ ] Code adheres to tidyverse style guide.
- [ ] Commented out code in your submission is rarely valuable and best omitted. It most often reflects older work that is no longer relevant. If you
do really want to leave in commented out code, be sure to add a comment describing why it is important to retain.
- [ ] Code should execute without warnings. If you have encountered a warning you are not sure how to resolve,
ask about it in a pre- code review discussion.

## Data "steps"

- [ ] are created in terms of unit of observation (1 row per subject, 1 row per imaging scan, etc.) and scope (demographics, endpoints, etc).
- [ ] are completed in a single data step when possible (i.e., don't create age category in one data step and then bmi category in a separate data set).
- [ ] are logically organized by joins, filters, and mutates.
- [ ] contain variable flags for filtering operations (instead of filtering "on the fly").
- [ ] are sufficiently retained for data validation (when reducing data, you will likely need to retain one data step to inspect derivations and a separate data step for reducing / refining the observations of interest).
- [ ] always confirm that there is 1 row per expected unit of observation (i.e., 1 row per subject, or 1 row per scan per date). This can be done via janitor::get_dupes().

## Variables

- [ ] Binary variables should be coded value as the logical `TRUE`/`FALSE`. If there is a strong rationale behind character coding (i.e., `yes`, `no`) the go forth with that; however, beconsistent in coded values (i.e., `Yes` vs `yes`).
- [ ] Consider if `NA` is a valid coded option. In general, the difference between coding an observation as `NA` vs `FALSE` is whether or not that observation should count in the denominator when tabulating the variable (values of `NA` by default generally do not contribute towards denominator, values of `FALSE` do).
- [ ]  `case_when` statements are preferred over `if_else()`; furthermore, they should not contain complex logic or variables derived on the fly - this makes it challenging to validate programming. Instead, derive simpler variables that can be easily inspected and then built upon for more complex logic.


## Data validation

- [ ] ALL newly created variables must have code inspecting and confirming the derivation.
- [ ] For quick checks, a `count` is sufficient. As in, `data |> count(variable_new, variable_old)`. To see all results, you can use `View()` or `print(n = Inf)`.
- [ ] For high impact, long lasting data curation, use the `affirm` package.


# Deliverables

## General

- [ ] All outgoing deliverables (pdf, excel, images, etc) have been reviewed for quality and consistency.

## Tables

- [ ] The underlying denominator must be consistent across multiple tables in the same report; otherwise differences in denominators should be explainable by table text or labelling.
- [ ] If multiple tables are present showing different populations with different denominators, there should be table that shows how the populations break down to explain how that number was arrived at.
- [ ] If needed, changing denominator within the same table is evident by showing n / N in the table body.
- [ ] Variables within a table are shown in a logical order, especially if demonstrating sub-population breakdowns. This order could be from largest to smallest sub-population, if applicable.
- [ ] Levels within a categorical variable have a logical ordering (alphabetical, by frequency, or whatever makes sense for the report).
- [ ] Unknown values are appropriately reported (often best practice to make NAs explicit).

# Ready?

## Before submitting the PR

- [ ] Consider if you should have a an initial conversation and feedback prior to officially requesting a review to ensure that to ensure that data context, areas of high code-complexity or risk, and request nuances are appropriately accounted for.
- [ ] Restart R and re-execute your script in a fresh environment. Alternatively, re-render your quarto or rmarkdown document. The results should render warning and error free prior to submission.


## After merging the PR

- [ ] Celebrate! 🥳
- [ ] 💡Consider if you should [make a release](https://github.mskcc.org/pages/pcctc/data-science-guide/403-ongoing.html#make-a-release) on the repository.
