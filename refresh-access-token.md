### 1. Refresh access token by refresh token
##### request url &emsp;`https://account.xiaomi.com/oauth2/token`
##### request method： &emsp;GET
##### request params

name | required | type | description
---|--- | --- | ---
client_id | yes | long | allocated ​APP ID​ during app requests
redirect_uri | yes | string | request redirect url, should be the same as the one in allocated APP ID (other data may be different)
client_secret | yes | string | allocated APP Secret during app request
grant_type | yes | string | grant_type = refresh_token
refresh_token | yes | string | issued by server when request authorization by authorization code model

##### response data

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
&&&START&&&{
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

__NOTE：__ `&&&​START​&&&`  can be deleted directly, preferably via `replace("&&&START&&&", "")`

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

__NOTE：__ `&&&​START​&&&`  can be deleted directly, preferably via `replace("&&&START&&&", "")`
