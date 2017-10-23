## OAuth Error Code List

| __Error Code__ | __Error Message__ | __Description__ |
|:--------------------|:--------------------|:--------------------|
|96001| OA_CLIENT_NOT_EXISTS | client doesn’t exist, or account services not enabled |
|96002| OA_INVALID_REQUEST | required data missing in request, or data format invalid |
|96003| OA_INVALID_CLIENT | client_id and client_secret don’t match (client_secret correspond to app_secret)，or request without url encode |
|96004| OA_INVALID_GRANT | grant invalid, grant type can be either token or code |
|96005| OA_UNAUTHORIZED_CLIENT | client has no permissions to use this request, application for permissions on an open platform required |
|96006| OA_UNSUPPORTED_GRANT_TYPE | grant type not supported by server |
|96007| OA_INVALID_SCOPE | scope invalid or unknown, or format invalid, see ​[scope permission​ list](scope-list.html) |
|96008| OA_INVALID_TOKEN | access token error or expired |
|96009| OA_INVALID_REFRESHTOKEN | refresh token invalid or expired |
|96010| OA_INVALID_REDIRECT_URI | redirect url doesn’t match with the previous registration url, or isn’t a valid url, redirect url should be the same as the one entered when enabling account services |
|96011| OA_UNSUPPORTED_RESPONSE_TYPE | response type unsupported by server |
|96012| OA_ACCESS_DENIED | user or server denied access, user may have canceled request, or MAC signature of the request can’t be verified |
|96013| OA_INVALID_CODE | code invalid or expired, authorization code can be used once, and it’s valid within 5 minutes |
