# Coding bible

*This project follows [PSR coding standards](https://www.php-fig.org/psr/) and those recommended by Sylius and Symfony projects in this order.
It is extended based on the experience of the whole BitBag team for everybody's sake.*

- [Code Style](#code-style)
- [General](#general)
- [Symfony / Sylius / Frameworks](#symfony--sylius--frameworks)
- [Testing](#testing)
- [OOP / Architecture](#oop--architecture)
- [Workflow](#workflow)
- [Open Source](#open-source)
- [Git](#git)


## Code Style

0. Starting a new project, always use the newest stable PHP, Symfony, Sylius, Shopware version.
1. Always follow [PSR-12](https://www.php-fig.org/psr/psr-12/) recommendations.
2. Always prefer newer syntax rules. In example:
   - Using `$elements = [1, 2, 3];` instead of `$elements = array(1, 2, 3);`
   - Using property / function parameter types, wherever possible
   - Using a new constructor syntax known from PHP 8.0
3. Always use a trailing comma in arrays and method parameters (if PHP version allows to do that).
4. Use Yoda-Style comparisons `null === $var->getResult($anotherVar)` instead of `$var->getResult($anotherVar) === null`.
5. Use class constants / enums for static (hardcoded) values.
6. Don't use annotations for framework configuration. If you consider using attributes (i.e. because of framework requirements), please do this in project-scope.
7. Don't use PHPDoc for things, that can be determined from the code level. Use it only when it is REALLY valuable and in interfaces
   that return array of objects or collections, like:

```php
interface Foo
{
    /** @return Collection|ItemInterface[] */
    public function getItems(): Collection;
}
```

8. Always use strict types declaration in each class header, like:

```php
<?php

declare(strict_types=1);

namespace Foo\Bar;

final class Foo
{
}
```

9. A method must not have more than one parameter inline. Otherwise, split them with `\n`. In an edge-case where two
parameters are too long to fit PSR line limit, split them as well. # TODO New PHP constructor topic to be discussed

Examples:

```php
public function foo(string $firstParam, string $secondParam): void;

public function bar(
    FirstParamInterface $firstParam, 
    SecondParamInterface $secondParam,
    ThirdParamInterface $thirdParam,
): void;

public function fooBarIsALongMethodName(
    WithEvenALongerParameter $firstParam,
    AndASecondParameterThatIsNotShorter $secondParameter,
): void;
```

10. Once you use PHPStorm (and yes, you do if you work at BitBag), you can open your IDE preferences (`PHPStorm -> Preferences`)
    and search for `File and Code Templates`. PHP Class Doc Comment, PHP File Header, PHP Interface Doc Comment are those
    templates that should at least be customized.

11. Always use [BitBag Coding Standard library](https://github.com/BitBagCommerce/coding-standard) for code cleanup.
    Also use PHPStan on level 8 wherever it's possible. It's included in [BitBag Coding Standard library](https://github.com/BitBagCommerce/coding-standard).
    Both ECS and PHPStan should be included in the CI process.

12. For any point not included in the current section please follow the https://mnapoli.fr/approaching-coding-style-rationally/ tips.

## General

0. No `/.idea` and other local config files in `.gitignore`. Put them into a global gitignore file,
    read more on https://help.github.com/articles/ignoring-files/#create-a-global-gitignore.

1. We are working on *NIX systems. We don't like Windows nor are we solving its existence goal and other problems related
   to code and application.

2. Code that is not documented doesn't exist. Writing documentation of a bundle/plugin/project is part of the
   development process. Remember that in the end, someone else is going to use your code who might not know each part of it.
   This also applies to writing GitHub repository descriptions, basic composer package information, etc.
   
   Specially write/update information of:
      - Package installation process
      - How to run the application (set of commands/steps needed to do it)
      - Information of needed tools, including their versions

## Symfony / Sylius / Frameworks

0. Use YAML (`*.yaml`) for defining routings and configs.
1. Use XML (`*.xml`) for defining services, doctrine mappings, and validation definitions.
2. For services definitions in a single bundle use `form.xml`, `event_listener.xml`, etc. Don't put everything.
   in the `services.xml` file, do it in public projects with only a few services. If you have more than one type of service.
   inside your app, create a separate config file under the `services/` directory.
3. Any information regarding external system (like DSN, service path, credentials) have to been placed in `.env` file as placeholders.
4. Please use `.env.local` file for all sensitive and environment-specific data.
5. Repositories in public projects should not (and cannot) be defined as `final`. 
6. Entity fields in public projects (vendors) should be `protected` instead of `private`.
7. Decorate resource factories with decoration pattern and do not call resource instance with `new` keyword directly.
   Instead, inject resource factory into the constructor and call `createNew()` on it.
   See `Sylius\Component\Product\Factory\ProductFactory`, `sylius.custom_factory.product` service definition
   and [Symfony Service Decoration](https://symfony.com/doc/current/service_container/service_decoration.html). The `priority` flag we are starting with equals 1 and is increased by one for each other decoration.
8. Don't include the entire service container into your service, if you don't have to. Instead of that use Symfony Dependency Injection.
9. For customizing forms use [Symfony Form Extension](https://symfony.com/doc/current/form/create_form_type_extension.html).
10. We follow command pattern. This means we use `Command` / `CommandHandler` / message bus approach. Consider using [Symfony Messenger](https://symfony.com/doc/current/messenger.html) for that. 
11. Creating a CLI Command using Symfony Console Component should follow the following rules:
    - `execute` method should have `int` as a return type. For the **successful** run, the command should return `0`. For any errors during execution, the return can be `1` or any different *error code number*.
12. In Sylius plugins, use traits for customizing models and use them inside your `tests/Application/src` for testing. This way we avoid handling reference conflicts in the final app.
13. We don't use either autowire nor autoconfigure Symfony options as it is a very "magic" way of defining services. We always prefer to manually define services and inject proper arguments into them to have better control of our Container.
14. Do not define services as public, if it's not necessary.
15. If some of the service definition is tagged, don't use FQCN (Fully Qualified Class Name) as the service id.
16. Don't use Sylius theme if you have one template in your project.

## Testing

0. Before starting implementing new functional code, make sure all your core logic is covered with PHPSpec.
1. We use PHPSpecs only for code related to bussiness logic, so please do not write them for controllers, repositories, fixture generators etc. 
2. PHPSpecs are always final classes with `function`s without `public` visibility and `: void` return type:

```php
final class ProductSpec extends ObjectBehavior
{
    function it_follows_bitbag_coding_standards(): void
    {
        Assert::true($this->followsStandards());
    }
}
```

3. For integration tests we use PHPUnit with [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
    The integration tests means testing code with integration to external services, like database, redis, file system. So please write
    them for repositories, cache drivers, file uploaders etc.
4. For functional API tests we use PHPUnit with [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
   The functional API tests means running application API endpoints, comparing their responses with additional database checks.
5. Please write your fixtures using [Nelmio Alice](https://github.com/nelmio/alice) package, which is included in [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
6. Before you implement any new functional feature, write Behat scenario first (Gherkin, `*.feature` file).
7. After writing the scenario, write a proper scenario execution (Contexts, Pages).
8. Use Behat Contexts that are divided into `Hooks` - generic app Background, `Setup` specific resource background, `Ui` - specific interaction.

## OOP / Architecture

0. Make your code as simple as it's possible (follow single responsibility principle and KISS principle).
1. Please use the Demeter Law to not to code trains as below:

```php
$product->getVariants()->first()->getNextThing()->getNextThing()->ohNoImInTheTrain();
```

2. Use interfaces for any core logic class implementation, especially Models and Services (so that you follow a single responsibility principle).
3. Use `final` any time it is possible (in order to avoid infinite inheritance chain, in order to customize some parts use Decorator and Dependency Injection patterns).
	- The only exception to this rule is only for a framework/library specific requirements. I.e Doctrine Entities cannot be final classes because of proxy classes existence.
4. Be more careful when you think Singleton is something you need in the project. If it is you should go and rethink the code.
5. Be careful with `static` statement, probably you will never need to use it.
6. Use ADR pattern for controllers. For instance, your controller should not extend any class and contain just an `__invoke` method. It should also be suffixed with `Action` keyword.

```php
<?php

declare(strict_types=1);

namespace App\Controller\Action;

use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use App\Repository\FooRepositoryInterface;

final class SayHelloToTheWorldAction
{
    /** @var FooRepositoryInterface */
    private $fooRepository;

    public function __construct(FooRepositoryInterface $fooRepository)
    {
        $this->fooRepository = $fooRepository;
    }

    public function __invoke(Request $request): Response
    {
        return new Response("Hello world!");
    }
}
```

## Workflow and Git

-1. Not confident with Git yet? Visit [the simple guide](http://rogerdudler.github.io/git-guide/)!
0. If you use Git in IDE - make sure it follows all standards. We don't care what GUI/CLI you use as long as you know what happens under the hood.
1. Commit messages should be written (if only it's possible) with the following convention:
   `TASK-123 - Max 64 characters description in english written in Present Simple.`. If you don't work with any ticket system,
   please skip the `TASK-123 - ` part. How to write a commit message and use Git in a proper way? Read [here](https://github.com/RomuloOliveira/commit-messages-guide).
2. We use Gitflow. So you have to:
   - Use correct branch prefixes: `hotfix/`, `feature/`, `refactoring/` etc,
   - Follow the Gitflow branching rules described in [the image](doc/gitflow.png),
   - If you use any ticket system, use the ticket name in the branch, after prefix. Jira can match branches and tasks together. 
   
   Any kind of change of the flow has to be discussed to the team leader. 


## Open Source

0. Open source is made by forks if only more than one person is in charge of maintenance of specific package.
1. We follow http://docs.sylius.org/en/latest/contributing/ contribution standards.
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
     public const SOME_CONST = 'foo';
     public const SOME_OTHER_CONST = 'bar';
     
     /** @var string */
     private $someProperty;
    
     public function inheritedMethod(): string
     {
         //some body
     }
     
     private function someFunction(SomeServiceInterface $someService): ?NullOrInterfacedObject
     {
         $items = $someService->getCollection();
         
         /** @var WeUseThisBlockDefinitionIfNecessaryOfCourseWithInterface $someOtherProperty */
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

**Be smart and keep in mind that once you do something stupid, I will find you and I will force you to work with Laravel or Magento.**
**There is nothing called a stupid question, but please ask it in a smart way :).**
**It's better to talk about a feature with the whole team for 30 minutes than lose 8 hours on implementing some dummy code that will destroy the current codebase order and deep the technical debt.**


