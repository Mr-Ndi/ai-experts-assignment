# EXPLANATION.md

## What was the bug?

`Client.request()` only refreshed tokens when `oauth2_token` was missing or an expired `OAuth2Token`. When `oauth2_token` was a plain dict, the refresh path did not run, and the request was sent without an `Authorization` header.

## Why did it happen?

The refresh check used a narrow condition that treated non-`OAuth2Token` values as valid, even though they could not be used to build an auth header.

## Why does your fix solve it?

The refresh condition now treats missing tokens, non-`OAuth2Token` values, and expired tokens as requiring a refresh. That ensures a valid `OAuth2Token` exists before adding the `Authorization` header.
