### Build.yml:
1. Setup PHP
   <details>
      <summary>Click for code</summary>

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
   </details>

2. Setup Node
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Setup Node
            uses: actions/setup-node@v4
            with:
              node-version: "${{ matrix.node }}"
      ```
   </details>

3. Shutdown default MySQL
    <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Shutdown default MySQL
            run: sudo service mysql stop
      ```
   </details>

4. Setup MySQL
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Setup MySQL
            uses: mirromutth/mysql-action@v1.1
            with:
              mysql version: "${{ matrix.mysql }}"
              mysql root password: "root"
      ```
   </details>

5. Output PHP version for Symfony CLI
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Output PHP version for Symfony CLI
            run: php -v | head -n 1 | awk '{ print $2 }' > .php-version
      ```
   </details>
6. Install certificates
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Install certificates
            run: symfony server:ca:install
      ```
   </details>

7. Run Chrome Headless
   <details>
      <summary>Click for code</summary>

      ```YML
         -
            name: Run Chrome Headless
            run: google-chrome-stable --enable-automation --disable-background-networking --no-default-browser-check --no-first-run --disable-popup-blocking --disable-default-apps --allow-insecure-localhost --disable-translate --disable-extensions --no-sandbox --enable-features=Metal --headless --remote-debugging-port=9222 --window-size=2880,1800 --proxy-server='direct://' --proxy-bypass-list='*' http://127.0.0.1 > /dev/null 2>&1 &
      ```
   </details>

8. Run webserver
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Run webserver
            run: (cd tests/Application && symfony server:start --port=8080 --dir=public --daemon)
      ```
   </details>
9. Get Composer cache directory
   <details>
      <summary>Click for code</summary>

      ```YML
           -
             name: Get Composer cache directory
             id: composer-cache
             run: echo "dir=$(composer config cache-files-dir)" >> $GITHUB_OUTPUT
      ```
   </details>

10. Cache Composer
   <details>
      <summary>Click for code</summary>

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
    </details>

11. Restrict Sylius version
   <details>
      <summary>Click for code</summary>

   ```YML
       -
         name: Restrict Sylius version
         if: matrix.sylius != ''
         run: composer require "sylius/sylius:${{ matrix.sylius }}" --no-update --no-scripts --no-interaction

   ```
    </details>

12. Install PHP dependencies
   <details>
      <summary>Click for code</summary>

   ```YML
       -
         name: Install PHP dependencies
         run: composer install --no-interaction
         env:
             SYMFONY_REQUIRE: ${{ matrix.symfony }}
   ```
    </details>

13. Install Behat driver
   <details>
      <summary>Click for code</summary>

   ```YML
       -
        name: Install Behat driver
        run: vendor/bin/bdi browser:google-chrome drivers
   ```
    </details>

14. Get Yarn cache directory
   <details>
      <summary>Click for code</summary>

   ```YML
       -
         name: Get Yarn cache directory
         id: yarn-cache
         run: echo "dir=$(yarn cache dir)" >> $GITHUB_OUTPUT
   ```
    </details>

15. Cache Yarn
   <details>
       <summary>Click for code</summary>

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
    </details>  

16. Install JS dependencies
   <details>
       <summary>Click for code</summary>

   ```YML
       -
         name: Install JS dependencies
         run: |
           (cd tests/Application && yarn install)
   ```
    </details>  

17. Prepare test application database
   <details>
       <summary>Click for code</summary>

   ```YML
      -
         name: Prepare test application database
         run: |
           (cd tests/Application && bin/console doctrine:database:create -vvv)
           (cd tests/Application && bin/console doctrine:migrations:migrate -n -vvv -q)
   ```
    </details>  

18. Prepare test application assets
   <details>
       <summary>Click for code</summary>

   ```YML
       -
         name: Prepare test application assets
         run: |
           (cd tests/Application && bin/console assets:install public -vvv)
           (cd tests/Application && yarn build:prod)
   ```
    </details>  

19. Prepare test application cache
   <details>
       <summary>Click for code</summary>

   ```YML
       -
         name: Prepare test application cache
         run: (cd tests/Application && bin/console cache:warmup -vvv)
   ```
    </details>  

20. Load fixtures in test application
   <details>
       <summary>Click for code</summary>

   ```YML
       -
         name: Load fixtures in test application
         run: (cd tests/Application && bin/console sylius:fixtures:load -n)
   ```
    </details>  

