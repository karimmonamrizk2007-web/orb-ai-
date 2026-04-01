# orb-aiORB AI is a cloud-first intelligent assistant platform designed to be useful today as a serious web product and ready later for physical embodiment through a Raspberry Pi device layer.

It is built around two clearly separated product experiences:

1. Assistant Layer for normal authenticated users
2. Owner Control Layer for protected device and operational management

The current repository already contains a working cloud application. Raspberry Pi integration is architecturally prepared, but real physical hardware execution is not complete and is not overclaimed in this project.

## What ORB AI Is

ORB AI is not a generic chatbot mockup and not yet a finished hardware appliance.

Today, ORB AI is:
- a Next.js web application deployed toward Vercel
- a Supabase-backed platform for authentication, persistence, storage, and realtime-ready state
- a user-facing assistant with persistent chat, streaming replies, multilingual UX, and a premium web interface
- a protected owner dashboard for device state, settings, logs, diagnostics, recognized users, and future hardware workflows

Later, ORB AI is intended to become:
- a physically embodied assistant powered by Raspberry Pi
- a cloud-connected device that can publish camera, microphone, speaker, LED, face, and diagnostic events into the same platform

## Current Product State

The working product today is the cloud layer.

That includes:
- authenticated assistant chat
- persistent sessions and message history
- server-side AI orchestration with the official OpenAI SDK and Responses API when configured
- optional OpenRouter participation in the server-side provider pool
- structured fallback and continuity behavior when live provider paths are degraded
- a protected owner dashboard with seeded demo support and simulation-safe device workflows
- responsive browser UI for desktop and mobile

What is not finished yet:
- real Raspberry Pi device execution
- live camera, microphone, speaker, and LED control from physical hardware
- a complete long-running cloud-to-device worker loop

## Two-Layer Architecture

### Layer 1: Assistant Layer

This is the primary user-facing product experience.

Users can:
- sign in
- chat with ORB AI
- return to saved conversations
- receive streamed AI replies
- use Arabic or English
- use browser voice capabilities where supported

This layer does not expose privileged owner or device controls.

Key files:
- `web/src/app/assistant/page.tsx`
- `web/src/components/assistant/assistant-workspace.tsx`
- `web/src/app/api/chat/route.ts`
- `web/src/lib/assistant/chat-route.ts`
- `web/src/lib/assistant/provider.ts`
- `web/src/lib/assistant/repository.ts`

### Layer 2: Owner Control Layer

This is the protected owner/admin experience.

Owners can:
- view the protected dashboard
- inspect device state
- manage owner-facing settings
- review logs and diagnostics
- review recognized and authorized face data
- use simulation-safe device actions
- reset Demo Mode state for presentation use

This layer is role-protected and intentionally separate from the public assistant flow.

Key files:
- `web/src/app/owner/page.tsx`
- `web/src/components/owner/owner-dashboard.tsx`
- `web/src/app/api/owner/devices/[deviceId]/settings/route.ts`
- `web/src/app/api/owner/devices/[deviceId]/commands/route.ts`
- `web/src/app/api/owner/demo/reset/route.ts`

### Future Device Runtime Layer

The web app does not talk to Raspberry Pi hardware directly.

Future hardware integration is reserved for the backend/device seam under:
- `backend/app/services/device/`
- `backend/app/services/hardware/`

This is where the real Pi bridge, local adapters, runtime loop, and cloud sync implementation belong.

## Current Working Features

### Assistant Product

- Supabase authentication and session persistence
- persistent chat sessions and message history
- streamed assistant replies through `/api/chat`
- bounded recent-context handling for conversation continuity
- centralized ORB AI system prompt
- Arabic and English support
- browser voice support where the browser exposes the required APIs
- continuity fallback when the live provider path is unavailable

### Owner Product

- role-protected owner route and owner APIs
- device registry and latest device-state presentation
- owner settings editing
- logs and diagnostics
- recognized-user and face-management surfaces
- simulation-safe device command flow
- honest future-facing camera and contract panels
- owner-only Demo Mode reset

### Demo And Graduation Support

- Demo Mode with seeded assistant sessions
- seeded owner dashboard data
- presentation-safe system status cards
- continuity messaging if provider or network conditions degrade
- a documented live-demo runbook in `reports/DEMO_RUNBOOK.md`

## AI Provider And Security Model

ORB AI keeps provider execution server-side only.

Current behavior:
- OpenAI can be used through the official SDK and Responses API via `OPENAI_API_KEY`
- OpenRouter remains supported as an alternate provider path in the server-side pool
- the provider layer can retry, classify failures, and fall back cleanly
- API keys are never exposed to the client

Relevant files:
- `web/src/lib/server-env.ts`
- `web/src/lib/assistant/provider.ts`
- `web/src/lib/assistant/generate-response.ts`
- `web/src/lib/assistant/chat-route.ts`

## What Is Planned For Raspberry Pi Later

The Raspberry Pi roadmap is real, but incomplete.

What is already prepared:
- typed cloud/device contracts
- a cloud-bridge seam
- a runtime seam
- skeletal local adapter contracts
- honest placeholder adapters
- owner-facing web panels that consume cloud-side device state only

What still needs real implementation:
- concrete Pi camera adapter
- concrete Pi microphone adapter
- concrete Pi speaker adapter
- concrete Pi LED adapter
- a real Supabase-backed device bridge
- a real device worker loop for heartbeat, command execution, preview publishing, and event sync
- device authentication and pairing flow

