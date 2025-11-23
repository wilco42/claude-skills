# Phase 1 Tasks: Database & Core Models

## Phase Overview
**Goal:** Set up database schema and data access layer with full CRUD operations for users, sessions, and password resets.

**Dependencies:** PostgreSQL server configured and accessible

**Deliverables:**
- Database migration files for all tables
- Repository classes with complete CRUD operations
- Unit tests passing with 80%+ coverage
- Database properly indexed for performance

Reference: See `plan.md` for full technical plan and `spec.md` for requirements.

## Detailed Task Breakdown

### Task 1: Database Setup & Configuration
**Description:** Initialize PostgreSQL database and create configuration for database connections

**Acceptance Criteria:**
- [ ] PostgreSQL database created (e.g., `auth_app_db`)
- [ ] Database user created with appropriate permissions
- [ ] Connection configuration file created with environment variables
- [ ] Connection pooling configured (max 20 connections)
- [ ] Database connection tested successfully

**Implementation Details:**
- Create database: `CREATE DATABASE auth_app_db;`
- Create `.env` file with `DATABASE_URL` connection string
- Create `src/config/database.js` with pg Pool configuration
- Use environment variables for credentials (not hardcoded)
- Add `.env` to `.gitignore`

**Files to Create/Modify:**
- `src/config/database.js` (new)
- `.env` (new)
- `.env.example` (new - template without secrets)
- `.gitignore` (modify - add .env)

**Dependencies:** None

**Estimated Effort:** Small

---

### Task 2: Create Database Migration System
**Description:** Set up migration system to manage database schema changes

**Acceptance Criteria:**
- [ ] Migration framework chosen and installed (e.g., node-pg-migrate)
- [ ] Migration scripts directory created
- [ ] Migration command scripts added to package.json
- [ ] README documentation for running migrations

**Implementation Details:**
- Install migration tool: `npm install node-pg-migrate`
- Create `migrations/` directory
- Add npm scripts:
  - `"migrate:up": "node-pg-migrate up"`
  - `"migrate:down": "node-pg-migrate down"`
  - `"migrate:create": "node-pg-migrate create"`
- Configure migration tool to use DATABASE_URL

**Files to Create/Modify:**
- `package.json` (modify - add scripts and dependency)
- `migrations/` (new directory)
- `README.md` (modify - add migration instructions)

**Dependencies:** Task 1

**Estimated Effort:** Small

---

### Task 3: Create Users Table Migration
**Description:** Create database migration for users table with all required fields and constraints

**Acceptance Criteria:**
- [ ] Users table created with all fields from spec
- [ ] Primary key on id (UUID)
- [ ] Unique constraint on email
- [ ] Default values set correctly (role='user', email_verified=false)
- [ ] Timestamps (created_at, updated_at) included
- [ ] Migration can be rolled back cleanly

**Implementation Details:**
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  name VARCHAR(255) NOT NULL,
  role VARCHAR(50) NOT NULL DEFAULT 'user',
  email_verified BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
```

**Files to Create/Modify:**
- `migrations/[timestamp]_create-users-table.js` (new)

**Dependencies:** Task 2

**Estimated Effort:** Small

---

### Task 4: Create Sessions Table Migration
**Description:** Create database migration for sessions table to track active user sessions

**Acceptance Criteria:**
- [ ] Sessions table created with all required fields
- [ ] Foreign key constraint to users table with CASCADE DELETE
- [ ] Unique constraint on token
- [ ] Index on user_id for fast lookups
- [ ] Index on expires_at for cleanup queries
- [ ] Migration can be rolled back cleanly

**Implementation Details:**
```sql
CREATE TABLE sessions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  token VARCHAR(255) UNIQUE NOT NULL,
  expires_at TIMESTAMP NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_sessions_user_id ON sessions(user_id);
