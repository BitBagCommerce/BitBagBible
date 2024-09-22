## Testing

1. Always write tests for your application.
2. When project starts, please choose a testing toolset and continue using it during project for the consistency.
3. Before starting implementing new functional code, make sure all your core logic is covered with unit tests (PHPUnit / PHPSpec).
4. We use unit tests only for code related to bussiness logic, so please do not write them for controllers, repositories, fixture generators, getters, setters etc.
5. For integration tests we use PHPUnit with [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
   The integration tests means testing code with integration to external services, like database, redis, file system. So please write
   them for repositories, cache drivers, file uploaders etc.
6. For functional API tests we use PHPUnit with [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
   The functional API tests means running application API endpoints, comparing their responses with additional database checks if needed.
7. Please write your fixtures using [Nelmio Alice](https://github.com/nelmio/alice) package, which is included in [lchrusciel/api-test-case](https://github.com/lchrusciel/ApiTestCase) library.
8. For functional and end to end GUI tests please use Behat.
9. After you implement any new functional feature, write Behat scenario (Gherkin, `*.feature` file).
10. When you're fixing a bug, it's recommended to write behat scenario first (TDD) to prove the fix works correctly.

### [Previous chapter](./1_CodeStyleSubchapter.md) / [Main page](../../README.md) / [Next chapter](./3_WorkflowSubchapter.md)