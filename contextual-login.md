## Contextual Sign-in with Mi Account

With MIUI 8.2 released, contextual sign-in with Mi Account is coming soon, which can improve the account's sign-in rate on MIUI platform. On Android OAuth.1.5.jar and the following versions, this feature will be available just by adjusting Fast OAuth call logic.

1. Fast OAuth will automatically detect if there is an account on the MIUI environment to determine whether to call popup windows.
2. General contextual sign-in logic has 3 cases: first download, high sign-in priority on MIUI environment, sign-in evoked by operational activities (you can decide other logic cases on your own after ensuring the user experience)
3. Fast OAuth interface documents & SDK: [https://github.com/xiaomi-passport/oauth-android-sdk](https://github.com/xiaomi-passport/oauth-android-sdk)

#### __Scene One__

![img](images/contextual_login_cache_one.png)

#### __Scene Two__

![img](images/contextual_login_cache_two.png)

#### __Scene Three__

![img](images/contextual_login_cache_three.png)
