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
clicks: 3
---

# What is a dev container?

Turns out VS Code is both a client and a server

<div class="diagram">
  <div class="box client">
    <div class="label">VS Code</div>
    <div class="sub">Client (UI)</div>
  </div>

  <div class="arrow">⟷</div>

  <div :class="{ machine: $clicks >= 2 }">
    <div v-if="$clicks >= 2" class="host-label">Your local machine</div>
    <div class="host" :class="{ highlight: $clicks >= 3 }">
      <div class="host-label">
        <span v-if="$clicks === 0">Your local machine</span>
        <span v-else-if="$clicks === 1">A remote server</span>
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

<div v-if="$clicks >= 3" class="callout">
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
  border: 2px solid #888;
  border-radius: 0.75rem;
  padding: 0.75rem 1rem 1rem;
  text-align: center;
}
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
  <div v-click>→ Remove them in <code>.devcontainer</code></div>
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
layout: center
class: text-center
---

# Template here

<div class="flex flex-col items-center gap-6 mt-8">
  <img src="/template-qr.svg" alt="QR code for the meta-repo-template repository" class="w-64 h-64" />
  <a href="https://github.com/dwjohnston/meta-repo-template" class="text-2xl">github.com/dwjohnston/meta-repo-template</a>
</div>

---
layout: center
---

<div class="text-9xl font-bold">Demo</div>

---

# What sucks

- Docker performance can be sluggish
- Changing the environment variables is tedious

---

# Secrets

For secrets, I'm really thinking a tool like **Infisical** is the way to go

<div class="secrets">
  <div class="hub">Infisical</div>
  <div class="targets">
    <div class="target"><div class="arrow">↓</div><div class="box">Host</div></div>
    <div class="target"><div class="arrow">↓</div><div class="box">Container</div></div>
    <div class="target"><div class="arrow">↓</div><div class="box">Claude cloud etc.</div></div>
    <div class="target"><div class="arrow">↓</div><div class="box">CI pipeline</div></div>
    <div class="target"><div class="arrow">↓</div><div class="box">Application</div></div>
  </div>
</div>

<style>
.secrets { display: flex; flex-direction: column; align-items: center; margin-top: 2.5rem; }
.hub {
  border: 2px solid #e8a33d; background: rgba(232, 163, 61, 0.12);
  border-radius: 0.5rem; padding: 0.75rem 2.5rem; font-size: 1.4rem; font-weight: 600;
}
.targets { display: grid; grid-template-columns: repeat(5, 1fr); gap: 1rem; margin-top: 0.5rem; width: 100%; }
.target { display: flex; flex-direction: column; align-items: center; }
.arrow { font-size: 2rem; opacity: 0.6; }
.box {
  border: 2px solid #4b82c4; background: rgba(75, 130, 196, 0.1);
  border-radius: 0.5rem; padding: 0.75rem 0.5rem; width: 100%; text-align: center; font-weight: 600;
}
</style>

---

# Bottom line

- This works
- As pace increases, I think secrets management is more important
- Thinking in containers has value beyond just information hygiene
  - e.g. evaluating agent behaviour

---
layout: center
class: text-center
---

# Thanks!

<div class="flex items-center justify-center gap-16 mt-8">
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
