---
id: redirection-callback
title: Redirection Callback Hook
hide_title: true
---

# Redirection Callback Hook

This function is used to overwrite where the user is redirected to post certain actions. For example, you can use this to redirect a user to a specific URL post sign in / up.

<!--DOCUSAURUS_CODE_TABS-->
<!--ReactJS-->
```js
EmailPassword.init({
    getRedirectionURL: async (context) => {
        if (context.action === "RESET_PASSWORD") {
            // called when the user clicked on the forgot password button
        } else if (context.action === "SIGN_IN_AND_UP") {
            // called when the user is navigating to sign in / up page
        } else if (context.action === "SUCCESS") {
            // called on a successful sign in / up. Where should the user go next?
            let redirectToPath = context.redirectToPath;
            if (redirectToPath !== undefined) {
                // we are navigating back to where the user was before they authenticated
                return redirectToPath;
            }
            if (context.isNewUser) {
                // user signed up
                return "/onboarding"
            } else {
                // user signed in
                return "/dashboard"
            }
        } else if (context.action === "VERIFY_EMAIL") {
            // called when the user is to be shown the verify email screen
        }
        // return undefined to let the default behaviour play out
        return undefined;
    }
});
```
<!--END_DOCUSAURUS_CODE_TABS-->