---
name: ohmyhost-deploy
description: Save managed app versions, deploy an exact managed or existing GitHub commit, inspect its Dev preview, and publish or roll back an authorized artifact through remote ohmyho.st MCP tools. Reuse the saved project and source; switch only on explicit request.
---

# Version, preview and publish through ohmyho.st

Use only the declared remote MCP connection and bundled access workflow. Read `project_get`, `project_context_get`, `project_status` and `source_get` with `include_binding: true`. Preserve the verified account, organization, project, source binding and generation. The product name or chat platform does not select a different source.

## Choose or continue the source

For an existing managed source, read and edit its hosted files using the bundled build Skill. For an existing GitHub source, deploy the concrete pushed commit the user selected. This remote plugin cannot inspect a local checkout, create a GitHub repository, edit GitHub files, commit or push to GitHub. If new GitHub source work is needed, explain that concrete limitation and ask the user to prepare it in their chosen GitHub environment; do not call unrelated GitHub tools from this plugin.

When the chosen GitHub workspace connection is missing, use `github_status` and `github_connect` with the verified organization and a retained non-secret request identifier. Give the returned portal link to the user for sign-in and GitHub authorization; check `github_status` after completion. Do not read a GitHub token or reconstruct an OAuth callback. Link only the user's expressly selected repository with `source_link`, then follow its operation and reread the source. An authorization failure repairs that connection; it does not switch the source silently.

A new ordinary app made entirely in chat uses `project_create` with approved organization/name, `source_provider: "managed"` and `template: "vite-react"`. Respect an explicit region; otherwise ask once for US or EU, without inferring it from a server IP. Preserve an existing project's immutable region. Explain protected versus public Dev and shared versus isolated data before creating them; isolated data consumes resources separately. Protected Dev is the default and the API region default is US. Use the user's chosen modes explicitly when known.

Poll the accepted project operation and `source_get` until the source is ready with a persisted commit. A completed project operation can precede source initialization. Follow a returned child initialization operation rather than queueing duplicates. Use `source_initialize` only for a genuinely unbound project without pending initialization, with the actual generation, selected template and retained request identifier.

## Save a complete managed tree

Small related changes use `source_changes`. For a complete replacement tree within the limits, `source_upload_prepare` creates a single upload operation using the current binding, generation and expected parent. Choose `mode: "commit"` for an existing managed source. Stage every intended retained/new file with `source_blob_put` using the same operation and actual text or binary content. Preserve the returned `{path, sha256, byte_length, executable}` receipts. They are measured by the server; never invent hashes or byte lengths. Each file is at most 2 MiB; the complete tree is at most 8 MiB and 2,000 files.

Finalize the complete manifest with `source_upload_commit` using only receipts from this upload, its original generation and one retained request identifier. Include unchanged files that should remain; omitted previously saved files are deleted from the replacement tree. After uncertainty, inspect `operation_get` and `source_upload` under the original operation and keys. Require operation success and upload `state: "committed"`; a sealed candidate is not a deployable version. Keep this operation's confirmed `commit_sha`, even if another chat later advances the branch.

Only on the user's explicit GitHub-to-managed switch request, explain the source replacement and use `source_upload_prepare` with `mode: "switch"`, the existing GitHub connection/generation and a null parent. Stage the complete intended tree and finalize through `source_switch` with its original guards, receipts and retained request identifier; do not use ordinary upload commit. Observe the same operation and reread the binding. The old source remains active until success; switching increases generation and disables this project's GitHub auto-deploy. It preserves the project identity, URLs and data, does not import GitHub history, delete the repository or publish an app.

`source_versions` and `source_diff` inspect concrete saved commits. On a requested source restore, use `source_restore` with the current generation/head and chosen earlier commit; observe its operation and reread the new head. This creates a new version without erasing history. Source restore and deployment rollback are separate actions.

## Build, inspect and publish

For the confirmed commit, call `deployment_plan` with `environment: "dev"`. Review its actual costs, requirements and effects within the user's request. Use `secret_input_request` when required, giving the user its protected portal link; inspect only the returned status and secret metadata. Secrets are environment-specific and are not copied from Dev to Prod. If hosting needs credits or a capability not offered by this plugin, direct the user to their signed-in workspace; do not start payments or invent an administration tool.

Start `deployment_create` with the exact plan ID and one retained non-secret request identifier. Follow the returned `operation_get` at the suggested interval. On failure, read `deployment_get` and `deployment_logs` for the actual deployment rather than starting duplicate builds. After success, read `project_status` for the actual Dev URL. Public Dev opens directly. For protected Dev, give the user the private `share_url` from `project_dev_share_link_get`; A requested one-use preview entry can use `project_dev_access_create`; give its returned link to the user and do not issue another unused ticket. Keep access links out of source and shared notes.

The user inspects the preview in their browser. Use MCP deployment/status results for server-side completion and health evidence; ask the user to confirm the requested visible interactions when publication depends on that review. This plugin invokes no external browser, program or interpreter. A server-side health result alone does not establish that every app interaction works; report that limitation instead of claiming an agent-run acceptance test.

Publish only when the user requested it and the concrete project grant permits it. Read `promotion_plan` for the verified Dev deployment and use `deployment_promote` with that same deployment ID, exact ETag/precondition, confirmation token and retained request identifier. This promotes the tested artifact without a new build. Follow the accepted operation and verify actual production status. Shared Dev/Prod data uses Dev followed by promotion; a separately requested direct Prod deployment is supported only when its current data mode and plan allow it.

For an explicitly requested artifact rollback, select the earlier deployment from `deployment_list`, read `rollback_plan`, and use `deployment_rollback` with its exact guards and retained request identifier. Rollback changes the deployed artifact; it does not reverse database migrations or restore source history. After a stale/expired plan, read a fresh plan before a new attempt. Report the resulting URL, saved version and actual operation outcome in plain language.
