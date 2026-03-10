# Roadmap

This document outlines planned features, improvements, and known limitations for Crustulum Bucket 2.0.

---

## ✅ Currently Implemented

- User signup and login with JWT authentication
- Create, read, update, and delete (CRUD) pastes
- UUID-based capability URLs — pastes are readable by anyone with the link, no account required
- Automatic paste expiry with a server-side cleanup job
- Configurable paste lifetime (`PASTE_PERIOD`) and per-user paste cap (`PASTE_PER_USER`)
- Copy-to-clipboard for share links
- Dark / light theme toggle
- GraphQL API with input validation and structured error handling
- Database seed script for local development

---

## 🚧 Planned Features

### User Experience
- [ ] **Syntax highlighting** — detect and highlight code snippets (e.g. via [Prism.js](https://prismjs.com/) or [highlight.js](https://highlightjs.org/))
- [ ] **Paste titles** — allow users to give pastes a human-readable name in addition to the UUID
- [ ] **Paste visibility toggle** — option for "public" vs "private" pastes (public pastes listed in a directory)
- [ ] **Paste tags / labels** — categorize pastes for easier organization
- [ ] **Search** — allow users to search or filter their own pastes

### Sharing & Access
- [ ] **Password-protected pastes** — add an optional password layer on top of the UUID capability URL
- [ ] **One-time view pastes** — pastes that self-destruct after being read once
- [ ] **Custom expiry per paste** — let users set a per-paste expiry instead of a global default

### Account & Auth
- [ ] **Password reset via email** — send a reset link for forgotten passwords
- [ ] **Account deletion** — allow users to permanently delete their account and all associated pastes
- [ ] **OAuth / social login** — support login via GitHub or Google

### Infrastructure & Performance
- [ ] **Rate limiting** — prevent abuse by throttling API requests per IP or per user
- [ ] **Pagination** — paginate the paste list on the dashboard instead of loading all pastes at once
- [ ] **Deployment guide** — document deployment steps for Render, Railway, or Fly.io (Heroku free tier discontinued)
- [ ] **CI/CD pipeline** — add GitHub Actions workflow for automated linting and testing

### Code Quality
- [ ] **Test suite** — add unit and integration tests for resolvers and React components
- [ ] **ESLint configuration** — add a consistent linting setup across client and server
- [ ] **TypeScript migration** — optionally migrate to TypeScript for improved type safety

---

## 🐛 Known Issues / Limitations

- The `PASTE_PERIOD` environment variable is documented as **minutes** in `server/.env` (see `server/models/Paste.js` line 6 and `server/utils/cleanup.js` line 4), but the `misc/api-doc.org` developer notes describe it as **seconds** — verify the intended unit before deploying.
- The Heroku deployment link that was in the previous README is no longer active. A new deployment target is needed (see [Roadmap](#-planned-features) for suggested platforms).
- The seed script bypasses Mongoose `pre('save')` hooks by using `collection.insertMany`, so seeded users will have **unhashed passwords** and cannot log in. Use `Paste.create` / `User.create` for correct seeding.
- `robots.txt` does not yet list the `/paste/` path as disallowed, which is recommended for capability URL security per the [W3C Capability URLs spec](https://w3ctag.github.io/capability-urls/#application-design).