CREATE INDEX idx_sessions_token ON sessions(token);
CREATE INDEX idx_sessions_expires_at ON sessions(expires_at);
```

**Files to Create/Modify:**
- `migrations/[timestamp]_create-sessions-table.js` (new)

**Dependencies:** Task 3 (users table must exist for foreign key)

**Estimated Effort:** Small

---

### Task 5: Create Password Resets Table Migration
**Description:** Create database migration for password_resets table to handle password reset requests

**Acceptance Criteria:**
- [ ] Password_resets table created with all required fields
- [ ] Foreign key constraint to users table with CASCADE DELETE
- [ ] Unique constraint on token
- [ ] used field defaults to false
- [ ] Indexes on token and user_id for performance
- [ ] Migration can be rolled back cleanly

**Implementation Details:**
```sql
CREATE TABLE password_resets (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  token VARCHAR(255) UNIQUE NOT NULL,
  expires_at TIMESTAMP NOT NULL,
  used BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_password_resets_token ON password_resets(token);
CREATE INDEX idx_password_resets_user_id ON password_resets(user_id);
CREATE INDEX idx_password_resets_expires_at ON password_resets(expires_at);
```

**Files to Create/Modify:**
- `migrations/[timestamp]_create-password-resets-table.js` (new)

**Dependencies:** Task 3 (users table must exist for foreign key)

**Estimated Effort:** Small

---

### Task 6: Create User Repository
**Description:** Implement repository class for user data access with full CRUD operations

**Acceptance Criteria:**
- [ ] UserRepository class created with all methods
- [ ] Methods: create, findById, findByEmail, update, delete, findAll
- [ ] All methods use parameterized queries (SQL injection safe)
- [ ] Error handling for database errors
- [ ] Returns consistent data format
- [ ] Uses database connection pool

**Implementation Details:**
```javascript
class UserRepository {
  constructor(dbPool) {
    this.db = dbPool;
  }

  async create({ email, passwordHash, name, role = 'user' }) {
    // INSERT query with RETURNING *
  }

  async findById(id) {
    // SELECT query by id
  }

  async findByEmail(email) {
    // SELECT query by email
  }

  async update(id, updates) {
    // UPDATE query with only provided fields
  }

  async delete(id) {
    // DELETE query
  }

  async findAll() {
    // SELECT all users (for admin)
  }
}
```

**Files to Create/Modify:**
- `src/repositories/UserRepository.js` (new)
- `src/repositories/index.js` (new - export all repositories)

**Dependencies:** Task 1, 3

**Estimated Effort:** Medium

---

### Task 7: Create Session Repository
**Description:** Implement repository class for session data access

**Acceptance Criteria:**
- [ ] SessionRepository class created with all methods
- [ ] Methods: create, findByToken, findByUserId, deleteById, deleteByUserId, deleteExpired
- [ ] All methods use parameterized queries
- [ ] deleteExpired method removes sessions where expires_at < NOW()
- [ ] Error handling implemented
- [ ] Returns consistent data format

**Implementation Details:**
```javascript
class SessionRepository {
  async create({ userId, token, expiresAt }) { }
  async findByToken(token) { }
  async findByUserId(userId) { }
  async deleteById(id) { }
  async deleteByUserId(userId) { }
  async deleteExpired() { }
  async countByUserId(userId) { } // For concurrent session limits
}
```

**Files to Create/Modify:**
- `src/repositories/SessionRepository.js` (new)
- `src/repositories/index.js` (modify - add export)

**Dependencies:** Task 1, 4

**Estimated Effort:** Medium

---

### Task 8: Create Password Reset Repository
**Description:** Implement repository class for password reset token management

**Acceptance Criteria:**
- [ ] PasswordResetRepository class created with all methods
- [ ] Methods: create, findByToken, markAsUsed, deleteByUserId, deleteExpired
- [ ] findByToken checks that token is not expired and not used
- [ ] All methods use parameterized queries
- [ ] Error handling implemented
- [ ] Returns consistent data format

**Implementation Details:**
```javascript
class PasswordResetRepository {
  async create({ userId, token, expiresAt }) { }
  async findByToken(token) {
    // WHERE token = ? AND expires_at > NOW() AND used = false
  }
  async markAsUsed(id) { }
  async deleteByUserId(userId) { }
  async deleteExpired() { }
}
```

**Files to Create/Modify:**
- `src/repositories/PasswordResetRepository.js` (new)
- `src/repositories/index.js` (modify - add export)

**Dependencies:** Task 1, 5

**Estimated Effort:** Medium

---

### Task 9: Add Database Utility Functions
**Description:** Create utility functions for common database operations

**Acceptance Criteria:**
- [ ] Transaction wrapper function created
- [ ] Query logging utility (for development)
- [ ] Database health check function
- [ ] Connection cleanup on shutdown
- [ ] Error mapping (map DB errors to app errors)

**Implementation Details:**
```javascript
// src/utils/database.js
async function withTransaction(callback) {
  const client = await pool.connect();
  try {
    await client.query('BEGIN');
    const result = await callback(client);
    await client.query('COMMIT');
    return result;
  } catch (error) {
    await client.query('ROLLBACK');
    throw error;
  } finally {
    client.release();
  }
}

async function healthCheck() {
  // Simple query to verify connection
}
```

**Files to Create/Modify:**
- `src/utils/database.js` (new)

**Dependencies:** Task 1

**Estimated Effort:** Small

---

## Testing Tasks

### Test Task 1: Unit Tests for User Repository
**Description:** Write comprehensive unit tests for all UserRepository methods

**Test Cases:**
- ✅ create: Successfully creates user with valid data
- ✅ create: Rejects duplicate email (unique constraint)
- ✅ create: Sets default values correctly (role='user', email_verified=false)
- ✅ findById: Returns user when exists
- ✅ findById: Returns null when not exists
- ✅ findByEmail: Returns user when exists
- ✅ findByEmail: Returns null when not exists
- ✅ update: Successfully updates user fields
- ✅ update: Does not update non-existent user
- ✅ delete: Successfully deletes user
- ✅ delete: Cascade deletes sessions and password_resets
- ✅ findAll: Returns all users

**Testing Approach:** Integration tests using test database

**Files to Create/Modify:**
- `tests/repositories/UserRepository.test.js` (new)
- `tests/setup.js` (new - test database setup)

**Dependencies:** Task 6

**Estimated Effort:** Medium

---

### Test Task 2: Unit Tests for Session Repository
**Description:** Write comprehensive unit tests for SessionRepository

**Test Cases:**
- ✅ create: Successfully creates session
- ✅ findByToken: Returns session when valid
- ✅ findByUserId: Returns all user sessions
- ✅ deleteById: Removes specific session
- ✅ deleteByUserId: Removes all user sessions
- ✅ deleteExpired: Removes only expired sessions
- ✅ countByUserId: Returns accurate count

**Testing Approach:** Integration tests using test database

**Files to Create/Modify:**
- `tests/repositories/SessionRepository.test.js` (new)

**Dependencies:** Task 7

**Estimated Effort:** Medium

---

### Test Task 3: Unit Tests for Password Reset Repository
**Description:** Write comprehensive unit tests for PasswordResetRepository

**Test Cases:**
- ✅ create: Successfully creates reset token
- ✅ findByToken: Returns token when valid and not expired
- ✅ findByToken: Returns null when expired
- ✅ findByToken: Returns null when already used
- ✅ markAsUsed: Successfully marks token as used
- ✅ markAsUsed: Used token not returned by findByToken
- ✅ deleteByUserId: Removes all user reset tokens
- ✅ deleteExpired: Removes only expired tokens

**Testing Approach:** Integration tests using test database

**Files to Create/Modify:**
- `tests/repositories/PasswordResetRepository.test.js` (new)

**Dependencies:** Task 8

**Estimated Effort:** Medium

---

### Test Task 4: Database Utilities Tests
**Description:** Test database utility functions

**Test Cases:**
- ✅ withTransaction: Commits on success
- ✅ withTransaction: Rolls back on error
- ✅ healthCheck: Returns success when DB connected
- ✅ healthCheck: Returns error when DB disconnected

**Testing Approach:** Unit tests with mocked database

**Files to Create/Modify:**
- `tests/utils/database.test.js` (new)

**Dependencies:** Task 9

**Estimated Effort:** Small

---

## Phase Completion Criteria

This phase is complete when:

- [ ] All database migrations created and run successfully
- [ ] All three tables (users, sessions, password_resets) exist with proper schema
- [ ] All indexes created for performance
- [ ] All repository classes implemented with full CRUD
- [ ] All repository methods use parameterized queries (SQL injection safe)
- [ ] All unit tests written and passing
- [ ] Test coverage > 80% for repository code
- [ ] Code reviewed by team member
- [ ] Documentation updated (README with database setup instructions)
- [ ] No linting errors
- [ ] Database can be reset cleanly using migrations

## Notes

**Performance Considerations:**
- Connection pooling configured for concurrent requests
- Indexes on all foreign keys and frequently queried fields
- Consider adding composite index on sessions(user_id, expires_at) if queries join these

**Security Considerations:**
- Never log or return password_hash to client
- Use parameterized queries exclusively (no string concatenation)
- Validate all input before database operations (will add in Phase 2)

**Future Enhancements:**
- Consider adding soft delete (deleted_at field) instead of hard delete
- Add updated_at trigger to auto-update on row changes
- Consider partitioning sessions table if scale requires
