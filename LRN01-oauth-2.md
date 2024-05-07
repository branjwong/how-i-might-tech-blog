# OAuth 2.0

- _authorization_ protocol, not authentication
- provides consented access and restricts actions of what the client app can perform on resources on behalf of the user, without sharing the user's credentials or identity
- uses **Access Tokens**, data that represents the authorization to access resources on behalf of the end-user. JWT is often used.

## Roles

- **Resource Owner**: entity that grants access to protected resource, typically the end-user
- **Resource Server**: server hosting protected resources. the API user wants to access. it accepts and validates Access Tokens from the Client and returns appropriate resources to it
- **Client**: application requesting access to a protected resource on behalf of resource owner (e.g. browser), using Access Tokens to do so
- **Authorization Server**: server that authenticates the Resource Owner and issues Access Tokens after getting proper authorization (e.g. Auth0)

## Scopes

- used to specificy the reason for which the access to resources may be granted.
- acceptable scope values are dependent on the Resource Server

## General Flow

- client acquires _its own credentials_, a `client_id` and a `client_secret` from the Authorization Server so it can identify and authenticate itself when requesting an Access Token

1. Client requests authorization (authorization request) from the Authorization Server, supplying the `client_id` and the `client_secret`, providing scopes and a redirect URI to send the Access Token or Authorization Code to
2. Authrozation Server authenticates the Client and verifies th at the requested scopes are permitted
3. Resource Owner interacts with Authorization Server to grant access
4. Authorization Server redirects back to the Client with Authorization Code or Access Token (depends on grant type)
5. Client uses Access Token to request access to the resource from Resource Server

## Grant Types, specific flows

**Grants** are the set of steps a Client has to perform to get resource access authorization. 0Auth 2.0 provides several grant types to address different scenarios.

- Authorization Code Flow: used by server-side web apps, or mobile apps with PKCE technique
- Implicit Flow with Form Post: used by SPAs executing on the user's browser
- Resource Owner Password Flow: used by highly-trusted apps
- Client Credentials Flow: used for M2M

### Authorization Code Grant

- deprecated in OAuth 2.1
- Authorization Server returns a single-use Authorization Code to the client, exchanged for Access Token
- best for traditional web apps where exchange can't securely happen on the client side (since it's insecure to store `client_secret` in the app), but can happen on the server side
- _insecure for SPA and mobile apps_ -> see Authorization Code with PKCE grant

### Authorization Code Grant with Proof Key for Code Exchange (PKCE)

- added to in OAuth 2.1
- additional step to generate a cryptographically-random `code_verifier` and `code_challenge` _= f ( code_verifier )_
- even if malicious attackers intercept the Authorization Code, they cannot exchange it for a token without the code_verifier

![diagram](img/auth-sequence-auth-code-pkce.png)

### Implicit Grant

- deprecated in OAuth 2.1
- simplified flow
- Access Token returned directly to client as a response to a form post

### Resource Owner Credentials Grant Type

- deprecated in OAuth 2.1
- client acquires resource owner's credentials, which are passed to the Authorization Server
- limited to Clients that are completely trusted
- advantage that no redirect to the Authorization Server is involved

### Client Credentials Grant Type

- non-interactive/automated, M2M
- authenticated using client id and secret

### Refresh Token Grant

- exchanging a **Refresh Token** for a new Access Token

## Security Considerations

https://datatracker.ietf.org/doc/html/rfc6749#section-10

# Resources

https://auth0.com/intro-to-iam/what-is-oauth-2
https://auth0.com/docs/authenticate/protocols/oauth
https://book.hacktricks.xyz/pentesting-web/oauth-to-account-takeover
https://blog.dixitaditya.com/oauth-account-takeover
https://oauth.net/2/
https://oauth.net/2.1/
https://aaronparecki.com/2019/12/12/21/its-time-for-oauth-2-dot-1
