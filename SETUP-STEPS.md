# Rails Notes App - Setup Steps

This document contains all the steps taken to set up the Rails Notes App from scratch, so you can replicate the process later.

## Prerequisites
- Ruby 3.2.2+ installed
- Git installed
- DigitalOcean account with PostgreSQL database cluster created

## Step-by-Step Setup

### Epic 1: Project Setup & Configuration

#### 1. Install Rails
```bash
gem install rails
```

#### 2. Create New Rails App
```bash
rails new notes-app --database=postgresql --css=tailwind
```

This creates a new Rails application with:
- PostgreSQL as the database
- Tailwind CSS for styling
- All standard Rails directories and files

#### 3. Navigate to Project Directory
```bash
cd notes-app
```

#### 4. Verify Rails App Structure
The following directories should be present:
- `app/` - Main application code (controllers, models, views)
- `config/` - Configuration files
- `db/` - Database migrations and schema
- `Gemfile` - Ruby gem dependencies
- Git repository already initialized

#### 5. Add Devise Gem for Authentication
Edit `Gemfile` and add:
```ruby
# Flexible authentication solution for Rails [https://github.com/heartcombo/devise]
gem "devise"
```

#### 6. Add dotenv-rails for Environment Variables
Edit `Gemfile` in the development/test group:
```ruby
group :development, :test do
  # ... existing gems ...
  
  # Load environment variables from .env file
  gem "dotenv-rails"
end
```

#### 7. Install Dependencies
```bash
bundle install
```

### Epic 2: Database Setup & Configuration

#### 8. Configure Database for DigitalOcean PostgreSQL
Edit `config/database.yml`:

**Development section:**
```yaml
development:
  <<: *default
  database: <%= ENV.fetch("DATABASE_NAME") { "notes_app_development" } %>
  username: <%= ENV.fetch("DATABASE_USERNAME") { "notes_app" } %>
  password: <%= ENV["DATABASE_PASSWORD"] %>
  host: <%= ENV.fetch("DATABASE_HOST") { "localhost" } %>
  port: <%= ENV.fetch("DATABASE_PORT") { 5432 } %>
```

**Test section:**
```yaml
test:
  <<: *default
  database: <%= ENV.fetch("DATABASE_NAME_TEST") { "notes_app_test" } %>
  username: <%= ENV.fetch("DATABASE_USERNAME") { "notes_app" } %>
  password: <%= ENV["DATABASE_PASSWORD"] %>
  host: <%= ENV.fetch("DATABASE_HOST") { "localhost" } %>
  port: <%= ENV.fetch("DATABASE_PORT") { 5432 } %>
```

#### 9. Create Environment Variables File
Create `.env` in the project root:
```bash
# DigitalOcean PostgreSQL Database Configuration
DATABASE_HOST=your-cluster-name-do-user-xxxxx-0.e.db.ondigitalocean.com
DATABASE_PORT=25060
DATABASE_NAME=defaultdb
DATABASE_USERNAME=doadmin
DATABASE_PASSWORD=your-actual-password
DATABASE_NAME_TEST=defaultdb_test
```

**Important:** Replace the values with your actual DigitalOcean database credentials:
- Get these from your DigitalOcean database cluster dashboard
- The `.env` file is already in `.gitignore` so it won't be committed to version control

#### 10. Test Database Connection
```bash
# Create the database
rails db:create

# Run initial migrations
rails db:migrate
```

#### 11. Test Rails Server
```bash
# Start the server
rails server

# Visit http://localhost:3000 to verify it's working
# Press Ctrl+C to stop the server
```

### Git Setup

#### 12. Create Initial Commit
```bash
# Check git status
git status

# Add all files
git add .

# Create initial commit
git commit -m "Initial Rails app setup with PostgreSQL and Tailwind CSS

- Created Rails 8.0.2 app with PostgreSQL database
- Added Tailwind CSS for styling
- Added Devise gem for authentication
- Added dotenv-rails for environment variable management
- Configured database.yml for DigitalOcean PostgreSQL
- Set up .env file for database credentials

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

## What's Next

After completing these steps, you'll have:
- ✅ Rails app created with PostgreSQL and Tailwind CSS
- ✅ Devise gem added for authentication
- ✅ Database configured for DigitalOcean PostgreSQL
- ✅ Environment variables set up securely
- ✅ Git repository initialized with initial commit

**Next Epic:** User Authentication (Devise Setup)
- Run Devise installer
- Generate User model
- Configure authentication views
- Test user registration and login

## File Structure Created
```
notes-app/
├── app/
│   ├── controllers/
│   ├── models/
│   ├── views/
│   └── assets/
├── config/
│   ├── database.yml (configured for DO PostgreSQL)
│   └── routes.rb
├── db/
├── Gemfile (with devise and dotenv-rails)
├── .env (with your database credentials)
├── .gitignore (includes .env*)
└── README.md
```

## Important Security Notes
- Never commit the `.env` file to version control
- Keep your database credentials secure
- The `.env` file is already ignored by git
- Use environment variables for all sensitive configuration

## Troubleshooting
- If `rails db:create` fails, verify your DigitalOcean database credentials
- If Tailwind CSS isn't working, ensure the `tailwindcss-rails` gem is installed
- If server won't start, check for port conflicts or database connection issues