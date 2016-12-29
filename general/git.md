# Git & GitHub
For source control we use GitHub. All projects are repositories under [scrambledeggs](https://github.com/scrambledeggs).

## Workflow
- Prefer to adapt [Gitlab flow](https://about.gitlab.com/2014/09/29/gitlab-flow/)
- The canonical version of the code for active development lives in `master`
- When a new feature is worked on, a branch will be created from `master`
- Use descriptive feature branch names and name them using [`kebab-case`](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles)
- For fixes, treat the same as features but add `fix` to branch name i.e `fix-your-face`
- `master` will be write protected and merging to `master` will require a [pull request](https://help.github.com/articles/about-pull-requests/).
  ### Deployments and Releases
  - For app submissions, squash commit to `release` branch and tag it with the version number
  - For deployed apps, squash commit to `production` branch and tag it with version number when applicable

## Versioning
- Prefer to use [semantic versioning](http://semver.org/) where applicable.

## Commits and Pull Requests
- Commit often. Small commits are acceptable. General guideline is to commit when tests are passing.
- Make your commits contextual. Avoid commits that does other things that the commit was intended to.
- Commit messages that makes sense. Usually in this pattern: `"{action} {purpose|reason} {target}"`, order may vary.
  Avoid:
  > 1. Fix syntax error
  > 2. Refactoring
  > 3. Added update() method
  > 4. Added some CSS

  Prefer:
  > 1. Refactored header logic in HomeViewController
  > 2. Added CSS for Home Screen
  > 3. Fixes input validation bug for user#save
  > 4. Added version update checker feature
  
- If your commit references a Github Issue, make sure to mention it in your PR's description using [this format](https://help.github.com/articles/closing-issues-via-commit-messages/)
- Rebase on top of the latest `master` before merging.
