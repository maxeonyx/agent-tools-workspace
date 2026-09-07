# agent-tools-workspace

Shared local base directory for isolated
[agent-tools](https://github.com/maxeonyx/agent-tools) feature clones.

Each mutable task gets a full clone directly under this repository, named
`at-<feature-branch>`. Only the submodules needed by that task are initialized.
This repository tracks only the shared operating instructions; `.gitignore` is a
whitelist, so clones, task files, logs, and any other scratch state live here as
plain files without being committed. Put working files here, not in `/tmp`.

Example:

```bash
git clone git@github.com:maxeonyx/agent-tools.git at-issue-8-workspace
cd at-issue-8-workspace
git switch -c issue-8-workspace
git submodule update --init tools/trunc
```

See [AGENTS.md](AGENTS.md) before creating, updating, or removing a clone.
