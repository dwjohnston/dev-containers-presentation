---
theme: default
title: "Containing the Oopsie: Dev Containers for AI-Assisted Development"
---

# Containing the Oopsie

## Dev Containers for AI-Assisted Development

<div class="flex items-center gap-4 mt-12">
  <img src="/david-johnston.jpg" alt="David Johnston" class="w-24 h-24 rounded-full object-cover" />
  <div class="text-xl">David Johnston</div>
</div>

---

# Why dev containers?

<img v-click src="/ai-thinking.png" alt="An AI assistant reasoning about how to work around limits on the local machine" class="mt-8 rounded shadow-lg" />

---

# You give AI an impossible task and it starts trying all different things.

---
clicks: 5
---

# What is a dev container?

<p v-if="$clicks >= 1">Turns out VS Code is both a client and a server</p>

<div class="diagram" v-if="$clicks >= 2">
  <div class="box client">
    <div class="label">VS Code</div>
    <div class="sub">Client (UI)</div>
  </div>

  <div class="arrow">⟷</div>

  <div class="machine" :class="{ shown: $clicks >= 4 }">
    <div class="host-label machine-label">Your local machine</div>
    <div class="host" :class="{ highlight: $clicks >= 5 }">
      <div class="host-label">
        <span v-if="$clicks === 2">Your local machine</span>
        <span v-else-if="$clicks === 3">A remote server</span>
        <span v-else>A Docker container</span>
      </div>
      <div class="box server">
        <div class="label">VS Code</div>
        <div class="sub">Server</div>
      </div>
      <div class="sub mt-2">Files · terminal · extensions · tools</div>
    </div>
  </div>
</div>

<div v-if="$clicks >= 5" class="callout">
  ↑ This is what we are talking about today
</div>

