# YouTube plugin notes

Implementation invariants for maintainers. The user-facing documentation is
`youtube/readme.md`.

- The plugin fetches a channel's recent videos through YouTube's internal browse
  API (`youtubei/v1/browse`), not the channel RSS. The RSS carries no video
  duration; browse returns duration (and views) for every entry in the same
  request. Browse reports publish times only as relative text ("3 days ago"),
  which the plugin converts to an absolute UTC timestamp at fetch time, so a
  stored item never shows a frozen relative date.
- The plugin derives the browse origin from the feed URL's host, so it talks to
  the same origin the feed names and a test can point it at a mock host with no
  environment plumbing.
- `Match` must honour the capability it is asked about. It claims `CapIcon` on
  YouTube hosts and returns the bundled SVG from `Icon`.
- Members-only videos are detected from the browse lockup's metadata badge
  (`badgeViewModel.badgeStyle == "BADGE_MEMBERS_ONLY"`, label "Members only")
  and tagged with the `members-only` category (`lockupMembersOnly`). The badge
  is absent on a normal video; do not infer it from the title or description.
  The host's tag filter uses the category to show or hide these posts.
