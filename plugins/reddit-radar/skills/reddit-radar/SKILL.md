---
name: reddit-radar
description: "Söker Reddit baserat på aktiva sökprofiler i Reddit-Bevakningslista.md och presenterar relevanta fynd kopplade till Niclas stack (MCP, Docker, Windows-automation, Obsidian, n8n, Local AI). Använd ALLTID denna skill när Niclas skriver fraser som \"Sök Reddit\", \"Reddit-radar\", \"vad finns på Reddit om\", \"kör Reddit-sökning\", \"Reddit-bevakning\", \"check Reddit\", eller ber om Reddit-nyheter. Även om frågan är vag (\"kolla Reddit\") – starta skill:en. Loggar fynd automatiskt tillbaka till bevakningslistan i Obsidian."
---

# 🔭 Reddit-Radar

Automatiserad Reddit-bevakning baserad på aktiva sökprofiler i din Obsidian-vault.
Använder `web_search` med `site:reddit.com` — ingen extra infra krävs.

---

## 🚀 Workflow (alltid i denna ordning)

### Fas 1 – Läs sökprofiler

Läs bevakningslistan ALLTID innan sökning:

```
Desktop Commander:read_file
path: Obsidian-main\31-RADAR\Reddit-Bevakningslista.md
```

Extrahera från `## 🧭 Sökprofiler`:
- Profilnamn (t.ex. "MCP & AI-agenter")
- Nyckelord per profil
- Subreddits per profil
- Eventuella aktiva/pausade flaggor

> **OBS:** Om en profil saknar nyckelord – skippa den. Kör aldrig generiska sökningar.

---

### Fas 2 – Parallella Reddit-sökningar

Kör **en sökning per aktiv profil** med `web_search`.

**Sökstrategi:**

För varje profil, bygg queries i detta format:
```
site:reddit.com/r/[subreddit] [nyckelord]
```

Eller bredare om subreddit är okänd:
```
site:reddit.com [nyckelord] [kontext]
```

**Standardparametrar:**
- Kör 1–2 queries per profil
- Prioritera specifika subreddits från bevakningslistan
- Välj det mest tekniska/specifika nyckelordet som primär query

**Exempel-queries baserat på startprofiler:**

| Profil | Primär query | Alternativ query |
|--------|-------------|-----------------|
| MCP & AI-agenter | `site:reddit.com/r/ClaudeAI MCP Model Context Protocol` | `site:reddit.com/r/AI_Agents MCP tool use` |
| Automation & Workflow | `site:reddit.com/r/n8n automation agent` | `site:reddit.com/r/ClaudeAI n8n workflow` |
| Docker & Homelab | `site:reddit.com/r/selfhosted Portainer Traefik` | `site:reddit.com/r/homelab self-hosted docker` |
| Obsidian & PKM | `site:reddit.com/r/ObsidianMD AI automation` | `site:reddit.com/r/ObsidianMD PKM agent` |
| Local AI | `site:reddit.com/r/LocalLLaMA ollama self-hosted` | `site:reddit.com/r/LocalLLaMA local LLM MCP` |

---

### Fas 3 – Filtrera & Presentera fynd

Efter att alla sökresultat kommit in, filtrera bort:
- Trådar redan listade i bevakningslistan (kolla `## 📚 Trådar`)
- Uppenbart irrelevanta träffar (kontrollera mot nyckelord)
- Trådar äldre än 3 månader (om inget annat anges)

**Presentera fynd i detta format:**

```
## 🆕 Reddit-fynd – [datum]

### [Profilnamn]

**[Trådtitel]**
- 🔗 Reddit: [url]
- 📌 Subreddit: r/[namn]
- 🎯 Relevansbedömning: [1-2 meningar — koppling till din stack]
- 🏷️ Föreslagna taggar: #[tag1] #[tag2]
- 📌 Status: 🔵 Noterad / 🟡 Värd att läsa / 🔴 Prioriterad

---
```

