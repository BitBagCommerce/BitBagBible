# Github CI process
We are required to test all changes through github actions, every change in plugin must have passed the test
before changes can be committed to the main branch.
Our standard is to create a build files in each of our plugins. The `.github/workflows` directory
must contain `build.yml` and `coding_standard.yml` files.
# Subchapters
### [1. Build file syntax]('./doc/GithubBuilds/1_BuildSyntaxSubchapter.md')
### [2. Events]('./doc/GithubBuilds/2_EventsSubchapter.md')
### [3. Jobs and Strategy for builds]('./doc/GithubBuilds/3_JobsAndStrategySubchapter.md')
### [4. Steps context of the builds files]('./doc/GithubBuilds/')
### [5. Example full builds files]('./doc/GithubBuilds/')

