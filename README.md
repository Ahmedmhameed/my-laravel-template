# Laravel Project Template

A ready-to-extend Laravel application template with a modern PHP and frontend toolchain, database-backed defaults, automated tests, and development-quality tools already configured.

This repository is intentionally a starting point rather than a finished product. It currently exposes Laravel's default welcome page at `/` and includes the default user, cache, and jobs migrations.

## Stack

- PHP `8.3+`
- Laravel `13.17+`
- SQLite by default, with support for other Laravel database drivers
- Vite `8` and Tailwind CSS `4`
- PHPUnit `12` with ParaTest available for parallel test runs

## Included Tools

The development dependencies include:

- Laravel Pint for PHP formatting
- Larastan for static analysis
- Laravel Debugbar and Telescope for local debugging and inspection
- Laravel Pail for readable application logs
- Scribe for API documentation
- Laravel Tinker for interactive application work
- Laravel IDE Helper for improved editor support
- Laravel Boost and Pao for Laravel development workflows

## Requirements

Install the following before starting:

- PHP `8.3` or later
- Composer
- Node.js and npm
- SQLite, or another database supported by Laravel

## Getting Started

Clone the repository and run the setup script:

```bash
composer run setup
```

The setup script:

1. Installs PHP dependencies.
2. Creates `.env` from `.env.example` when needed.
3. Generates the application key.
4. Runs database migrations.
5. Installs frontend dependencies.
6. Builds the frontend assets.

Start the local development environment with:

```bash
composer run dev
```

Then open [http://localhost:8000](http://localhost:8000). The default configuration uses SQLite and creates the database file during Laravel project setup. To use MySQL, PostgreSQL, or another driver, update the `DB_*` values in `.env` before running migrations.

For frontend-only development, use:

```bash
npm run dev
```

Create a production frontend build with:

```bash
npm run build
```

## Laravel Boost

Laravel Boost provides AI development guidelines, skills, and MCP integration for the application. Install or configure it interactively with:

```bash
php artisan boost:install
```

When the project gains new Composer packages, refresh Boost so its guidelines and skills reflect the updated application:

```bash
composer require vendor/package
php artisan boost:update
```

Run `boost:update` whenever the installed packages or development workflow changes significantly. The installer and updater may modify project-level AI configuration files, so review those changes before committing them.

## Testing And Quality

Run the test suite:

```bash
composer run test
```

Run the complete configured quality pipeline:

```bash
composer run quality
```

The quality pipeline validates Composer configuration, audits dependencies, checks formatting with Pint, runs Larastan against the current user model, and runs the tests. Individual checks are also available:

```bash
composer run quality:composer
composer run quality:audit
composer run quality:format
composer run quality:analyse
```

To format PHP files instead of only checking them:

```bash
vendor/bin/pint
```

## Project Structure

```text
app/                  Application code, models, controllers, and providers
bootstrap/             Framework bootstrap and cached services
config/                Application configuration
database/              Migrations, factories, and seeders
public/                Public entry point and built assets
resources/             Blade views, CSS, and JavaScript source
routes/                Web and console routes
storage/               Logs, cache, sessions, and generated files
tests/                 Unit and feature tests
```

## Adding A New Project

After creating a project from this template, update at least:

- `APP_NAME` and `APP_URL` in `.env`
- The database settings in `.env`
- The default route and welcome view
- The application-specific models, migrations, controllers, and tests
- Any development-only packages that are not needed in production

Keep `.env` and other secrets out of version control. Use `.env.example` to document required environment variables.

## Useful Commands

```bash
php artisan list       # List Artisan commands
php artisan migrate    # Run outstanding migrations
php artisan tinker     # Open the interactive Laravel shell
php artisan pail       # Tail application logs
```

## License

This template is released under the [MIT License](https://opensource.org/licenses/MIT).
