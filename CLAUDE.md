# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Architecture

This is a **fullstack NETI website project** using Git submodules to manage separate backend and frontend repositories:

- **Backend**: Laravel 12 API (PHP 8.2) - `backend/` submodule
- **Frontend**: Next.js 15 with App Router (TypeScript) - `frontend/` submodule

Both components are managed as Git submodules from their respective repositories:
- Backend: https://github.com/Dev-NETI/NETI-WEBSITE-V2025-BACKEND.git
- Frontend: https://github.com/Dev-NETI/NETI-WEBSITE-V2025.git

## Development Commands

### Backend (Laravel)
Navigate to `backend/` directory for all backend operations:

**Core Commands:**
- `composer dev` - Start complete development environment (server, queue, logs, Vite)
- `composer test` - Run PHPUnit test suite with config clearing
- `php artisan serve` - Start Laravel development server
- `php artisan test` - Run tests directly
- `php artisan queue:listen --tries=1` - Start queue worker
- `php artisan pail --timeout=0` - Real-time log monitoring

**Database & Migrations:**
- `php artisan migrate` - Run database migrations
- `php artisan migrate:fresh` - Fresh migration (drops all tables)
- `php artisan db:seed` - Run database seeders

### Frontend (Next.js)  
Navigate to `frontend/` directory for all frontend operations:

**Core Commands:**
- `npm run dev` - Start development server (auto-assigns port if 3000 busy)
- `npm run build` - Build production application
- `npm run start` - Start production server
- `npm run lint` - Run ESLint for code quality checks

### Root Level Operations
- `git submodule update --init --recursive` - Initialize/update submodules
- `git submodule foreach git pull origin main` - Update all submodules to latest

## Backend Architecture (Laravel 12)

**Technology Stack:**
- **Framework**: Laravel 12 with PHP 8.2+
- **Authentication**: Laravel Sanctum for API token management
- **Testing**: PHPUnit with Feature/Unit test structure
- **Development Tools**: Laravel Breeze, Pint (code style), Pail (logging), Tinker

**Key Features:**
- Sanctum API authentication for SPA/mobile apps
- Concurrent development script running server, queue, logs, and Vite
- Comprehensive testing setup with in-memory SQLite for tests
- Laravel Breeze for rapid auth scaffolding

**Directory Structure:**
```
backend/
├── app/                 # Application logic (Models, Controllers, Services)
├── database/           # Migrations, seeders, factories
├── routes/            # API routes definition
├── tests/             # PHPUnit tests (Feature/Unit)
├── config/            # Application configuration
└── storage/           # Logs, cache, uploads
```

## Frontend Architecture (Next.js 15)

**Technology Stack:**
- **Framework**: Next.js 15 with App Router and TypeScript
- **Database**: Hybrid approach - JSON files + Vercel Postgres
- **Authentication**: JWT with httpOnly cookies, bcrypt hashing
- **Styling**: Tailwind CSS v4 with PostCSS
- **Animation**: Framer Motion for page transitions
- **UI Components**: Custom React components with role-based access

**Key Features:**
- Document database system using JSON files (`data/` folder)
- Role-based access control (super_admin, user_manager, events_manager, news_manager)
- Admin management system with comprehensive CRUD operations
- News management system with animated slider component
- Hybrid database: JSON files for development, Vercel Postgres for production

**Directory Structure:**
```
frontend/
├── src/app/            # App Router pages and API routes
│   ├── admin/          # Protected admin panel
│   └── api/            # REST API endpoints
├── src/components/     # Reusable UI components
├── src/lib/            # Core business logic and database operations
├── data/              # JSON document database files
└── public/            # Static assets
```

## Database Architecture

### Backend (Laravel)
- Uses traditional Laravel Eloquent ORM
- SQLite in-memory database for testing
- Standard Laravel migration system

### Frontend (Hybrid Approach)
- **Development**: JSON files in `data/` folder
  - `data/users.json` - User accounts
  - `data/news.json` - News articles  
  - `data/events.json` - Events
  - `data/sessions.json` - Session management
- **Production**: Vercel Postgres with auto-initialization
- Database operations handled through `src/lib/*-db.ts` files

## Authentication Systems

### Backend (Laravel Sanctum)
- Token-based authentication for API access
- Stateless authentication suitable for SPA/mobile
- Personal access tokens with ability management

### Frontend (JWT + Cookies)
- JWT tokens stored in httpOnly cookies (`admin-token`)
- Role-based permissions with 4 user levels
- Session management with automatic token refresh

## API Integration
The frontend communicates with the Laravel backend through:
- RESTful API endpoints defined in `backend/routes/api.php`
- Sanctum token authentication for protected routes
- Frontend API routes in `frontend/src/app/api/` may proxy to backend or handle directly

## Development Workflow

1. **Initial Setup:**
   ```bash
   git submodule update --init --recursive
   cd backend && composer install
   cd ../frontend && npm install
   ```

2. **Development:**
   ```bash
   # Terminal 1: Backend
   cd backend && composer dev
   
   # Terminal 2: Frontend  
   cd frontend && npm run dev
   ```

3. **Testing:**
   ```bash
   # Backend tests
   cd backend && composer test
   
   # Frontend linting
   cd frontend && npm run lint
   ```

## Environment Configuration

### Backend (.env)
```env
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:...
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=neti_backend
DB_USERNAME=root
DB_PASSWORD=
```

### Frontend (.env.local)
```env
POSTGRES_URL=your_postgres_url
JWT_SECRET=your-secure-secret-key
ADMIN_EMAIL=admin@neti.com.ph
ADMIN_PASSWORD=admin123
NODE_ENV=development
```

## Submodule Management

When working with this project, remember:
- Each submodule has its own Git history and branches
- Changes in submodules should be committed in their respective repositories
- Root repository tracks specific commits of each submodule
- Use `git submodule foreach` for bulk operations across submodules