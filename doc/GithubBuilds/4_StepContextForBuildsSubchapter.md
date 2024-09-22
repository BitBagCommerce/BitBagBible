### The build.yml file:


   0. Use supported actions version
      ```yaml
      - uses: actions/checkout@v3
      ```

   1. PHP setup
      ```YML
           -
              name: Setup PHP
              uses: shivammathur/setup-php@v2
              with:
                php-version: "${{ matrix.php }}"
                extensions: intl
                tools: flex, symfony
                coverage: none
      ```
   
   2. Node setup
      ```YML
          -
            name: Setup Node
            uses: actions/setup-node@v4
            with:
              node-version: "${{ matrix.node }}"
      ```
   
   3. Shutdown default MySQL
      ```YML
          -
            name: Shutdown default MySQL
            run: sudo service mysql stop
      ```
   
   4. MySQL setup
      ```YML
          -
            name: Setup MySQL
            uses: mirromutth/mysql-action@v1.1
            with:
              mysql version: "${{ matrix.mysql }}"
              mysql root password: "root"
      ```
   
   5. Output PHP version for Symfony CLI
      ```YML
          -
            name: Output PHP version for Symfony CLI
            run: php -v | head -n 1 | awk '{ print $2 }' > .php-version
      ```

   6. Certificates installation
      ```YML
          -
            name: Install certificates
            run: symfony server:ca:install
      ```
   
   7. Running Chrome Headless
      ```YML
         -
            name: Run Chrome Headless
            run: google-chrome-stable --enable-automation --disable-background-networking --no-default-browser-check --no-first-run --disable-popup-blocking --disable-default-apps --allow-insecure-localhost --disable-translate --disable-extensions --no-sandbox --enable-features=Metal --headless --remote-debugging-port=9222 --window-size=2880,1800 --proxy-server='direct://' --proxy-bypass-list='*' http://127.0.0.1 > /dev/null 2>&1 &
      ```
   
   8. Running webserver   
      ```YML
          -
            name: Run webserver
            run: (cd tests/Application && symfony server:start --port=8080 --dir=public --daemon)
      ```

   9. Getting Composer cache directory   
      ```YML
           -
             name: Get Composer cache directory
             id: composer-cache
             run: echo "dir=$(composer config cache-files-dir)" >> $GITHUB_OUTPUT
      ```
   
   10. Caching Composer
       ```YML
           -
             name: Cache Composer
             uses: actions/cache@v4
             with:
               path: ${{ steps.composer-cache.outputs.dir }}
               key: ${{ runner.os }}-php-${{ matrix.php }}-composer-${{ hashFiles('**/composer.json **/composer.lock') }}
               restore-keys: |
                  ${{ runner.os }}-php-${{ matrix.php }}-composer-
       ```
   
   11. Restricting Sylius version
       ```YML
           -
             name: Restrict Sylius version
             if: matrix.sylius != ''
             run: composer require "sylius/sylius:${{ matrix.sylius }}" --no-update --no-scripts --no-interaction
   
       ```
   
   12. Installing PHP dependencies
   
       ```YML
           -
             name: Install PHP dependencies
             run: composer install --no-interaction
             env:
                 SYMFONY_REQUIRE: ${{ matrix.symfony }}
       ```
   
   13. Installing Behat driver
   
       ```YML
           -
            name: Install Behat driver
            run: vendor/bin/bdi browser:google-chrome drivers
       ```
   
   14. Getting Yarn cache directory
       ```YML
           -
             name: Get Yarn cache directory
             id: yarn-cache
             run: echo "dir=$(yarn cache dir)" >> $GITHUB_OUTPUT
       ```
   
   15. Caching Yarn
       ```YML
           -
             name: Cache Yarn
             uses: actions/cache@v4
             with:
               path: ${{ steps.yarn-cache.outputs.dir }}
               key: ${{ runner.os }}-node-${{ matrix.node }}-yarn-${{ hashFiles('**/package.json **/yarn.lock') }}
               restore-keys: |
                 ${{ runner.os }}-node-${{ matrix.node }}-yarn-
       ```
   
   16. Installing JS dependencies
       ```YML
           -
             name: Install JS dependencies
             run: |
               (cd tests/Application && yarn install)
       ```
   
   17. Preparing test application database
   
       ```YML
          -
             name: Prepare test application database
             run: |
               (cd tests/Application && bin/console doctrine:database:create -vvv)
               (cd tests/Application && bin/console doctrine:schema:create -vvv)
       ```
   
   18. Preparing test application assets
   
       ```YML
           -
             name: Prepare test application assets
             run: |
               (cd tests/Application && bin/console assets:install public -vvv)
               (cd tests/Application && yarn build:prod)
       ```
   
   19. Preparing test application cache
   
       ```YML
           -
             name: Prepare test application cache
             run: (cd tests/Application && bin/console cache:warmup -vvv)
       ```
   
   20. Loading fixtures in test application
   
       ```YML
           -
             name: Load fixtures in test application
             run: (cd tests/Application && bin/console sylius:fixtures:load -n)
       ```
   
   21. Validating composer.json
   
       ```YML
           -
             name: Validate composer.json
             run: composer validate --ansi --strict
       ```
   
   22. Validating database schema
   
       ```YML
           -
             name: Validate database schema
             run: (cd tests/Application && bin/console doctrine:schema:validate)
       ```
   
   23. Running PHPSpec
   
       ```YML
           -
             name: Run PHPSpec
             run: vendor/bin/phpspec run --ansi -f progress --no-interaction
       ```
   
   24. Running PHPUnit
   
       ```YML
          -
             name: Run PHPUnit
             run: vendor/bin/phpunit --colors=always
       ```
   
   25. Running Behat
       ```YML
           -
             name: Run Behat
             run: vendor/bin/behat --colors --strict -vvv --no-interaction -f progress  || vendor/bin/behat --colors --strict -vvv --no-interaction -f progress --rerun
       ```
   
   26. Uploading Behat logs
   
       ```YML
           -
             name: Upload Behat logs
             uses: actions/upload-artifact@v3
             if: failure()
             with:
               name: Behat logs
               path: etc/build/
               if-no-files-found: ignore
       ```
   
   27. Sending information regarding failed build to Slack
   
       ```YML
            -
               name: Failed build Slack notification
               uses: rtCamp/action-slack-notify@v2
               if: ${{ failure() && (GithubBuilds.ref == 'refs/heads/main' || GithubBuilds.ref == 'refs/heads/master') }}
               env:
                 SLACK_CHANNEL: ${{ secrets.FAILED_BUILD_SLACK_CHANNEL }}
                 SLACK_COLOR: ${{ job.status }}
                 SLACK_ICON: https://github.com/rtCamp.png?size=48
                 SLACK_MESSAGE: ':x:'
                 SLACK_TITLE: Failed build on ${{ GithubBuilds.event.repository.name }} repository
                 SLACK_USERNAME: ${{ secrets.FAILED_BUILD_SLACK_USERNAME }}
                 SLACK_WEBHOOK: ${{ secrets.FAILED_BUILD_SLACK_WEBHOOK }}
       ```

