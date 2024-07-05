### Jobs:
1. Tests should be run on the latest type of operating system compatible with sylius compilations, we use the latest version for sylius 1.13.
   ``runs-on: ubuntu-latest``, but you can use older versions if you need to.
2. Name

   The name should have a reference to the version of the environment:
   ```YML
      jobs:
        tests:
           name: "Sylius ${{ matrix.sylius }}, PHP ${{ matrix.php }}, Symfony ${{ matrix.symfony }}, MySQL ${{ matrix.mysql }}"
   ```
   Optionally, you can also add the action during which the build was performed, e.g. pull_request.
    ```YML
          jobs:
            tests:
               name: "Sylius ${{ matrix.sylius }}, PHP ${{ matrix.php }}, Symfony ${{ matrix.symfony }}, MySQL ${{ matrix.mysql }}, State Machine Adapter ${{ matrix.state_machine_adapter }}"
    ```
### Strategy:
1. Very important is set fail-fast to <b>false</b> - one unsuccessful test cannot decide the rest.
2. PHP - We need to check all versions supported by sylius version. If you want to write builds for sylius 1.13
   you should add all supported PHP versions.

   <b>Note: Remember to always check the supported PHP versions for the Sylius version you have chosen in the Sylius docs or repository.</b>

   <b>Note 2: If you are making builds for multiple Sylius versions, make sure you select a PHP version that is compatible with all the
   targeted Sylius versions.</b>
3. Symfony - Same case as PHP. Symfony version must be supported by sylius and PHP version.
4. Sylius - Sylius versions to check.
5. Node - Same case as PHP and Symfony, but we recommended to check on 2 versions of NODE  supported by the chosen sylius version.
6. Mysql - Same case as PHP, Symfony and Node.
7. Rest of the strategy configuration, (i.e. exclusions, state machine adapters, etc.) should be your specific requirements.
8. <b>Always work with the latest updates of PHP, Symfony, Sylius (prefix: "^" - example `sylius: ['^1.13']` )</b>

#### Example code:
   ```YML
    strategy:
      fail-fast: false
      matrix:
        php: ["8.1"]
        symfony: ["^5.4", "^6.0"]
        sylius: ["^1.12", "^1.13"]
        node: ["18.x", "20.x"]
        mysql: ["8.0"]
        state_machine_adapter: ["winzou_state_machine", "symfony_workflow"]

        exclude:
          -
            sylius: ~1.12.0
            state_machine_adapter: "symfony_workflow"
```
### [Previous chapter](./2_EventsSubchapter.md) / [Main page](../../README.md) / [Next chapter](./4_StepContextForBuildsSubchapter.md)