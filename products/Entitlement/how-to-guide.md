# Entitlement API How-To Guide

## Introduction

This guide provides step-by-step instructions for common operations using the Entitlement API. Each section includes example requests and responses to help you quickly implement your integration.

## Prerequisites

Before you begin:

1. Register for an API account at [developer.example.com](https://developer.example.com)
2. Obtain authentication credentials (see [Authentication Guide](./authentication.md))
3. Install an HTTP client like cURL, Postman, or use your preferred programming language

## Base URL

All examples use the following base URL:

```
https://api.example.com/v1
```

## Authentication

For simplicity, examples use a placeholder access token. Replace `YOUR_ACCESS_TOKEN` with your actual token:

```bash
# Header format
Authorization: Bearer YOUR_ACCESS_TOKEN
```

## Common Operations

### Retrieving Entitlements

#### List All Entitlements (with pagination)

```bash
curl -X GET "https://api.example.com/v1/entitlements?limit=20&offset=0" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json"
```

Example response:

```json
{
  "limit": 20,
  "offset": 0,
  "count": 2,
  "total": 2,
  "data": [
    {
      "entitlementID": 101,
      "entitlement_group_ID": "GRP123",
      "entitlement_version": "1.0",
      "sku": "PRO-2023",
      "ship_date": "2023-01-15",
      "start_date": "2023-01-20",
      "end_date": "2024-01-19",
      "activation_date": "2023-01-20",
      "status": "FULFILLED",
      "quantity": 5,
      "csp_ID": "CSP456",
      "term": "ANNUAL",
      "product_type": "SOFTWARE",
      "source_ID": "ORD789",
      "is_eval": false,
      "uom": "LICENSE"
    },
    {
      "entitlementID": 102,
      "entitlement_group_ID": "GRP456",
      "entitlement_version": "1.0",
      "sku": "ENT-2023",
      "ship_date": "2023-02-10",
      "start_date": "2023-02-15",
      "end_date": "2024-02-14",
      "activation_date": "2023-02-15",
      "status": "FULFILLED",
      "quantity": 10,
      "csp_ID": "CSP789",
      "term": "ANNUAL",
      "product_type": "SOFTWARE",
      "source_ID": "ORD012",
      "is_eval": false,
      "uom": "LICENSE"
    }
  ]
}
```

#### Get a Specific Entitlement by ID

```bash
curl -X GET "https://api.example.com/v1/entitlements/101" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json"
```

Example response:

```json
{
  "entitlementID": 101,
  "entitlement_group_ID": "GRP123",
  "entitlement_version": "1.0",
  "sku": "PRO-2023",
  "ship_date": "2023-01-15",
  "start_date": "2023-01-20",
  "end_date": "2024-01-19",
  "activation_date": "2023-01-20",
  "status": "FULFILLED",
  "quantity": 5,
  "csp_ID": "CSP456",
  "term": "ANNUAL",
  "product_type": "SOFTWARE",
  "source_ID": "ORD789",
  "is_eval": false,
  "uom": "LICENSE"
}
```

#### Search for Entitlements

```bash
curl -X POST "https://api.example.com/v1/entitlements/search" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "filter": "status=FULFILLED AND end_date>2023-06-01"
  }'
```

Example response:

```json
{
  "limit": 20,
  "offset": 0,
  "count": 1,
  "total": 1,
  "data": [
    {
      "entitlementID": 101,
      "entitlement_group_ID": "GRP123",
      "entitlement_version": "1.0",
      "sku": "PRO-2023",
      "ship_date": "2023-01-15",
      "start_date": "2023-01-20",
      "end_date": "2024-01-19",
      "activation_date": "2023-01-20",
      "status": "FULFILLED",
      "quantity": 5,
      "csp_ID": "CSP456",
      "term": "ANNUAL",
      "product_type": "SOFTWARE",
      "source_ID": "ORD789",
      "is_eval": false,
      "uom": "LICENSE"
    }
  ]
}
```

### Creating Entitlements

#### Create a New Entitlement

```bash
curl -X POST "https://api.example.com/v1/entitlements" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "entitlement_group_ID": "GRP789",
    "entitlement_version": "1.0",
    "sku": "STD-2023",
    "ship_date": "2023-03-10",
    "start_date": "2023-03-15",
    "end_date": "2024-03-14",
    "status": "PENDING",
    "quantity": 2,
    "csp_ID": "CSP123",
    "term": "ANNUAL",
    "product_type": "SOFTWARE",
    "source_ID": "ORD345",
    "is_eval": false,
    "uom": "LICENSE"
  }'
```

Example response:

```json
{
  "entitlementID": 103,
  "entitlement_group_ID": "GRP789",
  "entitlement_version": "1.0",
  "sku": "STD-2023",
  "ship_date": "2023-03-10",
  "start_date": "2023-03-15",
  "end_date": "2024-03-14",
  "status": "PENDING",
  "quantity": 2,
  "csp_ID": "CSP123",
  "term": "ANNUAL",
  "product_type": "SOFTWARE",
  "source_ID": "ORD345",
  "is_eval": false,
  "uom": "LICENSE"
}
```

### Updating Entitlements

#### Update an Entitlement

```bash
curl -X PATCH "https://api.example.com/v1/entitlements/103?updateMask=status,quantity" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "FULFILLED",
    "quantity": 3
  }'
```

Example response:

```json
{
  "entitlementID": 103,
  "entitlement_group_ID": "GRP789",
  "entitlement_version": "1.0",
  "sku": "STD-2023",
  "ship_date": "2023-03-10",
  "start_date": "2023-03-15",
  "end_date": "2024-03-14",
  "activation_date": "2023-03-15",
  "status": "FULFILLED",
  "quantity": 3,
  "csp_ID": "CSP123",
  "term": "ANNUAL",
  "product_type": "SOFTWARE",
  "source_ID": "ORD345",
  "is_eval": false,
  "uom": "LICENSE"
}
```

> **Note**: Always use the `updateMask` query parameter to specify which fields you want to update.

## Advanced Use Cases

### Working with Batch Operations

#### Create Multiple Entitlements

Although the API doesn't have a dedicated batch endpoint, you can efficiently create multiple entitlements using individual requests:

```javascript
// JavaScript example
const entitlements = [
  {
    "entitlement_group_ID": "GRP001",
    "sku": "STD-2023",
    // ... other fields
  },
  {
    "entitlement_group_ID": "GRP002",
    "sku": "PRO-2023",
    // ... other fields
  }
];

async function createEntitlements(entitlements) {
  const results = [];
  
  for (const entitlement of entitlements) {
    try {
      const response = await fetch("https://api.example.com/v1/entitlements", {
        method: "POST",
        headers: {
          "Authorization": "Bearer YOUR_ACCESS_TOKEN",
          "Content-Type": "application/json"
        },
        body: JSON.stringify(entitlement)
      });
      
      const data = await response.json();
      results.push(data);
    } catch (error) {
      console.error("Error creating entitlement:", error);
    }
  }
  
  return results;
}
```

### Managing Entitlement Lifecycle

#### Activation Flow

```javascript
// JavaScript example
async function activateEntitlement(entitlementID) {
  // 1. Retrieve current entitlement
  const getResponse = await fetch(`https://api.example.com/v1/entitlements/${entitlementID}`, {
    method: "GET",
    headers: {
      "Authorization": "Bearer YOUR_ACCESS_TOKEN",
      "Content-Type": "application/json"
    }
  });
  
  const entitlement = await getResponse.json();
  
  // 2. Update with activation date and status
  const today = new Date().toISOString().split('T')[0]; // YYYY-MM-DD
  
  const updateResponse = await fetch(
    `https://api.example.com/v1/entitlements/${entitlementID}?updateMask=status,activation_date`, 
    {
      method: "PATCH",
      headers: {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN",
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        "status": "FULFILLED",
        "activation_date": today
      })
    }
  );
  
  return await updateResponse.json();
}
```

#### Cancellation Flow

```javascript
// JavaScript example
async function cancelEntitlement(entitlementID, reason) {
  const updateResponse = await fetch(
    `https://api.example.com/v1/entitlements/${entitlementID}?updateMask=status`, 
    {
      method: "PATCH",
      headers: {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN",
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        "status": "CANCELED"
      })
    }
  );
  
  // Log cancellation reason (in your system)
  console.log(`Entitlement ${entitlementID} canceled. Reason: ${reason}`);
  
  return await updateResponse.json();
}
```

### Complex Search Examples

#### Finding Soon-to-Expire Entitlements

```bash
curl -X POST "https://api.example.com/v1/entitlements/search" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "filter": "status=FULFILLED AND end_date>2023-05-01 AND end_date<2023-06-01"
  }'
