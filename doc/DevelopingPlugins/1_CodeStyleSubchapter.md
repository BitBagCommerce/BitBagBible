## Code Style

1. Starting a new project, always use the newest stable PHP, Symfony, Sylius, Shopware version.
2. Always follow [PSR-12](https://www.php-fig.org/psr/psr-12/) recommendations.
3. Always prefer newer syntax rules. In example:
    - Using `$elements = [1, 2, 3];` instead of `$elements = array(1, 2, 3);`
    - Using property / function parameter types, wherever possible
    - Using a new constructor syntax known from PHP 8.0
4. Always use a trailing comma in arrays and method parameters (if PHP version allows to do that).
5. Use Yoda-Style comparisons `null === $var->getResult($anotherVar)` instead of `$var->getResult($anotherVar) === null`.
6. Use class constants / enums for static (hardcoded) values.
7. Don't use annotations for framework configuration. If you consider using attributes (i.e. because of framework requirements), please do this in project-scope.
8. Don't use PHPDoc for things, that can be determined from the code level. Use it only when it is REALLY valuable and in interfaces
   that return array of objects or collections, like:

    ```php
    interface Foo
    {
        /** @return Collection|ItemInterface[] */
        public function getItems(): Collection;
    }
    ```

9. Always use strict types declaration in each class header, like:
    
    ```php
    <?php
    
    declare(strict_types=1);
    
    namespace Foo\Bar;
    
    final class Foo
    {
    }
    ```

10. A method must not have more than one parameter inline. Otherwise, split them with `\n`. In an edge-case where two
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

11. Once you use PHPStorm (and yes, you do if you work at BitBag), you can open your IDE preferences (`PHPStorm -> Preferences`)
    and search for `File and Code Templates`. PHP Class Doc Comment, PHP File Header, PHP Interface Doc Comment are those
    templates that should at least be customized.

12. For any point not included in the current section and the PSR rules please consider the https://mnapoli.fr/approaching-coding-style-rationally/ tips (as very valuable propositions).

13. If you consider any changes in your project, please discuss it with your team and if decided to do so, please do this in project-level.

### [Previous chapter](../DevelopingPlugins.md) / [Main page](../../README.md) / [Next chapter](./2_TestingSubchapter.md)