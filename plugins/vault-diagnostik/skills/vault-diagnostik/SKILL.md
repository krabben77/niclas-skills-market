---
name: vault-diagnostik
description: Analyserar vaultets hälsa genom att jämföra CLAUDE.md-specifikationen mot verkligt tillstånd — hooks, sektioner, skills, projekt, Infrastructure.md. Rapporterar bara verkliga avvikelser, bara i chatten. Inga filer skrivs autonomt. Triggas av fraser som "vault-diagnostik", "granska vault", "audit", "hälsokontroll", "felsök vault", "vad är trasigt", "stämmer systemet", "kör diagnostik", "vault-check".
allowed-tools: Read, Glob, Grep, Bash(pwsh:*)
title: /vault-diagnostik — Systemdiagnos utan Script
created: 2026-05-20
updated: 2026-05-20
type: skill
status: seedling
tags:
  - diagnostik
  - vault
  - system
  - audit
intent: none
---

# /vault-diagnostik — Systemdiagnos utan Script

Läser systemet dynamiskt. Resonerar sig fram till avvikelser. Inga hårdkodade kontroller,
inga statiska scripts. Rapporterar bara i chatten — aldrig autonoma filskrivningar.

---

## Fas 1 — Läs in systemet

Kör alla läsningar parallellt. Bygg en bild av hur systemet *ser ut* just nu.

**Specifikation (vad systemet ska vara):**
- `G:\Obsidian-main\CLAUDE.md` — NIVÅ-tabell, hooks-lista, frontmatter-schema, skrivregler
- `G:\Obsidian-main\.claude\settings.json` — aktiva hooks (vilka scripts som faktiskt körs)

**Implementation (vad som faktiskt finns):**
- `G:\Obsidian-main\.claude\scripts\` — vilka script-filer existerar, och med vilket namn?
- `G:\Obsidian-main\00-SYSTEM\brain\Active-Projects.md` — pågående projekt
- `G:\Obsidian-main\00-SYSTEM\brain\Infrastructure.md` — stack, MCPs, sökvägar
- `G:\Obsidian-main\30-SKILLS\` — vilka skill-mappar finns i vaultet?
- Skills-plugin-katalog (installerade skills):
  `C:\Users\fremb\AppData\Roaming\Claude\local-agent-mode-sessions\skills-plugin\`
  (hitta rätt underkatalog med `Get-ChildItem -Recurse -Filter "SKILL.md"`)
- `G:\Obsidian-main\21-COWORK\agent-log.md` — senaste sessioner (sista 20 raderna räcker)

**Faktisk mappstruktur:**
```powershell
Get-ChildItem "G:\Obsidian-main" -Directory | Select-Object Name | Sort-Object Name
```

---

## Fas 2 — Jämför specifikation mot verklighet

Gå igenom fem kontrollpunkter. Resonera — leta inte bara efter exakta strängmatcher.

### 2a. Hooks: CLAUDE.md → settings.json → scripts/

Frågor att besvara:
- Stämmer hook-namnen i CLAUDE.md mot `hooks`-objektet i settings.json?
- Pekar settings.json på script-filer som faktiskt finns i `.claude/scripts/`?
- Finns det script-filer som *inte* är registrerade i settings.json?

### 2b. Sektioner: CLAUDE.md NIVÅ-tabell → faktiska mappar

Frågor att besvara:
- Finns alla sektioner som CLAUDE.md listar som skrivbara (20–79) som faktiska mappar?
- Finns det mappar i vault-roten som saknar NIVÅ-klassificering i CLAUDE.md?
- Finns det sektioner med `00-INDEX.md` som borde finnas men saknas?

### 2c. Skills: 30-SKILLS vault ↔ installerade i skills-plugin

Frågor att besvara:
- Vilka skills finns i `30-SKILLS/` men är *inte* installerade?
- Vilka skills är installerade men saknar vault-kopia i `30-SKILLS/`?
- Finns det skills vars vault-version och installerad version kan vara out-of-sync
  (jämför `updated:`-datum i frontmatter)?

### 2d. Projekt: Active-Projects.md ↔ agent-log.md

Frågor att besvara:
- Finns det projekt i Active-Projects.md utan aktivitet i agent-log de senaste 14 dagarna?
- Finns det aktivitet i agent-log om projekt som *inte* syns i Active-Projects.md?

### 2e. Infrastructure.md: MCPs och sökvägar

Frågor att besvara:
- Nämner Infrastructure.md MCP-servrar eller sökvägar som verkar inaktuella
  (t.ex. refers till saker som inte längre finns)?
- Är datumet på Infrastructure.md rimligt givet agent-log-historiken?

---

## Fas 3 — Rapportera

**Format:**

```
## Vault-diagnostik [DATUM]

### ✅ OK
[Lista vad som stämmer — bara kategorier, inte detaljer]

### ⚠️ Avvikelser
[Varje avvikelse: vad CLAUDE.md säger → vad som faktiskt finns → bedömning av allvar]

### ❓ Oklart
[Saker som kräver Niclas input för att avgöra om de är problem]

### Förslag
[Konkreta nästa steg — bara om avvikelser faktiskt hittades]
```

**Regler:**
- Inga autonoma filskrivningar. Rapporten stannar i chatten.
- Rapportera bara verkliga avvikelser — inte hypotetiska eller "kunde vara bättre".
- Om allt ser bra ut: säg det rakt. "Systemet ser konsistent ut."
- Fråga Niclas om du är osäker på om en avvikelse är ett problem eller ett medvetet val.

---

## Felhantering

| Situation | Hantering |
|-----------|-----------|
| Script-fil inte läsbar | Notera som oklar — rapportera filnamn |
| skills-plugin-katalog har flera nivåer | Sök rekursivt efter SKILL.md, matcha mot 30-SKILLS |
| Active-Projects.md saknas | Rapportera som avvikelse (brain/-fil saknas) |
| Infrastructure.md har inget datum | Flagga som "okänt tillstånd" |
