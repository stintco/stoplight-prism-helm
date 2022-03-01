# Contributing to Stoplight Prism Helm Chart

:+1::tada: First off, thanks for taking the time to contribute! :tada::+1:

The following is a set of guidelines for contributing to Stint Open Source codebase. These are mostly guidelines, not rules. Use your best judgment, and feel free to propose changes to this document in a pull request or in an issue.

#### Table Of Contents

[How Can I Contribute?](#how-can-i-contribute)
  * [Reporting Bugs](#reporting-bugs)
  * [Suggesting Enhancements](#suggesting-enhancements)
  * [Code Contribution](#code-contribution)
  * [Pull Requests](#pull-requests)

[Styleguides](#styleguides)
  * [Git Commit Messages](#git-commit-messages)
  * [Specs Styleguide](#specs-styleguide)


## How Can I Contribute?

### Reporting Bugs

This sections guides you through bug discovery and reporting process. Sometimes, you may encounter a bug but not have time to fix it properly. Reporting it tho may help one with the need to do required work on the codebase to potentially improve its general quality. Following these guidelines helps maintainers and the collective understand your report :pencil:, reproduce the behavior :computer: :computer:, and find related reports :mag_right:.

Before submitting a bug report:

- Check that this bug is not reproducible on the **latest stable** version.


### Suggesting Enhancements

When you see potential feature that could be added or rework, you can submit issues to help maintainer as well as the collective to understand what is at stake, and help defining our best approach to cover the general use-case.

Please, whenever you are about to create a significant change, make sure to create an issue first or speak with the maintainer of the repo before you gets your hands in the code.


### Code Contribution

When contributing to the codebase, do follow the guideline defined in this document to make sure you won't need extensive amount of rework when your PR will be reviewed.


### Pull Requests

Anyone can review pull requests, any comments rather it is a change request, as suggestion or a tip is welcome! For each pull requests one of the maintainers will have to review and approve the change.

Overall, the acceptance decision will follow this process:

- [ ] Is this change needed in the library, or does it relate to a very specific need
- [ ] Will this change invalidate some of the existing behavior? If yes, does it need to go under a new mayor release?
- [ ] Is this change keeping the library agnostic of the email service underneath?


## Styleguides

### Git Commit Messages

This project follows the [Angular Commit Message Format](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#-commit-message-format).

Scopes should be omitted, except if it helps understanding the change.

* Use the present tense ("Add feature" not "Added feature")
* Use the imperative mood ("Move cursor to..." not "Moves cursor to...")
* Limit the first line to 72 characters or less
* Reference issues, pull requests or shortcut liberally after the first line

Here is a summary of the template to follow template

```
<type>: <short summary>
(Optional)
<BLANK LINE>
<change description + migration instructions if any>
(Optional)
Closes #<issue number, SC ticket>
```

### Specs Styleguide

> Any changes should be reviewed and approved by the maintainers before any work get started on it.

When updating the spec:
- The documentation must be updated and completed
- Existing examples must be updated
- Additional examples are suggested but not recommended



