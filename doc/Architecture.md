# OOP / Architecture

1. Make your code as simple as it's possible (follow S.O.L.I.D. and KISS principles).
2. Please use the Demeter Law to not to code trains as below:

    ```php
    $product->getVariants()->first()->getNextThing()->getNextThing()->ohNoImInTheTrain();
    ```

3. Use interfaces for any core logic class implementation, especially when you create a plugin or bundle.
4. Use `final` any time it is possible (in order to avoid infinite inheritance chain, in order to customize some parts use Decorator and Dependency Injection patterns).
   - The only exception to this rule is only for a framework/library specific requirements. I.e Doctrine Entities cannot be final classes because of proxy classes existence.
5. Use ADR pattern for controllers. For instance, your controller should not extend any class and contain just an `__invoke` method. It should also be suffixed with `Action` keyword.

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

# Subchapters
### [1. Frameworks](./Architecture/1_FrameworksSubchapter.md)
### [2. Tools](./Architecture/2_ToolsSubchapter.md)

