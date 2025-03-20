# Entitlement API Authentication Guide

## Introduction

This document describes the authentication mechanisms supported by the Entitlement API, including how to obtain credentials, authenticate requests, and manage tokens securely.

## Authentication Methods

The Entitlement API supports the following authentication methods:

### OAuth 2.0 

OAuth 2.0 is the recommended authentication method for production environments.

#### Obtaining OAuth Credentials

1. Register your application in the Developer Portal at https://developer.example.com
2. Create a new OAuth 2.0 client
3. Note your `client_id` and `client_secret`
4. Configure your redirect URI

#### Authorization Code Flow

For server-side applications:

1. Redirect users to the authorization endpoint:

```
https://auth.example.com/authorize?
  response_type=code&
  client_id=YOUR_CLIENT_ID&
  redirect_uri=YOUR_REDIRECT_URI&
  scope=entitlements.read entitlements.write&
  state=RANDOM_STATE_VALUE
```

2. After user authorization, exchange the code for an access token:

```
POST https://auth.example.com/token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&
code=AUTHORIZATION_CODE&
client_id=YOUR_CLIENT_ID&
client_secret=YOUR_CLIENT_SECRET&
redirect_uri=YOUR_REDIRECT_URI
```

3. Use the access token to authenticate API requests:

```
GET https://api.example.com/v1/entitlements
Authorization: Bearer YOUR_ACCESS_TOKEN
```

#### Client Credentials Flow

For service-to-service integrations:

```
POST https://auth.example.com/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&
client_id=YOUR_CLIENT_ID&
client_secret=YOUR_CLIENT_SECRET&
scope=entitlements.read entitlements.write
```

### API Keys

For testing and development purposes, you can use API keys.

#### Obtaining API Keys

1. Log in to the Developer Portal
2. Navigate to the API Keys section
3. Generate a new API key
4. Copy the key securely; it won't be displayed again

#### Using API Keys

Include the API key in the request header:

```
GET https://api.example.com/v1/entitlements
X-API-Key: YOUR_API_KEY
```

> **Note**: API keys provide less security than OAuth 2.0 and should be used primarily for development and testing.

## Token Management

### Access Token Lifecycle

- **Token Expiration**: OAuth access tokens typically expire after 1 hour
- **Refresh Tokens**: Use refresh tokens to obtain new access tokens without user interaction
- **Token Revocation**: Revoke tokens that are no longer needed or may be compromised

### Using Refresh Tokens

When an access token expires, use the refresh token to obtain a new one:

```
POST https://auth.example.com/token
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&
refresh_token=YOUR_REFRESH_TOKEN&
client_id=YOUR_CLIENT_ID&
client_secret=YOUR_CLIENT_SECRET
```

### Token Revocation

To revoke a token:

```
POST https://auth.example.com/revoke
Content-Type: application/x-www-form-urlencoded

token=TOKEN_TO_REVOKE&
token_type_hint=access_token&
client_id=YOUR_CLIENT_ID&
client_secret=YOUR_CLIENT_SECRET
```

## Scopes

The Entitlement API uses OAuth scopes to control access levels:

| Scope | Description |
|-------|-------------|
| `entitlements.read` | Read-only access to entitlement data |
| `entitlements.write` | Ability to create and modify entitlements |
| `entitlements.admin` | Administrative access (restricted) |

Request only the scopes your application needs to follow the principle of least privilege.

## Security Best Practices

### Credential Storage

- Never store client secrets or access tokens in client-side code
- Use environment variables or secure credential storage systems
- Encrypt credentials at rest
- Rotate client secrets periodically

### Secure Communication

- Always use HTTPS for all API communication
- Validate SSL/TLS certificates
- Use certificate pinning in mobile applications

### Token Handling

- Store tokens securely
- Never transmit tokens over insecure channels
- Implement token refresh logic
- Handle token expiration gracefully
- Revoke tokens when they're no longer needed

### User Consent

- Clearly explain to users what permissions your application is requesting
- Only request scopes that are necessary for your application's functionality
- Provide a way for users to revoke access

## Troubleshooting Authentication Issues

### Common Error Codes

| HTTP Status | Error | Description | Resolution |
|-------------|-------|-------------|------------|
| 401 | `invalid_token` | The access token is invalid or expired | Refresh the access token |
| 401 | `invalid_client` | Client authentication failed | Verify client credentials |
| 403 | `insufficient_scope` | The token doesn't have the required scopes | Request the necessary scopes |
| 429 | `too_many_requests` | Rate limit exceeded | Implement backoff strategy |

### Debugging Tips

1. Verify your credentials are correct and not expired
2. Check that you're using the correct authentication method
3. Ensure all required parameters are included in your requests
4. Verify that your requested scopes match your API operations
5. Check for clock skew if you're experiencing token validation issues

## Contact Support

If you continue to experience authentication issues after trying the troubleshooting steps, contact our support team:

- Email: api-support@example.com
- Developer Portal: https://developer.example.com/support
- Support Hours: Monday-Friday, 9 AM - 5 PM EST