```

#### Finding Entitlements by Product Type and Quantity

```bash
curl -X POST "https://api.example.com/v1/entitlements/search" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "filter": "product_type=SOFTWARE AND quantity>5 AND is_eval=false"
  }'
```

## Error Handling Examples

### Handling Common Errors

```javascript
// JavaScript example
async function handleEntitlementRequest(url, method, body = null) {
  try {
    const options = {
      method: method,
      headers: {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN",
        "Content-Type": "application/json"
      }
    };
    
    if (body) {
      options.body = JSON.stringify(body);
    }
    
    const response = await fetch(url, options);
    
    // Handle different status codes
    if (response.status === 401) {
      // Unauthorized - token may be expired
      console.log("Authentication failed. Refreshing token...");
      // Implement token refresh logic
      return handleEntitlementRequest(url, method, body); // Retry with new token
    }
    
    if (response.status === 404) {
      console.log("Resource not found");
      return null;
    }
    
    if (response.status === 429) {
      // Rate limited - implement backoff
      console.log("Rate limited. Retrying after delay...");
      await new Promise(resolve => setTimeout(resolve, 2000)); // Wait 2 seconds
      return handleEntitlementRequest(url, method, body); // Retry
    }
    
    if (response.status >= 500) {
      console.log("Server error. Please try again later.");
      return null;
    }
    
    return await response.json();
  } catch (error) {
    console.error("Request failed:", error);
    return null;
  }
}
```

## Integration Examples

### Node.js Client

```javascript
// entitlement-client.js
const axios = require('axios');

class EntitlementClient {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.httpClient = axios.create({
      baseURL: baseUrl,
      headers: {
        'Authorization': `Bearer ${accessToken}`,
        'Content-Type': 'application/json'
      }
    });
  }
  
  async getEntitle