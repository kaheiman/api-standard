# Authentication

## API Authentication Scope

It is not always clear from the documentation what kind of authentication is required for a give API call.  For example the Data Management API for creating a new bucket says the authentication context is "app only." The docs for "app only" say "The endpoint accepts either a two-legged or three-legged token." However, when trying to use the API with a 3-legged user token, the API returns an error saying you must use two-legged authentication.  This should be clear from the documentation.

We should leverage the "Security Scheme Object" from OpenAPI 2.0 to describe the security requirements for each API, and ensure accurate documentation appears on our public site.

Having good client SDKs that automatically handle two-legged authentication and scopes when required would help.

We should have a consistent naming convention for scopes, and share scopes where possible.