<style>
.diagram {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2rem;
  margin-top: 3rem;
}
.box {
  border: 2px solid #4b82c4;
  border-radius: 0.5rem;
  padding: 1rem 1.5rem;
  text-align: center;
  background: rgba(75, 130, 196, 0.1);
}
.label { font-size: 1.25rem; font-weight: 600; }
.sub { font-size: 0.875rem; opacity: 0.7; }
.arrow { font-size: 2.5rem; }
.host {
  border: 2px dashed #888;
  border-radius: 0.75rem;
  padding: 1rem 1.5rem 1.25rem;
  text-align: center;
  min-width: 16rem;
  transition: border-color 0.3s, box-shadow 0.3s;
}
.host.highlight {
  border-color: #e8a33d;
  box-shadow: 0 0 0 4px rgba(232, 163, 61, 0.3);
}
.host-label { font-weight: 600; margin-bottom: 0.75rem; }
.machine {
  border: 2px solid transparent;
  border-radius: 0.75rem;
  padding: 0.75rem 1rem 1rem;
  text-align: center;
  transition: border-color 0.3s;
}
.machine .machine-label { visibility: hidden; }
.machine.shown { border-color: #888; }
.machine.shown .machine-label { visibility: visible; }
.callout {
  margin-top: 1.5rem;
  margin-left: 50%;
  font-size: 1.25rem;
  font-weight: 600;
  color: #e8a33d;
}
</style>

---
layout: center
---

<div class="text-9xl font-bold">Demo</div>

---

# Problems

<div class="problems">
  <div v-click>No tools</div>
  <div v-click>→ Update the Dockerfile with the requisite tools</div>

  <div v-click>No shell aliases</div>
  <div v-click>→ Mount them</div>

  <div v-click>Conversations are lost</div>
  <div v-click>→ Volumes to persist them</div>

  <div v-click>OAuth tokens are lost</div>
  <div v-click>→ Volumes to persist them</div>

  <div v-click>Platform incompatibilities</div>
  <div>
    <div v-click>→ Mount <code>node_modules</code> to a separate volume</div>
    <div v-click class="mt-1"><span class="opacity-60">or</span> → Just abandon the host mounting</div>
  </div>

  <div v-click>Extensions are not installed</div>
  <div v-click>→ Include them in <code>devcontainer.json</code></div>

  <div v-click>It has access to our host git credentials</div>
  <div v-click>→ Remove them in <code>devcontainer.json</code></div>
</div>

<style>
.problems {
  display: grid;
  grid-template-columns: max-content 1fr;
  column-gap: 2.5rem;
  row-gap: 0.6rem;
  margin-top: 1.5rem;
  font-size: 1.15rem;
}
.problems > div:nth-child(odd) { font-weight: 600; }
.problems > div:nth-child(even) { color: #2e8b57; }
</style>

---

# Removing host git credentials

`devcontainer.json`

```jsonc
"remoteEnv": {
  "SSH_AUTH_SOCK": null,
  "GIT_ASKPASS": null,
  "SSH_ASKPASS": null
},
"customizations": {
  "vscode": {
    "settings": {
      // Keep the host's Git configuration out of the container
      "dev.containers.copyGitConfig": false,
      // Don't install the host credential helper
      "dev.containers.gitCredentialHelperConfigLocation": "none",
      // Disable VS Code's askpass helper for Git prompts
      "git.useIntegratedAskPass": false,
      // Don't inject Git authentication into terminals
      "git.terminalAuthentication": false
    }
  }
}
```

---
layout: center
---

<div class="text-9xl font-bold">Demo</div>

---
layout: center
class: text-center
---

# Template here

<div class="flex flex-col items-center gap-6 mt-8">
  <img src="/template-qr.svg" alt="QR code for the meta-repo-template repository" class="w-64 h-64" />
  <a href="https://github.com/dwjohnston/meta-repo-template" class="text-2xl">github.com/dwjohnston/meta-repo-template</a>
</div>

---

# What sucks

<v-clicks>

- Docker performance can be sluggish
- Changing the environment variables is tedious

</v-clicks>

---

# Secrets

For secrets, I'm really thinking a tool like **Infisical** is the way to go

<svg viewBox="0 0 900 380" class="secrets-svg">
  <defs>
    <marker id="ah-req" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#888"/></marker>
    <marker id="ah-mint" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#8a5cc4"/></marker>
    <marker id="ah-tok" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#2e8b57"/></marker>
  </defs>
    <line v-click="1" class="req" x1="201" y1="167" x2="350" y2="200" marker-end="url(#ah-req)"/>
  <line v-click="3" class="tok" x1="354" y1="186" x2="205" y2="153" marker-end="url(#ah-tok)"/>
  <line v-click="1" class="req" x1="205" y1="284" x2="354" y2="247" marker-end="url(#ah-req)"/>
  <line v-click="3" class="tok" x1="350" y1="233" x2="201" y2="271" marker-end="url(#ah-tok)"/>
  <line v-click="1" class="req" x1="695" y1="153" x2="546" y2="186" marker-end="url(#ah-req)"/>
  <line v-click="3" class="tok" x1="550" y1="200" x2="699" y2="167" marker-end="url(#ah-tok)"/>
  <line v-click="1" class="req" x1="699" y1="271" x2="550" y2="233" marker-end="url(#ah-req)"/>
  <line v-click="3" class="tok" x1="546" y1="247" x2="695" y2="284" marker-end="url(#ah-tok)"/>
  <line v-click="1" class="req" x1="457" y1="320" x2="457" y2="253" marker-end="url(#ah-req)"/>
  <line v-click="3" class="tok" x1="443" y1="253" x2="443" y2="320" marker-end="url(#ah-tok)"/>
  <line v-click="2" class="mint-back" x1="354" y1="79" x2="420" y2="181" marker-end="url(#ah-mint)"/>
  <line v-click="2" class="mint" x1="431" y1="173" x2="365" y2="71" marker-end="url(#ah-mint)"/>
  <line v-click="2" class="mint-back" x1="535" y1="71" x2="469" y2="173" marker-end="url(#ah-mint)"/>
  <line v-click="2" class="mint" x1="480" y1="181" x2="546" y2="79" marker-end="url(#ah-mint)"/>
  <rect class="hub" x="360.0" y="185.0" width="180" height="60" rx="10"/>
  <text class="hub-t" x="450" y="219">Infisical</text>
  <text class="hub-s" x="450" y="237">holds admin tokens</text>
    <rect class="node" x="35.0" y="118.0" width="160" height="44" rx="8"/><text class="node-t" x="115" y="146">Host</text>
  <rect class="node" x="35.0" y="278.0" width="160" height="44" rx="8"/><text class="node-t" x="115" y="306">Container</text>
  <rect class="node" x="705.0" y="118.0" width="160" height="44" rx="8"/><text class="node-t" x="785" y="146">Claude cloud etc.</text>
  <rect class="node" x="705.0" y="278.0" width="160" height="44" rx="8"/><text class="node-t" x="785" y="306">CI pipeline</text>
  <rect class="node" x="370.0" y="328.0" width="160" height="44" rx="8"/><text class="node-t" x="450" y="356">Application</text>
  <rect class="prov" x="260.0" y="23.0" width="160" height="44" rx="8"/><text class="node-t" x="340" y="51">GitHub</text>
  <rect class="prov" x="480.0" y="23.0" width="160" height="44" rx="8"/><text class="node-t" x="560" y="51">Postgres</text>
</svg>

<div class="legend">
  <span v-click="1"><span class="sw req-sw"></span> requests a token</span>
  <span v-click="2"><span class="sw mint-sw"></span> mints it with the admin token</span>
  <span v-click="3"><span class="sw tok-sw"></span> gets a short-lived token</span>
</div>

<style>
.secrets-svg { width: 100%; height: 340px; margin-top: 0.25rem; }
.secrets-svg .hub { fill: rgba(232, 163, 61, 0.15); stroke: #e8a33d; stroke-width: 2; }
.secrets-svg .hub-t { font-size: 22px; font-weight: 600; text-anchor: middle; fill: currentColor; }
.secrets-svg .hub-s { font-size: 12px; text-anchor: middle; fill: currentColor; opacity: 0.7; }
.secrets-svg .node { fill: rgba(75, 130, 196, 0.1); stroke: #4b82c4; stroke-width: 2; }
.secrets-svg .node-t { font-size: 16px; font-weight: 600; text-anchor: middle; fill: currentColor; }
.secrets-svg .req { stroke: #888; stroke-width: 2; stroke-dasharray: 5 4; }
.secrets-svg .tok { stroke: #2e8b57; stroke-width: 2.5; }
.secrets-svg .mint, .secrets-svg .mint-back { stroke: #8a5cc4; stroke-width: 2; }
.secrets-svg .prov { fill: rgba(138, 92, 196, 0.1); stroke: #8a5cc4; stroke-width: 2; }
.legend { display: flex; gap: 2rem; justify-content: center; font-size: 0.95rem; }
.legend .sw { display: inline-block; width: 1.5rem; height: 0; vertical-align: middle; margin-right: 0.4rem; }
.req-sw { border-top: 2px dashed #888; }
.tok-sw { border-top: 3px solid #2e8b57; }
.mint-sw { border-top: 2px solid #8a5cc4; }
</style>

---

# Bottom line

<v-clicks depth="2">

- This works
- As pace increases, I think secrets management is more important
- Thinking in containers has value beyond just information hygiene
  - e.g. evaluating agent behaviour

</v-clicks>

---
layout: center
class: text-center
---

# Thanks!

<div class="flex items-center justify-center gap-16 mt-8">
  <div class="flex flex-col items-center gap-3">
    <img src="/bluesky-qr.svg" alt="QR code for David Johnston's Bluesky profile" class="w-48 h-48" />
    <div class="text-xl">Bluesky</div>
  </div>
  <div class="flex flex-col items-center gap-3">
    <img src="/david-johnston.jpg" alt="David Johnston" class="w-48 h-48 rounded-full object-cover" />
    <div class="text-xl font-semibold">David Johnston</div>
    <a href="https://www.blacksheepcode.com" class="text-xl">www.blacksheepcode.com</a>
    <a href="https://bsky.app/profile/djdave.bsky.social" class="text-xl">Bluesky: @djdave.bsky.social</a>
  </div>
  <div class="flex flex-col items-center gap-3">
    <img src="/linkedin-qr.svg" alt="QR code for David Johnston's LinkedIn profile" class="w-48 h-48" />
    <div class="text-xl">LinkedIn</div>
  </div>
</div>

<div class="text-sm opacity-70 mt-10">
  VS Code git credential sharing issue:
  <a href="https://github.com/microsoft/vscode-remote-release/issues/9466">github.com/microsoft/vscode-remote-release/issues/9466</a>
</div>
