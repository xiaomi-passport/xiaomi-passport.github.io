## MAC Signature Verification

### 1. API MAC Signature Algorithm

Some APIs on Mi Dev Platform need to be verified by MAC, the request will be encrypted by MAC algorithm during API calls, and the result will be put in the header of the request.

#### 1.1 MAC calculation

- __Request content standardization__

Standardized request strings are combined by specified request properties according to a certain rule. Here refers to the strings (`\n` is needed after a format string) that combined by nonce, HTTP METHOD, HOST, URI, QUERY with line breaks (i.e. `\n`), in which properties are involved:

Property | Description | Example
--- | --- | ---
nonce | composed of a random number and time stamp, format: `random number:current minutes` | 54897465748976549:21459478 (separated by a colon)
HTTP METHOD | request method during API calls | GET/POST (capitals)
HOST | host during API calls | E.g. open.account.xiaomi.com (without http or https)
URI | path during API calls (begins with `/`) | E.g. /user/profile
QUERY | query strings with request data names arranged by lexicographical order and empty query fields aren't involved in signatures. | clientId=xxx&token=xxx

- __Standardization sample__

```
54897465748976549:21459478\nGET\nopen.account.xiaomi.com\n/user/profile\nclientId=xxx&amp;token=xxx\n
```

__Attention:__ the above line break "`\n`" is only used for an example.

- __MAC Calculation__

With MAC algorithm and MAC key, the client will get the message authentication code by calculating the "standardized request string" in an encryption manner. MAC algorithm contains 2 methods: hmac-sha-1 and hmac-sha-256, and only hmac-sha-1 is supported now.

```
mac = HMAC-SHA1 (mac_key, standardized request string)
```

To explain MAC calculation, here is an example about requesting for the user information API:

> - Request URL: `https://open.account.xiaomi.com/user/profile`
> - Request Data:
> ```
clientId=179887661252608&token=eJxjYGAQydknLLCFsVyIR-DxSqdTnQFGfX4yDAwMjAzxQJIheJfnRTDtvAhMM8SE_2FgWDw7Rg3MYzdUMFIwVjABMplzE5MBClYRuw
```
> - nonce: `2870867952176701445:23282360`
> - HTTP Method: GET

Combine them to get the standardized string:

```
2870867952176701445:23282360\nGET\nopen.account.xiamomi.com\n/user/profile\nclientId=179887661252608&token=eJxjYGAQydknLLCFsVyIR-DxSqdTnQFGfX4yDAwMjAzxQJIheJfnRTDtvAhMM8SE_2FgWDw7Rg3MYzdUMFIwVjABMplzE5MBClYRuw\n
```

Mac Key:  

```
ORhx44qK6Alqf8vt2rGB5f-oPq0
```

Signature output (Base64 encoded):

```
9uvros2WcjMaJ3pH25eQZU9p5pA=
```

#### 1.2 MAC request format

- __Header format__

Verifying and making calls to API by MAC signature need to put related signature info into the request HTTP's header. When the third party sends an API request, authorization fields should be added to the request header. Such authorization fields are as follows:

```
Authorization: MAC access_token="token value",nonce="Random number" ,mac="Signature value"
```

- __Fields__

> - __access\_token__: Access token received by client after the user gives access
> - __nonce__: A random string, is the nonce used when calculating MAC
> - __mac__: The result calculated by the above method (e.g. 9uvros2WcjMaJ3pH25eQZU9p5pA=)

### 2. \_xmSign signature algorithm

\_xmSign field in the user password verification interface is generated based on API MAC signature algorithm. The difference is that mac_key is used when calculating MAC signature, while client_secret is used when calculating \_xmSign.

```
 _xmSign= HmacSha1 (client_secret, callback standardized string)
```

In which,  \_xmNonce and \_xmSign aren't involved in standardization.

Example for \_xmSign signature verification:

Callback provided by the third party: `http://third_url.com/xm`

Callback after Mi verification:

```
http://third_url.com/xm?xmResult=true&xmUserId=1909031&code=93D6A6663C1095587F68281E654D5526&_xmNonce=5964262989045079397%3A24012419&_xmSign=m%2FM1Ia6fOBfKWUbae5G5UXnqh5I%3D
```

In which \_xmNonce=`5964262989045079397:24012419`， \_xmSign=`m/M1Ia6fOBfKWUbae5G5UXnqh5I=`

> - parameter: xmResult=true&xmUserId=1909031&code=93D6A6663C1095587F68281E654D5526 (\_xmNonce and \_xmSign aren't involved in standardization)
> - nonce: 5964262989045079397:24012419
> - HTTP method: GET

Combine them to get the standardized string:

```
5964262989045079397:24012419\nGET\nxiaomi.com\n/\n&code=93D6A6663C1095587F68281E654D5526&xmUserId=1909031&xmResult=true\n
```

client secret:

```
ORhx44qK6Alqf8vt2rGB5f-oPq0
```

Signature output (Base64 encoded):

```
m/M1Ia6fOBfKWUbae5G5UXnqh5I=
```
