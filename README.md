# GitHub MCP Manager

A Laravel application to manage GitHub MCP (Model Context Protocol) configurations for multiple developer identities.

## Overview

This application allows you to:
- Centrally manage multiple GitHub identities (Backend, Frontend, PR Reviewer, Architect, Product Owner)
- Store and manage GitHub Personal Access Tokens securely
- Generate Claude Desktop configuration files
- Validate MCP configurations
- Export configurations for different environments

## Flow

1. **Central Project** - Main Laravel application
2. **Add Identities** - Create different developer personas
3. **Configure** - Set up GitHub tokens and permissions
4. **Generate** - Create Claude Desktop config files
5. **Export** - Get ready-to-use MCP configurations

## Installation

### Prerequisites
- PHP 8.1+
- Composer
- Node.js 16+
- MySQL/PostgreSQL

### Setup

```bash
# Clone the repository
git clone https://github.com/AtumG/github-mcp-manager.git
cd github-mcp-manager

# Install dependencies
composer install
npm install

# Copy environment file
cp .env.example .env

# Generate application key
php artisan key:generate

# Configure database in .env file
# Then run migrations
php artisan migrate

# Install MCP tools
npm install -g @modelcontextprotocol/server-github

# Start development server
php artisan serve
npm run dev
```

## Usage

1. **Add Identities**: Navigate to `/identities` to create new developer personas
2. **Configure Tokens**: Add GitHub Personal Access Tokens for each identity
3. **Set Permissions**: Define repository access and permissions per identity
4. **Generate Config**: Export Claude Desktop configuration files
5. **Apply Config**: Copy generated config to Claude Desktop config location

## Security

- All GitHub tokens are encrypted in the database
- Supports environment-based encryption keys
- Optional two-factor authentication for access
- Audit logging for all token operations

## Architecture

- **Models**: Identity, Token, Permission, Configuration
- **Controllers**: IdentityController, TokenController, ConfigController
- **Services**: TokenEncryption, ConfigGenerator, MCPValidator
- **Views**: Blade templates with Tailwind CSS

## License

MIT License