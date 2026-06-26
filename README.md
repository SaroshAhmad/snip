# snip

A URL shortener with click analytics, a deliberately scoped full-stack build.

## Overview

Snip lets users shorten long URLs into short codes and track how often each link is clicked. It is built as a complete engineering exercise - taking a small, focused product from requirements and design through to a deployed full-stack application - rather than a code-only project.

## Tech Stack

- **Backend:** Spring Boot 3, Java 21, Maven
- **Frontend:** React, Vite, Tailwind CSS
- **Database:** PostgreSQL (Neon), Flyway migrations
- **Auth:** Supabase (JWT validation in Spring Boot)
- **Testing:** JUnit, Testcontainers
- **Deployment:** Render (backend), Vercel (frontend)

## Engineering Process

This project was built using a real engineering workflow, with planning and design documented before any code:

- [Requirements](docs/requirements.md) - functional and non-functional requirements
- [User Stories](docs/user-stories.md) - user stories with acceptance criteria
- [Decision Log](docs/decisions.md) - architecture decision records (ADRs) capturing key technical choices and their rationale

## Status

In active development. Sprint 0 (inception and design) complete - requirements, user stories, and architecture decisions documented. Sprint 1 (backend core) in progress.

## Getting Started

Setup instructions will be added as the backend and frontend are built.

## Live Demo

Coming soon - will be live once the application is deployed.