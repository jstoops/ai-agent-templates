# [project name] MVP Plan

## Delivery Rules

- Work through the parts in order. A part is complete only when its checklist, tests, and success criteria are satisfied.
- Do not begin Part 2 until the user formally approves this plan.
- Do not begin Part 6 until the user formally approves the database design from Part 5.
- Keep credentials and `.env` files out of source control and browser bundles.
- Automated AI tests must mock OpenRouter HTTP responses. A separate opt-in manual smoke-test command will make the live `2+2` request.

## Part 1: Plan and Frontend Inventory

- [ ] Review the root project requirements.
- [ ] Review the existing frontend application, test setup, and current behavior.
- [ ] Create `frontend/AGENTS.md` documenting the existing frontend structure and conventions.
- [ ] Expand this document into implementation checklists, test expectations, and success criteria.
- [ ] Obtain formal user approval of this plan before starting Part 2.

### Tests

- Confirm documentation accurately identifies the current frontend commands and behavior.

### Success Criteria

- This plan has formal user approval.
- The frontend-specific instructions accurately describe the existing demo and its tests.

## Part 2: [container], [middleware], and Local Scripts

- [ ] Inspect backend and script-specific instructions before making changes in those directories.
- [ ] Create the [programming language] project metadata for [dependency manager] and [middleware] in `backend/`.
- [ ] Implement a minimal [middleware] application with a health endpoint and an example API endpoint.
- [ ] Configure the backend to serve a temporary static hello-world page at `/`.
- [ ] Create a [container] configuration that installs [programming language] dependencies with [dependency manager] and exposes the application port.
- [ ] Add documented start and stop scripts for Windows, macOS, and Linux in `scripts/`.
- [ ] Ensure scripts load root `.env` values without exposing them in output.

### Tests

- [ ] Backend unit test: health endpoint returns a successful response.
- [ ] Backend unit test: example API endpoint returns its documented JSON response.
- [ ] Container smoke test: build the image, start it, request `/` and the example API endpoint, then stop it.

### Success Criteria

- A single [container] instance starts locally using the platform-appropriate script.
- `/` returns the temporary static page and the API endpoint responds successfully.
- The container can be stopped by the corresponding stop script.

## Part 3: Serve the Existing Frontend

- [ ] Configure Next.js for a static production export compatible with [middleware] static-file serving.
- [ ] Update [container] build stages to install frontend dependencies, create the static export, and copy only the built assets to the runtime image.
- [ ] Replace the temporary root page with the existing [project name] application.
- [ ] Configure [middleware] static serving and fallback behavior required by the exported frontend.
- [ ] Preserve the existing frontend's features and interactive behavior.

### Tests

- [ ] Run existing frontend unit tests.
- [ ] Run existing Playwright tests against the container-served application.
- [ ] Add an integration check that `/` is served by [middleware] and displays the [project name] heading.

### Success Criteria

- The [container]-served root route displays the existing [project name] demo.
- All existing frontend unit and end-to-end behavior passes in the packaged application.

## Part 4: MVP Authentication

- [ ] Define a minimal server-managed authentication mechanism suitable for the local MVP.
- [ ] Implement a login endpoint accepting only `user` and `password`.
- [ ] Store authenticated state in a secure session mechanism; do not place the password in frontend code or storage.
- [ ] Gate application routes and API access behind authentication.
- [ ] Add a login view matching the existing visual language.
- [ ] Add logout and return the user to the login view.
- [ ] Preserve [project name] data for the authenticated browser session (originally in memory; superseded by [DB type] persistence in Part 6).

### Tests

- [ ] Backend unit tests for successful login, rejected credentials, authenticated access, unauthenticated rejection, and logout.
- [ ] Frontend tests for login form validation and logout controls.
- [ ] Playwright flow for login, application visibility, logout, and protected-route behavior.
- [ ] Playwright test that data changes survive a page reload within the browser session.

### Success Criteria

- Visiting `/` while unauthenticated presents login.
- `user` / `password` grants access to the application.
- Logout removes application access until the user logs in again.
- Data changes remain available after a reload and re-login through [DB type] persistence.

## Part 5: Database Design Approval

- [ ] Propose a [DB type] schema supporting multiple users, each owning their own data.
- [ ] Model the application's core entities, relationships, ordering, and timestamps needed for persistent edits.
- [ ] Define how initial seed data is created for a new user.
- [ ] Define JSON representations used by the API and AI features.
- [ ] Document schema, migration/initialization approach, constraints, and example payloads in `docs/`.
- [ ] Obtain formal user approval before implementing persistence.

### Tests

- Validate proposed example JSON against the documented API model.
- Review schema constraints against data-ownership and ordering requirements.
- Define Part 6 tests that prove plaintext passwords are neither persisted, logged, nor returned, and that password hashes verify securely.

### Success Criteria

- The user formally approves the database design document.
- The design supports the MVP and does not preclude future multiple-user operation.

