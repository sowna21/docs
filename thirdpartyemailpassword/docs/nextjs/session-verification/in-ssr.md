---
id: in-ssr
title: 5b. Session verification in getServerSideProps
hide_title: true
---

# 5b. Session verification in getServerSideProps

> This is applicable for when verifying a session in `getServerSideProps` or `getInitialProps`.

For this guide, we will assume that we want to pass the logged in user's ID as a prop to a protected route. An easier method to achieve this would be to [directly get the user ID from the frontend](../../common-customizations/sessions/user-information-front-end), but for this example, we will pass it from the server side.

This method is different to the API call method as you need to manually take care of session refreshing.

## 1) We use `getSession` in `getServerSideProps`

> If using `getInitialProps`, the method described below applies as well. The only difference is the way the props are returned (see comments in the code below).

- An example of this can be found [here](https://github.com/supertokens/next.js/blob/canary/examples/with-supertokens/pages/index.js#L15).
- Note that we do NOT need to call `supertokens.init` here since this is not an API route, and we already call it in `_app.js`

```js
import Session from 'supertokens-node/recipe/session'

export async function getServerSideProps(context) {

    // this will contain the session object post verification
    let session

    try {
        // getSession will do session verification for us
        session = await Session.getSession(context.req, context.res)
    } catch (err) {
        if (err.type === Session.Error.TRY_REFRESH_TOKEN) {
            
            // in this case, the session is still valid, only the refresh token has expired.
            // The refresh token is not sent to this route as it's tied to the /api/auth/session/refresh API paths.
            // So we must send a "signal" to the frontend which will then call the 
            // refresh API and reload the page.

            return { props: { fromSupertokens: 'needs-refresh' } }
            // or return {fromSupertokens: 'needs-refresh'} in case of getInitialProps
        } else if (err.type === Session.Error.UNAUTHORISED) {

            // user is logged out. Since this is for a protected route,
            // we can simple send an empty prop object. Alternatively,
            // you can pass anything else you would like here.
            return { props: {} }
            // or return {} in case of getInitialProps
        } else {
            throw err
        }
    }

    // session verification is successful and we can pass
    // the user's ID to the frontend.

    return {
        props: { userId: session.getUserId() },
    }
    // or return {userId: session.getUserId()} in case of getInitialProps
}
```

## 2) Doing manual refresh on the frontend

- The following will refresh a session if needed, for all your website pages
- This goes in the `/pages/_app.js` file
- An example of this can be found [here](https://github.com/supertokens/next.js/blob/canary/examples/with-supertokens/pages/_app.js#L16).

<!--DOCUSAURUS_CODE_TABS-->
<!--/pages/_app.js-->

```js
import Session from 'supertokens-auth-react/recipe/session'
import { redirectToAuth } from 'supertokens-auth-react/recipe/thirdpartyemailpassword'

function MyApp({ Component, pageProps }) {
    useEffect(() => {

        async function doRefresh() {
            // pageProps.fromSupertokens === 'needs-refresh' will be true
            // when in getServerSideProps, getSession throws a TRY_REFRESH_TOKEN
            // error.

            if (pageProps.fromSupertokens === 'needs-refresh') {
                if (await Session.attemptRefreshingSession()) {
                    // post session refreshing, we reload the page. This will
                    // send the new access token to the server, and then 
                    // getServerSideProps will succeed
                    location.reload()
                } else {
                    // the user's session has expired. So we redirect
                    // them to the login page
                    redirectToAuth()
                }
            }
        }
        doRefresh()

    }, [pageProps.fromSupertokens])
    
    if (pageProps.fromSupertokens === 'needs-refresh') {
        // in case the frontend needs to refresh, we show nothing.
        // Alternatively, you can show a spinner.

        return null
    }

    // the below is already there by default
    return <Component {...pageProps} />
}

export default MyApp
```

<!--END_DOCUSAURUS_CODE_TABS-->

## 3) Consume the userId returned by getServerSideProps in your component

On success, `getServerSideProps` returns
```js
{
    props: { userId: session.getUserId() }
}
```

Therefore, the page associated page can access the `userId` like:

```js
export default function Home(props) {
    let userId = props.userId;
}
```