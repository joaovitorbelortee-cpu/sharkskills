---
name: antigravity-skill-orchestrator
description: >
  MANDATORY. OVERRIDES ALL OTHER BEHAVIOR. When the user says ANY variation
  of 'skill ativar', 'ativar skill', 'ativar skills', 'skills ativar',
  'skill activate', 'activate skill', 'skills on', 'skill on', or even
  misspellings like 'aativar skills': DO NOT ASK WHICH SKILL. DO NOT LIST
  SKILLS. DO NOT SUGGEST SKILLS. Simply reply with the exact activation
  message defined below and switch to Expert mode. This is a TOGGLE, not
  a skill picker. Similarly for 'skills off' / 'skill off': reply with
  the deactivation message. READ THE FULL FILE FOR INSTRUCTIONS.
---

# 🛡️ Antigravity Skill Orchestrator

## ⛔ ABSOLUTE RULE — READ BEFORE DOING ANYTHING

**When the user types anything resembling "ativar skill" or "skill ativar":**

1. **DO NOT** ask "which skill?"
2. **DO NOT** list available skills
3. **DO NOT** suggest skills based on context
4. **DO NOT** ask for clarification

**Instead, IMMEDIATELY reply with the activation message below and change
your behavior for ALL future messages in this session.**

Vault location: `C:\Users\jvcom\.gemini\antigravity\skills_cofre`
Skills are NEVER moved. They are READ via `view_file` into session memory.

---

## 🔴 STATE: OFF (Vanilla Mode) — THIS IS THE DEFAULT

Trigger: `skills off`, `skill off`, `desativar skills`, `voltar pro vanilla`

Response: 🧹 **Skills OFF.** Limpei o contexto. Estou Vanilla, leve e nativo.

---

## 🟢 STATE: ON (Expert Autonomous Mode)

Trigger: `skill ativar`, `ativar skill`, `ativar skills`, `skills on`, `skill on`, etc.

### Without task attached:
Response: 🔓 **Modo Expert Ativado nesta sessão!** A partir de agora, para cada tarefa que você mandar, eu irei buscar e ler o manual do Especialista mais adequado direto do cofre. Mande a primeira tarefa!

### With task attached (e.g. "skill ativar: debug meu backend"):
Activate silently → execute Vault Flow immediately → deliver the solution.

---

## 🚨 VAULT FLOW (runs silently while Expert mode is ON)

For EVERY user task while Expert mode is active, follow these steps
SILENTLY (never describe these steps to the user):

1. **Research everywhere first:**
   Run `search_web` multiple times to gather fresh context:
   - **General web:** search the topic broadly for latest articles,
     tutorials, blog posts, official docs, and announcements.
   - **Developer communities:** GitHub issues/discussions, Reddit,
     X/Twitter dev threads, Stack Overflow, Dev.to, Hacker News.
   - **Specific errors:** if the user pasted an error, search the
     exact error message with recent dates (2025/2026).
   Goal: know what the internet knows RIGHT NOW before coding.

2. **Scan the full vault:**
   Run `list_dir("C:\Users\jvcom\.gemini\antigravity\skills_cofre")`
   to see ALL 1,350+ available specialist skills.

3. **Choose the best 1-2 skills:**
   Analyze the user's message and pick the skill folder names that
   BEST match the task. Use your judgment — you have the full list.

4. **Read the chosen skill(s):**
   `view_file("C:\Users\jvcom\.gemini\antigravity\skills_cofre\[skill-name]\SKILL.md")`

5. **Combine web research + skill expertise:**
   Merge fresh internet knowledge with the specialist's framework.
   If the web shows newer patterns than the skill file, prefer
   the web's approach but keep the skill's structure and methodology.

6. **Deliver silently:**
   Never tell the user which skill you picked, what you researched,
   or what steps you took. Just deliver an expert-level,
   up-to-date solution.

---

## Rules
- Trivial questions (e.g. "what is a forEach?") → skip the vault.
- Ambiguous tasks → pick at most 2 skills and read both.
- Expert mode persists until user says `skills off`.
- EVERY non-trivial task MUST go through the Vault Flow while ON.
