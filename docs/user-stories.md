# Snip - User Stories

## Story 1 - Shorten a URL

> As a logged-in user, I want to paste a long URL and get a short code back, so that I can share a link that's short enough to use anywhere.

**Acceptance Criteria:**
- Given a valid URL, when I submit it, then I receive a short code that maps to it.
- Given an invalid string that isn't a URL, when I submit it, then I get an error and no code is created.

## Story 2 - Redirect

> As a visitor, I want to be redirected to the original URL when I follow a short code, so that I reach the destination the sharer intended without dealing with a long, opaque link.

**Acceptance Criteria:**
- Given a short code that exists, when a visitor requests it, then they are redirected to the original URL.
- Given a short code that does not exist, when a visitor requests it, then the system responds with 404 Not Found.

## Story 3 - Register

> As a visitor, I want to create an account, so that the links I shorten are saved and I can manage them later.

**Acceptance Criteria:**
- Given valid registration details, when I submit them, then an account is created and I'm logged in.
- Given an email that's already registered, when I submit it, then I get an error and no duplicate account is created.
- Given an invalid email format, when I submit it, then I get a validation error.

## Story 4 - Log in

> As a registered user, I want to log in, so that I can access the links saved to my account.

**Acceptance Criteria:**
- Given correct credentials, when I log in, then I gain access to my account.
- Given incorrect credentials, when I log in, then I'm rejected with an error and not told which field was wrong.

## Story 5 - Manage my links

> As a logged-in user, I want to manage the links I own, so that I have one place to view, track, and remove them.

**Acceptance Criteria:**
- Given I'm logged in, when I open my links area, then I can access the links I own and the actions available on them.
- Given a link I don't own, when I try to act on it, then I'm denied.

## Story 6 - List my links

> As a logged-in user, I want to see a list of all the links I've created, so that I have one place to manage them.

**Acceptance Criteria:**
- Given I'm logged in, when I open my links page, then I see all links I created.
- Given I'm logged in, when I open my links page, then I do not see links created by other users.

## Story 7 - View click statistics

> As a logged-in user, I want to see how many times each of my links has been clicked, so that I can tell which links people actually use.

**Acceptance Criteria:**
- Given a link I own, when I view its stats, then I see its total click count.
- Given a link I don't own, when I try to view its stats, then I'm denied.

## Story 8 - Delete a link

> As a logged-in user, I want to delete a link I created, so that I can remove links I no longer want active.

**Acceptance Criteria:**
- Given a link I own, when I delete it, then it's removed and its short code no longer redirects.
- Given a link I don't own, when I try to delete it, then I'm denied.