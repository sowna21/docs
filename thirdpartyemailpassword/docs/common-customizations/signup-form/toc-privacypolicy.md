---
id: toc-privacypolicy
title: Terms of service & Privacy policy links
hide_title: true
---

# Terms of service & Privacy policy links

You can provide a Privacy policy and Terms of service link which will render the following text on the sign up page:
- Provided both links: "By signing up, you agree to our [Terms of Service]() and [Privacy Policy]()"
- Provided only Terms of service link: "By signing up, you agree to our [Terms of Service]()"
- Provided only Privacy policy link: "By signing up, you agree to our [Privacy Policy]()"
<!--DOCUSAURUS_CODE_TABS-->
<!--ReactJS-->
```js
SuperTokens.init({
    appInfo: {...},
    recipeList: [
        ThirdPartyEmailPassword.init({
__HIGHLIGHT__            signInUpFeature: {
                signUpForm: {
                    termsOfServiceLink: "https://example.com/terms-of-service",
                    privacyPolicyLink: "https://example.com/privacy-policy"  
                }
            } __END_HIGHLIGHT__
        }),
        Session.init({...})
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->