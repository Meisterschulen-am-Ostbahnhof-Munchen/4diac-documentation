---
name: eclipse-4diac-contribution-check
description: >-
  Use before every `git commit` and before creating or updating a pull
  request in any eclipse-4diac repository (4diac-ide, 4diac-forte,
  4diac-documentation) or a fork of one. Checks the commit message, PR
  description, file headers, code comments, and any new documentation
  files against the project's actual contribution guidelines (Eclipse
  Foundation Project Handbook + eclipse.dev/4diac/doc/development/contribute.html
  + the repos' own CONTRIBUTING.md). Trigger on: preparing a commit
  message, opening/editing a PR title or description, adding or editing
  a copyright/file header, writing a code comment of more than one line,
  creating a new `.md`, `.adoc`, or `.asciidoc` documentation file, or
  bumping a version number — in any of these three repos or their forks.
---

# Eclipse 4diac contribution compliance check

## Non-negotiable rules (repeat violations — read this block first, every time)

1. **Push once, at the very end of a review round — never after every
   individual fix, and never just because "this message's fix is done."**
   This still applies when comments arrive one per message, across
   several separate messages. Full detail and the exact wording that
   didn't stick the first two times: section 1, "Push once."
2. **Never post a GitHub comment, reply, or reaction addressed to a
   human** (a reviewer, the PR author, anyone with `type: "User"`) — not
   `gh pr comment`, not a review-thread reply, nothing. Only reply
   directly to **automated** review tools: CodeRabbit
   (`coderabbitai[bot]`), Sourcery, GitHub Copilot's review bot, or
   similar (`type: "Bot"`, or a recognizable bot account name). Fixing
   what a human reviewer asked for is expected; announcing or
   summarizing that fix back to them in a public comment is not your
   call to make — that's for the human user (Franz) to post himself, in
   his own voice, if he chooses to. Full detail: section 3a.

Run this checklist **before** every `git commit` and before creating/editing
a PR title or description in `eclipse-4diac/4diac-ide`,
`eclipse-4diac/4diac-forte`, `eclipse-4diac/4diac-documentation`, or a fork of
any of them. The goal is to catch guideline mismatches before a human
reviewer has to point them out.

Every rule below is either a **direct quote from an authoritative source**
(cited) or **observed project convention** from real review threads (noted
as such, since it isn't written down anywhere yet). Don't invent conventions
beyond what's here — if something isn't covered and you're unsure, ask
rather than guess, and consider whether the gap itself is worth a small doc
PR (see the last section).

**Fork-local policy precedence**: a fork's own `CONTRIBUTING.md` is
normally an unmodified copy of upstream's (see section 9's IEC 61131
example — a fork can simply be *behind* upstream, not deliberately
different). If a fork's `CONTRIBUTING.md` (or another local policy file)
has actually been edited to diverge from upstream on purpose, that
fork-local version is authoritative for PRs targeting that fork — note
the divergence rather than flagging the fork's own PR as non-compliant
with the upstream text it deliberately departs from.

**Not every checklist item applies to every invocation**: this skill
triggers on a commit, a PR title/description edit, or other individual
actions (see the frontmatter `description`) — run only the items that
have applicable content for what you're doing right now. A first commit
on a brand-new branch has no PR description yet (skip the PR-only
items), and a PR-description-only edit may not touch commit history
(skip the commit-only items). Don't treat an inapplicable item as
failing; skip it and say why if asked.

## 1. Commit messages

