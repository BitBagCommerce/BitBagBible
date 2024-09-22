# BitBag Coding Bible

![BitBag Bible](doc/image.png "BitBag Bible")

*This project follows [PSR coding standards](https://www.php-fig.org/psr/) and those recommended by Sylius and Symfony projects in this order.
It is extended based on the experience of the whole BitBag team for everybody's sake.*

# Contents
### [1. General](#general)
### [2. Architecture](doc/Architecture.md)
- #### [2.1 Frameworks](doc/Architecture/1_FrameworksSubchapter.md)
- #### [2.2 Tools](doc/Architecture/2_ToolsSubchapter.md)
### [3. Development](doc/DevelopingPlugins.md)
- #### [3.1 Code Style](doc/DevelopingPlugins/1_CodeStyleSubchapter.md)
- #### [3.2 Testing](doc/DevelopingPlugins/2_TestingSubchapter.md)
- #### [3.3 Workflow](doc/DevelopingPlugins/3_WorkflowSubchapter.md)
- #### [3.4 Open Source](doc/DevelopingPlugins/4_OpenSourceSubchapter.md)
### [4. Github CI](doc/GithubBuilds.md)
- #### [4.1 Build syntax](doc/GithubBuilds.md)
- #### [4.2 Events](doc/GithubBuilds/1_BuildSyntaxSubchapter.md)
- #### [4.3 Jobs and strategy](doc/GithubBuilds/2_EventsSubchapter.md)
- #### [4.4 Step context](doc/GithubBuilds/3_JobsAndStrategySubchapter.md)
- #### [4.5 Example builds](doc/GithubBuilds/4_StepContextForBuildsSubchapter.md)
- #### [4.6 Example builds](doc/GithubBuilds/5_ExampleBuildsSubchapter.md)

# General
1. No `/.idea` and other local config files in `.gitignore`. Put them into a global gitignore file,
   read more on https://help.github.com/articles/ignoring-files/#create-a-global-gitignore.

2. All side-effect files (or directories) from project dependencies should be put into project `.gitignore` file.

3. For project development we require *NIX system kernel (for working with Git, servers, maintaining Symfony application etc.). We require from you working on Windows (WSL only) / MacOS / Ubuntu.

4. Code that is not documented doesn't exist. Writing documentation of a bundle/plugin/project is part of the
   development process. Remember that in the end, someone else is going to use your code who might not know each part of it.
   This also applies to writing GitHub repository descriptions, basic composer package information, etc.

   Specially write/update information of:
   - Information of needed tools, including their versions.
   - Package installation process. Specially please follow your instructions from the beginning to end, to be sure the installation process is completed.
   - How to run the application / tests (set of commands/steps needed to do it).
   - If you prepare new major version of a package, please write/update UPGRADE.md file to describe the breaking changes. Please note, not every code-breaking change needs a new major version of application.
