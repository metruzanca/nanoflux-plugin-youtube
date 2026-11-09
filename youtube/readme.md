# youtube

Fetches a channel's recent videos through YouTube's internal browse API rather
than the channel RSS. The RSS carries no video duration; browse returns duration
(and view count) for every entry in the same request.

## Quirks

- **Duration is shown as a pill** on the card. It is read from the video's
  thumbnail badge, so a live stream (badged "LIVE") or a fresh upload (badged
  "New") has no known duration and shows none.
- **Publish time is made absolute at fetch time.** Browse reports it only as
  relative text ("3 days ago"), which is converted to a real timestamp when the
  feed is polled, so a stored item never shows a frozen "3 days ago".
- **Thumbnails are cached at poll time.** Browse serves thumbnail URLs that stop
  working after a short while, so each video's image is downloaded into
  nanoflux's own storage when the feed is fetched. A video whose creator later
  changes its thumbnail keeps the cached image; it is not re-downloaded.
- Any channel URL form is resolved to its channel id, so the feed keeps working
  regardless of how the channel was linked.
- Channel feeds are stored on `www.youtube.com`. YouTube answers its internal
  browse endpoint only on that host (a request to the bare `youtube.com` returns
  HTTP 400), so the plugin owns the feed URL shape and rewrites the bare host.
- **Videos resume where you left off.** A video watched partway is remembered
  per account on the server, so opening it again (from any device or login)
  starts at the saved position instead of the beginning, with no prompt. A
  finished video starts over; use the player's own scrubber to restart sooner.

## Filtering

Items carry the video title and description, so the `title` and `summary` filter
fields work as usual. A members-only video carries the `members-only` category,
so a single tag filter row can show or hide members-only posts.

Other than that, items carry no site-specific categories.
