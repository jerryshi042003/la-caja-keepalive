# la-caja-keepalive

Keeps the free-plan Supabase project behind https://la-caja-demo.vercel.app from pausing for inactivity.

A scheduled GitHub Actions workflow (every 3 days, plus manual `workflow_dispatch`) makes one read request to the
project's REST API (`/rest/v1/stores?select=id&limit=1`) with the public anon key. Row-level security returns an
empty list, which is enough to count as activity.

Secrets (set with `gh secret set`, never committed):

- `SUPABASE_URL`: the project URL used by the deployed app
- `SUPABASE_ANON_KEY`: the public publishable key from the deployed app's bundle

If the project is already paused, this request fails (DNS error) and the run goes red; resume the project from the
Supabase dashboard.
