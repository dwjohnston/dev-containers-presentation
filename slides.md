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

<div class="diagram">
  <div class="box client">
    <div class="label">VS Code</div>
    <div class="sub">Client (UI)</div>
  </div>

  <div class="arrow">⟷</div>

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
.callout {
  margin-top: 1.5rem;
  margin-left: 50%;
  font-size: 1.25rem;
  font-weight: 600;
  color: #e8a33d;
}
</style>
