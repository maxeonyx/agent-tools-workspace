# agent-tools-workspace

Shared local base directory for isolated
[agent-tools](https://github.com/maxeonyx/agent-tools) feature clones.

Each mutable task gets a full clone directly under this repository, named
`at-<feature-branch>`. Only the submodules needed by that task are initialized.
The clones are ignored by git; this repository tracks only the shared operating
instructions.

Example:

```bash
git clone git@github.com:maxeonyx/agent-tools.git at-issue-8-workspace
cd at-issue-8-workspace
git switch -c issue-8-workspace
git submodule update --init tools/trunc
```

See [AGENTS.md](AGENTS.md) before creating, updating, or removing a clone.
