Tip: If your app has server side, we recommend you to chose [authorization code grant type](authorization-code.html).

### __1. Getting Access Token__
#### __Request URL：__ &emsp; `https://account.xiaomi.com/oauth2/authorize`
#### __Request Method：__ &emsp; GET
#### __Request Data：__

name | required | type | description
---|--- | --- | ---
client_id | yes | long | allocated ​APP ID​ during app requests
redirect_uri | yes | string | request redirect url, should be the same as the one in allocated APP ID (other data may be different)
response_type | yes | string | description of response type, __response_type=token__
scope | optional | string | data required for getting scope permissions, multiple applications allowed (separated by a space), see [scope permission list](scope-list.html)
state | optional | string | used for maintaining correspondence with request and callback, given to a third party after the request is successful, used for preventing CSRF attacks, and strongly recommended for use by third parties
skip_confirm | optional | boolean | the signed in user will see a page for switching accounts, if this is not required by the app, you can add `skip_confirm=true`, __Yellow Pages app should be set as true__

#### __Response Data：__

- __SUCCESS__

Once permission request is successful, the server will give the user’s browser a redirect url with `access_token`, `token_type`, `expires_in`, `mac_algorithm`, `mac_key`, `state`, etc.:

```json
http://example.com/example#access_token=TOKEN&token_type=mac&expires_in=7776000&mac_algorithm=HmacSHA1&mac_key=MACKEY&scope=SCOPE
```

__Response Data Details：__

name | required | type | description
---|--- | --- | ---
access_token | yes | string | required access token
expires_in | yes | string | validity period of access token in seconds, see [Access Token Life Cycle](token-life-cycle.html)
scope | yes | string | scope of access token, see ​[scope permissions​ list](scope-list.html)
mac_key | yes | string | MAC key required for interactions between HTTP and Open API, validity period same as that of access token
mac_algorithm | yes | string | Algorithm used for for interactions between HTTP and Open API and digital signatures, currently supports `HmacSha1`
state | optional | string | If the data is passed during the request, the same data will be returned

- __FAILED__

Once permission request is unsuccessful, the server will give the user’s browser a redirect url with `error`, `error_description`, `state`, etc.:

```json
http://example.com/example?error=ERROR&error_description=ERROR_DESCRIPTION&state=STATE
```

name | required | type | description
---|--- | --- | ---
error | yes | int | [oauth error code list](error-code.html)
error_description | yes | string | simple error description
state | optional | string | if the data is passed during the request, the same data will be returned
