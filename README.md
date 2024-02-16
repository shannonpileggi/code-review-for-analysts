# Code Review For Analysts

## Contributing

These are my rough thoughts. I expect this document to change over time. I also greatly value community feedback on my thoughts. Providing feedback via issues, pull requests, or discussions is welcomed and encouraged.

## About

There is a lot written about the benefits of code review. There is also a lot written about code review processes for software engineers or people who work with code in production and can take advantage of CI/CD pipelines or testing environments. I think there is less written about code review for analysts. If you know of any existing resources along these lines, please share.

Even though this repository is named `Code Review For Analysts`, your job title does not need to be `analyst` to benefit from this document; this document is for teams who write code for the purposes of data analysis and employ code review. Other job titles that may find this document useful include data scientist, ...

## Scope

My team works with clinical trials data and programs in R using tidyverse syntax. The specifications written here are tailored to that context; I am happy to consider making it more generalizable in the future.

My team also works in a specific project template. For now, some of those details may go in here as well.

## Aims

The aims of this document are to:

1. Provide a team with a clear set of expectations and guidelines to follow before they submit code for review.
2. Empower a team to learn how to review code.

## Code review guidelines

### Is a review required? 

Code review is **not required** if:
* you are an experienced employee and you are re-executing existing work with minimal changes (i.e., updated data).
* you are an experienced employee and you are creating new work, but it has been agreed that there will be an alternative review process such as the clinical operations or data management team QCing the results.

Code review is **required** if you are a newer employee (<4 months) and for all other circumstances.

### Type of review

### How in depth should it be?

### How fast do you need it

### Am I ready to request a review?

Depending on the scope of the code in progress, it may be beneficial to have some initial conversation and feedback prior to officially requesting a review. This is especially important for newer employees or newer bodies of work. Having this initial conversation can help ensure that both the code submitter and reviewer are on the same page and greatly streamline the review process.

### Who should the reviewer be?

### Code style

### Code smells

#### Data steps

#### Case-when logic

#### Checking derived variables






