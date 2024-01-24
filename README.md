# BitBag Coding Bible

![BitBag Bible](doc/image.png "BitBag Bible")

*This project follows [PSR coding standards](https://www.php-fig.org/psr/) and those recommended by Sylius and Symfony projects in this order.
It is extended based on the experience of the whole BitBag team for everybody's sake.*

- [Code Style](#code-style)
- [General](#general)
- [Static analysis tools](#static-analysis-tools)
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
parameters are too long to fit PSR line limit, split them as well.

Examples:

```php

// Good patterns:

public function bar(FirstParamInterface $firstParam): void;

public function bar(
    FirstParamInterface $firstParam,
): void;

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

12. For any point not included in the current section and the PSR rules please consider the https://mnapoli.fr/approaching-coding-style-rationally/ tips (as very valuable propositions).

13. If you consider any changes in your project, please discuss it with your team and if decided to do so, please do this in project-level.

## General

0. No `/.idea` and other local config files in `.gitignore`. Put them into a global gitignore file,
    read more on https://help.github.com/articles/ignoring-files/#create-a-global-gitignore.

1. All side-effect files (or directories) from project dependencies should be put into project `.gitignore` file.

2. For project development we require *NIX system kernel (for working with Git, servers, maintaining Symfony application etc.). We require from you working on Windows (WSL only) / MacOS / Ubuntu.

3. Code that is not documented doesn't exist. Writing documentation of a bundle/plugin/project is part of the
   development process. Remember that in the end, someone else is going to use your code who might not know each part of it.
   This also applies to writing GitHub repository descriptions, basic composer package information, etc.
   
   Specially write/update information of:
      - Information of needed tools, including their versions.
      - Package installation process. Specially please follow your instructions from the beginning to end, to be sure the installation process is completed.
      - How to run the application / tests (set of commands/steps needed to do it).
      - If you prepare new major version of a package, please write/update UPGRADE.md file to describe the breaking changes. Please note, not every code-breaking change needs a new major version of application.
  
## Static analysis tools

1. Always use [BitBag Coding Standard library](https://github.com/BitBagCommerce/coding-standard) for code cleanup.
    Also use PHPStan on level 8 wherever it's possible. It's included in [BitBag Coding Standard library](https://github.com/BitBagCommerce/coding-standard).
    Both ECS and PHPStan should be included in the CI process.

## Symfony / Sylius / Frameworks

0. We recommend using YAML (`*.yaml`) for defining routings and configs.
1. We recommend using XML (`*.xml`) for defining services, doctrine mappings, and validation definitions.
2. If you prefer doing 0. and 1. it another way, please do it consistent in entire project.
3. For services definitions in a single bundle use `form.xml`, `event_listener.xml`, etc. Don't put everything
   in the `services.xml` file, do it in public projects with only a few services. If you have more than one type of service
   Inside your app, create a separate config file under the `services/` directory.
4. Any information regarding external system (like DSN, service path) have to been placed in `.env` file as placeholders. If you consider putting there any credentials, please put only empty placeholders there.
5. Please use `.env.local` (which is not commited to the repository) file for all sensitive and environment-specific data.
6. Use finals everywhere you can (specially in non-bundle projects).
7. Repositories and entities in public projects should not (and cannot) be defined as `final`.
8. Interface-driven development is not welcome, but if you consider extending your class or overriding your service, please use traits and interfaces.
9. Entity fields in public projects (vendors) should be `protected` instead of `private`.
10. Decorate resource factories with decoration pattern and do not call resource instance with `new` keyword directly.
   Instead, inject resource factory into the constructor and call `createNew()` on it.
   See `Sylius\Component\Product\Factory\ProductFactory`, `sylius.custom_factory.product` service definition
   and [Symfony Service Decoration](https://symfony.com/doc/current/service_container/service_decoration.html). The `priority` flag we are starting with equals 1 and is increased by one for each other decoration.
11. Don't include the entire service container into your service by default, if you don't have to. Instead of that use Symfony Dependency Injection.
12. For customizing forms use [Symfony Form Extension](https://symfony.com/doc/current/form/create_form_type_extension.html).
13. We don't use either autowire nor autoconfigure Symfony options as it is a very "magic" way of defining services. We always prefer to manually define services and inject proper arguments into them to have better control of our Container. # To be discussed
14. Do not define services as public, if it's not necessary.
15. Don't use Sylius theme if you have one template in your project.

## Testing

0. Always write tests for your application.
1. When project starts, please choose a testing toolset and continue using it during project for the consistency.
2. Before starting implementing new functional code, make sure all your core logic is covered with unit tests (PHPUnit / PHPSpec).
3. We use unit tests only for code related to bussiness logic, so please do not write them for controllers, repositories, fixture generators, getters, setters etc. 
4. For integration tests we use PHPUnit with [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
   The integration tests means testing code with integration to external services, like database, redis, file system. So please write
   them for repositories, cache drivers, file uploaders etc.
5. For functional API tests we use PHPUnit with [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
   The functional API tests means running application API endpoints, comparing their responses with additional database checks if needed.
6. Please write your fixtures using [Nelmio Alice](https://github.com/nelmio/alice) package, which is included in [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
7. For functional and end to end GUI tests please use Behat.
8. After you implement any new functional feature, write Behat scenario (Gherkin, `*.feature` file).
9. When you're fixing a bug, it's recommended to write behat scenario first (TDD) to prove the fix works correctly.

## OOP / Architecture

0. Make your code as simple as it's possible (follow S.O.L.I.D. and KISS principles).
1. Please use the Demeter Law to not to code trains as below:

```php
$product->getVariants()->first()->getNextThing()->getNextThing()->ohNoImInTheTrain();
```

2. Use interfaces for any core logic class implementation, especially when you create a plugin or bundle.
3. Use `final` any time it is possible (in order to avoid infinite inheritance chain, in order to customize some parts use Decorator and Dependency Injection patterns).
	- The only exception to this rule is only for a framework/library specific requirements. I.e Doctrine Entities cannot be final classes because of proxy classes existence.
4. Use ADR pattern for controllers. For instance, your controller should not extend any class and contain just an `__invoke` method. It should also be suffixed with `Action` keyword.

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

-1. Not confident with Git yet? Visit [the simple guide](https://rogerdudler.github.io/git-guide/)!

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



## Developing Sylius plugins

0. Use traits for customizing models and/or repositories and use them inside your `tests/Application/src` for testing. This way we avoid handling reference conflicts in the final app.
1. Entity fields in public projects (vendors) have to be `protected` instead of `private` to make them possible of being extended in final app.
2. Please use interfaces for every service you can. Also please prefer using interfaces in service constructors for better plugin extendability. It will make possible service overwriting and decoration.
3. Please follow [Symfony configuration structure rules](https://symfony.com/doc/current/bundles/best_practices.html). For projects with previous approach it's always welcome to do the refactor.
4. When using Sylius Resource based classes (controllers, entities, interfaces, factories, repositories etc.), instead of putting the hardcoded FCQN in configuration (i.e. grids) please use the auto-generated parameters.
5. Prefer using template events than overridding Sylius / Symfony templates in plugin.
6. When supporting multiple Sylius / Symfony versions you probably would to load different configurations for them. To do so, you can follow [our CMS Kernel logic](https://github.com/BitBagCommerce/SyliusCmsPlugin/blob/master/tests/Application/Kernel.php).
7. We use GitHub Actions for CI. When developing new plugin, please prepare the build file. As an example you can follow our [OpenMarketplace build.yml](https://github.com/BitBagCommerce/OpenMarketplace/blob/master/.github/workflows/build.yml) file.
8. README.md file is a must have. It has to contain at least description of plugin features and installation process. And please remember to test the installation process (following all the README commands) before publishing any changes.

**Be smart and keep in mind that once you do something stupid, I will find you and I will force you to work with Laravel or Magento.**
**There is nothing called a stupid question, but please ask it in a smart way :).**
**It's better to talk about a feature with the whole team for 30 minutes than lose 8 hours on implementing some dummy code that will destroy the current codebase order and deep the technical debt.**
