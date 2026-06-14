# AI CMS

A client management system with role-based access control.

## Description

AI CMS is a web application for managing clients and managers. The system provides different access levels depending on the user's role:

- **Manager**: works only with their own clients
- **Super Manager**: manages all clients and managers
- **Administrator**: full access to all sections of the system

## Technology Stack

### Backend
- PHP 8.2.29
- Laravel 12.40.2
- SQLite (for development)
- Laravel Fortify (authentication)

### Frontend
- Vue.js 3.5.22
- Inertia.js 2.0.11
- Tailwind CSS 4.1.14
- TypeScript (via Wayfinder)

### Development Tools
- Pest 3.8.4 (testing)
- Laravel Pint 1.26.0 (code formatting)
- Laravel Sail 1.48.1 (Docker environment)

## Requirements

- PHP >= 8.2
- Composer
- Node.js >= 18.x
- npm or yarn
- SQLite (for development) or MySQL/PostgreSQL (for production)

## Installation

### 1. Clone the Repository

```bash
git clone ai-orchestrator-cms
cd ai-orchestrator-cms
```

### 2. Install Dependencies

```bash
# Backend dependencies
composer install

# Frontend dependencies
npm install
```

### 3. Configure Environment

```bash
# Copy environment file
cp .env.example .env

# Generate application key
php artisan key:generate
```

Edit the `.env` file and configure:
- `DB_CONNECTION` (sqlite for development)
- `DB_DATABASE` (path to the database file for SQLite)
- Other required parameters

### 4. Set Up Database

```bash
# Run migrations
php artisan migrate

# Create administrator (optional)
php artisan db:seed
```

### 5. Build Frontend

```bash
# For development (with hot reload)
npm run dev

# For production
npm run build
```

### 6. Generate Wayfinder Types

```bash
php artisan wayfinder:generate
```

### 7. Start the Application

```bash
# Start Laravel built-in server
php artisan serve

# Or via Sail (Docker)
./vendor/bin/sail up
```

The application will be available at: `http://localhost:8000`

## Usage

### Creating the First Administrator

After installation, create the first administrator via tinker:

```bash
php artisan tinker
```

```php
User::create([
    'name' => 'Admin',
    'email' => 'admin@example.com',
    'password' => Hash::make('password'),
    'role' => \App\Enums\UserRole::ADMIN,
    'is_active' => true,
    'email_verified_at' => now(),
]);
```

### Logging In

1. Open the home page `/`
2. Enter email and password
3. After successful authentication, you will be redirected to `/dashboard`

### Roles and Permissions

#### Manager
- View and manage only their own clients
- Access to sections: Home, Clients

#### Super Manager
- View and manage all clients
- Manage managers (create, edit, delete)
- Change the responsible manager for a client
- Change manager passwords
- Manage manager activity status
- Access to sections: Home, Clients, Managers

#### Administrator
- Full access to all sections of the system
- All super manager capabilities
- Can manage managers of any type

## Development

### Project Structure

```
ai-cms/
├── app/                    # Backend code
│   ├── Enums/             # Enum classes
│   ├── Http/              # Controllers, Requests, Middleware
│   ├── Models/            # Eloquent models
│   └── Policies/          # Authorization policies
├── database/              # Migrations, factories, seeders
├── resources/             # Frontend resources
│   ├── js/
│   │   ├── components/   # Vue components
│   │   ├── pages/        # Inertia pages
│   │   └── types/        # TypeScript types
│   └── css/              # Styles
├── routes/                # Routes
├── tests/                 # Tests
└── public/               # Public files
```

### Running Tests

```bash
# All tests
php artisan test

# Specific test
php artisan test tests/Feature/ClientControllerTest.php

# With filter
php artisan test --filter=testName
```

### Code Formatting

```bash
# PHP (Laravel Pint)
vendor/bin/pint

# JavaScript/TypeScript (Prettier)
npm run format
```

### Generating Wayfinder Types

After changing routes, you need to regenerate types:

```bash
php artisan wayfinder:generate
```

## API Routes

### Clients

- `GET /dashboard/clients` - list of clients
- `POST /dashboard/clients` - create client
- `GET /dashboard/clients/{id}` - view client
- `PUT /dashboard/clients/{id}` - update client
- `DELETE /dashboard/clients/{id}` - delete client
- `POST /dashboard/clients/{id}/change-manager` - change manager

### Managers

- `GET /dashboard/managers` - list of managers
- `POST /dashboard/managers` - create manager
- `GET /dashboard/managers/{id}` - view manager
- `PUT /dashboard/managers/{id}` - update manager
- `DELETE /dashboard/managers/{id}` - delete manager
- `POST /dashboard/managers/{id}/change-password` - change password
- `POST /dashboard/managers/{id}/toggle-active` - toggle activity status

## Documentation

- [Development Document](DEVELOPMENT.md) - detailed description of architecture and implementation
- [Manual Testing Document](MANUAL_TESTING.md) - instructions for testing the system
- [Vibe Coding Process Document](VIBE_CODING.md) - description of the workflow in this project

## License

The project was created for research purposes.

## Authors

The project was developed using AI assistants to explore web development capabilities.

## Support

If you encounter any problems or have questions, please create an issue in the project repository.
