# Code Review For Analysts

## Contributing

These are my rough thoughts. I expect this document to change over time. I also greatly value community feedback on these thoughts. This includes both content and organization. Providing feedback via issues, pull requests, or discussions is welcomed and encouraged.

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

First, confirm that your code follows the guidelines below.

Depending on the scope of the code in progress, it may be beneficial to have some initial conversation and feedback prior to officially requesting a review. This is especially important for newer employees or newer bodies of work. Having this initial conversation can help ensure that both the code submitter and reviewer are on the same page and greatly streamline the review process.

## Code review guidelines

### Note

We started writing code before we had a team with code review. We have a lot of older code that does not conform to our new standards. This means:

1. If you use any exisiting code as a starting point note that the existing code might not have gone through this code review process. If this a new body of work, you are still responsible for updating it according to these guidelines.
2. The point of this is **not** to update our entire code base according to these standards. The point is to raise the bar for new work, or for previous high impact high friction work. 

### Type of review request

* **Standard**
* **Scoped:** This might be appropriate for pre-existing, legacy code that requires some updates. This body of code may not satisfy the guidelines below; however, the gains in re-factoring the code is low impact and therefore not worthwhile.
* **Expedited:** This is a review for work that requires a quick turnaround. The reviewer may be lenient on review specifications below; however, note that shortcuts taken in specifications will likely lengthen the review process.
  
### Docmentation

### Code style

### Code smells

#### Data steps

#### Case-when logic

#### Checking derived variables

#### Commented code

Don't do it. Just don't keep commented out code in your submission. If you are concerned about losing work, use of regular GitHub releases to mark merged work for specific deliverables or data changes would allow us to quickly return to this point in time.






