## Motivation

Sometime we want to keep sensitive data, such as API keys, from working directory tree so that when we publish our project, we don’t accidentally expose it. One common but effective solution for this issue is to treat all your secrets as environment variable. The environment variable can be injected to our code during our build time and can not be read at run-time like regular JS variable. So only way to define the environment variable for your app is through build command or configuration file.

Add env variable through build command (`npm` build command for example).

#### Using `dotenv` libs**

Most popular web framework integrate `dotenv` by default. As results, you can just create .env file without installing whole `dotenv` packet from `npm`. However, most of the time, you have to follow the name convention rule for these .env file so that the framework can read env file properly.

There is also a built-in environment variable called NODE_ENV which is also quite common in many framework including Create-react-app. You can read its value from `_process.env.NODE_ENV_`. When you run `npm start`, it is always equal to 'development', when you run `npm test` it is always equal to 'test', and when you run `npm run` build to make a production bundle, it is always equal to 'production'. You cannot override NODE_ENV manually.

**Creating multiple .env file for each stage.**