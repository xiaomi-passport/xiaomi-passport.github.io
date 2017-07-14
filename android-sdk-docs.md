## Quick Start

### 1. Preliminary steps

To create an account for your app on [xiaomi open platform](http://dev.xiaomi.com):

1. Go to the website and log in
2. Click on “管理控制台/Administrative Console”
3. Click on “手机及平板应用/Mobile and Tablets Apps”
4. Click on “创建应用/Create an app”
5. Fill in the app name and package name
6. Click on “创建/Create”
7. Write down the APP ID shows up
8. Look for “帐号接入/Account Access Services”
9. Click on “Details”
10. Click on “Enable now”
11. Fill in “回调地址/Authorized Redirect URL”
12. Click on “启用/Enable”
13. Enable needed Open API

### 2. Integrating the following code in “AndroidManifest.xml”

```xml
<uses-permission android:name="com.xiaomi.permission.AUTH_SERVICE" />
<uses-permission android:name="android.permission.GET_ACCOUNTS" />
<activity android:name="com.xiaomi.account.openauth.AuthorizeActivity" />
```

### 3. Authorizing and getting AccessToken/code

- Sdk is able to detect: if it’s on miui, taking system account to authorize; if it’s on other OEMs, taking webview to log in and then authorize.
- `setCustomizedAuthorizeActivityClass()`: it’s able to customize login page UI on non-miui roms, like actionbar, loading bar etc., please refer to `CustomizedAuthorizedActivity` in the demo.

```java
XiaomiOAuthFuture<XiaomiOAuthResults> future = new XiaomiOAuthorize()
     //The AppID you got from xiaomi.com
    .setAppId(appID)
     // The redirectUrl you filled in in application
    .setRedirectUrl(redirectUri)
    // int array, you can use constant like XiaomiOAuthConstants.SCOPE_*
    .setScope(scope)
     // set login page for non-miui roms(AuthorizeActivity is default)
    .setCustomizedAuthorizeActivityClass(CustomizedAuthorizedActivity.class)
     // If you want Code instead of AccessToken，please replace startGetAccessToken with startGetOAuthCode
    .startGetAccessToken(activity);
```

- Fast OAuth: authorize on pop-up window on miui

Effect: if sdk detect user has signed in with system account, the window will pop up

> - Support: miui v8.2+, On miui older than v8.2 and non-miui roms, `future.getResult()` will throw XMAuthericationException
> - System account/Mi account has to be logged in, otherwise `future.getResult()` will throw XMAuthericationException

```java
XiaomiOAuthFuture<XiaomiOAuthResults> future = new XiaomiOAuthorize()
    .setAppId(getAppId())
    .setRedirectUrl(getRedirectUri())
    .setScope(getScopeFromUi())
    .fastOAuth(MainActivity.this, XiaomiOAuthorize.TYPE_TOKEN);
```

- Getting authorized AccessToken/Code (call on a background thread)

```java
// Must call on the background thread
try {
    XiaomiOAuthResults result = future.getResult();
    if (results.hasError()) {
        int errorCode = results.getErrorCode();
        String errorMessage = results.getErrorMessage();
    } else {
        String accessToken = results.getAccessToken();
        String macKey = results.getMacKey();
        String macAlgorithm = results.getMacAlgorithm();
    }
} catch (IOException e1) {
    // error
} catch (OperationCanceledException e1) {
    // User cancel
} catch (XMAuthericationException e1) {
    // error
}
```

### 4. Getting user info with AccessToken

#### 4.1 Getting user card

```java
// Could call on the UI thread
XiaomiOAuthFuture<String> future = new XiaomiOAuthorize().callOpenApi(context,
    appId,
    XiaomiOAuthConstants.OPEN_API_PATH_PROFILE,
    results.getAccessToken(),
    results.getMacKey(),
    results.getMacAlgorithm());
```

```java
// Must call on the background thread
try {
    String result = future.getResult();
} catch (IOException e1) {
    // error
} catch (OperationCanceledException e1) {
    // error
} catch (XMAuthericationException e1) {
    // error
}
```

## Tips

### 1. Skip Confirm

- When user has already authorized, will not ask again for authorization
- It will be impossible for user to change an account
- The process of popping up authorizing page will be slow

```java
XiaomiOAuthFuture<XiaomiOAuthResults> future = new XiaomiOAuthorize()
    .setSkipConfirm(true)
    // .setOtherParams...
    // .startGetAccessToken(activity);
```

### 2. Scope

In simple terms, scope is representing a permission of access token. When using an access token to access open api, only if the scope of the access token is the permission that this open api needed, the server will return a correct result, otherwise will produce an error.

For the value of scope, please refer to [scope list](scope-list.html), and please choose the scope based on the api you needed. For example, if getting user’s personal data and friends’ list on Mi Talk, then value of scope would be 1 and 2. Another defined constant in `SDK XiaomiOAuthConstants.SCOPE_***` works as well, as long as relevant permissions were enabled on dev.xiaomi.com in preliminary steps.
