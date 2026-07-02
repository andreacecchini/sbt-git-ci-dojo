# Sbt-Git-CI Dojo

In this repository we will practice:

- `sbt` as build tool
- `git` as version control system
    - focus on `git flow`
- `Github Actions` as workflow system for CI/CD

---

## Git

### Git Flow

#### Branch Model

- 2 permanent branches
    - `main`
        - releases
        - production ready
    - `develop`
        - integration branch for `feature` branches.
    - `git flow init` setups them
- others live-short branches
    - `feature`
        - one for each feature
        - branch off from `develop`
            - `git flow feature start my-feature`
                - creates `feature/my-feature`
                - switches to it
        - merge back to `develop`
            - `git flow feature stop my-feature`
                - deletes `feature/my-feature`
                - switches back to `develop`
    - `release`
        - when `develop` has enough features
        - only bug fixes and documentation here
        - branch off from `develop`
            - `git flow release start 1.0.0`
                - creates `release/1.0.0`
                - switches to it
        - merge back to `main`
            - `git flow release finish 1.0.0`
                - tags the commit as `1.0.0`
                - merges even into `develop` to don't lost fixes
                -
    - `hotfix`
        - only for _emergency_
        - branch off from `main`
            - `git flow hotfix start fix-my-bug`
                - creates `hotfix/fix-my-bug`
                - switches to it
        - merge back to `main`
            - `git flow hotfix finish fix-my-bug`
                - merges even into `develop` to don't lost fixes

#### Cheatsheet

```bash
git flow init                     # Initialization
git flow feature start <name>     # Start new feature
git flow feature finish <name>    # Complete feature
git flow release start <version>  # Prepare new release
git flow release finish <version> # Complete release
git flow hotfix start <name>      # Start an emergency fix
git flow hotfix finish <name>     # Complete the emergency fix
```

#### PR

- `finish` commands merge **locally**
    - bypassing code review
    - we can not use such automatization...

- what to do instead:
    1. push feature to the remote
        - `git flow feature publish my-feature`
        - `git push -u origin feature/my-feature`
    2. create the PR
        - `gh pr create` $\to$ interactive
        - `gh pr create --base develop --head feature/my-feature --title "My Feature" --body "Feature description..."`
    3. wait for CI and code review
    4. after approval
        1. `git checkout develop`
        2. `git pull origin/develop`
        3. `git branch -d feature/my-feature`
        4. `git remote prune origin`

##### About Releases and HotFix

- **releases** and **hotfixes** merges automatically in `main` and `develop` when `finish` is triggered
    - we must open two PR!
        - one merging in `main`
        - one merging in `develop
    - we must handle release/fix tag!
        - tag the release on `main`
            - not in `develop`

#### Notes

- Do not forget to `push` to remote the branches along with their tags!
    - `--tags`

---

## Sbt

---

## CI/CD
