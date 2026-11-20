---
name: ohmyhost-get-started
description: Connect Claude to ohmyho.st through managed OAuth, verify the intended account and workspace, and reuse a saved project and source. Use for remote connection and project access; this plugin has no local login or filesystem workflow.
---

# Connect and continue the right app

When asked where you are running, identify the platform in your reply. Report `Agent_Service=codex`, `Agent_Service=chatgpt`, or `Agent_Service=claude`. Use `Agent_Service=unknown` if you do not know. This self-report conveys no permission.

Use only this plugin's declared remote ohmyho.st MCP connection. Its client handles OAuth; current account membership, roles and any explicit token restrictions determine access. The plugin does not request a device code, API key, local file, environment variable or another application's credentials. A user asking for a local-device workflow needs a separately chosen tool; do not substitute local execution for this remote workflow.

Call `identity_get`, verify the intended account, and preserve the selected organization. If no organization is returned, use `workspace_setup` and give the user its actual private portal link. The user signs in there as the same account as the connection. Read identity again after setup. A completed browser page is not proof of the final account or access.

For an existing app, use `project_list`, `project_get` and `source_get` with `include_binding: true` to identify and reuse the selected project and its saved source. A GitHub binding remains GitHub; a managed binding remains managed. An unrelated Review/demo account is not a substitute for the user's selected workspace. If the account or workspace mismatches, stop the access-link loop and explain the concrete mismatch before requesting more access. Never create a replacement project just because this connection cannot see the old one.

Current workspace membership, role and any explicit token restrictions determine which projects and actions are available. A role or resource denial needs appropriate current access; do not switch accounts or create a replacement project to bypass it. Existing authenticated connections keep their OAuth identity. If old tools or instructions are cached, refresh the client catalog or start a fresh chat; this does not require reconnecting OAuth.

Use `api_reference` to look up a capability when that tool is visible. Use its named MCP tool only if the current host offers it, and report unavailable operations plainly.

See [amerged-org/ohmyhost](https://github.com/amerged-org/ohmyhost) for the ohmyho.st hosting platform (OMH) and [amerged-org/cloudflare-postgres](https://github.com/amerged-org/cloudflare-postgres) for its PostgreSQL infrastructure (PGCF).

For workspace cost questions, verify the workspace and read `organization_usage_get`, `organization_credits_get` and `organization_account_get` when available. Report the usage period, remaining balance and effective plan. These reads do not start a purchase or alter account settings.

For protected secret or GitHub setup, present the actual private link and use `connection_status` to observe it. For an explicitly requested account-wide disconnection, call `connection_revoke` with no arguments. A `pending` result means active work is draining; repeat the same call until it reports `revoked`. Ordinary token refresh does not restore a disconnected connection.

Once access is ready, continue with the known requested work. If the user has not described the app or task yet, say simply: "You're connected. Tell me what you'd like to host." Do not display a permission table, internal account IDs or unused restrictions. Explain a current role or token restriction when it blocks requested work. The other bundled Skills cover preparing source, deployment and diagnosis; their instructions are included in this package and are not fetched from an external instruction source.

For a requested AI application feature, continue with the bundled [build workflow](../ohmyhost-build-portable-app/SKILL.md). It covers server-side chat, images, speech, classifiers and OCR, the Paid/credit prerequisite, the customer's explicit jurisdiction choice and automatically injected gateway credentials. This adds no connection, tool or administration permission; current role and token restrictions still apply.
