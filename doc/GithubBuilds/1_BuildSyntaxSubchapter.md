### Build file syntax
Builds should be compatible with [Sylius builds](https://github.com/Sylius/Sylius/tree/1.14/.github), for example if we have plugin for Sylius 1.13
all our vendors (OS, PHP, Node, Symfony, MySQL) should have the same types.
- [Builds for sylius 1.12](https://github.com/Sylius/Sylius/tree/1.12/.github/workflows)
- [Builds for sylius 1.13](https://github.com/Sylius/Sylius/tree/1.13/.github/workflows)

### Contents of the `builds.yml` file
The `build.yml` file is where we set up builds with the necessary database connection.
This file should include steps like unit tests, Behat tests etc.

### Contents of the `coding_standard.yml` file
This file is similar in structure to `build.yml`, but it doesn't specify the database connection.
`coding_standard.yml` file should check ECS, phpStan etc.

### [Previous chapter](../GithubBuilds.md) / [Main page](../../README.md) / [Next chapter](./2_EventsSubchapter.md)
