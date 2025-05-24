# Setup Guide

## Prerequisites

### System Requirements
- PHP 8.1 or higher
- Composer 2.x
- Node.js 16+ and npm
- MySQL 8.0+ or PostgreSQL 13+
- Redis (optional, for caching)

### Required PHP Extensions
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
- Ctype PHP Extension
- JSON PHP Extension
- BCMath PHP Extension

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/AtumG/github-mcp-manager.git
cd github-mcp-manager
```

### 2. Install Dependencies

```bash
# Install PHP dependencies
composer install

# Install Node dependencies
npm install

# Build assets
npm run build
```

### 3. Environment Configuration

```bash
# Copy the example environment file
cp .env.example .env

# Generate application key
php artisan key:generate
```

### 4. Configure Database

Edit `.env` file with your database credentials:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=github_mcp_manager
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### 5. Run Migrations

```bash
# Create database tables
php artisan migrate

# Seed with default data (optional)
php artisan db:seed
```

### 6. Configure Encryption

For additional security, you can set a custom encryption key for tokens:

```env
# In .env file
TOKEN_ENCRYPTION_KEY=your-32-character-encryption-key
```

### 7. Set Up Queue Worker (Optional)

For background jobs like token validation:

```bash
# Configure queue driver in .env
QUEUE_CONNECTION=redis

# Run queue worker
php artisan queue:work
```

### 8. Configure GitHub API Access

Add your GitHub API credentials for validation:

```env
GITHUB_CLIENT_ID=your_client_id
GITHUB_CLIENT_SECRET=your_client_secret
GITHUB_VALIDATION_TOKEN=your_validation_token
```

## Running the Application

### Development

```bash
# Start Laravel development server
php artisan serve

# In another terminal, start Vite for hot-reloading
npm run dev
```

Access the application at `http://localhost:8000`

### Production

```bash
# Optimize for production
php artisan config:cache
php artisan route:cache
php artisan view:cache

# Build production assets
npm run build
```

## First Time Setup

### 1. Create Admin User

```bash
php artisan tinker
```

```php
$user = new App\Models\User();
$user->name = 'Admin';
$user->email = 'admin@example.com';
$user->password = Hash::make('password');
$user->save();
```

### 2. Add First Identity

1. Navigate to `/identities`
2. Click "Add New Identity"
3. Fill in:
   - Name: `github-backend`
   - Display Name: `Backend Developer`
   - Role Type: `backend`
   - Description: `Full access to backend repositories`

### 3. Add GitHub Token

1. Go to GitHub Settings > Developer Settings > Personal Access Tokens
2. Generate a new token with appropriate permissions
3. In the app, navigate to the identity and click "Add Token"
4. Paste the token and save

### 4. Generate Configuration

1. Navigate to `/configurations`
2. Click "Generate New Configuration"
3. Select identities to include
4. Click "Generate"
5. Download the configuration file

### 5. Apply to Claude Desktop

Copy the generated configuration to:
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

## Troubleshooting

### Common Issues

1. **Database Connection Error**
   - Verify database credentials in `.env`
   - Ensure database server is running
   - Check if database exists

2. **Token Encryption Error**
   - Regenerate app key: `php artisan key:generate`
   - Clear config cache: `php artisan config:clear`

3. **Asset Compilation Error**
   - Delete `node_modules` and reinstall: `npm install`
   - Clear npm cache: `npm cache clean --force`

4. **Permission Denied Errors**
   - Set proper permissions: `chmod -R 755 storage bootstrap/cache`

## Security Checklist

- [ ] Change default admin credentials
- [ ] Set strong database password
- [ ] Configure HTTPS in production
- [ ] Enable two-factor authentication
- [ ] Regularly rotate encryption keys
- [ ] Set up automated backups
- [ ] Configure firewall rules
- [ ] Enable audit logging
- [ ] Review token permissions regularly
- [ ] Monitor for suspicious activity