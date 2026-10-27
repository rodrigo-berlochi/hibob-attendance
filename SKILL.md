---
name: hibob-attendance
description: Fill or repair the user's HiBob attendance for the current ongoing month through today using Chrome, including a daily lunch break. Use for requests such as "fill the time attendance in my hibob account for the current ongoing month until today". Save daily entries and leave monthly timesheet submission to the user.
---

# HiBob attendance

## Scope and schedule

Use the signed-in user's **My attendance** page at https://app.hibob.com/attendance/my-attendance in Chrome. Default to the current local calendar month, from its first day through today inclusive. Determine today's date at execution time using the Mac's local date/time; verify it against HiBob's Today label. Resolve a date disagreement before writing.

For each eligible working day, use this user-requested schedule:

| Entry | Clock in | Clock out | Project |
| --- | --- | --- | --- |
| Work | 09:00 | 14:00 | Teleworking |
| Break | 14:00 | 15:00 | None |
| Work | 15:00 | 18:00 | Teleworking |

The result is eight payable work hours and one unpaid break hour. Today is included in the requested full-day schedule even if it is before 18:00. An actively running clock or other conflicting entry requires clarification rather than replacement.

Daily **Save** is part of filling attendance. **Never click the monthly Submit button**, including when it becomes enabled; the user handles that final action. Do not clock in or out using the live attendance timer.

## Connect and inspect

1. Discover the available browser/computer tools and read their runtime documentation. Prefer the existing matching Chrome tab and preserve its session. The successful implementation used `cua_repl` with native accessibility controls; use the supported equivalent available in the CLI session. If no Chrome UI control is available, stop and explain that this skill requires it. A skill does not itself install browser automation or provide access to Chrome.
2. If the page shows login, SSO, or an expired session, stop and ask: "Please log in to HiBob in Chrome, then tell me you're logged in." Resume only after the user's reply, and recheck the page. Leave credentials and login completion to the user.
3. Select and verify the ongoing current month. Inspect every date through today, scrolling as needed. Exclude future dates, week subtotal rows, non-working days, holidays, and full-day time off. Do not infer working status from Monday–Friday alone. Partial-day leave, unusual schedules, locked periods, and conflicting entries need clarification.
4. Build a date-by-date inventory before changing records: complete, empty eligible day, standard unsplit day missing a break, or exception. The table can split dates, values, and action buttons into separate row groups. Match them using observed row identifiers or a verified date association; check the date again inside each editor.

## Idempotent changes

- If all eligible days are already complete, report that attendance is already filled through today and stop without changes.
- Preserve complete days, including their existing project choices and notes. Eight payable hours and a one-hour break are useful screening signals; inspect intervals when completeness is uncertain.
- For an empty eligible day, create all three entries from the schedule.
- For the known incomplete pattern—one Teleworking work entry from 09:00 to 18:00 with no break—change its clock out to 14:00, then add the break and afternoon work entry.
- For partially filled days, add only missing intervals when they are unambiguous and do not overlap. Preserve matching entries. If an existing time, project, note, live clock, or leave record conflicts with the schedule, leave that date unchanged and ask for clarification. Do not delete or overwrite conflicting records automatically.
- On a retry or resumed run, re-inventory saved state. Never assume that a previous Save failed simply because feedback was delayed.

## Editing through the UI

Open the date's rightmost three-dot menu and choose **Edit**. Confirm the full date in the side panel before typing. In the observed UI:

- Each time is two separate fields, hours and minutes. Set both explicitly, using two-digit values.
- The cup-plus icon adds a **Break** entry; the clock-plus icon adds a **Work** entry. Their observed accessibility names were `` and ``, respectively. Treat those glyphs as hints and verify the newly added entry type.
- New entries sometimes prefill their start from the preceding end; always verify or explicitly set it.
- A new Work entry requires **Project/Project task**. Choose **Teleworking** and verify the collapsed dropdown shows it before saving.
- Check the three durations before Save: Work 05h 00m, Break 01h 00m, Work 03h 00m. Check all six endpoints as well.

Read fresh UI state after actions before selecting the next target. Accessibility indexes and generated field IDs change as entries are added and panels reopen. Never persist IDs, coordinates, date-to-menu offsets, or a fixed list of dates from a previous month.

Playwright locators initially worked but later timed out despite visible controls; native accessibility `setValue` and `click` continued working. If semantic locators fail, inspect a fresh accessibility tree and use the documented native controls. Use a screenshot to resolve missing context. Avoid repeatedly retrying a failing locator or using hidden app state/private HTTP endpoints. The row menu's Edit path proved more dependable than the detail panel's pencil shortcut.

## Save and verify

Save each edited day individually. Verify the saved detail view shows 09:00–14:00, 14:00–15:00 Break, and 15:00–18:00. Confirm the table updates to eight payable hours and one unpaid break hour and that the date's missing-break warnings clear. Allow the UI to finish updating before moving to the next date; it can briefly show stale totals or an empty transitioning panel.

If saving or verification fails, inspect the current record before retrying. Stop on a persistent unexplained error and report the dates already saved and those remaining. Do not claim completion based only on clicking Save.

Finish by rechecking all eligible dates through today and any remaining warnings. Report the month, number of days changed, skipped exceptions, and whether everything was already complete. Explicitly state that the monthly timesheet remains unsubmitted. Leave the attendance page open for the user's review and capture proof of the final state if supported by the browser tools.
