## Workflow and Git

1. Not confident with Git yet? Visit [the simple guide](https://rogerdudler.github.io/git-guide/)!

2. If you use Git in IDE - make sure it follows all standards. We don't care what GUI/CLI you use as long as you know what happens under the hood.

3. Commit messages should be written (if only it's possible) with the following convention:
   `TASK-123 - Max 64 characters description in english written in Present Simple.`. If you don't work with any ticket system,
   please skip the `TASK-123 - ` part. How to write a commit message and use Git in a proper way? Read [here](https://github.com/RomuloOliveira/commit-messages-guide).
4. We use Gitflow. So you have to:
    - Use correct branch prefixes: `hotfix/`, `feature/`, `refactoring/` etc,
    - Follow the Gitflow branching rules described in [the image](./gitflow.png),
    - If you use any ticket system, use the ticket name in the branch, after prefix. Jira can match branches and tasks together.

   Any kind of change of the flow has to be discussed to the team leader. 

### [Previous chapter](./2_TestingSubchapter.md) / [Main page](../../README.md) / [Next chapter](./4_OpenSourceSubchapter.md)