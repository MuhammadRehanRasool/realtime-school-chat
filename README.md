# Realtime School Chat

A lightweight real-time chat server built on Express and Socket.IO. Chat rooms are generated automatically from a MySQL table of schools/universities and kept in sync while the server runs, so every institution gets its own room alongside a global room. A single-file HTML client (French UI) is included.

## Features

- Global room plus one room per row in the `school_data` table, re-synced every minute
- Join/leave notifications and a live list of connected users
- Group messages and private (direct) messages
- Basic HTML sanitising of message content
- Emoji picker in the bundled client
- Optional HTTPS mode using Let's Encrypt (or any) certificate files
- `GET /api/rooms` endpoint returning the current room list

## Tech stack

Node.js, Express, Socket.IO, mysql2, dotenv. Client: plain HTML/CSS/JS with socket.io-client from CDN.

## Project structure

```
server.js       Express + Socket.IO server, room sync, message handlers
db.js           MySQL connection pool (mysql2/promise)
chat_app.html   Standalone browser client
```

## Setup

```bash
cp .env.example .env   # fill in database settings
npm install
npm run dev            # or: npm start
```

The database must contain a table such as:

```sql
CREATE TABLE school_data (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(255) NOT NULL);
```

Open `chat_app.html` in a browser. Edit `SERVER_URL` near the bottom of the file to point at your server when deploying.

## Environment variables

| Variable        | Description                                             | Default |
|-----------------|---------------------------------------------------------|---------|
| `IS_PRODUCTION` | `true` serves HTTPS using the SSL paths below           | `false` |
| `PORT`          | Listen port                                             | `443`   |
| `DB_HOST`       | MySQL host                                              |         |
| `DB_PORT`       | MySQL port                                              |         |
| `DB_USER`       | MySQL user                                              |         |
| `DB_PASSWORD`   | MySQL password                                          |         |
| `DB_NAME`       | MySQL database                                          |         |
| `SSL_KEY_PATH`  | Path to the TLS private key (production only)           |         |
| `SSL_CERT_PATH` | Path to the TLS certificate chain (production only)     |         |
