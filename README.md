# Redis Caching with Node.js

A small Node.js project that demonstrates Redis caching for API responses. The Express server fetches a todo from JSONPlaceholder on a cache miss, stores the JSON response in Redis, and serves subsequent requests from Redis for 30 seconds.

The repository also includes simple examples for Redis string values and list operations using `ioredis`.

## Tech stack

- JavaScript (CommonJS)
- Node.js
- Express 5
- ioredis
- Axios
- Redis

## Project files

- `server.js` — starts an Express server on port `9000` and caches the JSONPlaceholder todo at the `todos` Redis key.
- `client.js` — creates the shared `ioredis` client (using the default Redis connection settings).
- `string.js` — demonstrates setting, expiring, and reading a Redis string key.
- `list.js` — demonstrates pushing, popping, and blocking pop operations on a Redis list.
- `package.json` — Node.js dependencies and npm scripts.

## How Redis caching works

When `GET /` is requested:

1. The server looks for a value at the Redis key `todos`.
2. If found, it parses the cached JSON and returns it immediately.
3. If missing, it requests `https://jsonplaceholder.typicode.com/todos/1` with Axios.
4. It stores the response under `todos` and sets a 30-second expiration.
5. Requests within those 30 seconds use the cache instead of calling the external API again.

## Prerequisites

- Node.js and npm
- A Redis server running locally on `localhost:6379`

The Redis connection in `client.js` uses `ioredis` defaults, so no environment variables are required for the standard local setup.

## Run locally

Start Redis using your local Redis installation:

```bash
redis-server
```

In another terminal, install dependencies and start the Express server:

```bash
npm install
npm start
```

Open:

```text
http://localhost:9000
```

The first request retrieves the todo from JSONPlaceholder; later requests within 30 seconds are returned from Redis.

To try the separate Redis examples:

```bash
node string.js
node list.js
```

> `string.js` and `list.js` are standalone examples. Their Redis keys and commands can be enabled or changed in the source files.

## Notes

- Redis data is cached for 30 seconds by `EXPIRE`.
- The Express server runs on port `9000`.
- This project is intended as a learning example and currently has no test suite.
