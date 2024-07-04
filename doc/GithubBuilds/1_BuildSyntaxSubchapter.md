### Build file syntax
Builds should be compatible with [Sylius builds](https://github.com/Sylius/Sylius/tree/1.14/.github), for example if we have plugin for Sylius 1.13
all our vendors (OS, PHP, Node, Symfony, MySQL) should have the same types.
- [Builds for sylius 1.12](https://github.com/Sylius/Sylius/tree/1.12/.github/workflows)
- [Builds for sylius 1.13](https://github.com/Sylius/Sylius/tree/1.13/.github/workflows)

### Contents of the builds.yml file
The `build.yml` file is where we create builds with the required database connection, for example.
In this file we should have steps like: unit test, behat tests etc.

### Contents of the coding_standard.yml file
This file is similar in structure to build.yml, for example the database connection isn't specified.
coding_standard.yml file should check ecs, phpStan etc.

### [Previous chapter](./GithubBuilds.md) / [Main page](./GithubBuilds.md) / [Next chapter](./GithubBuilds/2_EventsSubchapter.md)