Source: [eclipse.dev/4diac/doc/development/contribute.html](https://eclipse.dev/4diac/doc/development/contribute.html)
(4diac-specific, stricter than and takes precedence over the generic
Eclipse Handbook default of 72/72 below, for these three repos):

- **Subject line: max 50 characters.** Short, imperative, descriptive
  ("Fix X", "Add Y" — not "Fixed", not "Adds").
- **Body: wrap at ~72 characters per line.**
- **Self-contained and comprehensible without external links.** Explain the
  *why*, not just the *what* — the diff already shows the what.
- **No links to issues, discussions, or pull requests in the commit
  message.** Not `Fixes #1234`, not a bare URL, not a session-tracking
  link. Issue/PR references belong in the **PR description**, not the
  commit body. (This is the single most common thing to double-check —
  scan every commit message for a URL or `#<number>` before committing,
  every time.)
- **Clean history: exactly one commit per PR.** Observed convention across
  every PR actually opened by this workflow in these repos — squash to a
  single commit before pushing, always, including when a review round adds
  or changes something: `git commit --amend` (or `git reset --soft
  <merge-base>` + one new commit) rather than a second commit, and
  force-push. No `wip`, `fix typo`, `address review comment` commits
  surviving into the final PR, and no keeping separable sub-changes (e.g.
  "add feature" + "add tests" + "update docs") as multiple commits either —
  squash those into the one commit too, using the body to list each part if
  needed.
  - **Push once. Not after every fix. Not after every message either.**
    This still applies when the user pastes review-comment links **one at
    a time, in separate messages** — that is still ONE PR, ONE review
    round: fix the comment locally (`git commit --amend`), do **not**
    `git push`, and wait for the next input. Only push when you have
    explicit signal the round is over — the user says so, or moves on to
    a clearly different task/PR/repo. If you're not sure whether more
    comments are coming, don't guess "probably done" and push anyway —
    treat silence as "not yet" and hold the push; a late push costs
    nothing, an early one is the exact mistake this rule exists to stop.
    User had to repeat this correction three times in one session, the
    last time explicitly about this failure mode (fixing several
    CodeRabbit comments delivered one link per message, each followed by
    its own push): "war schon wieder falsch. du sollst am ENDE einmal
    pushen." / "du hast 'push once' immer noch nicht verstanden!" Do not
    treat "the current message's fix is done" as "time to push" — that
    conflation is precisely the bug. Only a genuine end-of-round signal
    triggers the push; the completion of an individual fix never does.
- **Don't over-explain.** A commit message is not the place to write a
  design essay. State what changed and the one or two non-obvious reasons
  why. If you find yourself writing more than ~4-6 short paragraphs, that's
  a sign to move detail into the PR description instead, or that the change
  itself should be split.
  - **Observed convention, repeat offense**: reviewer azoitl flagged this
    exact issue on two separate PRs in the same session (#2828, then again
    on #2835 as "Commit message is still horrible" — a second round on a
    *different* PR, i.e. not a one-off). A commit body that walks through
    root cause, fix, AND a paragraph describing the regression test in
    full prose is already too much, even if each paragraph individually
    looks short. Target **2 short paragraphs**: (1) what was wrong and the
    one-line reason why, (2) what the fix does. A one-line mention that a
    regression test was added is fine; a paragraph narrating the test's
    mechanics (what it exports, re-imports, and asserts) belongs in the PR
    description, not the commit. Before finalizing, count paragraphs and
    ask "would a reviewer skim this in under 10 seconds?" — if not, cut.

Per the Eclipse Foundation Handbook
([`#resources-commit`](https://www.eclipse.org/projects/handbook/#resources-commit),
generic default, still applies where 4diac doesn't say otherwise):
- Three-part structure: **summary line, description, footer.**
- `Author` field must be the real legal name + the email on file with the
  Eclipse Foundation account. Actual ECA-signed status isn't something
  this skill (or the agent) can verify — Eclipse's own automated ECA
  check on the PR does that. The agent's job is only to make sure the
  `Author`/`Co-authored-by` identities used are real people with a
  plausible name+email, not to independently confirm ECA coverage.
- Additional authors: `Also-by: Name <email>` or `Co-authored-by: Name
  <email>` — one entry per person, and per the Handbook this entry should be
  a real, ECA-covered contributor. This is attribution for a co-author, not
  AI-tool disclosure — see the `Assisted-by:` trailer below for that. (If
  you're using an AI coding assistant that appends its own fixed
  `Co-Authored-By:`-style trailer as a tool convention, that's not
  something to edit — just don't add anything *beyond* it, e.g. no extra
  session-link line, since that *is* a link. Note the casing difference is
  deliberate, not a typo: `Co-authored-by:` above is the Handbook's own
  spelling; `Co-Authored-By:` elsewhere in this document is the literal
  string a given AI tool actually emits — git trailer keys are
  case-insensitive, so both work, but don't "normalize" one to match the
  other since they're quoting two different sources.)
- **AI-tool disclosure — `Assisted-by:` trailer, only when disclosure is
  actually warranted.** Source: [Eclipse Foundation Project Handbook,
  "Identifying an AI Assistant"](https://www.eclipse.org/projects/handbook/#ai)
  (an emerging de facto standard per the Handbook, not yet a hard
  requirement — "reasonable effort should be undertaken to disclose the
  use of AI in project content"; "if providing all of this information is
  especially onerous or impossible, specify as much of it as you can") and
  the Handbook's own FAQ ([`#genai-faq`](https://www.eclipse.org/projects/handbook/#genai-faq),
  "Do I have to disclose the use of AI in all cases?"):
  > "A disclosure is likely not required when an AI generates rote or
  > simple boilerplate content that you then modify and supplement with
  > more creative behaviour. A disclosure is likely not required for a
  > spelling correction or the addition of a for loop, for example. A
  > simple rule of thumb is this: if you would give a human credit for the
  > contribution, you should likely provide an AI disclosure."

  So this is **not** an unconditional per-commit requirement: skip the
  trailer for trivial, boilerplate, or rote AI help (a typo fix, a
  mechanical one-line change) where you wouldn't credit a human co-author
  for it either; add it once the AI's contribution is substantive enough
  that you would credit a human for the same work — which in practice is
  most of what this skill's own commits look like (multi-file fixes,
  non-trivial logic, new tests), so don't use this exception to skip
  disclosure on real contributions.
  - Exact format: `Assisted-by: [Provider] [Model-Family] ([Version/ID])`.
    Handbook examples: `Assisted-by: Google gemini-2.5-pro (rev-1)`,
    `Assisted-by: GitHub Copilot (GPT-4o-2024-08-06)`.
  - This is a **separate trailer from** `Co-authored-by:`/`Also-by:` — it
    discloses which AI assisted, it doesn't claim ECA-covered co-authorship.
    Both can appear in the same commit footer.
  - For a file that is **largely or entirely AI-generated** (not just
    assisted), the Handbook also asks for an `Assisted-by:` line in the
    file's own copyright header, alongside an "AI Disclosure" note and an
    SPDX expression combining the project licence with `CC0-1.0` (AI-only
    output is likely not copyrightable) — see the Handbook section for the
    full header template. This is a different, stronger case than a
    human-authored change that merely used an AI assistant as a tool.
- No manual `Signed-off-by` needed — the ECA already covers DCO.

## 2. Copyright / file headers — **single year, not a range**

Source: [Eclipse Foundation Project Handbook, "Copyright Headers"](https://www.eclipse.org/projects/handbook/#ip-copyright-headers)
(this section **changed** at some point from an older "first year, last
year" range convention; do not follow examples elsewhere on the web or in
older files that show a year range):

> "*Copyright statements* take the form `Copyright (c) {year} {owner}`."
> "The `{year}` is the year in which the content was created (e.g. \"2004\")."
> "The `{year}` is the year of the initial creation."
> "The `{owner}` is the name of the copyright holder. If the content is
> subsequently modified and appended to by other copyright owners, the
> words \"and others\" are typically appended. [...] However, especially if
> the number of copyright owners is small (e.g. two), they may all be
> listed in a single copyright statement (for example: \"XYZ Corp., John
> Smith, and ABC Enterprises.\"). Alternatively, additional copyright
> holders may be added by including multiple copyright statements."

Concretely:
- **Exactly one year per copyright statement**: the year *that statement's*
  owner(s) first contributed. Never turn a single statement into a range
  (`2023, 2026` or `2023-2026`) — the year does not move just because the
  same owner keeps editing the file.
- **A new copyright *holder* (not just a new contributor)** — i.e. a
  genuinely different legal entity (a different company, or an individual
  not covered by an existing employer's copyright) — can be recorded three
  ways, per the Handbook text above; any is fine:
  - Append `and others` to the existing statement:
    `Copyright (c) {year} {owner} and others`.
  - List owners comma-separated in one statement, small numbers only —
    **one year, then one or more owners comma-separated, optionally ending
    in `and others`**: `Copyright (c) {year} {owner1}, {owner2}, and
    others`.
  - Or add a wholly separate `Copyright (c) {year2} {owner2}` statement,
    with `{year2}` being *that* owner's own first-contribution year — this
    is the one legitimate way a second year appears in the header: as its
    own separate statement, never merged into the first owner's line.
- **`Contributors:` is not the same thing as a copyright holder — don't
  conflate them.** The Handbook lists "the names of the contributors and
  the nature of their contribution" as a *separate*, optional part of the
  header from the copyright statement(s). The common case — you (or
  another individual) make a **significant** contribution to an
  **existing** file, without asserting a distinct copyright interest —
  is: add yourself to the `Contributors:` list with a short description of
  what you added, and leave every copyright statement untouched. Only
  touch the copyright statement itself (one of the three forms above) when
  a genuinely new copyright-holder legal entity is involved. (See the
  header template in the Handbook section above, or any recently-touched
  file in these repos for the exact EPL-2.0 block format.)
- `{owner}` is a legal entity (a person or a company) — see the Handbook
  section for the full nuance on employer-vs-individual ownership; don't
  overthink this for a small change.

**New source files** — Source: [contribute.html](https://eclipse.dev/4diac/doc/development/contribute.html)
("Every new source file must start with the Eclipse Public License 2.0
(EPL-2.0) header."):
- A brand-new Java or C++ source file must start with the full EPL-2.0
  header before anything else in the file (package/include statements come
  after it), using this exact template from contribute.html:
  ```text
  /*******************************************************************************
   * Copyright (c) {year} {owner} and others.
   *
   * This program and the accompanying materials are made available under the
   * terms of the Eclipse Public License 2.0 which is available at
   * http://www.eclipse.org/legal/epl-2.0.
   *
   * SPDX-License-Identifier: EPL-2.0
   *
   * Contributors:
   *   {name} - initial API and implementation and/or description of change
   *******************************************************************************/
  ```
  Fill in `{year}` (this file's single creation year, per the rule above),
  `{owner}`, and `{name}` (the actual author, with a short description of
  what they added).
- Don't skip this because the file is small, generated, or a test file —
  the same header applies regardless of file size or purpose.

## 3. PR title and description

Source: contribute.html:
- Explain **what** changed and **why**.
- **Issue/PR references go here**, not in the commit message. Use
  `Closes #N` / `Fixes #N` / `Resolves #N` **only** if this PR fully
  resolves that issue — otherwise just reference it as `- #N` or in prose,
  without the auto-close keyword.
- Keep the description in sync with the actual diff. If a review comment
  causes you to change the approach (e.g. drop a helper, split out part of
  the change into another PR), **update the PR description in the same
  turn** — a stale description describing removed/moved code is itself a
  guideline mismatch and a likely source of the next round of review.
- Cross-links to sibling PRs (a fork mirror, a split-out follow-up PR, a
  backport) are fine in the **PR body** — the "no links" rule is specifically
  about the commit message.

## 3a. GitHub comments and replies — automated tools only, never humans

Observed convention, repeat offense: the user told me this once already,
in an earlier session, and it wasn't written down anywhere — so it got
violated again. On PR #2834, after fixing what reviewer azoitl (a human
maintainer) asked for, I posted a `gh pr comment` summarizing the fix
directly on the PR, addressed to him. He reacted with mocking emoji
(😃🤪🤑). The user's correction, verbatim: "wieso hast du geantwortet
???? ... du darfst NICHT Menschen antworten nur KI (Coderabbit,
sourcery, copilot) aktualisiere den Skill nochmal." Translation: you
may not reply to humans, only to AI review tools.

- **Never** post a PR/issue comment, a review-thread reply, or a
  reaction that is addressed to a human — not the PR author, not a
  human reviewer, not anyone else. This includes a plain summary
  comment like "fixed per your review," even when factually accurate
  and even when it reads as helpful — posting it is not your call.
  Fixing the code a human reviewer asked for is expected and good;
  publicly telling them so on your own initiative is not.
- **Do** reply directly to comments from automated review tools:
  CodeRabbit (`coderabbitai[bot]`), Sourcery, GitHub Copilot's review
  bot, or similar. These are safe because there's no human on the
  other end to misjudge tone, and this session has done this
  successfully many times (e.g. this PR).
- **How to tell them apart**: check the comment/review JSON from `gh
  api` — `user.type` is `"Bot"` for automated tools (and the login
  usually ends in `[bot]`, e.g. `coderabbitai[bot]`), `"User"` for a
  human account (e.g. `azoitl`, `mx990`, `franz-ms-muc`). When in
  doubt, don't post — surface the review's content to the user instead
  and let them decide whether and how to respond themselves.
- This applies to `gh pr comment`, `gh api .../comments`, `gh api
  .../replies`, `gh pr review`, and any other posting mechanism — not
  just one specific command.

## 4. Comments — code and file headers

Observed convention (not written in contribute.html or any CONTRIBUTING.md;
inferred from surrounding-file style and real review-thread feedback):

- Match the length/density of comments already in the surrounding file. If
  every other comment in the file is 1-3 lines, don't write a 15-line essay
  for your addition — if the reasoning genuinely needs that much space, it
  probably belongs in the PR description or a linked design doc, not inline.
- Comment the **why**, not the what — the code already shows what it does.
- **English only, no exception**, for comments, commit messages, PR text,
  and documentation files in these repos — even if the surrounding
  conversation with the user is in another language. This is a firm,
  consistently expected project convention.

## 5. Documentation files

Observed convention (repo structure plus real review-thread feedback; not
written in contribute.html or any CONTRIBUTING.md):

- File format follows the repo, for actual documentation content:
  `4diac-ide` and `4diac-forte` use Markdown (`.md`) for repo-local docs;
  `4diac-documentation`'s published doc site (everything under `src/`) is
  AsciiDoc (`.adoc`, with a handful of legacy `.asciidoc` files; see
  section 7) — don't add a `.md` file under `src/` there. This doesn't
  apply to root-level project metadata files that are `.md` by Eclipse
  Foundation convention regardless of repo (`README.md`, `CONTRIBUTING.md`,
  `SECURITY.md`, `CODE_OF_CONDUCT.md`, `NOTICE.md`, `LICENSE.md`).
- Keep new documentation files **scoped, dated, and referenced** — a clear
  home (e.g. under `doc/`), a clear single topic, code references where
  relevant, and a link from somewhere so it's discoverable, rather than a
  free-floating file that only makes sense with tribal knowledge, and
  written in English only (see section 4) — never a German-original-plus-
  English-translation pair; write one English file.
- If a new `.md`/`.adoc`/`.asciidoc` file is genuinely a **working/discussion** artifact
  rather than end-state documentation, say so explicitly in the PR
  description and consider whether it belongs in the PR at all versus being
  pasted into the PR/issue description instead of committed as a file.
- Before adding a new doc file, check whether the content already belongs in
  an existing doc (e.g. `4diac-documentation`) rather than a repo-local
  scratch file.

## 6. Version numbers

No formal per-type `VersionInfo`/`Version=` bump policy is written in any of
the three CONTRIBUTING.md files or contribute.html, but version numbers in
these repos follow semantic-versioning-style reasoning about the *size* of
the change. **Terminology used in this project** (confirmed by maintainer
azoitl's own wording in PR #2814 review comments — use these names, not
generic semver terms like "patch"): a version `X.Y.Z`, e.g. `3.0.1`, reads
as **Major.Minor.Maintenance** — `3` = Major, `0` = Minor, `1` =
Maintenance.

- **Small changes** (comments, documentation, minor/cosmetic fixes with no
  interface or behavior change): bump the **Maintenance** number, e.g.
  `3.0` → `3.0.1`.
- **Functional extensions** (new features, new parameters, behavior
  changes that stay backward-compatible): bump the **Minor** number, e.g.
  `3.0` → `3.1`.
- **Massive, global changes**: bump the **Major** number. The normal case
  is a single-step bump, `X.Y` → `(X+1).0` (e.g. `3.0` → `4.0`). A
  multi-step jump like the real `1.0` → `3.0` precedent (every standard
  type library in `4diac-ide`'s `data/typelibrary/` exists as both
  `-1.0.0` and `-3.0.0`, with no `-2.0.0` at all) has genuinely happened,
  but that was a one-off decision made by project leadership when creating
  a new parallel library generation — not something a PR contributor
  originates themselves. As a PR author, don't bump a major version
  speculatively at all, single-step or otherwise; this is rare and, as a
  rule, never happens within a single ordinary contribution PR.

- **Bump the number only once per release cycle — never twice.** If a
  version component (say, Maintenance) was already bumped once during the
  current, not-yet-released cycle and another small change comes in
  before that version ships, do **not** bump it again to a second new
  number (e.g. `3.0.1` → `3.0.2`). Instead, fold the new change's
  documentation into the **existing, unreleased** `VersionInfo` entry
  (merge its description/Remarks text into that entry's, don't touch the
  Author/Date unless asked) and leave the version number as is. Only
  bump again once that version has actually been released and a new
  cycle has started.
  - **Real precedent**: PR #2814 (4diac-ide, signalprocessing typelib) —
    a `3.0.1` Maintenance bump already existed (unreleased) for an
    earlier description/formatting fix; a second, unrelated fix in the
    same PR round initially added a `3.0.2` entry, which maintainer
    azoitl asked to undo: "It should not be a new maintenance version
    but you should add the documentation of your change to the 3.0.1
    version" and, separately, "it should not be bumped twice in a
    [release] cycle." Fixed by merging both `Remarks` strings into the
    single `3.0.1` entry and removing the `3.0.2` one.
- Before bumping any version number (a `VersionInfo Version="X.Y.Z"` in a
  4diac-ide type-library XML file, a CMake `project(... VERSION ...)`, a
  plugin's `MANIFEST.MF` `Bundle-Version`, etc.), still find **at least two
  or three independent, unrelated, already-merged precedents** for the
  exact same kind of bump in the exact same kind of file, to confirm the
  scheme above actually applies to that file type — not just one example
  you happened to find (a single example may itself be non-standard or
  simply wrong).
- If you can't find solid precedent, don't bump the version speculatively —
  ask the maintainers rather than guessing.
- Double-check arithmetic: a Maintenance bump is `X.Y.Z` → `X.Y.(Z+1)`, not
  a reused or decremented number, and must not collide with a version
  already used elsewhere for a different, unrelated change to the same
  file.

## 7. Code style

Source: contribute.html:
- Run the project's own formatter/checker before committing: Checkstyle for
  4diac-ide (Java), the C++ style guide / clang-format for 4diac-forte,
  AsciiDoc conventions for 4diac-documentation. For `4diac-documentation`
  specifically, its own `CONTRIBUTING.md` documents a concrete build step
  beyond style convention: *"To generate the output run `mvn` in the root
  directory of this repository"* — run it before committing an `.adoc`/
  `.asciidoc` change to catch rendering errors (e.g. a malformed `part`/
  section structure) that a pure style read-through would miss.
- Existing tests must keep passing; add tests for new features/fixes
  (JUnit for 4diac-ide, Boost.Test for 4diac-forte).
- **Documentation for UI/parameter changes.** Source:
  `contribute.asciidoc`, section "Testing Your Changes": *"If your change
  affects the user interface or adds a new parameter, please update the
  corresponding documentation files."* If the diff adds/renames/removes a
  UI element (a preference, dialog, view, editor field) or adds a new
  parameter/property (an XML attribute, algorithm parameter, CLI flag),
  check whether `4diac-documentation` (or an in-repo `doc/`/`README`) has
  a page describing it, and update it in the same PR — don't leave this to
  a follow-up. If no such change is present, this bullet doesn't apply;
  don't add a documentation update for changes that don't touch UI or
  parameters.

## 8. AI-authored contributions

Source: contribute.html: *"we expect that all code remains maintainable by
humans"* — the human author remains responsible for the entire content and
must be able to personally explain any part of a PR. This skill's checks
exist to reduce review back-and-forth, not to replace the human author's own
understanding of what's being submitted — flag anything you're not fully
confident about rather than silently including it.

Also add an `Assisted-by:` commit trailer disclosing the AI tool used, when
the contribution is substantive enough to warrant it — see section 1's
`Assisted-by:` entry for the exact format, the "when is it warranted" rule
of thumb, and Handbook source. Don't skip it because a harness-level tool
convention already adds its own attribution trailer (e.g. `Co-Authored-By:`)
— that trailer claims co-authorship, it isn't the Handbook's AI-disclosure
mechanism, so both belong in the same commit footer when applicable.

Further "Responsibilities" from the same Handbook page
([`#genai-responsibilities`](https://www.eclipse.org/projects/handbook/#genai)),
verified against the raw page text directly:

- **Input sensitivity.** Don't put confidential content, personally
  identifiable information, health information, or other sensitive
  information into an AI platform's input — this applies to what you type
  into the assistant during the session, not just what ends up in the diff.
- **Verify accuracy and vet output.** AI output is prone to errors. Run it
  through the project's normal vetting: testing, IP due diligence, and
  security review — same as for human-authored content, not a lighter bar.
- **Terms of use / employer obligations.** The AI platform's terms of use
  must be consistent with your obligations to Eclipse as a Committer and to
  your employer as an employee (e.g. some platforms' terms preclude use for
  competitive purposes — check whether that applies).
- **Copyright/licensing awareness.** Content generated solely by an AI is
  likely not independently copyrightable; a human-authored portion of a
  mixed work might be. This matters for the CC0-1.0 SPDX combination
  mentioned in section 1's `Assisted-by:` entry for largely-AI-generated
  files — don't assume normal project-licence copyright applies to a
  purely-generated block without human modification/curation.

## 9. If the guidelines themselves are wrong or contradictory

Skill process guidance, not a contribution-guideline rule itself — this is a
meta-instruction for using the skill, so no external `Source:`/`Observed
convention:` citation applies here the way it does to sections 1-5 and 7-8.

Genuinely check for this — don't just apply the rules, notice when a source
document is itself inconsistent or incorrect, and propose a fix:

- **Example already found, fix pending**: `4diac-documentation`'s intro text
  says "IEC 61499 extends IEC 61131-**1**", while both `4diac-forte` and
  `4diac-ide`'s CONTRIBUTING.md say IEC 61131-**3** (the correct reference —
  IEC 61131-3 is the programming-languages part; IEC 61131-1 is "General
  information", unrelated to this claim). A small, single-word-scope fix is
  open against `eclipse-4diac/4diac-documentation`
  ([#120](https://github.com/eclipse-4diac/4diac-documentation/pull/120));
  until it merges, `eclipse-4diac/4diac-documentation`'s `CONTRIBUTING.md`
  still has the wrong reference — don't report this specific mismatch as a
  new finding there while #120 is open. This suppression is scoped to that
  one upstream repo: a fork's own `CONTRIBUTING.md` is an independent copy
  that #120 doesn't touch, so if a fork hasn't synced from upstream, its
  copy staying wrong is a separate, still-worth-flagging fact about that
  fork being behind — not the same tracked issue.
- The three repos' `CONTRIBUTING.md` files are otherwise structurally
  identical (same template) with no contradictions between them, only
  repo-specific gaps (e.g. forte's is missing the "usability and UI
  improvements" contribution-type bullet that ide/documentation have) —
  that's not necessarily worth a PR on its own, just don't be surprised by
  the asymmetry.
- If you find a NEW contradiction or error while doing this check, propose
  the fix rather than silently working around it — a wrong guideline is
  itself worth reporting/fixing, same as a wrong code comment.

## Quick pre-commit / pre-PR checklist

Run through this explicitly, every time, in these repos:

1. Exactly one commit on the branch (squashed/amended, not appended)?
   Commit subject ≤ 50 chars, body wrapped ~72 chars?
2. Zero URLs, zero `#<number>` issue/PR references anywhere in the commit
   message (title or body)?
3. Every touched/added copyright statement has exactly **one** year (that
   statement's own first-contribution year), never a range — a new
   copyright *holder* gets its own separate statement (section 2), not a
   range merged into an existing one? Any brand-new source file starts
   with the full EPL-2.0 header template?
4. Comments are in **English**, and no longer than the surrounding file's
   own comment style?
5. No new speculative version bump without solid precedent?
6. No new loose `.md`/`.adoc`/`.asciidoc` file unless it's genuine, scoped,
   permanent documentation, in the format the target repo actually uses?
7. PR description matches the *current* diff — not a stale description of
   an earlier version of the change?
8. PR description (not commit) carries the issue/PR cross-references?
9. If the AI's contribution is substantive enough that you'd credit a
   human for it (i.e. not a trivial/boilerplate/rote change — see section
   1): commit footer has an `Assisted-by: [Provider] [Model-Family]
   ([Version/ID])` trailer disclosing the AI tool used, alongside any
   `Co-Authored-By:`/`Also-by:` co-author trailer?
10. Formatter/checker run and existing tests still pass; a new test was
    added only if the change is itself a new feature or bug fix — not
    required for a documentation, comment, header, version-bump, or
    PR-metadata-only change (section 7)? For an `.adoc`/`.asciidoc`
    change in `4diac-documentation`, was `mvn` run from the repo root to
    confirm it actually renders?
11. If AI-assisted: output vetted for accuracy/security/IP due diligence,
    no confidential/PII/sensitive content was put into the AI's input, and
    the platform's terms of use don't conflict with your Eclipse or
    employer obligations (section 8)? If a file is largely or entirely
    AI-generated (not just assisted): does its own header carry an
    `Assisted-by:` line, an AI Disclosure note, and the project-licence +
    `CC0-1.0` SPDX expression (section 1)?
12. Any contradiction or error noticed in the guidelines themselves
    reported or proposed as a fix, rather than silently worked around
    (section 9)?
13. Every comment/reply you're about to post on GitHub is addressed to
    an automated review tool (`user.type == "Bot"`), never to a human
    reviewer or the PR author (section 3a)?
14. Every push in this session is the first one for this round of
    fixes, or an explicit end-of-round signal actually happened since
    the last push (section 1, "Push once")?
