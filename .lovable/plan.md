# Import Astra + fix Games

## 1. Import the project
- Copy all app code from the uploaded zip into this project (pages, chat, movies, music, web browser, games, voice, settings, admin, proxy engine files).
- Skip `.env` and git metadata.
- Turn on Lovable Cloud and recreate the database from the project's 4 migrations (profiles, threads, messages, memory, roles, images storage).
- Set the dev access code to `0307` as a secure server setting (used by the existing "dev sign-in" on the login page).
- Ask securely for any other keys the app uses (e.g. movie database key) when needed.

## 2. Fix "game loaded, then 404"
Cause: the proxy's background worker isn't controlling the page when the game frame opens (or a game pops a new tab outside the proxy), so the request hits the app instead and 404s.
- Make the proxy worker claim the page immediately and wait until it's ready before loading a game.
- Re-route any "open in new tab" from a game back into the in-app player instead of a raw tab.
- Add a catch for proxied paths so they never fall through to the 404 page.

## 3. Real game library (play games, not just sites)
- For each source (GN Math, UGS, Truffled, CKV, Seraph, Lumin, etc.), fetch its game list (most publish a JSON/HTML index of games) on the server and build one combined searchable grid with thumbnails.
- Clicking a game opens a player: game in a frame, with Fullscreen, Reload, and Back buttons.
- Sources without a readable game list stay available only in "website" mode.

## 4. Mode choice on the Games tab
- When opening Games, show two big options: **Just games** (the grid above) or **View actual website** (current behavior: pick a source site and browse it inside the app).
- Remember the last choice, with a toggle at the top to switch.

## Notes
- Some game sources may block embedding or change their lists; those games will be hidden automatically.
- A 4-digit code is easy to guess; consider a longer one later.
