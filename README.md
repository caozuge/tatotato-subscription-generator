# tatotato subscription generator

A small edgetunnel-compatible generator, with actual proxy checks executed by scheduled GitHub Actions. The user's computer does not run any service.

## How it connects

Set the **优选订阅生成器** hostname in the existing edgetunnel admin page to `subgen.tatotato.site`. Keep using the original client subscription URL supplied by that admin page.

The generator implements edgetunnel's `/sub?host=example.com&uuid=00000000-0000-4000-8000-000000000000` request contract. It returns complete original node URIs so transport, TLS and exit parameters are preserved rather than reconstructed as Cloudflare ingress addresses.

## Refresh

A standard Ubuntu GitHub-hosted runner checks the public sources every six hours, at minute 43 UTC. Manual runs are available from Actions. The exact schedule is best-effort and may be delayed by GitHub.

Each refresh collects a distributed sample of up to 120 public candidates and retests the last selected nodes. It validates sing-box configurations, tests real proxy connections, checks exit country against two destinations, and repeats successful candidates for three rounds. It publishes at most one node per selected country, ranked by measured median latency. The latest results are stored only in Cloudflare KV, not in this repository. Results older than 24 hours are not served.

Only program source is stored here. The publishing credential is an Actions secret. Node snapshots, subscription tokens and private inputs must not be committed. There are no workflow artifacts or caches.

## Limits

Free upstream nodes may fail or lack some countries. Cloud-side tests do not guarantee the same country on every network or future availability. Verify from the target phone using My IP. This checks connectivity and latency, not sustained bandwidth or streaming unlocks.

GitHub may delay scheduled runs and may disable schedules for a public repository after an extended period without repository activity. Check the Actions page if results become stale. This repository does not create artificial activity to circumvent that policy.