21. Validate composer.json
   <details>
       <summary>Click for code</summary>

   ```YML
       -
         name: Validate composer.json
         run: composer validate --ansi --strict
   ```
    </details>  

22. Validate database schema
   <details>
       <summary>Click for code</summary>

   ```YML
       -
         name: Validate database schema
         run: (cd tests/Application && bin/console doctrine:schema:validate)
   ```
    </details>  

23. Run PHPSpec
   <details>
       <summary>Click for code</summary>

   ```YML
       -
         name: Run PHPSpec
         run: vendor/bin/phpspec run --ansi -f progress --no-interaction
   ```
    </details>  

24. Run PHPUnit
   <details>
       <summary>Click for code</summary>

   ```YML
      -
         name: Run PHPUnit
         run: vendor/bin/phpunit --colors=always
   ```
    </details>  

25. Run Behat
   <details>
       <summary>Click for code</summary>

   ```YML
       -
         name: Run Behat
         run: vendor/bin/behat --colors --strict -vvv --no-interaction -f progress  || vendor/bin/behat --colors --strict -vvv --no-interaction -f progress --rerun
   ```
    </details>

26. Upload Behat logs
   <details>
       <summary>Click for code</summary>

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
    </details> 

27. Failed build Slack notification
   <details>
       <summary>Click for code</summary>

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
    </details>  

### coding_standard.yml:
1. Setup PHP
   <details>
      <summary>Click for code</summary>

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
   </details>

2. Setup Node
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Setup Node
            uses: actions/setup-node@v4
            with:
              node-version: "${{ matrix.node }}"
      ```
   </details>

3. Shutdown default MySQL
    <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Shutdown default MySQL
            run: sudo service mysql stop
      ```
   </details>

4. Setup MySQL
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Setup MySQL
            uses: mirromutth/mysql-action@v1.1
            with:
              mysql version: "${{ matrix.mysql }}"
              mysql root password: "root"
      ```
   </details>

5. Output PHP version for Symfony CLI
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Output PHP version for Symfony CLI
            run: php -v | head -n 1 | awk '{ print $2 }' > .php-version
      ```
   </details>
6. Install certificates
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Install certificates
            run: symfony server:ca:install
      ```
   </details>

7. Run Chrome Headless
   <details>
      <summary>Click for code</summary>

      ```YML
         -
            name: Run Chrome Headless
            run: google-chrome-stable --enable-automation --disable-background-networking --no-default-browser-check --no-first-run --disable-popup-blocking --disable-default-apps --allow-insecure-localhost --disable-translate --disable-extensions --no-sandbox --enable-features=Metal --headless --remote-debugging-port=9222 --window-size=2880,1800 --proxy-server='direct://' --proxy-bypass-list='*' http://127.0.0.1 > /dev/null 2>&1 &
      ```
   </details>

8. Run webserver
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Run webserver
            run: (cd tests/Application && symfony server:start --port=8080 --dir=public --daemon)
      ```
   </details>
9. Get Composer cache directory
   <details>
      <summary>Click for code</summary>

      ```YML
           -
             name: Get Composer cache directory
             id: composer-cache
             run: echo "dir=$(composer config cache-files-dir)" >> $GITHUB_OUTPUT
      ```
   </details>

10. Cache Composer
   <details>
      <summary>Click for code</summary>

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
    </details>

11. Restrict Sylius version
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Restrict Sylius version
            if: matrix.sylius != ''
            run: composer require "sylius/sylius:${{ matrix.sylius }}" --no-update --no-scripts --no-interaction
      
      ```
    </details>

12. Install PHP dependencies
   <details>
      <summary>Click for code</summary>

   ```YML
       -
         name: Install PHP dependencies
         run: composer install --no-interaction
         env:
             SYMFONY_REQUIRE: ${{ matrix.symfony }}
   ```
    </details>

13. Install Behat driver
   <details>
      <summary>Click for code</summary>
   
      ```YML
          -
           name: Install Behat driver
           run: vendor/bin/bdi browser:google-chrome drivers
      ```
    </details>

14. Get Yarn cache directory
   <details>
      <summary>Click for code</summary>

      ```YML
          -
            name: Get Yarn cache directory
            id: yarn-cache
            run: echo "dir=$(yarn cache dir)" >> $GITHUB_OUTPUT
      ```
    </details>

### [Previous chapter](./GithubBuilds/3_JobsAndStrategySubchapter.md) / [Main page](./GithubBuilds.md) / [Next chapter](./GithubBuilds/5_ExampleBuildsSubchapter.md)