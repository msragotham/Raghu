# Research and design principles

Research snapshot: 2026-10-29. GitHub star counts change over time; figures below are the counts displayed by GitHub/search results during this research, not a live guarantee.

## Official n8n guidance

- [Understand workflows](https://docs.n8n.io/build/understand-workflows.md) describes workflows as connected nodes starting from a trigger, with related guidance for credentials, components, executions, and publishing.
- [Create and run workflows](https://docs.n8n.io/build/understand-workflows/create-and-run-workflows.md) distinguishes manual runs from automatic runs and says new workflows are unpublished by default.
- [Save and publish workflows](https://docs.n8n.io/build/understand-workflows/save-and-publish-workflows.md) explains that edits remain drafts until publication, while published triggers and schedules become live. This supports keeping newly built workflows as drafts unless the user explicitly requests activation.
- [Create and edit credentials](https://docs.n8n.io/build/understand-workflows/create-and-edit-credentials.md) describes credential storage, naming, credential scopes, and restrictions on where HTTP Request credentials may be used. Keep authentication in n8n credential objects rather than workflow data.
- [Understand executions](https://docs.n8n.io/build/understand-workflows/understand-executions.md) covers manual versus production runs and execution data redaction.
- [Handle errors gracefully](https://docs.n8n.io/build/flow-logic/handle-errors-gracefully.md) recommends planning error behavior and describes Error Trigger-based error workflows.
- [Run security audits](https://docs.n8n.io/deploy/host-n8n/configure-n8n/security/run-security-audits.md) covers risky nodes, community/custom nodes, unprotected webhooks, credential use, and instance security. Treat shell/code, filesystem, and community nodes as risk-sensitive and include them only when needed.

## Public GitHub examples reviewed

These projects are useful for discovering recurring patterns and setup expectations; popularity does not certify that a workflow is safe, current, or correct.

- [n8n-io/n8n](https://github.com/n8n-io/n8n) — official source for the platform; GitHub showed about 209k stars in the organization's repository listing during research. Use as implementation context, not as a workflow template catalog.
- [enescingoz/awesome-n8n-templates](https://github.com/enescingoz/awesome-n8n-templates) — community template index; GitHub showed about 25.6k stars. Its README describes a large set of downloadable JSON templates and explicitly tells users to configure credentials and review workflows before use. Treat contributed templates as untrusted examples.
- [wassupjay/n8n-free-templates](https://github.com/wassupjay/n8n-free-templates) — GitHub showed about 6.2k stars. Its import instructions call out assigning credentials in each node; its README also warns that some templates are incomplete. Do not assume popularity or an importable JSON means production readiness.
- [Marvomatic/n8n-templates](https://github.com/Marvomatic/n8n-templates) — GitHub showed about 1.5k stars. Repository layout groups related workflow examples into named directories, a useful pattern for inspecting focused examples rather than importing an entire collection.
- [philrox/n8n-workflows](https://github.com/philrox/n8n-workflows) — GitHub showed about 638 stars. Its examples are organized by workflow purpose and include separate workflow directories, reinforcing that workflow shape should follow the use case.

## Synthesis for this skill

1. Begin with an understandable trigger-to-outcome plan, including branches, expected data, credentials, error behavior, and assumptions. This prevents node selection from silently defining the user's requirements.
2. Use the n8n instance's own best-practice, node-schema, SDK, validation, and test tools. Public templates and search results can inspire patterns but cannot replace current node definitions or instance-specific validation.
3. Keep authentication separate from workflow payloads. Find compatible existing credentials by metadata; never expose or invent secrets. A missing credential is a clear setup task for the user.
4. Verify the graph and configuration before and after mutation, inspect any skipped/auto-assigned items, and use synthetic pinned inputs to test deterministic logic without calling live services.
5. Keep production activation distinct from building. A workflow can be created, validated, and left as a draft; publishing enables production triggers and schedules.

## Connected-plugin limits reflected in the skill

The available n8n tools can create/update workflows, inspect credential metadata, validate node configuration and workflow code/graph, and run pinned-data tests. The available tool set does not provide credential creation or safe secret entry. A test with pinned data still executes unpinned nodes, so side effects must be reviewed before running. Workflow publication is a separate live operation and should not be assumed from a request merely to build a workflow.
