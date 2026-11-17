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
- **Availability:** When reviewed, issue #70 was open with no assignee, claim comment, or linked PR visible. A claim comment and the cohort ledger entry still need to be completed.

**Branch name:** `fix/70-readme-parser-fixture`

**Submission branch:** https://github.com/bibeshsedhain/pathreview-ai201-fa26-howard/tree/fix/70-readme-parser-fixture

**Setup confirmation:** [ ] App runs locally at localhost:5173

**Setup status:** Fork verified and working branch created on GitHub. This journal commit is the Unit 7 setup commit permitted by the assignment. Local cloning was attempted but terminal network access could not resolve `github.com`; `make setup`, `make run`, and the localhost check have not been completed.

**Cohort ledger:** [ ] Issue added to cohort ledger

**Ledger status:** Pending access to the cohort ledger. Entry to add on the Howard tab: Bibesh Sedhain / bibeshsedhain / issue #70.
