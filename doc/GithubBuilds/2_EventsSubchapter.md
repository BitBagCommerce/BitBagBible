### Events:
1. Builds must work for events listed bellow:
   - push
   - pull request
   - release, where releases has type 'created'.

   #### Example code:
   ```YML
      on:
        push:
          branches-ignore:
            - 'dependabot/**'
        pull_request: ~
        release:
          types: [created]
        workflow_dispatch: ~
   ```
### [Previous chapter](./1_BuildSyntaxSubchapter.md) / [Main page](../../README.md) / [Next chapter](./3_JobsAndStrategySubchapter.md)
