# Crustulum Bucket 2.0

> A full-stack pastebin web application — create, share, and manage expiring text snippets with secure shareable links.

![Home Page Screenshot](./client/public/images/Crustbuckethp.png)

---

## Table of Contents

1. [About the Project](#about-the-project)
2. [Features](#features)
3. [Tech Stack](#tech-stack)
4. [How It Works](#how-it-works)
5. [Getting Started](#getting-started)
   - [Prerequisites](#prerequisites)
   - [Installation](#installation)
   - [Environment Variables](#environment-variables)
   - [Seeding the Database](#seeding-the-database)
   - [Running Locally](#running-locally)
6. [Project Structure](#project-structure)
7. [API Overview](#api-overview)
8. [Screenshots](#screenshots)
9. [Roadmap](#roadmap)
10. [Contributors](#contributors)
11. [License](#license)

---

## About the Project

**Crustulum Bucket 2.0** is a full-stack pastebin application that lets users store, manage, and share text snippets via secure, shareable links — no account required for viewers.

Pastes are stored temporarily and automatically deleted after a configurable expiry period. Each paste is assigned a UUID-based capability URL, meaning anyone with the link can read the paste without logging in, while only the owner can edit or delete it.

This project was built to demonstrate a production-style MERN stack with a GraphQL API layer, JWT authentication, and automated server-side cleanup jobs.

---

## Features

- 🔐 **User authentication** — secure signup and login with JWT and bcrypt-hashed passwords
- 📝 **Create pastes** — write and store text snippets up to 10,000 characters
- ✏️ **Edit pastes** — update your own pastes at any time
- 🗑️ **Delete pastes** — remove pastes from your account
- 🔗 **Shareable links** — each paste gets a UUID-based URL; anyone with the link can view it, no account needed
- ⏱️ **Auto-expiry** — pastes are automatically deleted after a configurable time period
- 📋 **Copy to clipboard** — one-click copying of the paste's share link
- 🌗 **Dark / Light theme toggle** — switch between themes from the header
- 🚦 **Per-user paste limits** — configurable cap on how many pastes a user can hold at once
- 🌱 **Database seeding** — seed script included for local development and testing

---

## Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| [React](https://reactjs.org/) | UI framework |
| [Apollo Client](https://www.apollographql.com/docs/react/) | GraphQL client and cache |
| [React Router](https://reactrouter.com/) | Client-side routing |
| [Bootstrap](https://getbootstrap.com/) | Responsive layout and CSS utilities |
| [Font Awesome](https://fontawesome.com/) | Icons |

### Backend
| Technology | Purpose |
|---|---|
| [Node.js](https://nodejs.org/) | JavaScript runtime |
| [Express](https://expressjs.com/) | HTTP server |
| [Apollo Server](https://www.apollographql.com/docs/apollo-server/) | GraphQL server |
| [MongoDB](https://www.mongodb.com/) | NoSQL database |
| [Mongoose](https://mongoosejs.com/) | MongoDB ODM |
| [JSON Web Tokens](https://github.com/auth0/node-jsonwebtoken) | Authentication tokens |
| [bcrypt](https://github.com/kelektiv/node.bcrypt.js) | Password hashing |
| [UUID](https://www.npmjs.com/package/uuid) | Paste capability URL generation |
| [dotenv](https://www.npmjs.com/package/dotenv) | Environment variable management |

---

## How It Works

1. A user **signs up** with an email and password. Passwords are validated for strength and stored as bcrypt hashes.
2. On login, the server issues a **JWT** stored in `localStorage`. The token is attached to every subsequent GraphQL request via an `Authorization` header.
3. When a user **creates a paste**, the server generates a UUID and stores the paste in MongoDB with a reference to the owner and a creation timestamp.
4. The paste's **share URL** (`/paste/<uuid>`) is a capability URL — anyone with the link can read the paste without being logged in.
5. A **server-side cleanup job** runs every minute and permanently deletes pastes whose age exceeds the configured `PASTE_PERIOD`.
6. Users can **edit or delete** their own pastes from the home dashboard. Authorization checks ensure users can only modify their own content.

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) v18 or higher
- [MongoDB](https://www.mongodb.com/) running locally or a [MongoDB Atlas](https://www.mongodb.com/atlas) connection string

### Installation

1. Clone the repository:

```sh
git clone https://github.com/coleyrockin/Crustulum-Bucket-2.0.git
cd Crustulum-Bucket-2.0
```

2. Install root-level dependencies:

```sh
npm install
```

3. Install client and server dependencies:

```sh
npm run install
```

### Environment Variables

Create a `.env` file in the `./server` directory:

```
JWT_SECRET='your-super-secret-jwt-key'
PASTE_PER_USER=100
PASTE_PERIOD=1440
```

| Variable | Type | Description |
|---|---|---|
| `JWT_SECRET` | string | Secret key used to sign and verify JWTs |
| `PASTE_PER_USER` | integer | Maximum number of active pastes a user can hold |
| `PASTE_PERIOD` | integer | Paste lifetime in **minutes** before automatic deletion (e.g. `1440` = 24 hours) |

> If `PASTE_PERIOD` is not set, pastes default to a 24-hour lifespan.

### Seeding the Database

Populate the database with sample users and pastes for development:

```sh
npm run seed
```

### Running Locally

Start both the client (React) and server (Express/Apollo) in development mode:

```sh
npm run develop
```

- React app: `http://localhost:3000`
- GraphQL playground: `http://localhost:3001/graphql`

To run the server in production mode:

```sh
npm start
```

---

## Project Structure

```
Crustulum-Bucket-2.0/
├── client/                    # React frontend
│   ├── public/
│   │   └── images/            # Static images and icons
│   └── src/
│       ├── components/
│       │   ├── Header/        # Navigation bar with theme toggle
│       │   ├── Footer/        # Page footer
│       │   ├── PasteForm/     # Create-paste form
│       │   └── PasteList/     # Dashboard list of user's pastes
│       ├── pages/
│       │   ├── Home.js        # Dashboard (paste list or login prompt)
│       │   ├── Paste.js       # Create new paste page
│       │   ├── SinglePaste.js # Public paste viewer (shareable)
│       │   ├── UpdatePaste.js # Edit paste page
│       │   ├── Login.js       # Login form
│       │   └── Signup.js      # Registration form
│       └── utils/
│           ├── auth.js        # JWT helpers (login, logout, token decode)
│           ├── queries.js     # Apollo GraphQL queries
│           ├── mutations.js   # Apollo GraphQL mutations
│           └── date.js        # Date formatting utility
├── server/                    # Node/Express/Apollo backend
│   ├── config/
│   │   └── connection.js      # MongoDB connection
│   ├── models/
│   │   ├── User.js            # User schema (email, password, pastes)
│   │   └── Paste.js           # Paste schema (text, uuid, owner, created)
│   ├── schema/
│   │   ├── typeDefs.js        # GraphQL type definitions
│   │   ├── resolvers.js       # GraphQL resolvers with auth/validation
│   │   └── index.js           # Schema export
│   ├── seeds/
│   │   └── seed.js            # Database seed script
│   ├── utils/
│   │   ├── auth.js            # JWT middleware and token signing
│   │   └── cleanup.js         # Automated paste expiry job
│   └── server.js              # Express + Apollo server entry point
├── misc/
│   └── api-doc.org            # Developer notes and API reference
├── package.json               # Root scripts and dependencies
└── README.md
```

---

## API Overview

The app uses a GraphQL API served at `/graphql`.

### Queries

| Query | Auth Required | Description |
|---|---|---|
| `me` | ✅ Yes | Returns the logged-in user's profile and paste list |
| `readPaste(input)` | ❌ No | Fetches a single paste by UUID |

### Mutations

| Mutation | Auth Required | Description |
|---|---|---|
| `signup(input)` | ❌ No | Creates a new user account and returns a JWT |
| `login(input)` | ❌ No | Authenticates a user and returns a JWT |
| `createPaste(input)` | ✅ Yes | Creates a new paste for the logged-in user |
| `updatePaste(input)` | ✅ Yes | Updates the text of an existing owned paste |
| `deletePaste(input)` | ✅ Yes | Permanently deletes an owned paste |

> **Note:** All mutations enforce ownership — users can only modify or delete their own pastes.

---

## Screenshots

| Page | Description |
|---|---|
| ![Home](./client/public/images/Crustbuckethp.png) | Home dashboard showing a user's paste list |

> 💡 **Suggested screenshots to add:** login page, create paste form, single paste view with share link, mobile view.

---

## Roadmap

See [ROADMAP.md](./ROADMAP.md) for planned features and improvements.

---

## Contributors

| GitHub | Role |
|---|---|
| [@coleyrockin](https://github.com/coleyrockin) | Contributor |
| [@rrrbbbsss](https://github.com/rrrbbbsss) | Contributor |
| [@swvmpdad](https://github.com/swvmpdad) | Contributor |
| [@T-rummy](https://github.com/T-rummy) | Contributor |

---

## License

This project is licensed under the [ISC License](./LICENSE).

---

> **Suggested GitHub repo description:** `A full-stack pastebin app built with React, GraphQL, and MongoDB — create shareable, auto-expiring text snippets with JWT authentication.`
>
> **Suggested GitHub topics:** `react` `graphql` `mongodb` `nodejs` `apollo` `pastebin` `mern` `jwt` `express` `full-stack`
