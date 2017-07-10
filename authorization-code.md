### __1. Getting Authorization Code__

#### Request URL： &emsp; `https://account.xiaomi.com/oauth2/authorize`
#### __Request Method：__ &emsp; GET
#### __Request Data：__

name | required | type | description
---|--- | --- | ---
client_id | yes | long | allocated ​APP ID​ during app requests
redirect_uri | yes | string | request redirect url, should be the same as the one in allocated APP ID (other data may be different)
response_type | yes | string | description of response type, __response_type=code__
scope | optional | string | data required for getting scope permissions, multiple applications allowed (separated by a space), see [scope permission list](scope-list.html)
state | optional | string | used for maintaining correspondence with request and callback, given to a third party after the request is successful, used for preventing CSRF attacks, and strongly recommended for use by third parties
skip_confirm | optional | boolean | the signed in user will see a page for switching accounts, if this is not required by the app, you can add `skip_confirm=true`, __Yellow Pages app should be set as true__

#### __Response Data：__

- __SUCCESS__

Once permission request is successful, the server will give the user’s browser a redirect url with code, state, etc.:

```json
http://example.com/example?code=CODE&state=STATE
```

__Response Data Details：__

name | required | type | description
--- | --- | --- | ---
code | yes | string | authorization code for getting `access_token`, only can be used once, and it is valid within 5 minutes
state | optional | string | if the data is passed during the request, the same data will be returned

- __FAILED__

Once permission request is unsuccessful, the server will give the user’s browser a redirect url with `error`, `error_description`, `state`, etc.:

```json
http://example.com/example?error=ERROR&error_description=ERROR_DESCRIPTION&state=STATE
```

__Response Data Details：__

name | required | type | description
--- | --- | --- | ---
error | yes | int | [oauth error code list](error-code/)
error_description | yes | string | simple error description
state | optional | string | if the data is passed during the request, the same data will be returned

### __2. Getting Access Token__

#### __Request URL：__ &emsp; `https://account.xiaomi.com/oauth2/token`
#### __Request Method：__ &emsp; GET
#### __Request Data：__

name | required | type | description
---|--- | --- | ---
client_id | yes | long | allocated ​APP ID​ during app requests
redirect_uri | yes | string | request redirect url, should be the same as the one in allocated APP ID (other data may be different)
client_secret | yes | string | allocated APP Secret during app request
grant_type | yes | string | grant_type is fixed as authorization_code
code | yes | string | Authorization Code acquired in the step above

#### __Response Data：__

- __SUCCESS__

Once the request is accepted, the server will return strings in json format:

1. access_token: access token required to obtain
2. expires_in: access token’s validity period in seconds, see [Access Token Life Cycle](access-token-life-cycle/)
3. refresh_token: refresh token, all apps return this data (valid for 10 years)
4. scope: scope of access token, see [scope permission​ list](scopes/)
5. mac_key: MAC key required for interactions between HTTP and Open API, validity period same as that of access token
6. mac_algorithm: algorithm used for for interactions between HTTP and Open API and digital signatures, currently supports `HmacSha1`
7. openId: user’s openId, can be stored by the website or app for verifying the user when they sign in next time

```json
&&&START&&& {
  "access_token": "access token value",
  "expires_in": 360000,
  "refresh_token": "refresh token value",
  "scope": "scope value",
  "token_type ": "mac",
  "mac_key ": "mac key value",
  "mac_algorithm": " HmacSha1",
  "openId":"2.0XXXXXXXXX"
}
```

__NOTE：__ `&&&​START​&&&`  can be deleted directly, preferably via`replace("&&&START&&&", "")`

- __FAILED__

Once the request is denied, the server will return strings in json format:

1. error：error code, int number, see ​[oauth error code list](error-code.html)
2. error_description：text describe the error

```json
&&&START&&&{
  "error": "error_code",
  "error_description": "error description"
}
```

__NOTE：__ `&&&​START​&&&`  can be deleted directly, preferably via`replace("&&&START&&&", "")`
