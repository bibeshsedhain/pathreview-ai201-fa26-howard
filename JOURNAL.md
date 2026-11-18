# PathReview Development Journal

**Student:** Bibesh Sedhain  
**GitHub:** bibeshsedhain

## Unit 7 — Issue selection

**Issue link:** [codepath/pathreview-ai201-fa26-howard#70](https://github.com/codepath/pathreview-ai201-fa26-howard/issues/70)

**Issue title:** README parser test fixture is indented, so it contains no headings

**Tier:** [ ] Tier 1  [x] Tier 2  [ ] Tier 3

**Problem summary:**
The README parser test is supposed to check that a normal project README contains recognizable headings, but the shared sample text has four extra spaces at the start of every line. Those spaces prevent the heading syntax from matching, so the parser correctly reports zero headings and the test fails its expectation that at least one heading exists. The affected input is `sample_readme_text` in `tests/conftest.py`, used by `test_parse_standard_readme` in `tests/unit/test_readme_parser.py`. A successful fix would remove the unintended indentation from that fixture and remove this test's strict `xfail` marker for issue #70, allowing the existing assertions to pass as a normal regression test. This would restore useful coverage of standard README parsing without changing the parser to accommodate a malformed test input.

**Selection notes — scope and fit:**
- **Tier and comfort with the codebase:** I am approaching PathReview as a new contributor, so I want a focused task I can understand by following one fixture into one parser test. Although this issue is labeled `tier-2` (intermediate), its limited Python and pytest scope makes it a manageable learning step without requiring familiarity with the entire application.
- **Can I explain and locate the problem?** Yes: the issue identifies the shared fixture, the failing test, and `ingestion/parsers/readme_parser.py`. The existing test checks `heading_count > 0`, and its strict expected-failure marker explicitly references issue #70 and manifest H-03.
- **Is the scope realistic?** The issue estimates 2–4 hours. The intended change is fixture formatting plus removal of the matching expected-failure marker; it does not require a database migration, new endpoint, or LLM integration.
- **How will I know the fix works?** I plan to run `pytest tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme -v`, confirm that it passes without `xfail`, then run the README parser tests and `make test-unit` to check for effects on other consumers of the shared fixture. I will also run `make check` before opening a PR. These checks are planned, not yet executed.
- **What could expand the scope?** Since the fixture is shared, I need to inspect its other consumers before editing it and preserve intentional Markdown indentation inside code examples. Issue #71 covers a separate heading-hierarchy fixture and should remain separate.
- **Availability:** When reviewed, issue #70 was open with no assignee, claim comment, or linked PR visible. I posted a claim comment on the issue on November 17, 2026, and recorded my selection in the Howard cohort ledger.

**Branch name:** `fix/70-readme-parser-fixture`

**Submission branch:** https://github.com/bibeshsedhain/pathreview-ai201-fa26-howard/tree/fix/70-readme-parser-fixture

**Setup confirmation:** [x] App runs locally at localhost:5173

**Setup status:** Completed on November 17, 2026. Cloned my fork on this working branch and added `upstream` pointing to `https://github.com/codepath/pathreview-ai201-fa26-howard.git`. Created a Python 3.12 virtual environment and ran `make setup` successfully, including dependencies, migrations, seed accounts, and frontend installation. Docker is unavailable on this machine, so local development uses the existing PostgreSQL and Redis services with a separate database named `pathreview_unit7_bibesh`; the connection settings remain in the ignored `.env`. Started the servers with `make run`, verified the PathReview login page at `http://localhost:5173`, and confirmed the API root at `http://localhost:8000/` returns `PathReview API is running`. AI features use the default mock provider; external AI and vector-store integrations were not exercised.

**Cohort ledger:** [x] Issue added to cohort ledger

**Ledger status:** Added to the [Howard cohort ledger](https://docs.google.com/spreadsheets/d/1-39i16vRXrVwGKUHwxWfg2w_UAgsmxUYaR1oJ2djH8w/edit?gid=739942384#gid=739942384), row 26: Bibesh Sedhain / bibeshsedhain / issue #70. The issue title, Tier 2, and issue link filled automatically; status is `Claimed`.

## Unit 8 — Reproduction & solution planning

**Reproduction commit link:** [ae3ca5d — reproduce issue #70 locally](https://github.com/bibeshsedhain/pathreview-ai201-fa26-howard/commit/ae3ca5d5dbfe7129b28114a4e1c44bd98148affe)

**Reproduction summary:**
I ran `test_parse_standard_readme` locally with `--runxfail`, which exposed the failure `assert 0 > 0` because the shared README fixture's headings have four leading spaces. An in-memory dedented copy produced three headings and the same 39 words using the unchanged parser; the baseline README parser suite reports 13 passed and 2 xfailed.

**PLAN.md link:** [Solution plan for issue #70](https://github.com/bibeshsedhain/pathreview-ai201-fa26-howard/blob/fix/70-readme-parser-fixture/PLAN.md)

**Walkthrough video (recommended):** Not recorded; optional and not graded.

**Blockers or open questions:**
No blocker to starting the planned Unit 9 fixture/test fix. Recheck shared-fixture consumers before editing, preserve intentional relative indentation and newlines, and keep the separate #71 fixture/xfail unchanged. Detailed reproduction commands, observed results, and environment versions are in [docs/REPRODUCTION-70.md](docs/REPRODUCTION-70.md). Implementation and post-fix validation have not yet been performed.

## Unit 9 — Solution building & PR submission

### Check-in 1 (implementation progress — November 17, 2026)

**Current progress:**
Completed the local implementation steps from `PLAN.md`: corrected the shared README fixture with `textwrap.dedent`, removed the #70/H-03 strict `xfail`, and strengthened the standard README test to check all three heading names and levels. Added a boundary test confirming that genuinely indented content remains plain text; production parser behavior and issue #71 remain unchanged.

**Next steps:**
Finish local validation and self-review, prepare the commit and PR description, then publish the authorized commit/push and open a draft PR. Obtain peer or mentor feedback, address it, confirm all five CI jobs, and submit the ready PR through the branch URL.

**Blockers:**
The student has now authorized committing and pushing the prepared changes. Peer feedback and CI require the later PR workflow. This check-in records the actual work date rather than claiming an earlier mid-week submission.

---

### Check-in 2 (local validation; submission pending — November 17, 2026)

**PR link:** Not created. The prepared implementation is authorized for commit/push; the upstream PR is still pending.

**Branch:** `fix/70-readme-parser-fixture`

**What you built:**
Corrected the unintended common indentation in the sample README fixture so the existing parser recognizes its title and two section headings. The standard README test now runs as a normal regression test, and an additional test protects the parser's handling of genuinely indented content.

**Tests added or updated:**
Updated `tests/unit/test_readme_parser.py::TestReadmeParser.test_parse_standard_readme` to expect three headings and their names/levels, retaining its text-preservation and metadata checks. Added `test_parse_indented_heading_as_plain_text` for the four-space-indented input boundary. Existing tests cover empty input, whitespace-only input, heading-free text, and UTF-8 bytes; issue #71 keeps its separate expected-failure marker.

**Self-review confirmation:**
- [x] `make check` passes: Ruff and Black pass; mypy reports no issues in 76 source files.
- [x] `make test-unit` passes: 377 passed, 52 xfailed, no failures or strict XPASS results.
- [x] Focused README parser suite passes: 15 passed, 1 xfailed (the separate #71 test); `git diff --check` passes.
- [x] Frontend tests pass: 18 tests across two files.
- [ ] Integration tests pass: none exist in `tests/integration/`; `make test-integration` reports no tests and errors, while the CI workflow explicitly accepts pytest's no-tests exit status.
- [ ] All five CI checks are green on a PR: pending PR creation.

**Draft PR feedback received from:** none. Peer/mentor review has not occurred.

**Submission status:** Implementation and local validation complete; commit/push publication authorized. PR creation, peer feedback, live CI verification, ledger PR link/status update, and Unit 9 course submission remain pending. This second entry records validation now; finalize it with the actual PR link and feedback at submission.
