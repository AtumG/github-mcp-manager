# GitHub MCP Manager - Project Structure

This document outlines the complete project structure for the Laravel application that manages GitHub MCP configurations.

## Directory Structure

```
github-mcp-manager/
├── app/
│   ├── Console/
│   │   └── Kernel.php
│   ├── Exceptions/
│   │   └── Handler.php
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── Api/
│   │   │   │   ├── AuthController.php
│   │   │   │   ├── ConfigurationController.php
│   │   │   │   ├── IdentityController.php
│   │   │   │   ├── PermissionController.php
│   │   │   │   └── TokenController.php
│   │   │   ├── Auth/
│   │   │   ├── ConfigurationController.php
│   │   │   ├── DashboardController.php
│   │   │   ├── IdentityController.php
│   │   │   └── TokenController.php
│   │   ├── Middleware/
│   │   │   ├── EnsureTokenIsValid.php
│   │   │   └── LogActivity.php
│   │   └── Requests/
│   │       ├── StoreIdentityRequest.php
│   │       ├── StoreTokenRequest.php
│   │       └── GenerateConfigRequest.php
│   ├── Models/
│   │   ├── AuditLog.php
│   │   ├── Configuration.php
│   │   ├── Identity.php
│   │   ├── Permission.php
│   │   ├── Token.php
│   │   └── User.php
│   ├── Providers/
│   │   └── AppServiceProvider.php
│   └── Services/
│       ├── ConfigGeneratorService.php
│       ├── GitHubValidationService.php
│       └── TokenEncryptionService.php
├── bootstrap/
│   ├── app.php
│   └── cache/
├── config/
│   ├── app.php
│   ├── auth.php
│   ├── database.php
│   └── github-mcp.php
├── database/
│   ├── factories/
│   │   ├── IdentityFactory.php
│   │   └── UserFactory.php
│   ├── migrations/
│   │   ├── 2024_05_24_000001_create_users_table.php
│   │   ├── 2024_05_24_000002_create_identities_table.php
│   │   ├── 2024_05_24_000003_create_tokens_table.php
│   │   ├── 2024_05_24_000004_create_permissions_table.php
│   │   ├── 2024_05_24_000005_create_configurations_table.php
│   │   └── 2024_05_24_000006_create_audit_logs_table.php
│   └── seeders/
│       ├── DatabaseSeeder.php
│       └── DefaultIdentitiesSeeder.php
├── docker/
│   └── nginx/
│       └── conf.d/
│           └── app.conf
├── docs/
│   ├── API_REFERENCE.md
│   ├── ARCHITECTURE.md
│   └── SETUP_GUIDE.md
├── public/
│   ├── index.php
│   ├── robots.txt
│   └── .htaccess
├── resources/
│   ├── css/
│   │   └── app.css
│   ├── js/
│   │   ├── app.js
│   │   └── components/
│   │       ├── IdentityManager.vue
│   │       ├── TokenForm.vue
│   │       └── ConfigGenerator.vue
│   └── views/
│       ├── auth/
│       │   ├── login.blade.php
│       │   └── register.blade.php
│       ├── components/
│       │   ├── alert.blade.php
│       │   └── modal.blade.php
│       ├── configurations/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   └── show.blade.php
│       ├── dashboard.blade.php
│       ├── identities/
│       │   ├── index.blade.php
│       │   ├── create.blade.php
│       │   ├── edit.blade.php
│       │   └── show.blade.php
│       ├── layouts/
│       │   ├── app.blade.php
│       │   └── guest.blade.php
│       └── tokens/
│           ├── create.blade.php
│           └── edit.blade.php
├── routes/
│   ├── api.php
│   ├── auth.php
│   └── web.php
├── storage/
│   ├── app/
│   ├── framework/
│   └── logs/
├── tests/
│   ├── Feature/
│   │   ├── Api/
│   │   │   ├── IdentityApiTest.php
│   │   │   └── TokenApiTest.php
│   │   ├── ConfigurationTest.php
│   │   └── TokenEncryptionTest.php
│   └── Unit/
│       ├── ConfigGeneratorServiceTest.php
│       └── GitHubValidationServiceTest.php
├── .env.example
├── .gitignore
├── artisan
├── composer.json
├── docker-compose.yml
├── Dockerfile
├── package.json
├── phpunit.xml
├── README.md
├── tailwind.config.js
└── vite.config.js
```

## Key Components

### Models
- **Identity**: Represents a GitHub developer persona (backend, frontend, etc.)
- **Token**: Stores encrypted GitHub Personal Access Tokens
- **Permission**: Defines granular permissions for each identity
- **Configuration**: Stores generated MCP configurations
- **AuditLog**: Tracks all sensitive operations

### Services
- **TokenEncryptionService**: Handles secure token storage
- **ConfigGeneratorService**: Generates Claude Desktop configurations
- **GitHubValidationService**: Validates tokens with GitHub API

### Controllers
- **Web Controllers**: Handle UI interactions
- **API Controllers**: Provide RESTful API endpoints

### Views
- Built with Blade templates
- Styled with Tailwind CSS
- Optional Vue.js components for interactive features

## Database Schema

The application uses the following main tables:
- `users` - Application users
- `identities` - Developer personas
- `tokens` - Encrypted GitHub tokens
- `permissions` - Identity permissions
- `configurations` - Generated configurations
- `audit_logs` - Activity tracking

## Configuration Files

### Laravel Configuration
- `config/github-mcp.php` - Application-specific settings
- `.env` - Environment variables

### Docker Configuration
- `docker-compose.yml` - Multi-container setup
- `Dockerfile` - PHP application container

## Testing

The application includes comprehensive tests:
- Feature tests for API endpoints
- Unit tests for services
- Integration tests for token validation

## Security Features

1. **Encryption**: All tokens encrypted at rest
2. **Authentication**: Laravel Sanctum for API auth
3. **Authorization**: Role-based access control
4. **Auditing**: Complete audit trail
5. **Validation**: Token validation with GitHub API

## Frontend Assets

- **CSS**: Tailwind CSS for styling
- **JavaScript**: Alpine.js for simple interactions
- **Vue.js**: Optional components for complex features
- **Build Tool**: Vite for asset compilation

## Development Workflow

1. Clone repository
2. Install dependencies
3. Configure environment
4. Run migrations
5. Start development server
6. Make changes
7. Run tests
8. Build for production

This structure provides a solid foundation for managing GitHub MCP configurations with security, scalability, and maintainability in mind.