## Part 6: Persistent Data API

- [ ] Add [DB type] database initialization that creates the database and schema when absent.
- [ ] Add data-access code scoped to the authenticated user's data.
- [ ] Implement API routes to read, create, update, delete, and reorder the application's core entities.
- [ ] Validate request payloads and return clear errors for invalid or unauthorized operations.
- [ ] Ensure every route operates only on data owned by the authenticated user.

### Tests

- [ ] Unit tests using an isolated temporary [DB type] database for initialization and first-run seed data creation.
- [ ] API tests for reads, create/update/delete, reordering, validation failures, and authentication boundaries.
- [ ] Test that changes remain after a new application/database session.
- [ ] Test that passwords are Argon2id-hashed before persistence and are never stored, returned, or logged as plaintext.
- [ ] Test that the frontend does not retain passwords in browser storage; require HTTPS for non-local authentication traffic.

### Success Criteria

- The database is created automatically when missing.
- API changes persist and are returned in the documented JSON format.
- One user's data cannot be read or changed through another user's session.

## Part 7: Frontend Persistence Integration

- [ ] Load application data from the authenticated backend API rather than static demo data at runtime.
- [ ] Replace local-only mutations with API-backed operations.
- [ ] Handle loading, save failures, and retryable user feedback without losing a valid application state.
- [ ] Keep interactive operations (such as drag/drop or inline edits) responsive while preserving the server's resulting state.
- [ ] Retain the static demo data only as server-side initial seed data, if required.

### Tests

- [ ] Frontend unit tests mock API responses for loading, successful mutation, and failure states.
- [ ] Backend integration tests cover the API contract consumed by the UI.
- [ ] Playwright tests verify that edits survive a reload.
- [ ] Playwright regression tests cover ordering and other state-sensitive interactions persisting as intended.

### Success Criteria

- All edits made through the UI persist across a browser reload.
- The UI displays a clear, recoverable state when an API operation fails.
- Interactive operations preserve the intended result after reload.

## Part 8: OpenRouter Connectivity

- [ ] Decision: validate connectivity with an explicit, live OpenRouter request rather than mocked HTTP tests. The smoke test must run in [container] and send `2+2` to the configured model.
- [ ] Add backend-only OpenRouter configuration using `OPENROUTER_API_KEY` and a configured model.
- [ ] Implement an OpenRouter client with timeouts and actionable error handling.
- [ ] Add an opt-in manual smoke-test command that sends `2+2` to OpenRouter and reports the response without printing credentials.
- [ ] Keep the smoke-test command separate from the normal automated test suite.

### Tests

- [ ] The explicit smoke-test command makes a live request; it does not mock OpenRouter HTTP responses.
- Windows: run `./scripts/smoke-openrouter.ps1`. macOS/Linux: run `./scripts/smoke-openrouter.sh`.
- [ ] Manual smoke test: with a valid root `.env`, the explicit command returns `4` from the configured model.

### Success Criteria

- Automated tests run without network access or an API key.
- The documented manual smoke-test command confirms live OpenRouter connectivity when intentionally invoked.

## Part 9: AI Commands and Structured Output

- [ ] Define a versioned structured response schema containing assistant text and an optional patch-style data update.
- [ ] Include the relevant current application state as JSON, the authenticated user's question, and bounded conversation history in each model request.
- [ ] Validate model output against the schema before persisting any change.
- [ ] Apply valid AI-requested changes through the same service layer used by the application APIs.
- [ ] Reject invalid, unauthorized, or malformed AI changes without altering stored data.

### Tests

- [ ] Mocked OpenRouter tests for conversational replies without updates, valid single updates, multiple updates, and malformed output.
- [ ] Database/API tests confirm valid AI updates persist atomically and invalid updates make no changes.
- [ ] Test the request payload includes current application state and bounded history.

### Success Criteria

- The backend returns validated assistant text and, when appropriate, persisted updated data.
- No model response can mutate data unless it conforms to the documented structured schema.

## Part 10: AI Chat Sidebar

- [ ] Design and implement a responsive sidebar integrated with the authenticated application workspace.
- [ ] Display conversation history, message submission, pending state, errors, and assistant responses.
- [ ] Send chat requests to the authenticated backend endpoint.
- [ ] Refresh application state from the API after a successful AI-driven update.
- [ ] Ensure the desktop sidebar and mobile presentation preserve access to both chat and the main application controls.

### Tests

- [ ] Component tests for message submission, loading, error, and assistant-response states.
- [ ] Mocked integration tests for chat responses with and without data updates.
- [ ] Playwright tests verify that an AI update is reflected in the UI without a manual reload.

### Success Criteria

- A signed-in user can hold a chat conversation from the application workspace.
- Valid AI changes are visible in the UI immediately after the response.
- The completed application runs locally in [container] and all automated checks pass without live AI calls.
