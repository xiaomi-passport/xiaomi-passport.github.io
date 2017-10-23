## Refresh access token

__Attention__: A refresh token can only refresh the access token once, and we will issue a new refresh token after issuing new access token.

### 1. Refresh access token by refresh token
##### request url: &emsp;`https://account.xiaomi.com/oauth2/token`
##### request method: &emsp;GET
##### request params:

name | required | type | description
---|--- | --- | ---
client_id | yes | long | allocated ​APP ID​ during app requests
redirect_uri | yes | string | request redirect url, should be the same as the one in allocated APP ID (other data may be different)
client_secret | yes | string | allocated APP Secret during app request
grant_type | yes | string | grant_type = refresh_token
refresh_token | yes | string | issued by server when request authorization by authorization code model

##### response data:

- __SUCCESS__

Once the request is accepted, the server will return strings in json format:

1. __access_token__: access token required to obtain
2. __expires_in__: access token’s validity period in seconds, see [Token Life Cycle](token-life-cycle.html)
3. __refresh_token__: refresh token, all apps return this data (valid for 10 years)
4. __scope__: scope of access token, see [scope permission​ list](scope-list.html)
5. __mac_key__: MAC key required for interactions between HTTP and Open API, validity period same as that of access token
6. __mac_algorithm__: algorithm used for for interactions between HTTP and Open API and digital signatures, currently supports `HmacSha1`
7. __openId__: user’s openId, can be stored by the website or app for verifying the user when they sign in next time

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
