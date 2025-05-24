# API Reference

## Authentication

All API endpoints require authentication using Laravel Sanctum tokens.

### Get Authentication Token

```http
POST /api/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password"
}
```

Response:
```json
{
  "token": "1|xxxxxxxxxxxxxxxxxxx",
  "user": {
    "id": 1,
    "name": "User Name",
    "email": "user@example.com"
  }
}
```

Use the token in subsequent requests:
```http
Authorization: Bearer 1|xxxxxxxxxxxxxxxxxxx
```

## Identities

### List All Identities

```http
GET /api/identities
```

Response:
```json
{
  "data": [
    {
      "id": 1,
      "name": "github-backend",
      "display_name": "Backend Developer",
      "description": "Full access to backend repositories",
      "role_type": "backend",
      "is_active": true,
      "created_at": "2024-05-24T10:00:00Z",
      "updated_at": "2024-05-24T10:00:00Z"
    }
  ],
  "meta": {
    "current_page": 1,
    "total": 5
  }
}
```

### Create Identity

```http
POST /api/identities
Content-Type: application/json

{
  "name": "github-frontend",
  "display_name": "Frontend Developer",
  "description": "Access to frontend repositories",
  "role_type": "frontend"
}
```

### Get Identity

```http
GET /api/identities/{id}
```

### Update Identity

```http
PUT /api/identities/{id}
Content-Type: application/json

{
  "display_name": "Senior Frontend Developer",
  "is_active": true
}
```

### Delete Identity

```http
DELETE /api/identities/{id}
```

## Tokens

### List Tokens for Identity

```http
GET /api/identities/{identity_id}/tokens
```

Response:
```json
{
  "data": [
    {
      "id": 1,
      "identity_id": 1,
      "token_name": "Main Token",
      "token_preview": "ghp_xxxx...xxxx",
      "expires_at": "2025-05-24T00:00:00Z",
      "last_used_at": "2024-05-24T10:00:00Z",
      "is_active": true,
      "permissions": [
        "repo",
        "workflow"
      ]
    }
  ]
}
```

### Create Token

```http
POST /api/identities/{identity_id}/tokens
Content-Type: application/json

{
  "token_name": "Production Token",
  "token": "ghp_xxxxxxxxxxxxxxxxxx",
  "expires_at": "2025-12-31"
}
```

### Validate Token

```http
POST /api/tokens/{id}/validate
```

Response:
```json
{
  "valid": true,
  "permissions": [
    "repo",
    "workflow",
    "admin:repo_hook"
  ],
  "rate_limit": {
    "limit": 5000,
    "remaining": 4999,
    "reset": "2024-05-24T11:00:00Z"
  }
}
```

### Update Token

```http
PUT /api/tokens/{id}
Content-Type: application/json

{
  "token_name": "Updated Token Name",
  "is_active": false
}
```

### Delete Token

```http
DELETE /api/tokens/{id}
```

## Permissions

### List Permissions for Identity

```http
GET /api/identities/{identity_id}/permissions
```

### Set Permissions

```http
POST /api/identities/{identity_id}/permissions
Content-Type: application/json

{
  "permissions": [
    {
      "permission_type": "repo",
      "permission_level": "write"
    },
    {
      "permission_type": "workflow",
      "permission_level": "write"
    },
    {
      "permission_type": "packages",
      "permission_level": "read",
      "repository_filter": "frontend-*"
    }
  ]
}
```

## Configurations

### List Configurations

```http
GET /api/configurations
```

### Generate Configuration

```http
POST /api/configurations/generate
Content-Type: application/json

{
  "name": "Development Config",
  "identity_ids": [1, 2, 3],
  "environment": "development"
}
```

Response:
```json
{
  "id": 1,
  "name": "Development Config",
  "config": {
    "mcpServers": {
      "github-backend": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_PERSONAL_ACCESS_TOKEN": "encrypted_token_here"
        }
      },
      "github-frontend": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_PERSONAL_ACCESS_TOKEN": "encrypted_token_here"
        }
      }
    }
  },
  "identities_included": [1, 2],
  "environment": "development",
  "created_at": "2024-05-24T10:00:00Z"
}
```

### Get Configuration

```http
GET /api/configurations/{id}
```

### Export Configuration

```http
GET /api/configurations/{id}/export
```

Response:
```json
{
  "mcpServers": {
    "github-backend": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "actual_token_here"
      }
    }
  }
}
```

### Download Configuration File

```http
GET /api/configurations/{id}/download
```

Returns the configuration as a downloadable JSON file.

## Audit Logs

### Get Audit Logs

```http
GET /api/audit-logs?model_type=token&action=create
```

Response:
```json
{
  "data": [
    {
      "id": 1,
      "user_id": 1,
      "action": "create",
      "model_type": "App\\Models\\Token",
      "model_id": 5,
      "changes": {
        "token_name": "New Token",
        "identity_id": 1
      },
      "ip_address": "192.168.1.1",
      "user_agent": "Mozilla/5.0...",
      "created_at": "2024-05-24T10:00:00Z"
    }
  ]
}
```

## Error Responses

All endpoints return standard error responses:

### Validation Error (422)
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": [
      "The name field is required."
    ]
  }
}
```

### Not Found (404)
```json
{
  "message": "Resource not found."
}
```

### Unauthorized (401)
```json
{
  "message": "Unauthenticated."
}
```

### Server Error (500)
```json
{
  "message": "Server error occurred.",
  "error": "Error details in development mode only"
}
```

## Rate Limiting

API endpoints are rate limited:
- Authentication endpoints: 5 requests per minute
- Other endpoints: 60 requests per minute

Rate limit headers:
```http
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 59
X-RateLimit-Reset: 1716540000
```