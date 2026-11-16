# Changelog — Helix & SilentQuants (preview)

Every change below is a git commit in this repository. This file is the readable version of that history.
Dates are KST. Newest first.

## 2026-11-16

**SilentQuants site (preview at /site/sq, deployed later to silentquants.com)**
- Restructured as Why / What / How, grounded in the SilentQuants README: purpose, the journey
  (Learn → Research → Develop → Validate → Fund → Operate → Improve), current milestone v0.1.0.
- Status vocabulary reduced to three words with a printed key: LIVE / BUILDING / PLANNED.
- Cost calculator restored: fee ÷ pass rate, with assumptions and exclusions printed next to the answer.
- New section "The odds": 100 squares — 7 paid, 7 passed without payout, 86 neither — sourced from a
  published aggregate of ~300,000 accounts across ten firms.
- New section "Objections": the five questions a careful reader has, answered before they ask.
- Library page stops pretending: three guides listed as drafts, not published.
- Pastel teal accent (#5fe0c8) replacing violet; every violet `rgba()` in the shared stylesheet rewritten to a
  variable so hover borders and focus rings follow the accent.
- Floating pill header with a wordmark that swaps colour on hover; compact one-line footer.

**Helix**
- PropFirmMatch partner codes removed entirely: no static code table, no runtime merge, no PFM columns or
  filters. The offers page now shows only codes read from a firm's own page.
- PropFirmMatch ratings removed from the table and the drawer — a competitor's score is not our evidence.
- Leverage column added and shown by default; values inherited from third parties carry a superscript `C`
  until verified against the firm's own page (172 rows today).
- Header navigation settled as 챌린지 · 프랍펌 (전체/규칙/스프레드) · 브로커 · 프로모션 · 도구.
- Filter rail: the "More filters" wrapper removed (it doubled the groups), every group collapsible and
  draggable, scroll position preserved across rebuilds.
- Accent moved from violet to deeper teal (#35c4ae).
- Fixed price matching, discontinued-row detection, rule coverage and duplicate collapsing after a lost
  rebuild — 2,166 rows, 71 firms, 85% grade A.
