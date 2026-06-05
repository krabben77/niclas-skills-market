---
name: hn-radar
description: >
  Söker Hacker News baserat på aktiva sökprofiler i HN-Bevakningslista.md och presenterar relevanta fynd kopplade till Niclas stack (MCP, Docker, Windows-automation, Obsidian, FastAPI). Använd ALLTID denna skill när Niclas skriver fraser som "Sök HN", "HN-radar", "vad finns på HN om", "kör HN-sökning", "HN-bevakning", "check HN", "search HN", eller ber om Hacker News-nyheter. Även om frågan är vag ("kolla HN") – starta skill:en. Loggar fynd automatiskt tillbaka till bevakningslistan i Obsidian.
---

# 🔭 HN-Radar

Automatiserad Hacker News-bevakning baserad på aktiva sökprofiler i din Obsidian-vault.

---

## 🚀 Workflow (alltid i denna ordning)

### Fas 1 – Läs sökprofiler

Läs bevakningslistan ALLTID innan sökning:

```
Desktop Commander:read_file
path: G:\Obsidian-main\31-RADAR\HN-Bevakningslista.md
```

Extrahera från `## 🧭 Sökprofiler`:
- Profilnamn (t.ex. "MCP & AI-agenter")
- Nyckelord per profil
- Eventuella aktiva/pausade flaggor (om de finns)

> **OBS:** Om en profil saknar nyckelord – skippa den. Kör aldrig generiska sökningar.

---

### Fas 2 – Parallella HN-sökningar

Kör **en sökning per aktiv profil** med `MCP_DOCKER:search_stories`.

**Standardparametrar:**
- `num_results`: 5 per query (max 8 om profilen är prioriterad)
- `search_by_date`: `true` (senaste artiklarna, inte mest populära)

**Sökstrategi per profil:**
- Välj det mest specifika/tekniska nyckelordet som primär query
- Kör gärna 2 queries per profil om det finns tydliga varianter (t.ex. "MCP" + "Model Context Protocol")

**Exempel-queries baserat på nuvarande profiler:**

| Profil | Primär query | Alternativ query |
|--------|-------------|-----------------|
| MCP & AI-agenter | `Model Context Protocol` | `MCP agent tool use` |
| Windows-automation | `desktop automation accessibility` | `UI Automation Windows` |
| Docker & Infrastruktur | `self-hosted homelab Docker` | `Portainer Traefik container` |
| Obsidian & Knowledge | `Obsidian PKM` | `knowledge graph second brain` |
| FastAPI & Backend | `FastAPI Python async` | `MCP server Python` |

---

### Fas 3 – Filtrera & Presentera fynd

Efter att alla sökresultat kommit in, filtrera bort:
- Artiklar redan listade i bevakningslistan (kolla `## 📚 Artiklar`)
- Artiklar med poäng < 3 (om inget annat anges)
- Uppenbart irrelevanta träffar (kontrollera mot nyckelord)

**Presentera fynd i detta format:**

```
## 🆕 HN-fynd – [datum]

### [Profilnamn]

**[Artikeltitel]**
- 🔗 HN: https://news.ycombinator.com/item?id=[id]
- 📦 Repo: [url om tillgänglig, annars "–"]
- ⭐ Poäng: [points] | 💬 Kommentarer: [num_comments]
- 🎯 Relevansbedömning: [1-2 meningar — koppling till din stack]
- 🏷️ Föreslagna taggar: #[tag1] #[tag2]
- 📌 Status: 🔵 Noterad / 🟡 Värd att testa / 🔴 Prioriterad

---
```

**Relevansbedömning** – ange specifikt vilken del av stacken som berörs:
- MCP-integration → MetaMCP, claude_desktop_config
- Windows-automation → ersätter/kompletterar Windows-MCP
- Docker/infra → Portainer, Traefik, docker_proxy-nätverket
- Obsidian → G:\Obsidian-main vault
- Backend → FastAPI-tjänster, MCP-server Python

---

### Fas 4 – Valfri djupdyk (på begäran)

Om en artikel ser extra intressant ut och Niclas vill ha mer:

```
MCP_DOCKER:get_story_info
story_id: [id]
```

Presentera topp 3-5 kommentarer som är tekniskt relevanta. Skippa diskussioner om UI/hype.

---

### Fas 5 – Logga fynd till Obsidian

Fråga alltid: **"Vill du att jag loggar [X] av dessa fynd till HN-Bevakningslistan?"**

Om ja – för varje artikel att logga:

**Lägg till under rätt datumsektion i `## 📚 Artiklar`:**

```markdown
#### [Artikeltitel]
- **HN:** https://news.ycombinator.com/item?id=[id]
- **Repo:** [url eller "–"]
- **Poäng:** [points] | **Kommentarer:** [num_comments]
- **Taggar:** #[tag1] #[tag2] #[tag3]
- **Relaterat:** [[relaterad-skill-eller-noter]]
- **Varför relevant:** [Relevansbeskrivning kopplad till stack]
- **Status:** 🔵 Noterad / 🟡 Värd att testa / 🔴 Prioriterad
```

**Uppdatera sedan `## 🗂️ Tagg-index`:**
- Lägg till nya taggar som ny rad i tabellen
- Lägg till artikelnamnet under befintliga taggar om de matchar
- Format: `| #[tagg] | [Artikel1], [Artikel2] |`

**Uppdatera `Senast uppdaterad:` längst ned i filen.**

Använd `Desktop Commander:edit_block` för alla ändringar — aldrig full fil-överskrivning.

---

## ⚙️ Konfiguration

### Bevakningslistans sökväg
```
G:\Obsidian-main\31-RADAR\HN-Bevakningslista.md
```

### MCP-verktyg
| Verktyg | Syfte |
|---------|-------|
| `MCP_DOCKER:search_stories` | Söka HN-artiklar |
| `MCP_DOCKER:get_story_info` | Hämta kommentarer |
| `Desktop Commander:read_file` | Läsa bevakningslistan |
| `Desktop Commander:edit_block` | Skriva tillbaka fynd |

### Statusikoner (konsekvent användning)
| Ikon | Betydelse |
|------|-----------|
| 🔵 | Noterad – låg prioritet |
| 🟡 | Värd att testa – medium prioritet |
| 🔴 | Prioriterad – testa snarast |
| ✅ | Testad – arkiveras |

---

## 📋 Snabbkommandon

| Phrase (SV/EN) | Åtgärd |
|----------------|--------|
| `Sök HN` | Full radar – alla profiler |
| `HN-radar` | Full radar – alla profiler |
| `vad finns på HN om [ämne]` | Sökning på specifikt ämne |
| `HN djupdyk [id]` | Hämta kommentarer för story |
| `logga HN-fynd` | Logga senaste presenterade fynd |
| `kör HN-bevakning` | Full radar – alla profiler |

---

## ✅ Success criteria

- ✅ Bevakningslistan läst och alla aktiva profiler identifierade
- ✅ Parallella sökningar körda (en+ per profil)
- ✅ Dubletter mot existerande artiklar filtrerade bort
- ✅ Relevansbedömning kopplad till faktisk stack (inte generisk)
- ✅ Fynd loggade med korrekt format och tagg-index uppdaterat

---

**Version:** 1.0
**Skapad:** 2026-03-11
**Bevakningslista:** `G:\Obsidian-main\31-RADAR\HN-Bevakningslista.md`
