# semantic-release-workshop-test

A **Node.js + Express** showcase app demonstrating how to integrate **Semantic Release**, **Conventional Commits**, and **Husky** into a real-world project. Use this repo as a template for your own projects.

---

## 🚀 Tech Stack

| Tool | Purpose |
|------|---------|
| [Node.js](https://nodejs.org/) + [Express](https://expressjs.com/) | Web application framework |
| [Semantic Release](https://semantic-release.gitbook.io/) | Automated versioning and changelog generation |
| [Conventional Commits](https://www.conventionalcommits.org/) | Standardised commit message format |
| [Commitlint](https://commitlint.js.org/) | Enforces Conventional Commit format at commit-time |
| [Husky](https://typicode.github.io/husky/) | Git hooks to run linting/tests before commits |
| [ESLint](https://eslint.org/) | JavaScript linting |
| [Jest](https://jestjs.io/) + [Supertest](https://github.com/ladjs/supertest) | Unit and integration testing |

---

## 📁 Project Structure

```
├── src/
│   ├── app.js            # Express application setup
│   ├── server.js         # Entry point (starts HTTP server)
│   └── routes/
│       ├── index.js      # Home + health check routes
│       └── users.js      # CRUD routes for users
├── tests/
│   └── app.test.js       # Integration tests
├── .husky/
│   ├── commit-msg        # Runs commitlint on commit messages
│   └── pre-commit        # Runs lint + tests before every commit
├── .github/
│   └── workflows/
│       └── release.yml   # CI/CD: test → release pipeline
├── .commitlintrc.json    # Commitlint configuration
├── .eslintrc.json        # ESLint configuration
├── .releaserc.json       # Semantic Release configuration
└── package.json
```

---

## ⚡ Quick Start

```bash
# Clone the repo
git clone https://github.com/HasithaPriyasad/semantic-release-workshop-test.git
cd semantic-release-workshop-test

# Install dependencies (also sets up Husky hooks automatically)
npm install

# Start the development server
npm run dev

# Or start in production mode
npm start
```

The API will be available at `http://localhost:3000`.

---

## 📡 API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/` | API information and available endpoints |
| GET | `/health` | Health check |
| GET | `/users` | List all users |
| GET | `/users/:id` | Get user by ID |
| POST | `/users` | Create a new user |
| PUT | `/users/:id` | Update an existing user |
| DELETE | `/users/:id` | Delete a user |

### Example Requests

```bash
# Health check
curl http://localhost:3000/health

# Get all users
curl http://localhost:3000/users

# Create a user
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com", "role": "user"}'

# Update a user
curl -X PUT http://localhost:3000/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name": "John Updated"}'

# Delete a user
curl -X DELETE http://localhost:3000/users/1
```

---

## 🛠 Development Workflow

### Running Tests

```bash
npm test              # Run all tests with coverage
```

### Linting

```bash
npm run lint          # Check for lint errors
npm run lint:fix      # Auto-fix lint errors
```

---

## 📝 Conventional Commits

This project enforces the [Conventional Commits](https://www.conventionalcommits.org/) specification via **Commitlint** + **Husky**. Every commit message must follow this format:

```
<type>(<optional scope>): <subject>
```

### Allowed Types

| Type | Description | Semantic Release Effect |
|------|-------------|------------------------|
| `feat` | A new feature | Bumps **MINOR** version |
| `fix` | A bug fix | Bumps **PATCH** version |
| `perf` | Performance improvement | Bumps **PATCH** version |
| `docs` | Documentation only | No release |
| `style` | Formatting changes | No release |
| `refactor` | Code refactoring | No release |
| `test` | Adding or updating tests | No release |
| `build` | Build system changes | No release |
| `ci` | CI/CD changes | No release |
| `chore` | Maintenance tasks | No release |
| `revert` | Revert a commit | Bumps **PATCH** version |

> **Breaking Change** → Bumps **MAJOR** version. Add `!` after the type or add `BREAKING CHANGE:` in the footer.

### Examples

```bash
# Feature (MINOR bump: 1.0.0 → 1.1.0)
git commit -m "feat(users): add pagination support"

# Bug fix (PATCH bump: 1.0.0 → 1.0.1)
git commit -m "fix(users): handle missing email validation"

# Breaking change (MAJOR bump: 1.0.0 → 2.0.0)
git commit -m "feat!: redesign user API response format"

# Documentation (no release)
git commit -m "docs: update API endpoint examples"
```

---

## 🔖 Semantic Release

Semantic Release automates the entire release workflow:

1. **Analyzes commits** since the last release using Conventional Commits
2. **Determines the next version** (major/minor/patch) automatically
3. **Generates a CHANGELOG.md** from commit messages
4. **Updates `package.json`** version
5. **Creates a GitHub Release** with release notes
6. **Commits release artifacts** back to the repository

### Configuration (`.releaserc.json`)

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    ["@semantic-release/changelog", { "changelogFile": "CHANGELOG.md" }],
    ["@semantic-release/npm", { "npmPublish": false }],
    ["@semantic-release/git", { "assets": ["CHANGELOG.md", "package.json"] }],
    "@semantic-release/github"
  ]
}
```

Releases are triggered automatically on every push to `main` via the GitHub Actions workflow.

---

## 🪝 Husky Git Hooks

Husky runs two hooks automatically:

| Hook | Trigger | Action |
|------|---------|--------|
| `pre-commit` | Before every `git commit` | Runs `npm run lint` + `npm test` |
| `commit-msg` | After writing a commit message | Runs `commitlint` to validate the message |

If either hook fails, the commit is **rejected** — keeping the codebase clean.

---

## ⚙️ CI/CD Pipeline (GitHub Actions)

The workflow in `.github/workflows/release.yml` runs on every push/PR to `main`:

```
push to main
    └── test job
        ├── npm ci
        ├── npm run lint
        └── npm test
            └── release job (main branch only)
                └── npx semantic-release
```

### Required Secrets

| Secret | Description |
|--------|-------------|
| `GITHUB_TOKEN` | Automatically provided by GitHub Actions |

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/my-feature`
3. Make your changes
4. Commit using Conventional Commits: `git commit -m "feat: add my feature"`
5. Push and open a Pull Request

---

## 📜 License

MIT