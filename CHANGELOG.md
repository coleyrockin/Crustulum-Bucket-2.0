# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [2.0.0] — Current

### Added
- Full MERN stack rewrite with a GraphQL API layer (Apollo Server + Apollo Client)
- JWT-based authentication with `jsonwebtoken` and `bcrypt` password hashing
- UUID capability URLs for pastes — shareable links that do not require a viewer account
- Server-side automated paste cleanup job running every minute
- Configurable environment variables: `JWT_SECRET`, `PASTE_PER_USER`, `PASTE_PERIOD`
- Input validation for email format, password strength (length, uppercase, lowercase, number, special character), and paste text length
- Structured resolver error handling with typed errors (`Authentication`, `Authorization`, `Accounting`, `InputValidation`, `NotFound`)
- Per-user paste count limit enforced at the API level
- Copy-to-clipboard button on paste view and dashboard
- Dark / light theme toggle via header logo click
- Database seed script using `@faker-js/faker`
- React frontend with React Router for client-side navigation
- Pages: Home (dashboard), Create Paste, View Paste (public), Edit Paste, Login, Signup

---

## [1.0.0] — Initial Version

- Initial project scaffolding
