# CampusFlow — implementation and design plan

## Approved scope

Build the approved **full student MVP** for CampusFlow, a responsive student web application for university and college students, with the tagline **“Learn. Connect. Trade. Grow.”** The attached source ends mid-list after the service example “Music”; implement only the product/service examples actually provided and do not invent additional marketplace categories or payment workflows. The MVP uses real student sign-in, persistent user-owned records, document storage, AI-backed study experiences, marketplace listings and student messaging.

## Implementation approach

- Preserve and extend the initialized Cloud **React 19 + TypeScript + Tailwind 4 + Wouter** frontend and **Express + tRPC + Drizzle/MySQL** backend. Keep the supplied OAuth/session SDK, protected tRPC procedures, storage helpers, LLM helper, UI primitives, error boundary and toast infrastructure.
- Use the initialized managed server and database. Use Manus OAuth for student login; retain the starter's `webdev_app_session` session name, nonce-bound OAuth callback, HS256/appId validation, and `SameSite=None; Secure` cookie behavior for embedded HTTPS Preview. Until a real session exists, show a logged-out state and a sign-in action—never a simulated Preview user.
- Extend the Drizzle schema and additive migrations for profile fields and separately owned records: study documents, study tasks, tutor conversations/messages, summaries, quizzes/attempts, flashcard decks/cards/review results, marketplace listings, message threads/messages and notification/read state. Each query and mutation must derive its owner from the authenticated user, check object ownership, validate inputs, and avoid trusting a browser-supplied user ID.
- Store uploaded document bytes in Manus project object storage, with randomized keys; store only indexes/metadata and owner IDs in MySQL. Validate Base64 integrity, file signatures/Office container structure, size and ownership before upload; provide a signed download only after authorization. Extract text from supported academic formats for AI context, use signed, fetchable asset URLs for supported PDF/image multimodal requests, and handle unsupported/empty extraction as a clear recoverable state rather than pretending the document was processed. Never put platform credentials or temporary signed URLs in browser code or logs.
- Use `officeparser` 8.1.1 for server-side PDF/DOCX/PPTX extraction with `OfficeParser.parseOffice(buffer)` and `ast.to("text")`; its official format list does not include legacy DOC/PPT, so accept and store those formats but show “Review extraction” and request a converted PDF/DOCX/PPTX or pasted text if extraction is unavailable. Source: [officeparser on npm](https://www.npmjs.com/package/officeparser) and [official usage documentation](https://github.com/harshankur/officeParser).
- Route AI tutor turns, summaries, quizzes and flashcard generation through the existing server-side built-in Manus LLM helper. Use educational system instructions, user-owned document context, server-side validation of generated results, persistent history, useful loading/error states and retry without duplicate saved turns. Grade objective quiz answers deterministically and short answers semantically server-side, accepting equivalent wording while returning specific feedback. Render model output as safe Markdown.
- Implement the dashboard and module views in modular React feature folders. Keep navigation routes in Wouter, surface recent documents and quizzes in Continue Studying, and expose the route manifest at `/manus-routes.json` before the development server starts.
- No payment/checkout feature: the approved marketplace scope is product/service listings and student contact through messaging, not buying/selling money flow.

## Project structure

- `client/src/App.tsx`: route registration and application providers.
- `client/src/pages/`: page-level entry points for Dashboard and product areas.
- `client/src/features/campus/CampusApp.tsx`: authenticated module switching, current student state, and cross-feature handoff for selected documents, study tools, tutor prompts, and message threads.
- `client/src/features/campus/Shell.tsx` and `shared.tsx`: the original brand mark, responsive sidebar/mobile navigation, page top bar, shared page primitives, avatar and format helpers.
- `client/src/features/campus/{Dashboard,Tutor,Documents,StudyTools,Marketplace,Messages,Profile}.tsx`: independently focused feature interfaces, with the rich AI views loaded on demand.
- `client/src/index.css`: CampusFlow's responsive design tokens, layouts, typography and interaction styles.
- `client/src/lib/`: existing tRPC client plus shared formatters and typed client helpers.
- `server/routers.ts`: compose feature routers with public authentication state and protected feature procedures.
- `server/features/campus/{index,common,documents,file-validation,study,social}.ts`: namespaced domain routers, tested file signatures, ownership validation, study AI orchestration, storage-backed document processing, dashboard data, messaging and listing APIs.
- `server/db.ts`: existing database connection/user queries plus ownership-aware domain queries as needed.
- `drizzle/schema.ts` and `drizzle/`: schema types and additive migrations.
- `shared/`: cross-runtime Zod schemas and type definitions where appropriate.
- `client/public/manus-routes.json`: complete static page-route manifest served as `/manus-routes.json` by the starter's configured Vite public directory.
- `client/public/` and root `app.config.ts`: site icon/brand asset and the required HTTPS project-logo metadata. The literal logo URL is a direct public CDN URL returned by the supported `manus-upload-file` upload, not a signed URL, and has been verified to return the SVG successfully.
- `plan.md` and `TODO.md`: approved implementation plan and the explicit, grouped product outcome criteria.

## Design system

- **Design Movement:** Contemporary campus utility, informed by Swiss/International typographic clarity and friendly modern SaaS rather than sci-fi futurism.
- **Core Principles:** (1) Calm academic focus; (2) warm, peer-friendly interactions; (3) clear status and next actions; (4) accessible, dependable controls.
- **Color Philosophy:** Blue signals trust, learning and connection; white and cool near-white backgrounds keep long study sessions calm; deep navy carries readable text; restrained green/orange/yellow communicate success, due dates and emphasis. Avoid neon, heavy gradients and excessive glass effects.
- **Layout Paradigm:** A persistent, compact desktop navigation rail opens into a flexible, left-anchored work area; dashboards place current focus and progress first, while the responsive mobile experience uses a bottom navigation bar and scrollable content. Avoid one oversized centered grid.
- **Signature Elements:** An original two-stream “open book / flowing path” CampusFlow mark; small blue section labels and progress tracks; softly rounded learning cards with clear, compact status chips.
- **Interaction Philosophy:** Direct, discoverable controls; concise confirmation for saved/destructive changes; accessible keyboard focus; loading, empty, success and retry states are explicit. Responsive behavior keeps primary actions reachable without crowding.
- **Animation:** Subtle 150–220 ms transitions for hover, selection and drawers; cards lift only slightly; no looping or large decorative motion; honor reduced-motion preferences.
- **Typography System:** Inter with a system sans-serif fallback; strong but compact page headings, readable 14–16 px body text, and quiet metadata. Use weight/spacing, not oversized type, to distinguish hierarchy.
- **Brand Essence:** A trusted campus companion that helps students learn, share and earn in one useful place. Personality: **capable, welcoming, resourceful**.
- **Brand Voice:** Encouraging and specific; study-focused, never hype-heavy. Examples: “A little progress adds up.” and “Pick up where your notes left off.”
- **Wordmark & Logo:** A compact monogram showing two curved blue study paths that meet as an open book, paired with a crisp CampusFlow wordmark; use the mark as the app icon and never rely on a default typed brand name alone.
- **Signature Brand Color:** CampusFlow blue `#315FEF`.

## Required product behavior

Preserve the brief's responsive navigation, all named page areas and exact dashboard actions/messages; implement the full Dashboard, education-focused AI Tutor, Documents Hub, AI Study Tools (summaries, quizzes and flashcards), and student Marketplace details. Add the approved MVP's real account isolation, durable content, message contact flow and functional listing create/browse experience. Keep course, semester, recent and favorite document organization. Surface progress from saved user activity rather than unrelated static counters. Because the original brief has no specific message/profile subfeatures and is cut off in the services list, implement a useful minimum profile and student-to-student messaging experience without claiming extra marketplace terms the source did not state.

## Build and delivery workflow

1. Extend the initialized project and record grouped requirements in `TODO.md` before writing application code. Keep the plan and any assumptions synchronized with the approved scope.
2. Implement schema/migrations and ownership-checked backend procedures, then build the responsive feature UI using existing dependencies and components. Keep platform OAuth/AI/storage credentials server-side.
3. Register the complete route manifest, configure the required logo metadata and run the existing TypeScript, test and build commands. Start the configured port-3000 development server only after the app has meaningful content; validate readiness and inspect the manifest. Use code inspection/diagnostics and existing checks; use visual/browser inspection only if a concrete defect needs investigation.
4. Fix observed type/build/runtime defects, then commit and push the intended deliverable to the project's canonical `main` checkpoint without force-pushing. Do not publish unless the user asks or the existing setting explicitly auto-publishes on checkpoint.
