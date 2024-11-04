# PR Process

## Trunk based development diagram
```mermaid
gitGraph TB:
  checkout main
  commit tag: "1.0.0"
  commit tag: "2.0.0"
  commit tag: "3.0.0"
  branch develop
  
  branch feat/dev1
  checkout feat/dev1
  commit
  commit
  commit
  commit
  checkout develop
  branch feat/dev2
  commit
  commit
  checkout develop
  merge feat/dev1
  checkout feat/dev2
  merge develop
  commit
  commit
  checkout main
  merge develop tag: "4.0.0"
  branch hotfix/error
  commit
  commit
  checkout main
  merge hotfix/error tag: "4.0.1"
  checkout develop
  merge main
  checkout feat/dev2
  merge develop
```
## Workflow

```mermaid
flowchart
  subgraph Development Phase
    start([development start]) --> cbranch
    cbranch[create branch from develop] ---> commit[commit changes]
    commit ---> push[push changes]
    push ---> initialpr[PR to develop]
    initialpr ---> devapp{PR Approved?}
    devapp -- Yes --> merge[merge + delete branch]
    devapp -- No --> commit
  end
  subgraph Testing Phase
    merge --> deploytest[deploy to testing environment]
    deploytest --> testendorse[endorse to QA]
    testendorse ---> qaapp{QA Result}
    qaapp -- FAILED --> fixeta[estimate fix time]
    fixeta --> cm[report estimated fix time to QA]
    cm ---> commit
  end
  subgraph Deployment Phase
    qaapp -- APPROVED --> crelease[create release branch]
    crelease --> vchange[apply version file changes]
    vchange --> initialpr
    vchange --> mainpr[PR to main]
    mainpr --> mainapp{PR approved?}
    mainapp -- Yes --> rel[create release]
    mainapp -- No --> cbranch
    rel --> prod[deploy tag to production]
  end
  subgraph Post Deployment Phase
    prod --> PPT
    PPT --> pptbugs{has bugs?}
    pptbugs -- YES --> sev[determine 
      + bug severity
      +       bugfix strat 
      + timeline
    ]

    sev --> dec{ hotfix or rollback?  }
    dec -- hotfix --> hf[create hotfix branch]
    hf --> mainpr
    dec -- rollback --> rb[rollback strategy + deploy prev version]
  end
  pptbugs -- NO --> complete([have fun 🥳])
  

```

### Development Phase
- create a new branch from `develop`
- commit changes
- make sure pull the latest `develop` branch
- push your branch to remote
- create a Pull Request - base branch: `develop`
- merge upon approvals and self confidence on your branch (Note: use merge commit)

### Testing Phase
- Make sure your branch is merged to `develop`
- Deploy `develop` to [testing environment]
- Endorse to QA with 7 characters commit hash
- Wait for QA results

#### Bug fixing in Testing Phase

- Estimate time needed to address bugs
- Report estimated time to QA
- Go back to Development Phase step

### Deployment Phase
- Adjust version files in repository

Node: in `package.json` - `version` field

Go: in `template.yml` - `Metadata.SemanticVersion` field

Rails: `.version` file

- Commit and push changes
- Create PRs for version change to both `main` and `develop` branches
- Wait for `main` PR approval
- create a release (in github)
- create tag (Note: don't put `v` prefix)
- Deploy tag created to production

### Post-deployment phase

## PR Time Period

- PRs should be resolved before the estimated Time commitment in ticket (ideally: the day before QA testing phase)

## Code Reviewers

Note: All are required unless stated

### PR to `develop`

- All *present* members of vertical team
- 1 Horizontal lead - If template.yml/database structure changed (BE)
- 1 Horizontal lead - Site/Screen structure (FE)
- 1 representative from other verticals (ideally junior)

Note:
Liability of Vertical Lead + Sub lead/s if branch merged to `develop` caused errors

### PR to `main`

- Same as above
- CTO/Engineering Manager
- 2 Horizontal Leads - If CTO/Engineering Manager is not available
- Other Technical leads if No Horizontal leads available

Note:
Liability of Vertical Lead + Sub lead/s + Approvers if `main` branch is broken



## Branch Naming Guide

- name pattern `prefix/context`
- All characters should be lowercase
- use `kebab-case` naming
- descriptive name but not too long
- **Max characters:** `15` (not counting prefix)



Use the following prefixes for branch names:

### `feat/`

for code changes that provide new features for the application

PR this branch to `develop` branch

examples:  
- New endpoints
- New web pages/app screens
- New sections in the app frontend


### `fix/`

for code changes that provides fixes for bugs found in the application

PR this branch to `develop` branch


### `chore/`

for maintenance code in app

PR this branch to `develop` branch


examples:
- updated dependencies
- copy changes
- asset changes
- code refactors
- deleting unused code/files

### `release/`

Merge `develop` branch changes to production

branch name will be the version of the release ex: `release/1.2.3`

PR this branch to `main` and `develop` branches


### `hotfix/`

- This must be checked out from `main` branch

- branch name will be the version of the release ex: `hotfix/1.2.3`

PR this branch to `main` and `develop` branches

### `test/`

For code changes that you want to test but not necessarily want to merge it (maybe for poc)

Rename to proper branch if you want to merge




## PR Description Outline

- change description
- breaking changes
- screenshots/videos (FE)
- route list for new pages/screens with short description
- related architecture diagrams/tables (BE)
- feature flag list/description updates
- environment variables list/description updates
- links to previous related and merged PRs
- related tickets



## Version Naming
Use semantic versioning system: https://semver.org/

Note: do not add `v` prefix in your versions!

## Notes

- Responsibility of the Requester to follow-up Reviewers