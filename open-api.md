## Xiaomi Open API

### 1. Getting User's Profile
##### request url: &emsp; `https://open.account.xiaomi.com/user/profile`
##### request method: &emsp; GET
##### request scope: &emsp; 1
##### request params:

name | required | type | description
---|--- | --- | ---
clientId | yes | long | allocated ​APP ID​ during app requests
token | yes | string | access token received by client, after the user gives access

##### response data:

- __SUCCESS__

```json
{
  "result": "ok",
  "description": "success",
  "data": {
            "miliaoNick": "xiaomi user nickname",
            "unionId": "xiaomi user uniquely identify within all of your APP scope",
            "miliaoIcon": "profile pic url (several resolutions)"
           },
  "code": 0
}
```

- __FAILED__ ([oauth error code list](error-code.html))

```json
{
   "result": "error",
   "description": "error description",
   "code": "error code"
}
```

### 2. Getting User's OpenId
##### request url: &emsp; `https://open.account.xiaomi.com/user/openidV2`
##### request method: &emsp; GET
##### request scope: &emsp; 3
##### request params:

name | required | type | description
---|--- | --- | ---
clientId | yes | long | allocated ​APP ID​ during app requests
token | yes | string | access token received by client, after the user gives access

###### response data:

- __SUCCESS__

```json
{
  "result": "ok",
  "description": "success",
  "data": {
             "openid": "openid"
          },
  "code": 0
}
```

- __FAILED__ ([oauth error code list](error-code.html))

```json
{
   "result": "error",
   "description": "error description",
   "code": "error code"
}
```

### 3. Getting User's Phone Number and Email Address
##### request url: &emsp; `https://open.account.xiaomi.com/user/phoneAndEmail`
##### request method: &emsp; GET
##### request scope: &emsp; 4 and 6
##### request params:

name | required | type | description
---|--- | --- | ---
clientId | yes | long | allocated ​APP ID​ during app requests
token | yes | string | access token received by client, after the user gives access

###### response data:

- __SUCCESS__

```json
{
  "result": "ok",
  "description": "success",
  "data": {
              "phone": "user’s phone number， returned empty in case of abscence",
              "email":  "user’s email address， returned empty in case of abscence"
          },
  "code": 0
}
```

- __FAILED__ ([oauth error code list](error-code.html))

```json
{
   "result": "error",
   "description": "error description",
   "code": "error code"
}
```

### 4. Get User's MiChat Friend List
###### request url: &emsp; `https://open.account.xiaomi.com/user/relation`
###### request method: &emsp; GET
###### request scope: &emsp; 2
###### request params:

name | required | type | description
---|--- | --- | ---
clientId | yes | long | allocated ​APP ID​ during app requests
token | yes | string | access token received by client, after the user gives access

###### response data:

- __SUCCESS__

```json
{
  "result": "ok",
  "description": "success",
  "data": {
            "friends": "friend list"
          },
  "code": 0
}
```

- __FAILED__ ([oauth error code list](error-code.html))

```json
{
   "result": "error",
   "description": "error description",
   "code": "error code"
}
```

### 5. Verificate User's Password
##### request url: &emsp; `https://open.account.xiaomi.com/checkPassword`
##### request method: &emsp; GET
##### request params:

name | required | type | description
---|--- | --- | ---
clientId | yes | long | allocated ​APP ID​ during app requests
xmUserId | yes | long | user id, acquired through `user/profile`
callback | yes | string | full url which starts with http or https and is in the same domain as the redirect url, for notifying about password verification results get request type must be used

##### response data:

If the request was successful, the server will send a callback to the user’s browser and add `xmResult`， `_xmNonce`, `_xmSign`, `code`, `xmUserId`, etc.

name | type | description
--- | --- | ---
xmResult | boolean | true: verification successful, false or no data: verification error
\_xmNonce | string | composed of a random number and timestamp, format: random number:current minutes (from 00:00 January 1, 1970)
\_xmSign | string | response to make sure it wasn’t altered
code | string | new authorization code which can be used by third party for replacing access token (in places with higher security standards access token can be used again to get user id)
xmUserId | long | actual account verified by Xiaomi (not always coming from a third party), may be maliciously tamper with

__NOTE：__ Third parties must verify `_xmSign` which comes in callback, otherwise responsibility for any loss or damage will be borne by the respective third parties.
