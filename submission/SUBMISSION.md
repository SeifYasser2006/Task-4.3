# UI/UX Rescue Challenge — Dorm Room Booking

## User goal
A student needs to find a common room available for a complete date range, reserve it, and later adjust their own reservation. End dates are exclusive throughout the app.

## Three problems and justifications

1. **Finding availability required mental calculation.** The original 18-room calendar showed individual open days but did not tell the student which rooms were free for a whole stay. An open start day could lead to a later conflict. The redesign adds date-range search with a count and book buttons for rooms free across the entire interval; selecting a result fills both dates and the room. The original calendar, room filter, and week navigation remain available.

2. **Failure feedback hid the cause.** Submitting missing fields or overlapping dates produced the same generic failure message. The student could not tell what to change. The redesign displays the actual validation reason, names the conflicting date range, preserves entered values, and explains how to recover. Required labels, exclusive-end guidance, live announcements, and detailed success confirmations make the outcome clearer. Clicking someone else's reservation now explains its occupied range rather than filling a knowingly unavailable request.

3. **Managing an existing booking lost context.** “My bookings” originally scrolled below the entire calendar, and Change populated a potentially off-screen form. It was easy to miss that editing had started. The redesign gives owned bookings a dedicated view, names each Change button for assistive technology, scrolls/focuses the edit form, and keeps Save and Cancel explicit. Own calendar reservations are also distinct and open editing directly. Larger text, visible focus rings, and phone layouts support these workflows.

## AI alternatives compared before implementation

| Problem | Alternatives | Decision and trade-off |
| --- | --- | --- |
| Full-stay availability | Drag across calendar cells; date-range search | Choose date inputs and matching room buttons. Dragging is spatially direct but harder to discover and operate on phones or keyboards. Inputs are explicit but add two fields. Keep the calendar for browsing. |
| Unclear failures | Disable submission until valid; explain errors on submission | Keep submission available and explain its exact failure. Disabled buttons can hide why a task is blocked. Feedback preserves the draft and gives a recovery path. |
| Off-screen editing | Modal editor; dedicated My bookings view with adjacent form | Choose the dedicated view and reusable form. A modal shortens travel but adds focus trapping and cramped mobile content. The chosen layout stays consistent with creating a booking. |

## AI suggestion refined/rejected
The AI design discussion proposed drag-to-select calendar dates as an alternative. This was rejected as the primary interaction because a student using a phone or keyboard would have more difficulty choosing an exact multi-day interval. The implemented refinement uses explicit From/Until inputs and shows only full-range matches, while retaining single-day calendar selection as a shortcut. These are AI-assisted implementation choices prepared for student review, not a claim that a separate usability study was conducted.

## Demonstration (start with Reset demo)
1. Search From **2026-11-11** Until **2026-11-13**. Four rooms appear: A1-203, B1-205, B2-104, C2-202. This demonstrates whole-range matching and exclusive boundaries.
2. Select **Book A1-203**, optionally enter a purpose, then Confirm booking. The detailed success message appears and the room disappears from matching results.
3. Enter A1-203, start **2026-11-10**, end **2026-11-12** and confirm. The request is rejected, names the existing 9–11 November reservation, and retains the entered dates. No extra reservation is created.
4. Open **My bookings**, choose the new A1-203 booking, change its end to **2026-11-12**, then Save booking. The form is brought into view and a detailed update confirmation appears. Reload to verify persistence.

## Verification performed
- Production build: `npm run build` — passed.
- Whitespace check: `git diff --check` — passed.
- Browser: successful create with optional purpose; overlapping create rejected; owned booking edited; updated booking survived reload.
- Browser: empty required fields rejected; equal start/end rejected; availability check accepts a range adjacent to existing reservations.
- Browser: 11–13 November returns four correct rooms; 9–20 November returns zero and suggests changing dates.
- Browser: Reset demo restores 40 example bookings and clears form/search.
- Phone layout at 390 × 844: page width equals viewport width; calendar scroll stays inside its container; choosing a result brings the filled form into view.
- All 18 directory rooms, browsing/filtering/week navigation, create/edit, optional purpose, localStorage, ownership restrictions, date ordering, overlap rejection, and reset remain in the app.

## Evidence
- `screenshots/before.jpg`: original app before changes.
- `screenshots/after.jpg`: final full-page availability screen.
- `screenshots/success.jpg`: successful reservation.
- `screenshots/conflict.jpg`: rejected overlap with corrective feedback.
- `screenshots/editing.jpg`: owned booking and populated edit form.
- `screenshots/mobile.jpg`: phone layout (intermediate state before final legend wording).

## Run
`npm install` then `npm run dev`; open the printed local URL. Data stays in the current browser. No server API or database was added.
