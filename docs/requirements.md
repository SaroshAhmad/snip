# Snip - Requirements

## Functional Requirements

1. A logged-in user can shorten a long URL into a short code.
2. The system redirects a visitor from a short code to the original URL.
3. A user can register for an account.
4. A registered user can log in.
5. A logged-in user can manage the links they own.
6. A logged-in user can view a list of their links.
7. A logged-in user can view click statistics for their links.
8. A logged-in user can delete a link they created.

## Non-Functional Requirements

### Performance
- Redirects must resolve in under 100ms via a single indexed lookup.

### Security
- Short codes must be non-sequential and unguessable to prevent enumeration of other users' links

