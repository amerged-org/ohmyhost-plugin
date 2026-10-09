---
name: ohmyhost-usage-and-budgets
description: Explain ohmyho.st usage, remaining credits, project spending limits and HTTP 402 pauses. Read costs and, when requested, adjust non-payment project budget limits. Purchases, subscriptions and payment settings remain manual actions in the user's portal.
---

# Explain usage and project budgets

This Claude workflow reports hosting consumption and manages a project's non-payment spending limit. It does not initiate a purchase, subscribe, change a payment method, enable or disable auto-recharge, authorize recurring charges, or pay an invoice. This boundary applies even when an existing local CLI/MCP exposes additional billing commands; the product's wider command catalog is not authorization or a capability of this plugin.

## Read the actual account and usage

Verify the intended account and workspace with `identity_get` and reuse the selected project's saved context. Discover the current tool contract before calling it. With a matching existing local CLI/MCP, use the available read-only `organization_credits_get`, `organization_usage_get`, `organization_account_get` and `project_budget_get` functions for the requested workspace and period. Follow pagination and use actual returned values rather than fixed prices or assumed allowances.

A native remote chat currently has the narrower ohmyho.st connector. Use `project_context_get` or `project_status` for the authorized project and report only the balance, allowance or pause information those results actually expose. If a requested usage report or budget function is unavailable, explain that limit briefly and direct the user to the signed-in portal. Do not install a local client or borrow unrelated credentials to expand a remote chat's access.

State available and reserved credits, the measured period, and the largest returned project, environment or meter costs. Separate posted consumption from reservations; delayed measurements do not establish zero usage. An organization shares its wallet across projects. A project budget limits consumption and is not another balance. Preserve the requested workspace, period and environment, and never print credentials or payment details.

## A requested project budget change

Read the current budget first. Explain that `stop` pauses both Dev and Prod when measured usage plus reservations reach the configured limit, while `continue` permits continued consumption. Adjust or remove a project limit only when the user requested that exact project, mode and limit. Use the discovered `project_budget_set` contract or its existing local CLI equivalent with a saved idempotency key. The current local contract takes `amount_micros` and `mode`: one credit is 1,000,000 microcredits; a null limit clears the budget and requires `continue`. Read the result back; do not change a budget merely to explain costs.

A project budget change does not buy credits, activate Paid, configure a card or create payment consent. Keep it separate from the organization's payment settings. If the command is unavailable or the caller lacks permission, use the portal handoff instead of inventing an endpoint or switching accounts.

## An application paused with HTTP 402

Read the actual project status, current context and error before choosing the next step. Identify whether the pause comes from a project budget, unavailable credits, or a domain's eligibility. For a requested non-payment budget correction, follow the budget workflow above. For an authorized hosting change, use its relevant deployment, database or domain Skill within the user's request.

If the resolution needs funding, a subscription, invoices, a payment method or auto-recharge, ask the user to open [their ohmyho.st workspace](https://app.ohmyho.st/) and complete that action themselves. Report the prerequisite in one plain sentence. Do not call a billing mutation through MCP, REST, CLI or a browser on their behalf. After they say it is complete, read the original project and usage state again; the portal return alone does not establish payment or available credit. Do not repeatedly start new operations while the original pause remains.

For a suspected incorrect charge or a billing diagnostic that requires support, share [ohmyho.st support](https://docs.ohmyho.st/support) or [the contact page](https://ohmyho.st/contact) without payment details or raw records. Preserve the original operation and invoice identifiers when the user needs them; never repair a billing problem by creating a replacement purchase or payment consent.
