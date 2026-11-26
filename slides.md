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
    <marker id="ah-tok" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#2e8b57"/></marker>
  </defs>
    <line v-click="1" class="req" x1="218" y1="118" x2="355" y2="165" marker-end="url(#ah-req)"/>
  <line v-click="2" class="tok" x1="359" y1="151" x2="222" y2="104" marker-end="url(#ah-tok)"/>
  <line v-click="1" class="req" x1="222" y1="276" x2="359" y2="229" marker-end="url(#ah-req)"/>
  <line v-click="2" class="tok" x1="355" y1="215" x2="218" y2="262" marker-end="url(#ah-tok)"/>
  <line v-click="1" class="req" x1="678" y1="104" x2="541" y2="151" marker-end="url(#ah-req)"/>
  <line v-click="2" class="tok" x1="545" y1="165" x2="682" y2="118" marker-end="url(#ah-tok)"/>
  <line v-click="1" class="req" x1="682" y1="262" x2="545" y2="215" marker-end="url(#ah-req)"/>
  <line v-click="2" class="tok" x1="541" y1="229" x2="678" y2="276" marker-end="url(#ah-tok)"/>
  <line v-click="1" class="req" x1="457" y1="314" x2="457" y2="228" marker-end="url(#ah-req)"/>
  <line v-click="2" class="tok" x1="443" y1="228" x2="443" y2="314" marker-end="url(#ah-tok)"/>
  <rect class="hub" x="365.0" y="160.0" width="170" height="60" rx="10"/>
  <text class="hub-t" x="450" y="194">Infisical</text>
  <text class="hub-s" x="450" y="212">holds an admin token</text>
    <rect class="node" x="45.0" y="57.0" width="170" height="46" rx="8"/><text class="node-t" x="130" y="86">Host</text>
  <rect class="node" x="45.0" y="277.0" width="170" height="46" rx="8"/><text class="node-t" x="130" y="306">Container</text>
  <rect class="node" x="685.0" y="57.0" width="170" height="46" rx="8"/><text class="node-t" x="770" y="86">Claude cloud etc.</text>
  <rect class="node" x="685.0" y="277.0" width="170" height="46" rx="8"/><text class="node-t" x="770" y="306">CI pipeline</text>
  <rect class="node" x="365.0" y="322.0" width="170" height="46" rx="8"/><text class="node-t" x="450" y="351">Application</text>
</svg>

<div class="legend">
  <span v-click="1"><span class="sw req-sw"></span> requests a token</span>
  <span v-click="2"><span class="sw tok-sw"></span> gets a short-lived token</span>
</div>

<style>
.secrets-svg { width: 100%; height: 330px; margin-top: 0.5rem; }
.secrets-svg .hub { fill: rgba(232, 163, 61, 0.15); stroke: #e8a33d; stroke-width: 2; }
.secrets-svg .hub-t { font-size: 22px; font-weight: 600; text-anchor: middle; fill: currentColor; }
.secrets-svg .hub-s { font-size: 12px; text-anchor: middle; fill: currentColor; opacity: 0.7; }
.secrets-svg .node { fill: rgba(75, 130, 196, 0.1); stroke: #4b82c4; stroke-width: 2; }
.secrets-svg .node-t { font-size: 16px; font-weight: 600; text-anchor: middle; fill: currentColor; }
.secrets-svg .req { stroke: #888; stroke-width: 2; stroke-dasharray: 5 4; }
.secrets-svg .tok { stroke: #2e8b57; stroke-width: 2.5; }
.legend { display: flex; gap: 2rem; justify-content: center; font-size: 0.95rem; }
.legend .sw { display: inline-block; width: 1.5rem; height: 0; vertical-align: middle; margin-right: 0.4rem; }
.req-sw { border-top: 2px dashed #888; }
.tok-sw { border-top: 3px solid #2e8b57; }
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
