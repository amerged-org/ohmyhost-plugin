---
name: ohmyhost-deploy
description: Save managed app versions, open a private live preview, and deploy, publish or roll back on request through remote ohmyho.st MCP tools. Reuse the saved project and source and switch only on explicit request.
---

# Version, preview and publish through ohmyho.st

Use the [customer voice](../ohmyhost-get-started/references/customer-voice.md) in replies.

Use only the declared remote MCP connection and bundled access workflow. Read `project_get`, `project_context_get`, `project_status` and `source_get` with `include_binding: true`. Preserve the verified account, organization, project, source binding and generation. The product name or chat platform does not select a different source.

## Choose or continue the source

For an existing managed source, read and edit its hosted files using the bundled build Skill. For an existing GitHub source, deploy the concrete pushed commit the user selected. This remote plugin cannot inspect a local checkout, create a GitHub repository, edit GitHub files, commit or push to GitHub. If new GitHub source work is needed, explain that concrete limitation and ask the user to prepare it in their chosen GitHub environment; do not call unrelated GitHub tools from this plugin.

When the chosen GitHub workspace connection is missing, use `github_status` and `github_connect` with the verified organization and a retained non-secret request identifier. Give the returned portal link to the user for sign-in and GitHub authorization; check `github_status` after completion. Do not read a GitHub token or reconstruct an OAuth callback. Link only the user's expressly selected repository with `source_link`, then follow its operation and reread the source. An authorization failure repairs that connection; it does not switch the source silently.

A new ordinary app made entirely in chat uses `project_create` with the selected organization and project name, `source_provider: "managed"` and `template: "vite-react"`. Respect an explicit region; otherwise ask once for US or EU, without inferring it from a server IP. Preserve an existing project's immutable region. Explain protected versus public Dev and shared versus isolated data before creating them; isolated data consumes resources separately. Protected Dev is the default and the API region default is US. Use the user's chosen modes explicitly when known.

Poll the accepted project operation and `source_get` until the source is ready with a persisted commit. A completed project operation can precede source initialization. Follow a returned child initialization operation rather than queueing duplicates. Use `source_initialize` only for a genuinely unbound project without pending initialization, with the actual generation, selected template and retained request identifier.

## Save a complete managed tree

Small related changes use `source_changes`. For a complete replacement tree within the limits, `source_upload_prepare` creates a single upload operation using the current binding, generation and expected parent. Choose `mode: "commit"` for an existing managed source. Stage every intended retained/new file with `source_blob_put` using the same operation and actual text or binary content. Preserve the returned `{path, sha256, byte_length, executable}` receipts. They are measured by the server; never invent hashes or byte lengths. Each file is at most 2 MiB; the complete tree is at most 8 MiB and 2,000 files.

Finalize the complete manifest with `source_upload_commit` using only receipts from this upload, its original generation and one retained request identifier. Include unchanged files that should remain; omitted previously saved files are deleted from the replacement tree. After uncertainty, inspect `operation_get` and `source_upload` under the original operation and keys. Require operation success and upload `state: "committed"`; a sealed candidate is not a deployable version. Keep this operation's confirmed `commit_sha`, even if another chat later advances the branch.

Only on the user's explicit GitHub-to-managed switch request, explain the source replacement and use `source_upload_prepare` with `mode: "switch"`, the existing GitHub connection/generation and a null parent. Stage the complete intended tree and finalize through `source_switch` with its original guards, receipts and retained request identifier; do not use ordinary upload commit. Observe the same operation and reread the binding. The old source remains active until success; switching increases generation and disables this project's GitHub auto-deploy. It preserves the project identity, URLs and data, does not import GitHub history, delete the repository or publish an app.

`source_versions` and `source_diff` inspect concrete saved commits. On a requested source restore, use `source_restore` with the current generation/head and chosen earlier commit; observe its operation and reread the new head. This creates a new version without erasing history. Source restore and deployment rollback are separate actions.

## Show the app to the user

Use the [preview workflow](references/preview.md) before building a deployed version.
Give the link first and explain when saving works. For a data app that needs setup,
follow the [database setup workflow](references/database-setup.md).
For GitHub, compare `applied_source.commit_sha` with the exact pushed commit the user
selected. If they differ or the applied commit is unavailable, explain that the preview
follows the bound branch and the selected commit has not been verified there.

## Build, inspect and publish

For a requested deployment or publication, use the exact commit selected for deployment, including when the branch preview shows a different version. Call `deployment_plan` for that commit and the requested environment. Review its actual costs, requirements and effects within the user's request. Use `secret_input_request` when required, give the user its protected portal link, and inspect only the returned status and secret metadata. Set required secrets for each target environment. If funding or another prerequisite blocks deployment, direct the user to their signed-in workspace.

Start `deployment_create` with the exact plan ID and one retained non-secret request identifier. Follow the returned `operation_get` at the suggested interval. On failure, read `deployment_get` and `deployment_logs` for the actual deployment before retrying. After success, read `project_status` to verify the deployed version. For a requested Dev address, use the handoff in the preview workflow. For a requested Prod deployment, share the verified public address.

The user inspects the preview in their browser. Use MCP deployment/status results for server-side completion and health evidence. For a data app, ask the customer to submit a permitted test record on the published site and check it after a reload. For other apps, invite them to try the main action. State which checks the customer still needs to perform. This plugin invokes no external browser, program or interpreter.

If the requested deployment already went to Prod, verify that operation. To publish a verified Dev deployment with the required role and token, read `promotion_plan` and use `deployment_promote` with the same deployment ID, exact ETag/precondition, confirmation token and retained request identifier. Follow the accepted operation, verify production status and share the public address. Use an exact-commit Prod plan when the user requests direct deployment and the plan supports it, including with shared data.

For an explicitly requested artifact rollback, select the earlier deployment from `deployment_list`, read `rollback_plan`, and use `deployment_rollback` with its exact guards and retained request identifier. Rollback changes the deployed artifact; it does not reverse database migrations or restore source history. After a stale/expired plan, read a fresh plan before a new attempt. Report the verified result in plain language.

For `auth.provider: omh-native`, a deployment plan or request can return `native_auth_launch_unavailable` (HTTP 409). Ask ohmyho.st support to confirm rollout access for the existing workspace and project, then plan the saved commit again. Check the selected environment’s Paid access, verified sender and approved application origin.

The service continues checking a sealed source candidate. Follow its original operation and receipt. If you repeat the mutation, keep the original arguments and idempotency key. For finalization retain the same complete manifest, source generation, upload operation ID and finalize key. Use the committed SHA returned for that operation. After a terminal failure, read the current source and resolve the cause before starting a new request with fresh guards and a new key.

## Delete a project

For a requested project deletion, call `delete_plan` for the selected project. Read its effects and explain what the deletion will remove. The plan returns a `resource_etag` and a `confirmation_token` valid for ten minutes. `delete_execute` takes the selected `project_id`, `if_match` set to the returned `resource_etag`, the `confirmation_token` and a retained `idempotency_key`. Follow the returned `operation_get` through completion. Deletion removes deployments, databases, files, schedules, stored SQL exports and the domain, mail and DNS records ohmyho.st created. If database records or files need to be retained, direct the user to export or download them before deleting. If the operation fails, follow its suggested action and plan again once no other project operation is running.

Skill release 0.1.84. Compare it with the version at <https://ohmyho.st/client-release.json>. If your copy is older, refresh the installed Skill or plugin before following its instructions.
