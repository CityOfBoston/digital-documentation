# Metrolist

&#x20;[![Build Status](https://camo.githubusercontent.com/dbfa303dc33d4a2b6d763a1010adb7d405ff5fad/68747470733a2f2f6170692e7472617669732d63692e636f6d2f686775696e65792f6d6574726f6c6973742e7376673f6272616e63683d6d6173746572)](https://travis-ci.com/hguiney/metrolist) [![Code Coverage](https://camo.githubusercontent.com/215f42bd6141aaca6c18a94a918b465462b55c09/68747470733a2f2f696d672e736869656c64732e696f2f636f6465636f762f632f6769746875622f686775696e65792f6d6574726f6c6973742f6d61737465722e737667)](https://codecov.io/gh/hguiney/metrolist/)

## Project Overview

Metrolist allows Boston residents to search for affordable housing. The **Search** and **AMI Estimator** experiences are built in React (this repository). The rest of the app is built in Drupal, with the underlying data layer provided by Salesforce. The core UX is composed of the following:

**Homepage**\
Links to Search, AMI Estimator, and introductory information.\
Route: [/metrolist/](http://www.boston.gov/metrolist/)\
Controlled by: Drupal\
\
**Search**\
Lists housing opportunities in a paginated fashion and allows user to filter according to various criteria.\
Route: [/metrolist/search](http://www.boston.gov/metrolist/search/)\
Controlled by: React\
APIs in use: Developments API\
\
**AMI Estimator**\
****Takes user’s household income and household size, and calculates a recommendation for which housing opportunities to look at.\
URL: [/metrolist/ami-estimator/](http://www.boston.gov/metrolist/ami-estimator/)\
Sub-routes:

* /metrolist/ami-estimator/household-income
* /metrolist/ami-estimator/disclosure
* /metrolist/ami-estimator/result

Controlled by: React\
APIs in use: AMI API\
\
**Property Pages**\
****Route: /metrolist/search/housing/\[property]?\[parameters]\
Controlled by: Drupal\
\
**Developments API**\
****Lists housing opportunities as a JSON object.\
URL: [/metrolist/api/v1/developments?\_format=json](https://www.boston.gov/metrolist/api/v1/developments?\_format=json)\
\
**AMI API**\
****Lists income qualification brackets as a JSON object, taken from HUD (Department of Housing and Urban Development) data.\
URL: [/metrolist/api/v1/ami/hud/base?\_format=json](https://www.boston.gov/metrolist/api/v1/ami/hud/base?\_format=json)

## Installing

Prerequisites:

* Node.js
* Yarn or NPM (These docs use `yarn` but it can be substituted for `npm` if you prefer.)
* Git
* Read/write access to `CityOfBoston` GitHub

```
# Clone the Boston.gov Drupal monorepo
git checkout git@github.com:CityOfBoston/boston.gov-d8.git

# If installing directly from the monorepo:
cd boston.gov-d8/docroot/modules/custom/bos_components/modules/bos_web_app/metrolist

# If installing standalone:
git checkout git@github.com:hguiney/metrolist.git
cd metrolist

# Dependencies
yarn install
yarn sync:ami ci # Grabs a copy of the latest AMI figures from the CI development server in Acquia.
```

**⚠️ Warning:** These docs were written for a standalone installation of the Metrolist React codebase, which outputs JavaScript files that can be committed to the Drupal monorepo separately. However, the React codebase has since been subsumed into the monorepo, rendering certain build instructions herein out-of-date. Please refer to the Boston.gov documentation for further instruction.

## Development

`yarn start` runs:

1. `ipconfig getifaddr en6` (or `ipconfig getifaddr en0` if `en6` isn’t found), which determines which LAN IP to bind to. This allows testing on mobile devices connected to the same network.
2. `webpack-dev-server`. This compiles the ES6+ JavaScript and starts an HTTP server on port 8080 at the address found in the previous step.

Note: The `ipconfig` command has only been tested on a Mac, and it also may not work if your connection isn’t located at `en6` or `en0`.

This runs `webpack-dev-server` without launching a new browser window automatically.

## Command-line Tools

There are Node.js scripts available under `_scripts/` to aid development efforts.

### Component CLI

Located at `_scripts/component.js`, this facilitates CRUD-style operations on components.

#### Add

```
yarn component add Widget
```

This copies everything under `_templates/components/Component` to `src/components/Widget` and does a case-sensitive find-and-replace on the term “component”, replacing it with your new component’s name. For instance, this `index.js` template:

```
import React from 'react';
import PropTypes from 'prop-types';

import './Component.scss';

function Component( props ) {
  return (
    <div data-testid="ml-component" className={ `ml-component${props.className ? ` ${props.className}` : ''}` }>
      { props.children }
    div>
  );
}

Component.displayName = 'Component';

Component.propTypes = {
  "children": PropTypes.node,
  "className": PropTypes.string,
};

export default Component;
```

…becomes this:

```
import React from 'react';
import PropTypes from 'prop-types';

import './Widget.scss';

function Widget( props ) {
  return (
    <div data-testid="ml-widget" className={ `ml-widget${props.className ? ` ${props.className}` : ''}` }>
      { props.children }
    </div>
  );
}

Widget.displayName = 'Widget';

Widget.propTypes = {
  "children": PropTypes.node,
  "className": PropTypes.string,
};

export default Widget;
```

Subcomponents can also be added. These are useful if you want to encapsulate some functionality inside of a larger component, but this smaller component isn’t useful elsewhere in the app.

```
yarn component add Widget/Gadget
```

This creates the directory `src/components/Widget/_WidgetGadget` containing this `index.js`:

```
import React from 'react';
import PropTypes from 'prop-types';

import './WidgetGadget.scss';

function WidgetGadget( props ) {
  return (
    <div data-testid="ml-widget__gadget" className={ `ml-widget__gadget${props.className ? ` ${props.className}` : ''}` }>
      { props.children }
    div>
  );
}

WidgetGadget.displayName = 'WidgetGadget';

WidgetGadget.propTypes = {
  "children": PropTypes.node,
  "className": PropTypes.string,
};

export default WidgetGadget;
```

As you can see, the hierarchical relationship between Widget and Gadget is reflected in the naming. The React display name is `WidgetGadget`, and the CSS class name uses a [BEM](broken-reference) element `gadget` belonging to the `widget` block, i.e. `widget__gadget`.

#### Rename

```
yarn component rename Widget Doohickey
```

This renames the directory and does a find-and-replace on its contents.

**⚠️ Known issue:** The component renaming algorithm does not fully find/replace on subcomponents.

#### Remove

```
yarn component remove Doohickey
```

### Sync AMI CLI

```
yarn sync:ami [environment id]
```

Due to [compatibility issues with Google Translate](broken-reference), the AMI API is not fetched live from the AMI Estimator. Instead, it is fetched at compile time using this script, which caches it as a local JSON file at `src/components/AmiEstimator/ami-definitions.json`.

The domain from which this data is fetched can be specified with the following environment IDs:

* `www` or `prod` → [https://www.boston.gov](https://www.boston.gov/)
* Acquia environment
  * `ci` → [https://d8-ci.boston.gov](https://d8-ci.boston.gov/)
  * `dev2` → [https://d8-dev2.boston.gov](https://d8-dev2.boston.gov/)
  * etc.

The default value is `ci`, as that should have the most recent data set in most cases.

### Version CLI

```
yarn version [-m major] [-n minor] [-p patch]
```

Sets the version number for Metrolist in Drupal’s `libraries.yml` file and this project’s `package.json` file.

| Option          | Description                                                                                                                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `-m`, `--major` | <p>Sets the left version part, e.g. 2.x.x.<br>If omitted, major will be taken from existing Metrolist version.</p>                                                                                           |
| `-n`, `--minor` | <p>Sets the middle version part, e.g. x.5.x.<br>If omitted, minor will be a hash of index.bundle.js for cache-busting.</p>                                                                                   |
| `-p`, `--patch` | <p>Sets the right version part, e.g. x.x.3289.<br>If omitted while minor is set, patch will be a hash of index.bundle.js for cache-busting.<br>If omitted while minor is not set, patch will not be set.</p> |
| `-f`, `--force` | Allow downgrading of Metrolist version.                                                                                                                                                                      |
| `--help`        | This screen.                                                                                                                                                                                                 |

## General Naming Conventions

### DAMP (Descriptive And Meaningful Phrases).

Prefer readability for other developers over less typing for yourself.

#### Examples

**HTML/CSS:**

```
<h2 class="sh">Section Headerh2>
<h2 class="section-header">Section Headerh2>
```

**JavaScript:**

```
const newElCmpShrtNm = 'Header'; // Bad
const newElementComponentShortName = 'Header'; // Good
```

## Programming Conventions

Consistent and readable JavaScript formatting is enforced by [`eslint-config-hughx`](https://github.com/hguiney/eslint-config-hughx) + an ESLint auto-formatter of your choice, such as [ESLint for VS Code](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint).

### Functional Programming

Use Functional Programming principals as often as possible to aid maintainability and predictability. The basic idea is for every function to produce the same output for a given set of inputs regardless of when/where/how often they are called. This means a preference for functions taking their values from explicit parameters as opposed to reading variables from the surrounding scope. Additionally, a function should not produce side-effects by e.g. changing the value of a variable in the surrounding scope.

### Directory Layout

* `metrolist/`
  * `__mocks__/`: Mocked functions for unit/integration tests.
  * `_scripts/`: CLI tools
  * `_templates/`: Stubbed files for project scaffolding. Used by CLI tools.
  * `coverage/`: Code coverage report. Auto-generated. (`.gitignore`’d)
  * `dist/`: Build output. Auto-generated. (`.gitignore`’d)
  * `public/`: Static files such as images, favicon, etc. These files are not used by Drupal, which uses its own tempalting; only in development. Thus, images have to be copied to the appropriate directory prior to deployment.
  * `src/`: React source.
    * `components/`: React components.
    * `globals/`: SASS variables, mixins, etc. which are used cross-component.
    * `util/`: Utility functions.
    * `index.js`: React entrypoint.
    * `index.scss`: App-wide styles. (Use sparinginly; prefer component-scoped.)
    * `serviceWorker.js`: Service Worker code from Create React App; not currently used.
    * `setupTests.js`: Jest configuration.
  * `_redirects`: Netlify redirects.
  * `.env`, `.env.development`, `.env.production`: [Dotenv](https://github.com/motdotla/dotenv#readme) configuration (environment variables).
  * `.eslintrc.js`: [ESLint](https://eslint.org/) configuration.
  * `.travis.yml`: [Travis CI](https://travis-ci.com/) configuration.
  * `babel.config.js`: [Babel](https://babeljs.io/) configuration.
  * `DEVNOTES.md`: Notes taken during development.
  * `package.json`: Project metadata/NPM dependencies.
  * `postcss.config.js`: [PostCSS](https://postcss.org/) configuration. Used to postprocess CSS output.
  * `README.md`: Project documentation (this file).
  * `webpack.config.js`, `webpack.production.js`, `webpack.staging.js`: [Webpack](https://webpack.js.org/) configurations for different environments.
  * `yarn.lock`/`package-lock.json`: Yarn/NPM dependency lock file.

### Component Layout

Every React component consists of the following structure:

* `Component/`
  * `__tests__`: Integration tests (optional)
  * `Component.scss`: SASS styling
  * `Component.test.js`: Unit test
  * `index.js`: React component
  * `methods.js`: Any methods that don’t need to go in the render function, for tidiness. (optional)

## CSS Conventions

### Namespacing

All classes namespaced as `ml-` for Metrolist to avoid collisions with main Boston.gov site and/or third-party libraries.

### BEM Methodology

Vanilla BEM (Block-Element-Modifier):

* Blocks: Lowercase name (`block`)
* Elements: two underscores appended to block (`block__element`)
* Modifiers: two dashes appended to block or element (`block--modifier`, `block__element--modifier`).

When writing modifiers, ensure the base class is also present; modifiers should not mean anything on their own. This also gives modifiers higher specificity than regular classes, which helps ensure that they actually get applied.

```
.block--modifier {} // Bad
.block.block--modifier {} // Good
```

An exception to this would be for mixin classes that are intended to be used broadly. For example, responsive utilities to show/hide elements at different breakpoints:

```
.ml-hide-until-large {
  display: none;
}
@media screen and (min-width: $large) {
  .ml-hide-until-large {
    display: inline-block; // IE/Edge compat
    display: unset;
  }
}
```

Don’t reflect the expected DOM structure in class names, as this expectation is likely to break as projects evolve. Only indicate which block owns the element. This allows components to be transposable and avoids extremely long class names.

```
.ml-block__element__subelement {} // Bad
.ml-block__subelement {} // Good
```

#### BEM within Sass

Avoid parent selectors when constructing BEM classes. This allows the full selector to be searchable in IDEs. (Though there is a VS Code extension, [CSS Navigation](https://marketplace.visualstudio.com/items?itemName=pucelle.vscode-css-navigation), that solves this problem, we can’t assume everyone will have it or VS Code installed.)

```
.ml-block {
  &__element {} // Bad
}
.ml-block {}
.ml-block__element {} // Good
```

### Sass

Always include parentheses when calling mixins, even if they have no arguments.

```
@mixin mixin() {
  // …
}
@include mixin; // Bad
@include mixin(); // Good
```

### Spacing

Don’t declare margins directly on components, only in wrappers.

#### Resources

* [Margin considered harmful](https://mxstbr.com/thoughts/margin)
* [The Stack](https://absolutely.every-layout.dev/layouts/stack/)
* [Braid Design System](https://seek-oss.github.io/braid-design-system/foundations/layout)

### Postprocessing

[Rucksack](https://www.rucksackcss.org/) is installed to enable the same CSS helper functions that are used on Patterns, such as `font-size: responsive 16px 24px`.

## Build Process

### Development

Currently this is used for previewing on Netlify, to get a live URL up without going through the lengthy Travis and Acquia build process.

### Production

This first runs a production Webpack build (referencing `webpack.config.js`), then copies the result of that build to `../boston.gov-d8/docroot/modules/custom/bos_components/modules/bos_web_app/apps/metrolist/`, replacing whatever was there beforehand. This requires you to have the [`boston.gov-d8`](https://github.com/CityOfBoston/boston.gov-d8) repo checked out and up-to-date one directory up from the project root.

To make asset URLs work both locally and on Drupal, all references to `/images/` get find-and-replaced to `https://assets.boston.gov/icons/metrolist/` when building for production. Note that this requires assets to be uploaded to `assets.boston.gov` first, by someone with appropriate access. If you want to look at a production build without uploading to `assets.boston.gov` first, you can run a staging build instead.

### Staging

This is identical to the production build, except Webpack replaces references to `/images/` with `/modules/custom/bos_components/modules/bos_web_app/apps/metrolist/images/`. This is where images normally wind up when running `yarn copy:drupal`.

### Module Resolution

Aliases exist to avoid long pathnames, e.g. `import '@components/Foo'` instead of `import '../../../components/Foo'`. Any time an alias is added or removed, three configuration files have to be updated: `webpack.config.js` for compilation, `jest.config.js` for testing, and `.eslintrc.js` for linting. Each one has a slightly different syntax but they all boil down to JSON key-value pairs of the form \[alias] → \[full path]. Here are the same aliases defined across all three configs:

`webpack.config.js`:

```
module.exports = {
  "resolve": {
    "alias": {
      "@patterns": path.resolve( __dirname, 'patterns' ),
      "@util": path.resolve( __dirname, 'src/util' ),
      "@globals": path.resolve( __dirname, 'src/globals' ),
      "@components": path.resolve( __dirname, 'src/components' ),
      "__mocks__": path.resolve( __dirname, '__mocks__' ),
    },
  }
};
```

`jest.config.js`:

```
module.exports = {
  "moduleNameMapper": {
    "^@patterns/(.*)": "/patterns/$1",
    "^@util/(.*)": "/src/util/$1",
    "^@globals/(.*)$": "/src/globals/$1",
    "^@components/(.*)$": "/src/components/$1",
    "^__mocks__/(.*)$": "/__mocks__/$1",
    "\\.(css|s[ca]ss|less|styl)$": "/__mocks__/styleMock.js",
  },
};
```

`.eslintrc.js`:

```
module.exports = {
  "settings": {
    "import/resolver": {
      "alias": {
        "map": [
          ["@patterns", "./patterns"],
          ["@util", "./src/util"],
          ["@globals", "./src/globals"],
          ["@components", "./src/components"],
          ["__mocks__", "./__mocks__"]
        ],
        "extensions": [".js", ".scss"],
      },
    },
  },
};
```

## Interfacing with Main Site

* All `mailto:` links require the class `hide-form` to be set, otherwise they will trigger the generic feedback form.

## Testing

We’re using [Jest](https://jestjs.io/) + [React Testing Library](https://testing-library.com/) to ensure that future development doesn’t break existing functionality.

### Unit Tests

Every component should have its own unit test in the same directory. This is enforced by the Component test stub (`_templates/components/Component/Component.test.js`), which contains the following:

```
describe( 'Component', () => {
  it( 'Renders', () => {
    // render(  );
    throw new Error( 'Test missing' );
  } );
} );
```

So when running `yarn component add`, you automatically generate a test that fails by default. You have to manually uncomment the call to `render` (and ideally write more specific tests) in order to pass. This is designed to be annoying so it isn’t neglected.

### Integration Tests

When testing interactions between two or more components, or for utility functions (`src/util`), put tests in a nested `__tests__` directory.

One example of this is the `Search` component, which contains a separate test file for every `FiltersPanel` + `ResultsPanel` interaction,:

```
.
├── Search.scss
├── Search.test.js
├── __tests__
│   ├── bedrooms.test.js
│   ├── incomeEligibility.test.js
│   ├── incomeFeasibility.test.js
│   ├── location.test.js
│   ├── offer.test.js
│   └── rentalPrice.test.js
├── index.js
└── methods.js
```

## Testing API integrations locally

You have to run a browser without CORS restrictions enabled. For Chrome on macOS, you can add this to your `~/.bash_profile`, `~/.zshrc`, or equivalent for convenience:

```
alias chrome-insecure='open -n -a Google\ Chrome --args --disable-web-security --user-data-dir=/tmp/chrome --disable-site-isolation-trials --allow-running-insecure-content'
```

This will prevent you from running your normal Chrome profile. To run both simultaneously, install an alternate Chrome such as Canary or Chromium. For Canary you would use this command instead:

```
alias chrome-insecure='open -n -a Google\ Chrome\ Canary --args --disable-web-security --user-data-dir=/tmp/chrome --disable-site-isolation-trials --allow-running-insecure-content'
```

Then in a terminal, just type `chrome-insecure` and you will get a separate window with no security and no user profile attached. Sometimes Google changes the necessary commands to disable security, so check around online if this command doesn’t work for you. Unfortunately no extensions will be installed for this profile, and if you install any they will only exist for that session since your data directory is under `/tmp/`.

## Google Translate Compatibility

We’re using React Router for routing, which provides a `Link` component to use in place of `a`. `Link` uses `history.pushState` under the hood, but this will fail inside the Google Translate iframe due to cross-domain security features in the browser. (For an in-depth technical explanation of why this happens, see [DEVNOTES](https://github.com/hguiney/metrolist/blob/master/DEVNOTES.md#metrolist-fix-for-google-translate)). So in order to make app navigation work again, we have to hack around the issue like so:

* Change `base.href` to the Google Translate iframe domain,
* Perform the navigation,
* Change `base.href` back to boston.gov immediately afterward to make sure normal links and assets don’t break.

To do this automatically, there is a custom Metrolist `Link` which wraps the React Router `Link` and attaches a click handler with the workaround logic. So, anytime you want to use React Router’s `Link`, you need to import and use `@components/Link` instead. This is the technique used by the Search component to link to the different pages of results.

If instead you want to use React Router’s `history.push` (or the browser-native `history.pushState`) manually, you can import these helper functions individually:

```
import {
  switchToGoogleTranslateBaseIfNeeded,
  switchBackToMetrolistBaseIfNeeded,
} from '@util/translation';
import { useHistory } from 'react-router-dom';

const history = useHistory();

switchToGoogleTranslateBaseIfNeeded();

history.push( newUrlPath );

switchBackToMetrolistBaseIfNeeded();
```

This is the technique used by the AMI Estimator component to navigate between the different steps in the form.
