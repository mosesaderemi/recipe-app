# Recipe App

A cross-platform recipe application. A React Native (Expo) mobile client lets users browse and search recipes, watch video tutorials and save favourites, backed by an Express and PostgreSQL API. Recipe content comes from TheMealDB, and accounts are handled by Clerk.

## Features

- Sign up, sign in and six-digit email verification with Clerk
- Browse featured recipes and filter by category
- Search recipes with debounced input
- Recipe detail pages with ingredients, instructions and an embedded YouTube tutorial
- Favourites saved per user in PostgreSQL, with a dedicated Favourites tab
- Eight selectable colour themes (coffee, forest, purple, ocean, sunset, mint, midnight and rose gold)
- Keep-alive cron job so a free-tier host does not put the API to sleep

## Tech Stack

| Area | Technology |
| --- | --- |
| Mobile | React Native, Expo, Expo Router |
| Authentication | Clerk (`@clerk/clerk-expo`) |
| Backend | Node.js, Express 5 |
| Database | PostgreSQL on Neon (serverless driver), Drizzle ORM and Drizzle Kit |
| Recipe data | TheMealDB public API |

## Project Structure

```text
.
├── backend
│   ├── src
│   │   ├── server.js          Express app and favourites endpoints
│   │   ├── config             env.js, db.js, cron.js
│   │   └── db                 Drizzle schema and migrations
│   └── drizzle.config.js
└── mobile
    ├── app
    │   ├── (auth)             sign-in, sign-up, verify-email
    │   ├── (tabs)             home, search, favourites
    │   └── recipe/[id].jsx    Recipe detail
    ├── components             RecipeCard, CategoryFilter, LoadingSpinner ...
    ├── constants              api.js, colors.js (themes)
    ├── hooks                  useDebounce
    └── services/mealAPI.js    TheMealDB client
```

## API Reference

| Method | Path | Description |
| --- | --- | --- |
| GET | `/api/health` | Health check |
| POST | `/api/favorites` | Save a recipe as a favourite |
| GET | `/api/favorites/:userId` | List a user's favourites |
| DELETE | `/api/favorites/:userId/:recipeId` | Remove a favourite |

## Getting Started

### Prerequisites

- Node.js 18 or later
- A free Neon PostgreSQL database
- A free Clerk application
- Expo Go on a phone, or an emulator

### Backend

```bash
git clone https://github.com/mosesaderemi/recipe-app.git
cd recipe-app/backend
npm install
```

Create `backend/.env`:

```bash
PORT=5001
DATABASE_URL=your_neon_connection_string
NODE_ENV=development
API_URL=your_deployed_api_health_url
```

`API_URL` is only used by the keep-alive job in production. Apply the schema with `npx drizzle-kit push`, then start the server:

```bash
npm run dev
```

### Mobile app

```bash
cd mobile
npm install
```

Create `mobile/.env`:

```bash
EXPO_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
```

Set `API_URL` in `mobile/constants/api.js` to your backend address. When testing on a physical phone, use your computer's local network address or the deployed URL instead of `localhost`. Then start Expo:

```bash
npx expo start
```

## Author

Aderemi Moses Timileyin
GitHub: [@mosesaderemi](https://github.com/mosesaderemi)
LinkedIn: [linkedin.com/in/mosesaderemi-6a70a5405](https://www.linkedin.com/in/mosesaderemi-6a70a5405)

## License

Released under the MIT License. See the `LICENSE` file for details.
