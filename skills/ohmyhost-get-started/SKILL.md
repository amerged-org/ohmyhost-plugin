---
name: ohmyhost-get-started
description: Connect Claude to ohmyho.st through managed OAuth, verify the intended account and workspace, and reuse a saved project and source. Use for remote connection and project access; this plugin has no local login or filesystem workflow.
---

# Connect and continue the right app

Use only this plugin's declared remote ohmyho.st MCP connection. Its client handles OAuth; project access is granted separately in the signed-in ohmyho.st portal. The plugin does not request a device code, API key, local file, environment variable or another application's credentials. A user asking for a local-device workflow needs a separately chosen tool; do not substitute local execution for this remote workflow.

Call `identity_get`, verify the intended account, and preserve the selected organization. If no organization is returned, use `workspace_setup` and give the user its actual private portal link. The user signs in there as the same account as the connection. Read identity again after setup. A completed browser page is not proof of the final account or access.

For an existing app, use `project_list`, `project_get` and `source_get` with `include_binding: true` to identify and reuse the selected project and its saved source. A GitHub binding remains GitHub; a managed binding remains managed. An unrelated Review/demo account is not a substitute for the user's selected workspace. If the account or workspace mismatches, stop the access-link loop and explain the concrete mismatch before requesting more access. Never create a replacement project just because this connection cannot see the old one.

Read `connection_grants` for the verified organization silently. Consent to create new apps can exist with no existing projects selected. Editing/previews and production publication have separate permissions. The `allow_publish_created_projects` grant covers newly created projects under that consent; existing projects require their own publication grant. When access is missing for the requested action, use `connection_request` for that organization: select the intended `project_ids`, `allow_create` only for requested app creation, and `allow_publish` only for requested publication. For a portal picker of an existing project, send `project_ids: []`, `allow_create: false` and `allow_publish: false`; the user selects their actual project there. Do not invent a project ID.

Present one short localized sentence and the actual returned approval link or connection card. Pass `return_url` only when the actual current conversation URL is known; do not construct it from a session identifier. After the user approves, call `connection_status` with the returned `request_id`, then reread grants and project visibility. Preserve the same request while pending; a declined, expired or mismatched request is not permission to retry under another identity. For an explicitly requested disconnection, `connection_revoke` revokes this organization's connection access; verify its result.

Once access is ready, continue with the known requested work. If the user has not described the app or task yet, say simply: "You're connected. Tell me what you'd like to host." Do not display a permission table, internal grant IDs or unused restrictions. Explain publication permission only when a requested publication needs it. The other bundled Skills cover preparing source, deployment and diagnosis; their instructions are included in this package and are not fetched from an external instruction source.

For a requested AI application feature, continue with the bundled [build workflow](../ohmyhost-build-portable-app/SKILL.md). It covers server-side chat, images, speech, classifiers and OCR, the Paid/credit prerequisite, the customer's explicit jurisdiction choice and automatically injected gateway credentials. This adds no connection, tool or administration permission; existing project editing and publication grants still apply.
