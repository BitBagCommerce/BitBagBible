# Github CI process
We require to run GitHub Actions builds on all PRs. Every change must pass the build
before changes can be merged to the main/master branch.
Our standard is to create build files for each of our plugins. The `.github/workflows` directory
must contain `build.yml` and `coding_standard.yml` files.
# Subchapters
### [1. Build file syntax](./GithubBuilds/1_BuildSyntaxSubchapter.md)
### [2. Events](./GithubBuilds/2_EventsSubchapter.md)
### [3. Jobs and Strategy for builds](./GithubBuilds/3_JobsAndStrategySubchapter.md)
### [4. Steps context of the builds files](./GithubBuilds/4_StepContextForBuildsSubchapter.md)
### [5. Builds files examples](./GithubBuilds/5_ExampleBuildsSubchapter.md)

