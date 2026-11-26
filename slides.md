---
theme: default
title: Dev Containers
---

# Dev Containers

Reproducible development environments

---

# The problem

- "Works on my machine"
- Long onboarding docs
- Tool version drift across the team

---

# What is a dev container?

A container configured as a full development environment, described in `.devcontainer/devcontainer.json`.

```json
{
  "name": "My Project",
  "image": "mcr.microsoft.com/devcontainers/typescript-node:22",
  "customizations": {
    "vscode": { "extensions": ["dbaeumer.vscode-eslint"] }
  }
}
```

---

# Questions?
