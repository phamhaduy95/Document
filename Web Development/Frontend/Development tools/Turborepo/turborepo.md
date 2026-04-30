This page is created with the initial purpose of keeping all findings and knowledge about turborepo. Since it is just a temporary note, it may look quite messy for few first version. However, after enough data is gathered, I would then write a completed document which may include many sub pages each of which focus on one single characteristic or feature offered by turborepo.

Things to do in the journey of exploring tubrorepo.

- [ ] Learn to how initiate a monorepo with turbopack.
- [ ] Try to register a new project inside turbopack.
- [ ] Test if we can define a local configuration for each project but still extend the common config file.
- [ ] Differ library package from application project and try to set up one.
- [ ] Attempt to build one specific application alongside with its dependency and then deploy it via a docker image. (mostly for testing packet pruning and remote cache)
- [ ] Find a way to specify the URL for storing cached build using remoting cache feature.

Since everything is kept under one single repository, It is hard to do version switching for one library

crafting an internal package is a great way to share code and functionality in your code base. One application of its is to keep all of our reusable components which are then imported into multiple react project.

Beware of circle dependency, It is your responsibility to ensure the correct dependency graph

Some of your application project may depend on several internal package.

The default starter pack consists of two applications and three library packages. What interests me most is that all ts-configs file are stored within type-script-config packages. Inside it, there is one base config and other configs which extends from the base file.

having $schema value inside one json file will help your IDE provides some basic type-checking and autocompletion mechanism.

```json
{
  "$schema": "<https://json.schemastore.org/tsconfig>",
  "display": "Next.js",
  "extends": "./base.json",
  "compilerOptions": {
    "plugins": [{ "name": "next" }],
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "allowJs": true,
    "jsx": "preserve",
    "noEmit": true
  }
}
```

Each project in turborepo has it own node_modules folders unlike NX platform which I found extremely useful when we want to build just one specific project since it only includes dependency on which the project rely. Combining with built-in command turbo prune, it helps minimize the final bundle size in productions.

recently, I have found a very nice feature that turborepo offers. That is the ability to define a template file which then is consumed to generate a

How we can store a cached build in a different storage ?

By default,