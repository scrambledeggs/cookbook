# Git & GitHub
For source control we use GitHub. All projects are repositories under [scrambledeggs](https://github.com/scrambledeggs).

## Workflow
- Prefer to adapt [Gitlab flow](https://about.gitlab.com/2014/09/29/gitlab-flow/)
- Use descriptive feature branch names and name them using [`kebab-case`](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles)

## Versioning
- Prefer to use [semantic versioning](http://semver.org/) where applicable.

## Branching

### `main` / `master`

- The canonical version of the code for active development lives in
- The main working branch of the application. All releases should be merged to this branch
- Write protected and merging to `main`/`master` will require a [pull request](https://help.github.com/articles/about-pull-requests/).

### `feat/feature-name`

feat - short for feature - this branch will contain code for a new feature or functionality such as:
- new textbox
- new banner
- new web page or screen
- new login flow

### `chore/chore-name`

this branch contains code for code maintenance such as:
- refactoring code
- upgrading dependencies
- copy changes
- deleting unused files
- updating assets/images

### `fix/fix-name`
this branch contains fixes on bugs found in the application

### `hotfix/1.2.3`
this is a special `fix/` branch that needs immediate deployment on production due to the bug blocking major flows/behavior in the application

### Naming Conventions

- all `lowercase`
- use `-` as spaces
- add a prefix on the type of your branch (chore/fix/feat/hotfix)

## Branching and Pull Request Process

```mermaid
---
config:
  layout: dagre
---
flowchart TD
    start(["start"]) --> checkout["Branch out from main"]
    checkout --> develop["develop/code feature"]
    develop --> prmain["PR to main"]
    prmain --> approved{"approved? 3 reviewers - any"}
    approved -- NO --> develop
    approved -- YES --> mergemain["merge to main"]
    mergemain --> n1["deploy main branch to staging + endorse to QA"]
    n1 --> n2["passed?"]
    n2 -- NO --> checkout
    n2 -- YES --> n3["backend/frontend?"]
    n3 -- BACKEND/FRONTEND(web, microsite,etc) --> n31["QA sign off"]
    n4["create release/x.xx.xx from main"] --> n5["PR (3 reviewers including 1 TL)<br>+ merge"]
    n5 --> n6["create TAG x.xx.xx from release/x.xx.xx"]
    n6 --> n7["deploy TAG x.xx.xx to prod"]
    n7 --> n8["PPT"]
    n8 -- FAIL --> n9["hotfix or rollback?"]
    n8 -- SUCCESS --> n11(["done"])
    n9 -- hotfix --> n10["create hotfix/x.xx.xx from TAG x.xx.xx"]
    n10 --> n12["PR (3 reviewers including 1 TL)<br>merge"]
    n12 --> n13["create TAG x.xx.x from hotfix/x.xx.xx"]
    n13 --> n7
    n3 -- FRONTEND (APP/MP) --> n14["create release/x.xx.xx-rc-(n+1) from main<br>ex: release/6.5.2-rc-1"]
    n14 --> n18@{ label: "PR (any 3 reviewers )<br style=\"--tw-scale-x:\">merge" }
    n15["create TAG x.xx.xx-rc-(n+1) from release/x.xx.xx-rc-(n+1)"] --> n16["build pre-prod<br>endorse TAG to QA"]
    n16 --> n17["PPT"]
    n18 --> n15
    n17 -- FAIL --> n14
    n17 -- SUCCESS --> n19["QA signoff"]
    n19 --> n20["create release/x.xx.x from tag/x.xx.xx-rc-(n+1)"]
    n20 --> n21@{ label: "PR (3 reviewers including 1 TL)<br style=\"--tw-scale-x:\">merge" }
    n21 --> n22["create tag/x.xx.x"]
    n22 --> n23["deploy to prod/app store/play store"]
    n23 --> n24["hotfix?"]
    n24 -- YES --> n25["create hotfix/x.xx.xx from TAG x.xx.xx"]
    n24 -- NO --> n28(["done"])
    n25 --> n26["PR (3 reviewers including 1 TL)<br>+ merge"]
    n26 --> n27["create TAG x.xx.x from hotfix/x.xx.x"]
    n27 --> n23
    n9 -- rollback --> n29["deploy previous tag"]
    n31 --> n4
    n2@{ shape: diam}
    n3@{ shape: diam}
    n8@{ shape: diam}
    n9@{ shape: diam}
    n18@{ shape: rect}
    n17@{ shape: diam}
    n21@{ shape: rect}
    n24@{ shape: diam}
    n26@{ shape: rect}

```



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
- Rebase on top of the latest `main`/`master` before merging.
  - If your branch is not synced/has conflicts with `main`/`master` (For example when a PR gets merged while you have a pending PR)
    1. On your local copy `git pull origin master`
    2. Fix conflicts
      - On things like `db/schema.rb` it's better to regenerate schema i.e. `bundle exec rails db:schema:dump`