### The coding_standard.yml file:
0. Use supported actions version
   ```yaml
   - uses: actions/checkout@v3
   ```

1. PHP setup

   ```yaml
   - name: Setup PHP
     uses: shivammathur/setup-php@v2
     with:
       php-version: "${{ matrix.php }}"
       extensions: intl
       tools: flex, symfony
       coverage: none
   ```

2. Getting Composer cache directory

   ```yaml
   - name: Get Composer cache directory
     id: composer-cache
     run: echo "::set-output name=dir::$(composer config cache-files-dir)"
   ```

3. Caching Composer

   ```yaml
   - name: Cache Composer
     uses: actions/cache@v4
     with:
       path: ${{ steps.composer-cache.outputs.dir }}
       key: ${{ runner.os }}-php-${{ matrix.php }}-composer-${{ hashFiles('**/composer.json **/composer.lock') }}
       restore-keys: |
         ${{ runner.os }}-php-${{ matrix.php }}-composer-
   ```

4. Restricting Symfony version

   ```yaml
   - name: Restrict Symfony version
     if: matrix.symfony != ''
     run: |
       composer global config --no-plugins allow-plugins.symfony/flex true
       composer global require --no-progress --no-scripts --no-plugins "symfony/flex:^1.10"
       composer config extra.symfony.require "${{ matrix.symfony }}"
   ```

5. Restricting Sylius version
   ```yaml
   - name: Restrict Sylius version
     if: matrix.sylius != ''
     run: composer require "sylius/sylius:${{ matrix.sylius }}" --no-update --no-scripts --no-interaction
   ```

6. Installing PHP dependencies

   ```yaml
   - name: Install PHP dependencies
     run: composer install --no-interaction
     env:
       SYMFONY_REQUIRE: ${{ matrix.symfony }}
   ```

7. Running PHPStan

   ```yaml
   - name: Run PHPStan src dir
     run: vendor/bin/phpstan analyse -c phpstan.neon -l 8 src/
   ```

8. Running ECS
   ```yaml
   - name: Run ECS
     run: vendor/bin/ecs
   ```
   
9. Sending information regarding failed build to Slack

    ```yaml
    - name: Failed build Slack notification
      uses: rtCamp/action-slack-notify@v2
      if: ${{ failure() && (github.ref == 'refs/heads/main' || github.ref == 'refs/heads/master') }}
      env:
        SLACK_CHANNEL: ${{ secrets.FAILED_BUILD_SLACK_CHANNEL }}
        SLACK_COLOR: ${{ job.status }}
        SLACK_ICON: https://github.com/rtCamp.png?size=48
        SLACK_MESSAGE: ':x:'
        SLACK_TITLE: Failed build on ${{ github.event.repository.name }} repository
        SLACK_USERNAME: ${{ secrets.FAILED_BUILD_SLACK_USERNAME }}
        SLACK_WEBHOOK: ${{ secrets.FAILED_BUILD_SLACK_WEBHOOK }}
    ```


### [Previous chapter](./3_JobsAndStrategySubchapter.md) / [Main page](../../README.md) / [Next chapter](./5_ExampleBuildsSubchapter.md)
