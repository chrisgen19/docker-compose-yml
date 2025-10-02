# Docker PostgreSQL + Adminer Setup

This Docker Compose configuration sets up PostgreSQL database with Adminer for database management.

## Setup

1. **Start the services:**
   ```bash
   docker-compose up -d
   ```

2. **Stop the services:**
   ```bash
   docker-compose down
   ```

3. **Stop and remove data:**
   ```bash
   docker-compose down -v
   ```

## Services

### PostgreSQL
- **Port:** 5432
- **Database:** mydatabase
- **User:** myuser
- **Password:** mypassword

### Adminer (Database Management UI)
- **URL:** http://localhost:8080
- **Server:** postgres
- **Username:** myuser
- **Password:** mypassword
- **Database:** mydatabase

## Using with Next.js

### 1. Install PostgreSQL client
```bash
npm install pg
# or with Prisma
npm install prisma @prisma/client
npm install -D prisma
```

### 2. Connection String
Add to your `.env.local`:
```env
DATABASE_URL="postgresql://myuser:mypassword@localhost:5432/mydatabase"
```

### 3. Example Connection (with pg)
```javascript
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

export default pool;
```

### 4. Example with Prisma
Initialize Prisma:
```bash
npx prisma init
```

Update `prisma/schema.prisma` datasource:
```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

Run migrations:
```bash
npx prisma migrate dev
npx prisma generate
```

## URLs Summary
- **Adminer:** http://localhost:8080
- **PostgreSQL:** localhost:5432
- **Next.js API routes:** Use `DATABASE_URL` from `.env.local`