**Relevansbedömning** – ange specifikt vilken del av stacken som berörs:
- MCP-integration → MetaMCP, claude_desktop_config
- Windows-automation → Desktop Commander, PowerShell MCP
- Docker/infra → Portainer, Traefik, docker-stack
- Obsidian → G:\Obsidian-sandlada vault
- n8n → workflow-automation
- Local AI → Ollama, lokal modell

---

### Fas 4 – Valfri djupdyk (på begäran)

Om en tråd ser extra intressant ut och Niclas vill ha mer:

```
web_fetch
url: [reddit-url]
```

Presentera:
- Ursprungsinlägget i sammandrag
- Topp 3–5 kommentarer som är tekniskt relevanta
- Skippa diskussioner om UI/hype/off-topic

> **Tips:** Använd `old.reddit.com` istället för `reddit.com` i web_fetch — ger renare HTML.

---

### Fas 5 – Logga fynd till Obsidian

Fråga alltid: **"Vill du att jag loggar [X] av dessa fynd till Reddit-Bevakningslistan?"**

Om ja – för varje tråd att logga:

**Lägg till under rätt datumsektion i `## 📚 Trådar`:**

```markdown
#### [Trådtitel]
- **Reddit:** [url]
- **Subreddit:** r/[namn]
- **Taggar:** #[tag1] #[tag2] #[tag3]
- **Relaterat:** [[relaterad-skill-eller-noter]]
- **Varför relevant:** [Relevansbeskrivning kopplad till stack]
- **Status:** 🔵 Noterad / 🟡 Värd att läsa / 🔴 Prioriterad
```

**Uppdatera sedan `## 🗂️ Tagg-index`:**
- Lägg till nya taggar som ny rad i tabellen
- Format: `| #[tagg] | [Tråd1], [Tråd2] |`

**Uppdatera `Senast uppdaterad:` längst ned i filen.**

Använd `Desktop Commander:edit_block` för alla ändringar — aldrig full fil-överskrivning.

---

## ⚙️ Konfiguration

### Bevakningslistans sökväg
```
G:\Obsidian-sandlada\06-RADAR\Reddit-Bevakningslista.md
```

### Verktyg
| Verktyg | Syfte |
|---------|-------|
| `web_search` | Söka Reddit via site:-operator |
| `web_fetch` | Hämta trådinnehåll för djupdyk |
| `Desktop Commander:read_file` | Läsa bevakningslistan |
| `Desktop Commander:edit_block` | Skriva tillbaka fynd |

### Statusikoner (konsekvent användning)
| Ikon | Betydelse |
|------|-----------|
| 🔵 | Noterad – låg prioritet |
| 🟡 | Värd att läsa – medium prioritet |
| 🔴 | Prioriterad – läs snarast |
| ✅ | Läst – arkiveras |

---

## 📋 Snabbkommandon

| Fras (SV/EN) | Åtgärd |
|--------------|--------|
| `Sök Reddit` | Full radar – alla profiler |
| `Reddit-radar` | Full radar – alla profiler |
| `vad finns på Reddit om [ämne]` | Sökning på specifikt ämne |
| `Reddit djupdyk [url]` | Hämta trådinnehåll |
| `logga Reddit-fynd` | Logga senaste presenterade fynd |
| `kör Reddit-bevakning` | Full radar – alla profiler |

---

## ✅ Success criteria

- ✅ Bevakningslistan läst och alla aktiva profiler identifierade
- ✅ Parallella sökningar körda (en+ per profil)
- ✅ Dubletter mot existerande trådar filtrerade bort
- ✅ Relevansbedömning kopplad till faktisk stack (inte generisk)
- ✅ Fynd loggade med korrekt format och tagg-index uppdaterat

---

**Version:** 1.0
**Skapad:** 2026-04-04
**Bevakningslista:** `Obsidian-main\31-RADAR\Reddit-Bevakningslista.md`