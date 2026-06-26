# Snip - Decision Log

This document records key technical and product decisions, why they were made,
and their consequences.

## ADR-001: No anonymous link shortening

**Context:** A URL shortener could allow anyone to shorten a link, or require
an account first. We needed to decide whether shortening is open to anonymous
visitors or gated behind login.

**Decision:** Only logged-in users can shorten URLs. Following a short code
(the redirect) remains open to anyone.

**Rationale:** Allowing anonymous shortening opens the system to abuse. A script
could create unlimited junk links with no accountable owner, flooding the
database. Gating creation behind an account makes every link traceable to a user
and simplifies the data model, since every link always has an owner.

**Consequences:** Creating a link now requires authentication (the write path is
gated), while the redirect stays public (the read path is open). The link's owner
field can be NOT NULL at the database level, enforcing the rule in the schema.
The tradeoff is higher friction - a first-time user must register before they can
shorten anything, which may reduce casual use.

## ADR-002: Non-sequential short codes

**Context:** Short codes are public and appear in URLs. We needed to decide how codes are generated - sequentially (aaa, aab, aac...) or unpredictably.

**Decision:** Short codes will be generated non-sequentially, from a random or hashed source, so consecutive codes are unrelated.

**Rationale:** Sequential codes (the rejected option) would let an attacker walk the sequence - aaa, aab, aac - and reach every other user's links by counting. Non-sequential codes can't be enumerated this way.

**Consequences:** Random codes can collide, two links generating the same code. The system must detect collisions (check if a code already exists before assigning it) and regenerate when one occurs.

## ADR-003: 404 for missing short codes

**Context:** When a visitor follows a short code, the code may not exist - it was never created, was mistyped, or has been deleted. We needed to decide how the system responds to a request for a non-existent code.

**Decision:** The system responds with HTTP 404 Not Found when a requested short code does not exist.

**Rationale:** A 404 is the web's standard "not found" signal - it tells the visitor the short code doesn't exist. The rejected alternative was redirecting to a custom path like /notexist, which is wrong because routing to that page returns an HTTP 200 (success) response for a request that actually failed, which is fundamentally incorrect.

**Consequences:** A visitor requesting a missing code gets a 404, so we'll want a proper branded "link not found" page rather than a raw browser error (friendly on the surface, correct 404 status underneath). This same behaviour also covers deletion: when a user deletes a link (Story 8), its short code stops existing and following it returns the same 404 - so one rule handles both "never existed" and "deleted."

## ADR-004: Login non-disclosure on failed authentication

**Context:** When a login attempt fails, the system can either tell the user exactly what went wrong (wrong email vs wrong password) or give a single generic message. We needed to decide which.

**Decision:** On failed login, the system returns a generic "invalid credentials" message for both wrong-email and wrong-password cases, without distinguishing which was incorrect.

**Rationale:** Telling a user "this email isn't registered" versus "wrong password" reveals whether an email is registered in the system, enabling user enumeration - the same enemy as ADR-002, here applied to accounts rather than links. A generic message gives an attacker no way to tell registered emails from unregistered ones, since every failed attempt returns the same response.

**Consequences:** There's a small usability cost - a legitimate user who genuinely forgot whether they registered gets less specific help. We accept this tradeoff because the same information that would help an honest user equally helps an attacker probing for valid accounts, so security wins over convenience.

## ADR-005: Redirect performance target

**Context:** The redirect is the system's hot path - a link is created once but followed many times. We needed a defined performance target.

**Decision:** Redirects must resolve in under 100ms via a single indexed database lookup.

**Rationale:** 100ms is below the threshold where a human perceives delay, so the redirect feels instant. It is realistically achievable with a single indexed lookup, unlike a tighter target such as 20ms, which could not be guaranteed once database and network time are accounted for - and any number we commit to must be one we can defend.

**Consequences:** The short-code column must be indexed, or lookups slow down as the table grows. The target also rules out slow per-request work on the redirect path - synchronous external calls, heavy computation, or making the redirect wait on click-tracking writes - which foreshadows a Sprint 2 decision: click tracking must not block the redirect.