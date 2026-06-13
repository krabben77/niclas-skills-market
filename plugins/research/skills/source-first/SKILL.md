---
name: source-first
description: "Svara ALLTID med källhänvisning när frågan rör verktyg, produkter, versioner, konfiguration, eller ekosystem — även om svaret verkar känt. Täcker hela Niclas stack: Claude Code, Anthropic API, Claude.ai, Obsidian, MCP, Docker, FastAPI, n8n, Windows-automation. Triggas av fraser som \"hur fungerar\", \"hur gör man\", \"vad är skillnaden\", \"finns det\", \"vilken version\", \"hur konfigurerar man\", \"stöds X\", \"vad heter det som\", \"är det möjligt att\" — inom dessa domäner. Triggas ALLTID när frågan handlar om ett verktyg eller en produkt och svaret kräver ett faktapåstående. Gissa aldrig — hämta alltid från rätt källa och svara med URL. Använd även när frågan verkar enkel: enkla frågor har enkla svar som ändå bör verifieras."
---

# Source First

Svara aldrig på faktafrågor om verktyg och produkter från minnet. Hämta alltid
från rätt källa och ange URL i svaret. Det är poängen med den här skill:en.

## Källhierarki per domän

| Domän | Primär källa | URL |
|-------|-------------|-----|
| Claude Code / CLI | Officiella docs | https://docs.anthropic.com/en/docs/claude-code/overview |
| Claude Code / CLI | Community (sekundär) | https://community.anthropic.com/ |
| Anthropic API | API-referens | https://docs.anthropic.com/en/api/overview |
| Anthropic API | Community (sekundär) | https://community.anthropic.com/ |
| Claude.ai | Support | https://support.claude.com |
| Obsidian | Help docs | https://help.obsidian.md/ |
| Obsidian | Community forum (sekundär) | https://forum.obsidian.md/ |
| MCP (protokoll) | Spec | https://modelcontextprotocol.io/docs |
| Docker | Officiella docs | https://docs.docker.com |
| Windows / PowerShell | MS Learn | https://learn.microsoft.com/en-us/powershell/ |

## Workflow

1. **Identifiera domän** — vilken produkt/verktyg handlar frågan om?
2. **Bedöm om hämtning behövs** — om svaret är ett faktapåstående: hämta alltid
3. **Välj verktyg:**
   - Känd URL → web_fetch direkt på doc-sidan
   - Discovery / "finns det" → web_search mot domänens källsite
   - Anthropic-produkter → hämta docs map först, navigera sedan
4. **Svara med källa** — inkludera alltid URL till exakt sida, inte bara domän

## Anthropic docs maps (starta här för Anthropic-frågor)

- Claude API + allmänt: https://docs.anthropic.com/en/docs_site_map.md
- Claude Code: https://docs.anthropic.com/en/docs/claude-code/claude_code_docs_map.md

## Svarsformat

[Direktsvar]

**Källa:** [Sidnamn] — [URL]

Håll svaret kort. Länken är lika viktig som svaret — den låter användaren läsa mer
och verifiera själv. Ange alltid exakt URL, inte bara domännamnet.

## Viktiga principer

**Gissa aldrig versioner, priser, limits eller feature-flaggor.** Dessa ändras utan
förvarning och träningsdata är alltid potentiellt inaktuell.

**En enkel fråga är inte ett undantag.** "Stöds Windows?" är lika mycket en faktafråga
som "vad är batch API-limiten?" — hämta ändå.

**Om sidan inte ger svaret**, säg det och länka till närmast relevanta sida. Spekulera
inte för att fylla luckan.