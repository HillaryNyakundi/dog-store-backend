# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Getting Started (Database to Server)

### 1. Database Setup
1. **Create PostgreSQL Database**:
   ```bash
   # Connect to PostgreSQL
   psql -U postgres
   
   # Create database
   CREATE DATABASE your_database_name;
   
   # Create user (optional)
   CREATE USER your_username WITH PASSWORD 'your_password';
   GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_username;
   ```

2. **Environment Configuration**:
   Create `.env` file in project root:
   ```env
   # Database Config
   db_username=your_username
   db_password=your_password
   db_hostname=localhost
   db_port=5432
   db_name=your_database_name
   
   # JWT Config
   secret_key=your_secret_key_here
   algorithm=HS256
   access_token_expire_minutes=30
   ```

### 2. Application Setup
1. **Virtual Environment**:
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Run Database Migrations**:
   ```bash
   python migrate.py
   ```

4. **Start Development Server**:
   ```bash
   python run.py
   ```

### 3. Access Points
- API Server: http://127.0.0.1:8000/
- Swagger UI: http://127.0.0.1:8000/docs/
- ReDoc: http://127.0.0.1:8000/redoc/

## Development Commands

### Database Operations
- Run migrations: `python migrate.py`
- Alembic configuration is in `alembic.ini`
- Database models are defined in `app/models/models.py`

## Architecture Overview

This is a FastAPI-based e-commerce backend with the following structure:

### Core Components
- **Main Application**: `app/main.py` - FastAPI app configuration and router inclusion
- **Entry Point**: `main.py` and `run.py` - Application startup
- **Configuration**: `app/core/config.py` - Settings management using Pydantic BaseSettings
- **Database**: `app/db/database.py` - SQLAlchemy setup with PostgreSQL

### Layer Structure
- **Models** (`app/models/`): SQLAlchemy ORM models (User, Cart, CartItem, Product, Category)
- **Schemas** (`app/schemas/`): Pydantic models for request/response validation
- **Services** (`app/services/`): Business logic layer
- **Routers** (`app/routers/`): FastAPI route handlers
- **Utils** (`app/utils/`): Utility functions including response helpers

### Key Features
- JWT authentication with role-based access (admin/user)
- CRUD operations for products, categories, users, carts
- Shopping cart management with cart items
- User account management
- Database migrations with Alembic

### Database Models
- **User**: Authentication and profile management with role field
- **Cart**: User shopping carts with total amount tracking
- **CartItem**: Individual items in carts with quantity and subtotal
- **Product**: Product catalog (referenced by CartItem)
- **Category**: Product categorization

### Environment Configuration
The application uses `.env` file for configuration with these required variables:
- Database: `db_username`, `db_password`, `db_hostname`, `db_port`, `db_name`
- JWT: `secret_key`, `algorithm`, `access_token_expire_minutes`

### API Structure
All routers follow RESTful conventions with proper HTTP methods and status codes. The API supports both user and admin operations with JWT-based authentication and role-based authorization.