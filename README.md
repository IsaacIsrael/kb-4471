# kb-4471

Triage of support ticket #4471 (412 reports about wrong dates). Three problems, one ticket each:

- [#1](https://github.com/IsaacIsrael/kb-4471/issues/1) parser reads day-first dates month first: 379 reports (5 of them also in #3)
- [#2](https://github.com/IsaacIsrael/kb-4471/issues/2) booking-v3 form hints MM/DD/YYYY to day-first tenants: 33 reports
- [#3](https://github.com/IsaacIsrael/kb-4471/issues/3) dates move by one day, likely timezone: 5 reports

The page for the kb:

- `src/routes/kb/incidents/4471-three-problems/+page.svx` (source, signed)
- `src/lib/kb/ProblemCounts.svelte` (computes the counts)
- `kb-page.html` (the page as the desk sees it)
