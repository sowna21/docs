---
id: init
title: 1. Configuration
hide_title: true
---

# 1. Configuration

## 1) Install supertokens package
```js
yarn install supertokens-node
yarn install supertokens-auth-react
yarn install nextjs-cors
```

## 2) Create a configuration file (`config/supertokensConfig.js`)
- Create a `config` folder in the root directory of your project
- Create a `supertokensConfig.js` inside the `config` folder.
- An example of this file can be found [here](https://github.com/supertokens/next.js/blob/canary/examples/with-supertokens/config/supertokensConfig.js).


## 3) Create a frontend config function
This function will return the config object used to configure supertokens-auth-react:
- Fill in your information in the `appInfo` object. To learn more about this config, please visit [the appInfo page](../appinfo).
- All configuration options can be found [here](/docs/auth-react/emailpassword/init)

<!--DOCUSAURUS_CODE_TABS-->
<!--/config/supertokensConfig.js-->
```js

import EmailPasswordReact, {Google, Facebook} from 'supertokens-auth-react/recipe/emailpassword'
import SessionReact from 'supertokens-auth-react/recipe/session'

const appInfo = {
  // learn more about this on https://supertokens.io/docs/emailpassword/appinfo
  appName: 'SuperTokens Demo App', // TODO: Your app name
  websiteDomain: "http://localhost:3000", // TODO: Add your website domain
  apiDomain: "http://localhost:3000", // TODO: should be equal to `websiteDomain` in case using the `api` folder for APIs
  apiBasePath: "/api/auth/", // /api/auth/* will be where APIs like sign out, sign in will be exposed
}

export const frontendConfig = () => {
  return {
    appInfo,
    recipeList: [
      EmailPasswordReact.init(),
      SessionReact.init(),
    ],
  }
}

```
<!--/config/supertokensConfig.ts-->
```js

import EmailPasswordReact, {Google, Facebook} from 'supertokens-auth-react/recipe/emailpassword'
import SessionReact from 'supertokens-auth-react/recipe/session'

const appInfo = {
  // learn more about this on https://supertokens.io/docs/emailpassword/appinfo
  appName: 'SuperTokens Demo App', // TODO: Your app name
  websiteDomain: "http://localhost:3000", // TODO: Add your website domain
  apiDomain: "http://localhost:3000", // TODO: should be equal to `websiteDomain` in case using the `api` folder for APIs
  apiBasePath: "/api/auth/", // /api/auth/* will be where APIs like sign out, sign in will be exposed
}

export const frontendConfig = () => {
  return {
    appInfo,
    recipeList: [
      EmailPasswordReact.init(),
      SessionReact.init(),
    ],
  }
}

```
<!--END_DOCUSAURUS_CODE_TABS-->

## 4) Create a backend config function
This function will return the config object used to configure supertokens-node:
- Add secrets for your third party login providers
- All configuration options can be found [here](/docs/nodejs/emailpassword/init)

<!--DOCUSAURUS_CODE_TABS-->
<!--/config/supertokensConfig.js-->
```js

import EmailPasswordNode, {Google, Facebook} from 'supertokens-node/recipe/emailpassword'
import SessionNode from 'supertokens-node/recipe/session'

export const backendConfig = () => {
  return {
    supertokens: {
      connectionURI: 'https://try.supertokens.io',
    },
    appInfo,
    recipeList: [
      EmailPasswordNode.init(),
      SessionNode.init(),
    ],
    isInServerlessEnv: true,
  }
}

```
<!--/config/supertokensConfig.ts-->
```js

import EmailPasswordNode, {Google, Facebook} from 'supertokens-node/recipe/emailpassword'
import SessionNode from 'supertokens-node/recipe/session'

export const backendConfig = () => {
  return {
    supertokens: {
      connectionURI: 'https://try.supertokens.io',
    },
    appInfo,
    recipeList: [
      EmailPasswordNode.init(),
      SessionNode.init(),
    ],
    isInServerlessEnv: true,
  }
}

```
<!--END_DOCUSAURUS_CODE_TABS-->

## 5) Call the `init` functions

- Create a `/pages/_app.js` file. You can learn more about this file [here](https://nextjs.org/docs/advanced-features/custom-app).
- An example of this can be found [here](https://github.com/supertokens/next.js/blob/canary/examples/with-supertokens/pages/_app.js)

<!--DOCUSAURUS_CODE_TABS-->
<!--/pages/_app.js-->

```js

import '../styles/globals.css'
import React from 'react'
import SuperTokensReact from 'supertokens-auth-react'
import SuperTokensNode from 'supertokens-node'

import * as SuperTokensConfig from '../config/supertokensConfig'

if (typeof window !== 'undefined') {
  SuperTokensReact.init(SuperTokensConfig.frontendConfig())
} else {
  SuperTokensNode.init(SuperTokensConfig.backendConfig())
}

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}

export default MyApp
```
<!--/pages/_app.ts-->

```js

import '../styles/globals.css'
import React from 'react'
import { AppProps } from 'next/app'
import SuperTokensReact from 'supertokens-auth-react'
import SuperTokensNode from 'supertokens-node'

import * as SuperTokensConfig from '../config/supertokensConfig'

if (typeof window !== 'undefined') {
  SuperTokensReact.init(SuperTokensConfig.frontendConfig())
} else {
  SuperTokensNode.init(SuperTokensConfig.backendConfig())
}

function MyApp({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
}

export default MyApp
```

<!--END_DOCUSAURUS_CODE_TABS-->