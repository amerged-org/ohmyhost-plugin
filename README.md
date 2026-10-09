# ohmyho.st

Build and host web apps from Claude. Tell your agent what you want to make, review a live preview, and publish when you are ready. Keep your application's files and version history in ohmyho.st, or continue using your existing GitHub repository. You can continue the same application in another chat or from a local checkout.

![ohmyho.st logo](./assets/icon.png)

## Connect and build

Install the plugin, then connect its ohmyho.st connector using your own ohmyho.st account. The server is `https://app.ohmyho.st/mcp`; sign-in uses managed OAuth, with no API key or device token to paste into the chat. Choose the workspace and applications your agent may access. You choose whether to grant production publishing separately from editing and previews.

Start with a request such as "Create a simple web app and show me a preview" or "Continue my existing application". For an existing application, the agent checks the reachable checkout, saved project binding, account, and workspace first. A local agent reuses matching existing CLI/MCP access unless you choose the remote connection. An unrelated account or an unreachable local workspace does not change your project's source.

The included Skills cover account and workspace access, portable TypeScript applications, managed source and GitHub deployments, domains and mail, PostgreSQL operations and encrypted database exports, selected Supabase migrations, deployment troubleshooting, and usage reports and non-payment project budget limits. Vite, Next.js, and TanStack Start applications follow the documented framework and runtime requirements. Hosting requires workspace access and available credits.

## What the plugin does

This package contains ten readable workflow Skills, their references and migration helpers, and one remote MCP configuration. It contains no hooks, local MCP executable, automatic install command, API keys, device tokens, or reviewer credentials. Installing it does not sign you in or start a deployment. The migration helpers run only when the agent follows a user-selected migration workflow; they inspect and prepare the chosen application's source. OpenAI agent metadata alongside two Skills is retained from the canonical source and has no Claude runtime role.

The Skills guide the agent to use ohmyho.st's own API through the connected server or an existing local CLI/MCP. Authorized requests can read project status, source and version history, deployment logs, configuration and usage, and can write application source, create previews, publish, or perform the other selected project operations. Requested source files, configuration and authorized project data are sent to ohmyho.st. OAuth identity is handled by WorkOS. Builds, hosting and private files use Cloudflare; managed PostgreSQL uses Neon; domains, mail and billing use the providers described in the public documentation. An explicitly selected GitHub workflow also uses the user's GitHub tools and repository access. External websites or installed tooling are used only as required by the selected workflow.

Secret setup uses protected portal input: do not paste a production secret into a conversation. Local credential context is verified without publishing credentials or transferring access from an unrelated account. The user controls the concrete workspace/project grants, and may revoke them. Operations that change production or delete data require the user's requested scope and consent. This Claude plugin does not initiate purchases, subscriptions, invoice payments or auto-recharge configuration. Funding and payment settings are manual actions in the user's signed-in portal. The current remote connector exposes no billing tools. An independently installed local CLI can expose wider product commands; this plugin does not disable that CLI, and its Skills do not authorize using its payment commands. The retained canonical surface reference describes that wider product catalog rather than expanding this Claude workflow.

## Documentation and support

Read the [documentation](https://docs.ohmyho.st/), [support guide](https://docs.ohmyho.st/support), [privacy policy](https://ohmyho.st/privacy), and [terms](https://ohmyho.st/terms). Contact ohmyho.st through [the contact page](https://ohmyho.st/contact).

This Apache-2.0 package uses the canonical customer Skills from release 0.1.31, source commit `f9de246dbdef023d1aa17e52353b486daa46ff4f`. The private hosting platform is not included. The README and Claude listing manifest are packaging additions. Eight workflow Skills and all references and helpers retain their canonical bytes. The usage-and-budgets Skill is a clearly recorded Claude-specific variant for usage reporting and non-payment budget limits; its payment workflows are excluded. The deploy-github Skill clarifies that the user activates Paid manually in the signed-in portal before the agent configures already requested mail. The source receipt records the original and derived hashes. This package does not assert that every directory policy check or native workflow test has passed.
