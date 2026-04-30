The module federation was just a prominent feature that was introduced to webpack as an experience way for sharing code between applications. The module federation received lots of appreciations from FE community and soon became a popular choice for setting up micro-frontend architect

the pros:

it helps share your module at application level between different projects or repositories. The marketed benefit to ensure three characteristics, modularity, maintainability and last but not least scalability. The application level modules can run like a dependent application which may use different framework. Routing between the host and remote app.

How can we

install the necessary package and plugin base on the framework and bundle tool that are used in your project.

set up a new project which become a provider whose task is to serve remote modules for different project called consumer. In the provider

on the consumer side, you need to create a Bridge Component which receive the module sent by the provider and render it on the consumer ‘s screen.

the cons:

You have to setup a standalone source code which hold a role of provider that serve the remote module to many consumers or host applications.

It only provides full support for rspack and webpack, and partially for vite. At the time for writing this article, module federation allows only code written in react and vue3.

With NextJS, you are still able to apply MF for legacy page router while React Server Component (RSC) hasn’t received any integration for MF yet. In fact, the Next dev team is not interest in MF at all, thus, they decided to drop support for it for their new architect.

Question. Will MF work well with other meta frameworks such as Remix or Nuxt?

You can manually specify shared dependencies between applications, which greatly improves performance cause it reduce amount of files to load. If there is a version mismatch in shared package, MF will opt to latest versions instead unless you explicitly set the version in case of mismatch.

You