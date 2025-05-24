# Architecture Overview

## System Design

The GitHub MCP Manager follows a modular Laravel architecture with clear separation of concerns.

## Core Components

### 1. Identity Management
- **Purpose**: Manage different developer personas (Backend, Frontend, PR Reviewer, etc.)
- **Features**: 
  - CRUD operations for identities
  - Role-based permission templates
  - Identity validation

### 2. Token Management
- **Purpose**: Securely store and manage GitHub Personal Access Tokens
- **Features**:
  - AES-256 encryption for tokens
  - Token validation with GitHub API
  - Expiration tracking
  - Audit logging

### 3. Configuration Generator
- **Purpose**: Generate Claude Desktop MCP configuration files
- **Features**:
  - JSON configuration builder
  - Multi-identity support
  - Environment-specific configs
  - Validation before export

### 4. Permission System
- **Purpose**: Define granular GitHub permissions per identity
- **Features**:
  - Repository-level access control
  - Permission templates by role
  - Custom permission sets

## Database Schema

### identities
- id
- name (e.g., 'github-backend')
- display_name (e.g., 'Backend Developer')
- description
- role_type (backend|frontend|reviewer|architect|product)
- is_active
- created_at
- updated_at

### tokens
- id
- identity_id
- encrypted_token
- token_name
- expires_at
- last_used_at
- is_active
- created_at
- updated_at

### permissions
- id
- identity_id
- permission_type (repo|workflow|admin|packages|etc.)
- permission_level (read|write|admin)
- repository_filter (optional, for limiting to specific repos)
- created_at
- updated_at

### configurations
- id
- name
- config_json
- identities_included (JSON array)
- environment (development|staging|production)
- created_at
- updated_at

### audit_logs
- id
- user_id
- action
- model_type
- model_id
- changes (JSON)
- ip_address
- user_agent
- created_at

## Service Layer

### TokenEncryptionService
```php
class TokenEncryptionService
{
    public function encrypt(string $token): string;
    public function decrypt(string $encryptedToken): string;
    public function validateToken(string $token): bool;
}
```

### ConfigGeneratorService
```php
class ConfigGeneratorService
{
    public function generate(array $identityIds): array;
    public function validate(array $config): bool;
    public function export(array $config, string $format = 'json'): string;
}
```

### GitHubValidationService
```php
class GitHubValidationService
{
    public function validateToken(string $token): bool;
    public function getTokenPermissions(string $token): array;
    public function checkTokenExpiration(string $token): ?Carbon;
}
```

## Security Considerations

1. **Token Storage**: All tokens encrypted using Laravel's encryption
2. **Access Control**: Middleware for authentication and authorization
3. **Audit Trail**: All sensitive operations logged
4. **Environment Isolation**: Separate configs for different environments
5. **API Rate Limiting**: Prevent abuse of GitHub API validation

## API Endpoints

### Identities
- GET /api/identities - List all identities
- POST /api/identities - Create new identity
- GET /api/identities/{id} - Get specific identity
- PUT /api/identities/{id} - Update identity
- DELETE /api/identities/{id} - Delete identity

### Tokens
- GET /api/identities/{id}/tokens - List tokens for identity
- POST /api/identities/{id}/tokens - Add new token
- PUT /api/tokens/{id} - Update token
- DELETE /api/tokens/{id} - Delete token
- POST /api/tokens/{id}/validate - Validate token with GitHub

### Configurations
- GET /api/configurations - List all configurations
- POST /api/configurations/generate - Generate new configuration
- GET /api/configurations/{id} - Get specific configuration
- GET /api/configurations/{id}/export - Export configuration file

## Future Enhancements

1. **GitHub App Integration**: Support for GitHub Apps instead of PATs
2. **Team Management**: Multi-user support with team features
3. **Webhook Integration**: Auto-update on token expiration
4. **CLI Tool**: Command-line interface for config management
5. **Backup/Restore**: Configuration backup and restore functionality