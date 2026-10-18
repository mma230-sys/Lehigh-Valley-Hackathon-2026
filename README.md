# Roster — Hackathon prototype

A fictional campus demo with twenty-four professors, seventy-eight sample reviews, teaching and difficulty ratings, survival tips, workload estimates, and forty-eight downloadable course PDFs.

Two-step signup selects Lehigh, Bucknell, or Harvard before collecting a name, email, throwaway password, and major. Credentials are checked only in memory using salted PBKDF2 hashes. No Firebase, database, email delivery, or production authentication is used. Reviews, bookmarks, and uploaded PDFs are held only in page memory. Refresh or Reset demo restores the samples. Uploads are validated as PDFs (10 MB maximum) and never sent to a server.

Campus names are navigation examples, not university affiliations. Professors, courses, reviews, ratings, prices, workload figures, and sample materials are invented.

## Run

Use Node 22.13 or newer.

```sh
npm install
npm run build
npm start -- --port 5173
```

Open http://127.0.0.1:5173. For development, use `npm run dev`.
Run `npm test` and `npx tsc --noEmit` to validate.
The existing Vinext/Cloudflare runtime serves the app without database or storage bindings.

## Presentation behavior

Signup signs in automatically with an empty schedule and two campus friends. Log out and use the separate email/password login to return during the same page session. Everyone and Friends contain only students from the account’s university, and the signed-in campus selector is locked. Refresh clears all profiles and credentials. Main screens use presentation-ready copy; Privacy and source documentation explain the simulated data.

Explore lists friends taking each course, including sections. Workload totals the signed-in schedule (or saved courses when signed out). Friends and Everyone show sections, rooms, and classes sorted by start time for a selected weekday.

## Demo walkthrough

- Create a demo account in Friends, select classes, add/remove fictional friends, and compare everyone’s schedules at your chosen university.
- Open any course for its full roster, section-specific students and instructor, reviews, and materials. Schedule entries open their exact section.
- The separate Reviews tab and toolkit sidebar are removed; the course catalog uses the available width.
- Each campus has eight courses and twenty-four fictional student profiles.

- Search courses, filter departments, and switch among three campuses.
- Bookmark a course and find it in Saved courses.
- Open Professors to view name, department, and overall teaching rating. Click one to see their courses and pair-specific difficulty. Nova Quill and Milo Orbit each teach two courses.
- Reviews link a professor to a course. Filter professor reviews by course or course reviews by professor; overall teaching averages count each review once.
- Write a review: the rating averages and survival tips update immediately.
- Download a sample syllabus or notes PDF from Materials.
- Upload that PDF, download it again, or delete the session upload.
- Use Reset demo before presenting again.

No cloud project was deleted; this source no longer connects to any Firebase services.


