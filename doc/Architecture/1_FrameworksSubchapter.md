## Symfony / Sylius / Frameworks

1. We recommend using YAML (`*.yaml`) for defining routings and configs.
2. We recommend using XML (`*.xml`) for defining services, doctrine mappings, and validation definitions.
3. If you prefer handling points 1 and 2 differently, please do it consistent in entire project.
4. For services definitions in a single bundle use `form.xml`, `event_listener.xml`, etc. Don't put everything
   in the `services.xml` file, do it in public projects with only a few services. If you have more than one type of service
   Inside your app, create a separate config file under the `services/` directory.
5. Any information regarding external system (like DSN, service path) have to been placed in `.env` file as placeholders. If you consider putting there any credentials, please put only empty placeholders there.
6. Please use `.env.local` (which is not commited to the repository) file for all sensitive and environment-specific data.
7. Use finals everywhere you can (specially in non-bundle projects).
8. Repositories and entities in public projects should not (and cannot) be defined as `final`.
9. Interface-driven development is not welcome, but if you consider extending your class or overriding your service, please use traits and interfaces.
10. Entity fields in public projects (vendors) should be `protected` instead of `private`.
11. Decorate resource factories with decoration pattern and do not call resource instance with `new` keyword directly.
    Instead, inject resource factory into the constructor and call `createNew()` on it.
    See `Sylius\Component\Product\Factory\ProductFactory`, `sylius.custom_factory.product` service definition
    and [Symfony Service Decoration](https://symfony.com/doc/current/service_container/service_decoration.html). The `priority` flag we are starting with equals 1 and is increased by one for each other decoration.
12. Don't include the entire service container into your service by default, if you don't have to. Instead of that use Symfony Dependency Injection.
13. For customizing forms use [Symfony Form Extension](https://symfony.com/doc/current/form/create_form_type_extension.html).
14. We don't use either autowire nor autoconfigure Symfony options as it is a very "magic" way of defining services. We always prefer to manually define services and inject proper arguments into them to have better control of our Container. # To be discussed
15. Do not define services as public, if it's not necessary.
16. Don't use Sylius theme if you have one template in your project.


### [Previous chapter](../Architecture.md) / [Main page](../../README.md) / [Next chapter](./2_ToolsSubchapter.md)
