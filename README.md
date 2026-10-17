# Roster

A mobile-friendly campus survival app built for a hackathon. Next.js App Router components and route handlers run through Vinext/Vite on Sites (Cloudflare Workers). The UI uses React, TypeScript, Lucide icons, and custom responsive CSS.

## Open the self-contained demo

Open the adjacent `Roster-demo.html` file in a normal browser. It contains the bundled React app and CSS, so it does not require Node or a server. Font loading is optional. Local-file storage persistence depends on the browser; use a local server for consistent storage behavior. The same source UI can also be served with `node scripts/preview-demo.mjs` at http://127.0.0.1:5174. This portable preview uses explicit demo API responses; it is not a test of the production Worker routes.

## Run

Node 22.13+ is required.

```sh
npm install
npm run dev
```

The dev command prints the local URL. `npm run build` produces the Worker deployment. The Sites build integration is retained. This checkout uses Vinext for deployment compatibility; it is not a stock `next build` deployment.

## Demo mode

No credentials are required. The app persists reviews, saved courses, schedule selections, friend selections, strong subjects, and demo messages in browser localStorage (`roster-v1`). Messages never reach real people. All people, professors, reviews, prices, and workloads are illustrative. Course codes are examples, not a current catalog import.

- Search by name, course code, or professor; filter by department, friends, or workload.
- Read course survival tips and download original practice guides.
- Add a review; weighted course difficulty updates from the demo baseline plus new reviews.
- Add/remove courses; conflicting section meeting times are rejected.
- View friends' complete example schedules and friend icons on courses.
- Change strong subjects to recompute two-way study trades.
- Open a trade to draft a message, then send it into the persistent demo conversation.
- Decode an example syllabus into materials, USD costs, hours, and grading weights.

Instagram, Snapchat, and Rate My Professors are external links, not authenticated API integrations. Demo handles and professors are fictional. Seat Sniper and real student-to-student messaging are not implemented.

## Firebase connection

1. Create a Firebase project, register a Web app, and create Firestore.
2. Enable anonymous sign-in in Firebase Authentication.
3. Deploy `firestore.rules` using `firebase deploy --only firestore:rules --project YOUR_PROJECT_ID`.
4. Configure runtime variables `FIREBASE_API_KEY`, `FIREBASE_PROJECT_ID`, `FIREBASE_AUTH_DOMAIN`, and `FIREBASE_APP_ID` in Sites. These are public Web app config, not service-account credentials.
5. Redeploy. `/api/config` returns only the public Firebase configuration; the client signs in anonymously and synchronizes its private workspace through Firestore `onSnapshot`.

Storage: `Users/{firebase_uid}/workspace/demo` contains the private demo state and update timestamp. `Classes` has authenticated read-only rules reserved for a future server-seeded catalog. Only the owner can read/write their demo workspace. No open database rules or service-account keys are used. Firestore credentials were not supplied during this build, so live sync is not verified. Anonymous auth does not provide cross-device account recovery.

For a multiuser production app, migrate the fixture data into normalized `Users`, `Classes`, `User_Academics`, `Reviews`, and `Conversations` collections; add verified student accounts, opt-in schedule sharing, member-restricted conversation rules, moderation, and rate limits. Do not publish this private hackathon workspace as a multiuser messaging service without those changes.

## OpenAI PDF analysis

Configure `OPENAI_API_KEY` as a server secret and optionally `OPENAI_MODEL` (default `gpt-4.1-mini`). In local Worker development, use an ignored `.dev.vars` file with those names. Never put the secret in `NEXT_PUBLIC_*` or Firebase client config. Hosted secrets are configured through Sites and require redeployment.

`POST /api/syllabus` requires the Sites authenticated user, enforces same-origin requests, validates a PDF signature and 10 MB size, then sends the PDF to the OpenAI Responses API with a strict JSON schema. It validates the response with Zod and computes the total of explicitly stated USD material costs. Unstated costs, hours, and grading weights remain null. A known-cost total is not an estimate of unknown fees. Uploaded PDFs are not saved by this app; OpenAI retention follows the API account's data policy. With no key, the endpoint returns an explicit 503 configuration message and does not pretend to analyze the upload. Live AI was not tested because demo mode was selected.

Official API reference: https://developers.openai.com/api/docs/guides/file-inputs

Lehigh workload reference: https://data.lehigh.edu/weekly-course-commitment . Lehigh describes total time including class and outside work; Roster uses that definition, but no numerical Lehigh data were imported.

## Match and rating logic

A two-way trade requires both intersections to be nonempty: your strong subjects with their current courses, and their strong subjects with your current courses. Edit your strong subjects on Study trades to demonstrate this.

Difficulty = `(baseline difficulty × baseline review count + sum of new ratings) / (baseline review count + new review count)`. The fixture has one professor per course, so this is also that professor's course difficulty. The displayed baseline review count represents an illustrative aggregate; only two representative demo reviews are listed. Material prices and weekly hours do not change with user review submissions.

## Verification

Run `node --experimental-strip-types --test tests/logic.test.mjs` for match/rating checks. Run `npx tsc --noEmit` for type checking and `npm run build` for compilation. See the final handoff for the checks actually run in this environment.

Verified in this build: TypeScript check, production Vinext build, and both logic tests passed. The portable React bundle was generated successfully. Visual/browser interaction checks could not run: the local Worker runtime failed to start, localhost preview connections timed out, and the in-app browser disallows local-file URLs. Firebase and OpenAI were not connected. Publication did not complete because automatic approval review rejected the credential handoff to the Sites source publishing helper. The registered Site remains unpublished.
