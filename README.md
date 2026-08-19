# WP Cron Pinger

Keeps scheduled tasks running on low-traffic WordPress sites.

WordPress fires its scheduled jobs (`wp-cron`) only when someone loads a page. If a
site is quiet, time-based work silently stalls: automated emails sit in the queue,
welcome sequences pause between steps, and scheduled posts miss their slot.

This repo pings `wp-cron.php` every 5 minutes via GitHub Actions, so the site's
schedule runs on the clock instead of on visitor luck.

## Sites covered

| Site | Purpose |
|---|---|
| niplifted.com | FluentCRM welcome sequence + FluentSMTP/Mailgun email sends |

## Notes

- `wp-cron.php` is a public endpoint on every WordPress site. No credentials are
  used or stored here, which is why this repo can safely be public (public repos
  get unlimited free Actions minutes).
- WordPress's built-in visitor-triggered cron is left enabled as a fallback, so
  the two mechanisms are redundant rather than exclusive.
- `keepalive.yml` makes a small weekly commit because GitHub turns off scheduled
  workflows after 60 days of repo inactivity.
- To add a site, add another ping step in `.github/workflows/wp-cron.yml`.
- Scheduled runs on GitHub are best-effort and can drift a few minutes under load.
  That is well within tolerance for email sequences measured in hours and days.
