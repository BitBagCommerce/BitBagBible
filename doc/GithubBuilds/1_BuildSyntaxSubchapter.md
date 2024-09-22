### Build file syntax
Builds should be compatible with [Sylius builds](https://github.com/Sylius/Sylius/tree/1.14/.github). For example, if we have plugin for Sylius 1.13,
all our vendors (OS, PHP, Node, Symfony, MySQL) should have the same types.
- [Builds for sylius 1.12](https://github.com/Sylius/Sylius/tree/1.12/.github/workflows)
- [Builds for sylius 1.13](https://github.com/Sylius/Sylius/tree/1.13/.github/workflows)

### Contents of the `builds.yml` file
The `build.yml` file is the place where we set up build commands which require database connection.
This file should include steps like unit tests, behat tests etc.

### Contents of the `coding_standard.yml` file
This file is similar in structure to `build.yml`, but it doesn't run commands which require database connection.
The `coding_standard.yml` file should check ECS, PHPStan etc.

### [Previous chapter](../GithubBuilds.md) / [Main page](../../README.md) / [Next chapter](./2_EventsSubchapter.md)
