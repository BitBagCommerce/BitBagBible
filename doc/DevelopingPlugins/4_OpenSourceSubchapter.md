## Open Source

0. If your're external contributor, you should use forks and pull-requests for the contribution.
1. A good example of open source contribution rules you can find here: https://docs.sylius.com/en/latest/book/contributing/index.html.
2. Any `*.php` file created by the BitBag developer (in Open Source project) needs to have at least the following definition where the author is the user who created this file:

    ```php
    <?php
    
    /*
     * This file has been created by developers from BitBag.
     * Feel free to contact us once you face any issues or want to start
     * You can find more information about us on https://bitbag.io and write us
     * an email on hello@bitbag.io.
     */
    
     declare(strict_types=1);
     
     namespace Foo;
     
     use Foo\Bar\App;
     
     final class Bar implements BarInterface
     {
         // ...
     }
    ```

### [Previous chapter](./3_WorkflowSubchapter.md) / [Main page](../../README.md) )