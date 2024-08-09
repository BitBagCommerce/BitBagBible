### Jobs:
1. Tests should be run on the latest operating system that is compatible with Sylius compilations.
   We use the latest version for Sylius 1.13 with `runs-on: ubuntu-latest`, but you can use older versions if needed.
2. The name should include reference to the environment version:
   ```YML
      jobs:
        tests:
           name: "Sylius ${{ matrix.sylius }}, PHP ${{ matrix.php }}, Symfony ${{ matrix.symfony }}, MySQL ${{ matrix.mysql }}"
   ```
   Optionally, you can add the action during which the build was performed, such as `pull_request`.
    ```YML
          jobs:
            tests:
               name: "Sylius ${{ matrix.sylius }}, PHP ${{ matrix.php }}, Symfony ${{ matrix.symfony }}, MySQL ${{ matrix.mysql }}, State Machine Adapter ${{ matrix.state_machine_adapter }}"
    ```
### Strategy:
1. Very important is set fail-fast to <b>false</b> - one unsuccessful test cannot affect the others.
2. PHP - We need to check all PHP versions supported by Sylius version. If you're writing builds for sylius 1.13
   you should add all supported PHP versions.

   <b>Note: Always check the supported PHP versions for the Sylius version you've chosen in the Sylius docs or repository.</b>

   <b>Note 2: If you are making builds for multiple Sylius versions, make sure you select a PHP version that is compatible with all the
   targeted Sylius versions.</b>
3. Symfony - Symfony version must be supported by Sylius and PHP version.
4. Sylius - Versions of Sylius to check.
5. Node - The Node version must be compatible with the Sylius version. We recommend checking the two LTS (Long-Term Support) versions supported by the Sylius version you are using.
6. Mysql - Same case as PHP, Symfony and Node.
7. Rest of the strategy configuration, such as exclusions and state machine adapters, should be based on your specific needs.
8. <b>Always work with the latest updates of PHP, Symfony, Sylius (prefix: "^", like: `sylius: ['^1.13']` )</b>

#### Example code:
   ```YML
    strategy:
      fail-fast: false
      matrix:
        php: ["8.1", "8.2", "8.3"]
        symfony: ["^5.4", "^6.0"]
        sylius: ["^1.13"]
        node: ["18.x", "20.x"]
        mysql: ["8.0"]
        state_machine_adapter: ["winzou_state_machine", "symfony_workflow"]
```
### [Previous chapter](./2_EventsSubchapter.md) / [Main page](../../README.md) / [Next chapter](./4_StepContextForBuildsSubchapter.md)
