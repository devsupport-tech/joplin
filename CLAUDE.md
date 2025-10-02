# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Joplin is a multi-platform note-taking application built as a monorepo with the following key platforms:
- **Desktop** (Electron-based) - packages/app-desktop
- **Mobile** (React Native) - packages/app-mobile
- **Server** (Node.js/Koa) - packages/server
- **CLI** (Node.js) - packages/app-cli
- **Web Clipper** (Browser extension) - packages/app-clipper

## Build System & Package Management

The project uses **Yarn workspaces** (v4.9.2) with **Lerna** for monorepo management.

### Common Development Commands

```bash
# Build all packages (parallel - faster)
yarn buildParallel

# Build all packages (sequential - for debugging)
yarn buildSequential

# Run tests across all packages
yarn test

# Run tests for CI
yarn test-ci

# TypeScript compilation across all packages
yarn tsc

# Linting
yarn linter

# Interactive linting with fixes
yarn linter-interactive

# Watch mode for development
yarn watch

# Clean all build artifacts
yarn clean
```

### Package-Specific Commands

Navigate to individual packages to run specific commands:

```bash
# Desktop app development
cd packages/app-desktop
yarn start                    # Start in development mode
yarn build                    # Build the app
yarn test-ui                  # Run Playwright UI tests

# Mobile app development
cd packages/app-mobile
yarn start                    # Start React Native metro server
yarn android                  # Run on Android
yarn buildInjectedJs          # Build injected JavaScript

# Server development
cd packages/server
yarn start-dev                # Start in development mode with nodemon
yarn devCreateDb              # Create development database
yarn generateTypes            # Generate database types

# CLI development
cd packages/app-cli
yarn start                    # Start CLI application
```

## Architecture

### Core Architecture
- **packages/lib** - Shared core library containing models, services, and business logic
- **packages/utils** - Common utilities shared across all platforms
- **packages/renderer** - Markdown rendering engine
- **packages/editor** - Rich text editor component

### Application Packages
- **packages/app-desktop** - Electron desktop application
- **packages/app-mobile** - React Native mobile apps (iOS/Android)
- **packages/server** - Joplin Server for synchronization
- **packages/app-cli** - Command-line interface
- **packages/app-clipper** - Browser extension for web clipping

### Supporting Packages
- **packages/tools** - Build tools and development scripts
- **packages/generator-joplin** - Plugin generator
- **packages/pdf-viewer** - PDF viewing component
- **packages/htmlpack** - HTML packaging utilities

## Testing

### Running Tests
```bash
# All tests
yarn test

# Individual package tests
cd packages/[package-name]
yarn test

# Desktop UI tests (Playwright)
cd packages/app-desktop
yarn test-ui
```

### Test File Patterns
- Unit tests: `*.test.ts` or `*.test.js`
- Integration tests: Often in `tests/` directories
- Desktop UI tests: `tests-playwright/` directory

## Code Style & Linting

The project uses ESLint with TypeScript parser:

```bash
# Check linting issues
yarn linter-ci

# Fix linting issues automatically
yarn linter

# Interactive linting
yarn linter-interactive
```

### TypeScript
All packages use TypeScript. Compile with:
```bash
yarn tsc          # All packages
cd packages/[name] && yarn tsc  # Individual package
```

## Database & Synchronization

The core uses a SQLite-based architecture:
- **packages/lib/services/database** - Database layer
- **packages/lib/models** - Data models (Note, Notebook, Tag, etc.)
- **packages/lib/services/synchronizer** - Sync logic

Key models include:
- `BaseModel` - Base class for all models
- `Note` - Individual notes
- `Notebook` - Note containers
- `Tag` - Note tags
- `Resource` - File attachments

## Plugin System

Joplin has an extensive plugin system:
- **packages/lib/services/plugins** - Plugin engine
- **packages/generator-joplin** - Plugin generator tool
- Plugins are TypeScript/JavaScript modules with manifest files

## Development Guidelines

### Working with Shared Code
- Shared business logic belongs in `packages/lib`
- UI-agnostic utilities go in `packages/utils`
- Platform-specific code stays in respective app packages

### Adding Dependencies
- Add shared dependencies to the root `package.json`
- Add package-specific dependencies to individual `package.json` files
- Use exact versions for consistency

### Database Changes
- Database migrations are in `packages/lib/services/database/migrations`
- Update `packages/lib/models/` for schema changes
- Run `yarn generateTypes` in server package after schema changes

### Release Process
The project has automated release scripts:
```bash
yarn releaseDesktop    # Desktop releases
yarn releaseAndroid    # Android releases
yarn releaseServer     # Server releases
```

## Key Configuration Files

- **lerna.json** - Lerna monorepo configuration
- **tsconfig.json** - Root TypeScript configuration
- **.eslintrc.js** - ESLint configuration
- **gulpfile.js** - Gulp build tasks
- **package.json** - Root package with workspace configuration