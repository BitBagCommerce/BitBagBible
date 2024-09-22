# Development

1. Use traits for customizing models and/or repositories and use them inside your `tests/Application/src` for testing. This way we avoid handling reference conflicts in the final app.
2. Entity fields in public projects (vendors) have to be `protected` instead of `private` to make them possible of being extended in final app.
3. Please use interfaces for every service you can. Also please prefer using interfaces in service constructors for better plugin extendability. It will make possible service overwriting and decoration.
4. Please follow [Symfony configuration structure rules](https://symfony.com/doc/current/bundles/best_practices.html). For projects with previous approach it's always welcome to do the refactor.
5. When using Sylius Resource based classes (controllers, entities, interfaces, factories, repositories etc.), instead of putting the hardcoded FCQN in configuration (i.e. grids) please use the auto-generated parameters.
6. Prefer using template events than overridding Sylius / Symfony templates in plugin.
7. When supporting multiple Sylius / Symfony versions you probably would to load different configurations for them. To do so, you can follow [our CMS Kernel logic](https://github.com/BitBagCommerce/SyliusCmsPlugin/blob/master/tests/Application/Kernel.php).
8. We use GitHub Actions for CI. When developing new plugin. You can see the full instructions on how to create a build file [here](./doc/GithubBuilds.md)
9. README.md file is a must have. It has to contain at least description of plugin features and installation process. And please remember to test the installation process (following all the README commands) before publishing any changes.

**Be smart and keep in mind that once you do something stupid, I will find you and I will force you to work with Laravel or Magento.**
**There is nothing called a stupid question, but please ask it in a smart way :).**
**It's better to talk about a feature with the whole team for 30 minutes than lose 8 hours on implementing some dummy code that will destroy the current codebase order and deep the technical debt.**

# Subchapters
### [1. Code Style](./DevelopingPlugins/1_CodeStyleSubchapter.md)
### [2. Testing](./DevelopingPlugins/2_TestingSubchapter.md)
### [3. Workflow](./DevelopingPlugins/3_WorkflowSubchapter.md)
### [4. Open Source](./DevelopingPlugins/4_OpenSourceSubchapter.md)


