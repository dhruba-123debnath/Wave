# Wave — React Spotify-style frontend

Built for the provided `complete-backend` Express application. Uses React, Vite, lucide-react, and CSS.

## Run

1. Start the existing backend in a separate terminal (from `complete-backend`):
   ```bash
   npm install
   npx nodemon server.js
   ```
   Backend must be running on port **3000**, with its MongoDB, JWT_SECRET and IMAGEKIT_PRIVATE_KEY configured in a local `.env` file.
2. In this frontend folder:
   ```bash
   npm install
   npm run dev
   ```
3. Open **http://localhost:5173**.

Vite proxies `/api` requests to `http://localhost:3000`, including login cookies, so you do not need to add CORS for local development.

## Backend endpoints used

- `POST /api/auth/register`, `POST /api/auth/login`, `POST /api/auth/logout`
- `GET /api/music/`, `GET /api/music/albums`, `GET /api/music/albums/:albumId`
- `POST /api/music/upload` (multipart form: `music`, `title`)
- `POST /api/music/album` (JSON: `title`, `musics`)

## Important limitations of the supplied backend

- `authUser` middleware only allows `role: "user"` to access GET music and album routes; artists can upload/create albums but **cannot browse songs/albums** with the current backend. The frontend respects that limitation.
- There is **no endpoint to list an artist's own uploaded tracks**. The artist album builder can select only tracks uploaded in the current page session. To persist artist library across sessions, add a backend endpoint to fetch the artist's tracks.
- The backend does not expose a `GET /me` or token-refresh endpoint. Frontend remembers the displayed user in `sessionStorage`; if the cookie expires, sign in again.
- ImageKit must be configured before backend startup. Never put private API keys in frontend code.
- If you see 401/403, check login and backend role restrictions. If uploaded audio does not play, verify the uploaded file format and ImageKit URL.
- Backend's `authUser` handler should `return res.status(401).json(...)` when token is missing, to avoid trying to verify an undefined token.
