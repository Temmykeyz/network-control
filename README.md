# Network Control Room

One web page for running a network of Claude agent chats from any browser, outside Claude. You sign in with your email and get:

1. **Needs you**: the questions your agents have for you, with answer buttons.
2. **Your projects**: one tile per project, showing its status, its next step and a button that opens the lead chat.
3. **Run a task now**: start any scheduled Claude task straight away.
4. **The network**: how much the network handled for you today, and how many problems are open.

The page is static, with no server of its own. It talks to your hub, a Supabase project, through one Edge Function (`owner-api`), which only answers the owner's signed-in email.

## Reuse it for your own network

1. Set up the hub from the Agent Network Blueprint (Supabase tables, functions and `owner-api`).
2. In `index.html`, edit the `window.HUB` settings block: your Supabase URL, your publishable key, your HQ chat link and your project leads.
3. In the hub, set the owner email: `insert into network_facts(key,fact) values ('owner.email','you@example.com');`
4. Publish the folder with GitHub Pages (Settings → Pages → Deploy from branch → main).
5. In Supabase → Authentication → URL Configuration, set the Site URL to your Pages address.
6. To use "Run now" for a task: in the Claude app, add an API trigger to the task and copy its key. Save the key in Supabase → Vault as `task_token_<task id>`. Only the owner ever handles keys.

## Security

- The publishable key is meant to be public. All hub tables have row-level security, and public API access to functions is revoked.
- `owner-api` checks that the signed-in user's email matches `owner.email` before it does anything.
- Task keys live only in Supabase Vault. They are never in this page.
