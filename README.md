# Docker PostgreSQL + MySQL + Adminer Setup

This Docker Compose configuration sets up PostgreSQL and MySQL databases with Adminer for database management.

## Quick Start

1. **Start all services:**
   ```bash
   docker-compose up -d
   ```

2. **Check running containers:**
   ```bash
   docker-compose ps
   ```

3. **View logs:**
   ```bash
   docker-compose logs
   # or for specific service
   docker-compose logs postgres
   docker-compose logs mysql
   ```

## Docker Commands Reference

- **Start services (detached mode):**
  ```bash
  docker-compose up -d
  ```

- **Stop services (keeps containers):**
  ```bash
  docker-compose stop
  ```

- **Stop and remove containers:**
  ```bash
  docker-compose down
  ```

- **Stop, remove containers AND delete all data:**
  ```bash
  docker-compose down -v
  ```

- **View running containers:**
  ```bash
  docker-compose ps
  ```

- **Restart services:**
  ```bash
  docker-compose restart
  ```

## Services & Credentials

### PostgreSQL
- **Port:** 5432
- **Database:** mydatabase
- **User:** myuser
- **Password:** mypassword
- **Container Name:** postgres_db

### MySQL
- **Port:** 3306
- **Database:** mydatabase
- **User:** myuser
- **Password:** zxczxc123
- **Root Password:** zxczxc123
- **Container Name:** mysql_db

### Adminer (Database Management UI)
- **URL:** http://localhost:8080
- **Container Name:** adminer

#### Accessing PostgreSQL via Adminer:
- **System:** PostgreSQL
- **Server:** postgres
- **Username:** myuser
- **Password:** mypassword
- **Database:** mydatabase

#### Accessing MySQL via Adminer:
- **System:** MySQL
- **Server:** mysql
- **Username:** myuser (or root)
- **Password:** zxczxc123
- **Database:** mydatabase

## Using with Next.js

### PostgreSQL Connection

#### 1. Install PostgreSQL client
```bash
npm install pg
# or with Prisma
npm install prisma @prisma/client
npm install -D prisma
```

#### 2. Connection String
Add to your `.env.local`:
```env
DATABASE_URL="postgresql://myuser:mypassword@localhost:5432/mydatabase"
```

#### 3. Example Connection (with pg)
```javascript
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

export default pool;
```

#### 4. Example with Prisma
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

### MySQL Connection

#### 1. Install MySQL client
```bash
npm install mysql2
# or with Prisma (already installed above)
```

#### 2. Connection String
Add to your `.env.local`:
```env
MYSQL_URL="mysql://myuser:zxczxc123@localhost:3306/mydatabase"
```

#### 3. Example Connection (with mysql2)
```javascript
import mysql from 'mysql2/promise';

const connection = await mysql.createConnection({
  host: 'localhost',
  user: 'myuser',
  password: 'zxczxc123',
  database: 'mydatabase'
});

export default connection;
```

#### 4. Example with Prisma
Update `prisma/schema.prisma` datasource:
```prisma
datasource db {
  provider = "mysql"
  url      = env("MYSQL_URL")
}
```

## URLs Summary
- **Adminer UI:** http://localhost:8080
- **PostgreSQL:** localhost:5432
- **MySQL:** localhost:3306

## Troubleshooting

- **Port already in use:** If you get port conflicts, stop the conflicting service or change the port mapping in `docker-compose.yml`
- **Connection refused:** Make sure containers are running with `docker-compose ps`
- **Data persistence:** Data is stored in Docker volumes (`postgres_data`, `mysql_data`). Use `docker-compose down -v` only if you want to delete all data
