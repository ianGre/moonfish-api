
[![Build Status](https://img.shields.io/travis/rekallai/moonfish-api.svg?branch=master&style=flat-square)](https://travis-ci.org/rekallai/moonfish-api)
[![Dependencies Status](https://david-dm.org/rekallai/moonfish-api/status.svg)](https://david-dm.org/rekallai/moonfish-api)

_Disclaimer: This is experimental work in progress. Do not use this in any production ICOs yet._

# Moonfish API

Moonfish is an open source platform for doing Token Sales and Initial Coin Offerings (ICOs).

* ICO portal with marketing placeholders
* Whitelist signup and management
* Applicant communication
* Token sale controls (deadlines, limits)
* Token sale monitoring (projected funds raised, etc.)
* KYC workflow
* Legal templates for KYC compliance
* Security best practices: Not storing passwords, Magic Tokens, SSL, strong hashes, expiring JWT, etc.
* Full test coverage, quality code
* Deployable on any containerized environment

## Security & Reliability Features

* No user passwords are solicited or stored.
* No private keys or ethereum wallets are managed by the system.
* Uses temporary "magic tokens" to authenticate. Tokens are generated with a 512 random byte seed and SHA512
* Admin passwords are stored using BCRYPT
* Email verification using Postmark (DKIM and SPF)
* Full unit test coverage of business logic and APIs
* AirBnB ES lint standard enforced
* CI integration of unit tests and linting
* Auto check for misconfiguration (default keys) when running in production mode
* Client-server authentication is done using JSON Web Tokens. Different keys are used for admin accounts.
* Both JWT and and magic tokens expire within short time period (2h and 1h respectively)
* All solicited input fields are validated and that validation is unit tested
* Planned: Solicited information from users is stored in an encrypted way in DB
* Planned: Each user gets two secret words to authenticate official communication (anti-phishing)
* Planned: Strict CORS configuration to prevent cross-site contamination

## Directory Structure

* `package.json` - Configure dependencies
* `config/defaults.json` - Default configuration, all values can be controlled via env vars
* `dist/*` - Files generated by babel
* `src` - All source code
* `src/*/__tests__` - Unit tests
* `src/run.js` - Entrypoint for running and binding API
* `src/lib` - Library files like utils etc
* `src/api` - Express routes
* `src/middleware` - Middleware libs
* `src/models` - Models for ORM (Mongoose)
* `src/index.js` - Entrypoint into API (does not bind, so can be used in unit tests)

## API Routes

All routes are name spaced with a v1 version:

```
GET     /1/info                        # Get tokensale details and status
POST    /1/info/configuration          # Admin: Configure tokensale
POST    /1/applicants                  # Applicants: Apply to participate
POST    /1/applicants/sessions         # Applicants: Exchange `magicToken` for temp JWT token
POST    /1/applicants/register         # Applicants: Complete registration (finishes KYC)
POST    /1/applicants/participate      # Applicants: Store account info
POST    /1/users/sessions              # Admin: Create session / jwt (login)
GET     /1/users/self                  # Admin: Get my user info
DELETE  /1/users/self                  # Admin: Delete my account
POST    /1/users/self                  # Admin: Update my account
GET     /1/users/:user_id              # Admin: Admin: Get user
DELETE  /1/users/:user_id              # Admin: Admin: Delete user
POST    /1/users/:user_id              # Admin: Admin: Update user
```

## Install Dependencies

```
yarn install
```

## Testing & Linting

```
yarn test
yarn lint
```

## Running in Development

Code reload using nodemon:

```
yarn dev
```

## Configuration

All values in `config/defaults.json` can be overwritten using environment variables. For example `bind.host` becomes can be overwritten using the `API_BIND_HOST` environment variable.

- `API_BIND_HOST` - Host to bind to, defaults to `"0.0.0.0"`
- `API_BIND_PORT` - Port to bind to, defaults to `3005`
- `API_MONGO_URI` - MongoDB URI to connect to, defaults to `mongodb://localhost/skeleton_dev`
- `API_ADMIN_EMAIL` - Default root admin user `admin@moonfish.one`
- `API_ADMIN_PASSWORD` - Default root admin password `changeme`
- `API_JWT_SECRET` - Secret key for generating JWT tokens `changeme`
- `API_JWT_ADMINSECRET` - Secret key for generating admin JWT tokens `changeme`
- `API_APP_NAME` - Application name `Moonfish`
- `API_APP_DOMAIN` - Domain of token sale web interface `localhost`
- `API_POSTMARK_APIKEY` - Postmark API key - used for email communication
- `API_POSTMARK_FROM` - From address used for mail communication

## Building the Container

```
docker build -t ico-template-auction-api .
```

## Todo

- [x] Improve JWT configuration
- [x] Harden JWT tests
- [x] Remove certain user routes for security
- [x] Add info/details API
- [x] Core applicant logic + tests
- [x] Applicant API + tests
- [x] Tokensale Status core logic + tests
- [x] Add email delivery
- [x] Improve magic token
- [x] Add ethAmount validation
- [x] Allow JWT based access to register API call
- [x] Make sure magic tokens expire
- [x] Unit test for JWT expiry
- [x] Make sure application errors in prod when defaults are not changed
- [x] Allow oversubscribing (optionally)
- [x] Use native Node instead of babel
- [x] Set limits to the amount of ether that's whitelisted
- [x] Setup CI
- [ ] Setup coveralls code coverage reporting
- [ ] HTML email templates
- [ ] Improve documentation
- [ ] Add license information
- [ ] Add disclaimers
- [ ] Add unique communication keyphrase for each user
- [ ] Add improved CORS security
- [ ] Add improved encryption of applicant data
- [ ] Add settings admin API
- [ ] Add captcha security
