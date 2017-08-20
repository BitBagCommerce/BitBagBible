Coding standards
-------------

This project follows PSR-4 coding standards and those recommended by Sylius and Symfony projects in this order.

It also has some basic rules described below:

1. Use `*.yml` for defining services, doctrine, routing and validation definitions.
2. Don't use annotations because and don't mess up the definition with implementation.
3. Use YAML over XML and keep one definition format for the whole project.
4. Use doc block in any `*.php` file
5. Use `{@inheritdoc}` whenever it's possible, don't repeat yourself.
6. Keep a blank line above the `@return` method definition in case it has more than `@return` annotation.
7. Whenever you create a merge request to the master run all tests first.
8. Before you implement any new feature, write Behat scenario first.
9. After writing the scenario, write a proper scenario execution.
10. Before starting implementing new functional code, make sure all your core logic is covered with PHPSpec.
11. Make your code as simple as it's possible.
12. Use interfaces for any core logic class implementation, especially Models and Services.
13. Use `final` any time it is possible.
14. For Symfony services definitions in a single bundle use `form.yml`, `event_listener.yml`, etc. Don't put everything in the `services.yml` file, do it in public projects with only a few services.
15. Any `*.php` file created by the BitBag developer needs to have at least the following definition where the author is the user who created this file:

```php
<?php

/**
 * This file was created by the developers from BitBag.
 * Feel free to contact us once you face any issues or want to start
 * another great project.
 * You can find more information about us on https://bitbag.shop and write us
 * an email on kontakt@bitbag.pl.
 */
 
 namespace Foo;
 
 use Foo\Bar\App;
 
 /**
  * @author Mikołaj Król <mikolaj.krol@bitbag.pl>
  * @author [Another author] <who.messed.up.with.this.file@bitbag.pl>
  */
 final class Bar implements BarInterface
 {
     const SOME_CONST = 'foo';
     const SOME_OTHER_CONST = 'bar';
     
     private $someProperty;
     
     /**
      * {@inheritdoc}
      */
     public function inheritedMethod()
     {
         //some body
     }
     
     /** 
       * @param SomeServiceInterface $someService 
       *
       * @return null|SomeClassInterface 
       */
     private function someFunction(SomeServiceInterface $someService)
     {
         $items = $someService->getCollection();
         /** @var WeUseThisBlockDefinitionIfNecessary $someOtherProperty */
         $someOtherProperty = $someService->getSomething();
         // Use break line between any operation in your code. Imagine the code as block diagram, where every new line is an arrow between operations.
         foreach ($items as $item) {
             $item->doSomething();
             
             if (false === $item->getProperty()) { // Always use strict comparison with expected result on the left
                 
                 return;
             }
             
             continue;
         }
         
         $someService->someOutputAction();
         $this->someProperty->someOtherOutputAction();
         
         return $someOtherProperty;
     }
 }
```

Once you use PHPStorm (and yes, you do if you work at BitBag), 
you can open your IDE preferences (`PHPStorm -> Preferences`) and search for `File and Code Templates`. 
PHP Class Doc Comment, PHP File Header, PHP Interface Doc Comment 
are those templates that should at least be customized.

16. Use Behat Contexts are divided into `Hooks` - generic app Background, `Setup` specific resource background, `Ui` - specific interaction
17. Commit messages should be written (if only it's possible) with the following convention:
`[Project spec state][Bundle] max 64 characters description in english written in Present Simple.`
18. If there is an opened issue on Jira for a specific task, your branch should be named `sit_[ISSUE_NUMBER]`. If not, it should be named with the first letter of your name and your surname. In my case (Mikołaj Król) it would be `mkrol`.
19. Be careful with using static fields.
20. Be more careful when you think Singleton is something you need in project.
21. Laravel is not that bad.
22. Just kidding. Fuck that shit.
23. Open source is made by forks.
24. We follow http://docs.sylius.org/en/latest/contributing/ contribution standards
25. `null === $var->getResult($anotherVar)` instead of `$var->getResult($anotherVar) === null`
26. If that's not obvious yet, be careful with `static`, probably you will never need to use it.
27. No `/.idea` and other local config files in `.gitignore`. Put them into global gitignore file, read more on https://help.github.com/articles/ignoring-files/#create-a-global-gitignore.
28. We are working on NIX systems and we don't like Windows nor are we solving it's existance goal and other problems.
29. Repositories and Entities in public projects should not be defined as `final`. 
30. Entity fields in public projects should be `protected` instad of `private`
31. Decorate resource factories with decoration pattern and do not call resource instance with `new` keyword directly. Instead, inject resource factory into constructor and call `createNew()` on it. See `Sylius\Component\Product\Factory\ProductFactory`, `sylius.custom_factory.product` service definition and https://symfony.com/doc/current/service_container/service_decoration.html. The `priority` flag we are starting with equals 1 and is increased by one for each other decoration.

**Be smart and keep in mind that once you do something stupid, I will find you and I will force you to work with Laravel or Magento.**
**There is nothing called stupid question, but before you ask one, think twice.**
**It's better to talk about a feature with the whole team for 30 minutes than lose 8 hours on implementing some dummy code.**
