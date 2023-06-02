---
description: A listing of libraries and modules we use in every app.
---

# Web applications

## Libraries

| Library                                                                    | Purpose                                                                                 |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| [Babel](https://babeljs.io/)                                               | Can transform and polyfill advanced JavaScript syntax to be supported on older browsers |
| [Emotion](https://emotion.sh/docs/css)                                     | CSS-in-JS library                                                                       |
| [ESLint](https://eslint.org/)                                              | Static code analysis for JavaScript                                                     |
| [Formik](https://jaredpalmer.com/formik)                                   | React library for building forms                                                        |
| [GraphQL](https://graphql.org/) + [Apollo](https://www.apollographql.com/) | Query language for APIs; GraphQL client                                                 |
| [Hapi](https://hapijs.com/)                                                | Node.js web server framework                                                            |
| [Jest](https://jestjs.io/)                                                 | JavaScript unit testing library                                                         |
| [Lerna](https://lernajs.io/)                                               | Monorepo management library                                                             |
| [NextJS](https://nextjs.org/)                                              | Provides server-side rendering for React                                                |
| [Prettier](https://prettier.io/)                                           | Code style enforcement tool                                                             |
| [React](https://reactjs.org/)                                              | Front end UI library                                                                    |
| [Rollup](https://rollupjs.org/)                                            | A module bundler for JavaScript                                                         |
| [Yarn](https://yarnpkg.com/)                                               | Node package manager                                                                    |

## @cityofboston internal modules

Useful modules we’ve created and use across our apps in the monorepo.

| Module                                                                                                           | Purpose                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| [config-babel](https://github.com/CityOfBoston/digital/tree/develop/modules-js/config-babel)                     |                                                                                                                              |
| [config-jest-babel](https://github.com/CityOfBoston/digital/tree/develop/modules-js/config-jest-babel)           | Jest preset for projects that use Babel in their build process                                                               |
| [config-jest-typescript](https://github.com/CityOfBoston/digital/tree/develop/modules-js/config-jest-typescript) | Jest preset for pure TypeScript projects. Loads files with `ts-jest-babel-7`                                                 |
| [config-typescript](https://github.com/CityOfBoston/digital/tree/develop/modules-js/config-typescript)           | TypeScript configuration files that can be used in other packages so that we have a default set of TypeScript configurations |
| [deploy-tools](https://github.com/CityOfBoston/digital/tree/develop/modules-js/deploy-tools)                     |                                                                                                                              |
| [graphql-typescript](https://github.com/CityOfBoston/digital/tree/develop/modules-js/graphql-typescript)         | Utilities for type-safe GraphQL resolvers                                                                                    |
| [hapi-common](https://github.com/CityOfBoston/digital/tree/develop/modules-js/hapi-common)                       |                                                                                                                              |
| [hapi-next](https://github.com/CityOfBoston/digital/tree/develop/modules-js/hapi-next)                           |                                                                                                                              |
| [mssql-common](https://github.com/CityOfBoston/digital/tree/develop/modules-js/mssql-common)                     |                                                                                                                              |
| [mssql-typescript](https://github.com/CityOfBoston/digital/tree/develop/modules-js/mssql-typescript)             |                                                                                                                              |
| [next-client-common](https://github.com/CityOfBoston/digital/tree/develop/modules-js/next-client-common)         |                                                                                                                              |
| [percy-common](https://github.com/CityOfBoston/digital/tree/develop/modules-js/percy-common)                     |                                                                                                                              |
| [react-fleet](https://github.com/CityOfBoston/digital/tree/develop/modules-js/react-fleet)                       | Common React components                                                                                                      |
| [srv-decrypt-env](https://github.com/CityOfBoston/digital/tree/develop/modules-js/srv-decrypt-env)               |                                                                                                                              |