Relevant documentation:
- `reports/APP_TWO_LAYER_ARCHITECTURE.md`
- `reports/PI_INTEGRATION_READINESS.md`
- `reports/RPI_INTEGRATION_CONTRACT.md`

## Technology Stack

- Frontend: Next.js, React, TypeScript
- Platform: Supabase
- AI: OpenAI Responses API and optional OpenRouter provider pool
- Deployment target: Vercel
- Device seam: Python backend structure for future Raspberry Pi integration

## Repository Structure

- `web/` - primary web product
- `supabase/` - database migrations and schema changes
- `backend/` - future device bridge and hardware integration seam
- `mobile/` - companion/mobile work area
- `reports/` - architecture, readiness, QA, handoff, and demo documentation

## Running ORB AI Locally

### Prerequisites

- Node.js 20+
- npm
- Python 3.11+ for backend validation and future device-side work
- a Supabase project
- at least one server-side AI provider key for live assistant responses

### Environment Setup

Use [`.env.example`](./.env.example) as the canonical checklist.

Do not commit real credentials.

For the web app, the minimum useful environment groups are:

#### Public Supabase client

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`

#### Server-side Supabase access

- `SUPABASE_URL`
- `SUPABASE_SERVICE_ROLE_KEY`
- `DATABASE_URL` or `SUPABASE_ACCESS_TOKEN` for migrations

#### AI provider

At least one of:
- `OPENAI_API_KEY`
- `OPENROUTER_API_KEY`

Optional overrides:
- `OPENAI_MODEL`
- `OPENROUTER_MODEL`
- `AI_PROVIDER_POOL`
- `ASSISTANT_PROVIDER_TIMEOUT_MS`
- `ASSISTANT_PROVIDER_RETRY_COUNT`

#### Owner bootstrap

- `OWNER_BOOTSTRAP_EMAIL`
- `OWNER_BOOTSTRAP_PASSWORD`

#### Optional demo mode

- `DEMO_MODE`
- `NEXT_PUBLIC_DEMO_MODE`

Note:
- `.env.example` also contains future backend/device variables. Those are part of the broader repository roadmap and are not required for the normal web product to run locally.

### Local Web Setup

```bash
cd web
npm install
npm run db:migrate
npm run owner:bootstrap
npm run device:seed
npm run dev:lan
```

Primary local routes:
- `http://127.0.0.1:3000`
- `http://127.0.0.1:3000/auth`
- `http://127.0.0.1:3000/assistant`
- `http://127.0.0.1:3000/owner`
- `http://127.0.0.1:3000/settings`

### Demo Mode

Demo Mode is intended for predictable presentations and graduation review.

Useful command:

```bash
cd web
npm run demo:reset
```

Demo Mode provides:
- seeded chat history
- seeded owner/device state
- owner-only reset
- continuity behavior if the live provider path degrades

Recommended presentation guide:
- `reports/DEMO_RUNBOOK.md`

## Validation

Core web validation:

```bash
cd web
npm run lint
npx tsc --noEmit
npm run build
```

Useful additional checks:

```bash
cd web
npm run validate:ai
npm run test:e2e
```

Backend validation:

```bash
cd backend
.venv\\Scripts\\python.exe -m pytest -q
```

## Product Direction

ORB AI is being developed in this order:

1. stabilize the cloud-first assistant product
2. preserve the separation between Assistant Layer and Owner Control Layer
3. improve demo reliability, mobile quality, and usability
4. strengthen provider resilience and operational safety
5. connect a future Raspberry Pi runtime through the prepared backend bridge

This order is deliberate. It makes the product useful now, reviewable for graduation, and technically credible before hardware variability is introduced.

## Roadmap

### Near term

- improve regression coverage for critical assistant and owner flows
- continue tightening deployment readiness and demo rehearsal quality
- expand reliability checks for streaming, fallback, and persistence edge cases

### Next platform milestone

- implement the first concrete cloud-to-device bridge slice
- publish real device state into the protected owner dashboard
- replace simulation-only device data with bridge-fed truth incrementally

### Later physical embodiment milestone

- add real Raspberry Pi adapters for camera, microphone, speaker, and LED
- run a real device worker loop against the existing cloud contracts
- connect live hardware execution without collapsing the current web architecture

## Documentation

Recommended project documents:
- [reports/STATUS_REPORT.md](./reports/STATUS_REPORT.md)
- [reports/CODE_INVENTORY.md](./reports/CODE_INVENTORY.md)
- [reports/APP_TWO_LAYER_ARCHITECTURE.md](./reports/APP_TWO_LAYER_ARCHITECTURE.md)
- [reports/PI_INTEGRATION_READINESS.md](./reports/PI_INTEGRATION_READINESS.md)
- [reports/DEMO_RUNBOOK.md](./reports/DEMO_RUNBOOK.md)
- [reports/DEPLOYMENT_READINESS_AUDIT.md](./reports/DEPLOYMENT_READINESS_AUDIT.md)

## Review Note

This repository is being prepared as a graduation project and technical portfolio artifact. The README is intentionally written to be technically honest: the cloud product is real and working, the protected owner layer is real and working, and the Raspberry Pi path is prepared but not yet complete.-
