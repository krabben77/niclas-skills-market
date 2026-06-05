---
name: research-grund
description: >
  Skriver en permanent research-not till vault (12-KUNSKAP\). Grund för specialiserade
  research-skills. Triggas när Niclas explicit vill spara research: "spara en not om",
  "skapa en not om", "dokumentera", "skriv till vault", "kör research och spara",
  eller ger en URL/ämne med tydlig intent att skapa ett permanent kunskapsdokument.
  Täcker alla inputtyper: URL, ämne, fråga, halvformulerad idé.
allowed-tools: Read, WebSearch, WebFetch, Bash(python:*)
title: /research-grund — Fokuserad Research till Vault
created: 2026-05-19
updated: 2026-05-19
type: skill
status: seedling
tags: [research, pipeline, grund, foundation]
intent: none
---

# /research-grund — Fokuserad Research till Vault

Söker parallellt, syntetiserar opinionerat, skriver ett permanent kunskapsdokument.

**Input:** `$ARGUMENTS` — en URL, ett ämne, en fråga, eller en halvformulerad idé.
**Output:** En strukturerad research-not i `12-KUNSKAP\` med frontmatter och evidens.

---

## Steg 1 — Bedöm inputen

Läs `$ARGUMENTS`. Avgör:

**Fokuserad input** — en tydlig fråga, ett konkret ämne, en URL.
→ Gå direkt till Steg 3.

**Vag input** — en idé, ett intresseområde, en riktning utan precisering.
→ Gå till Steg 2.

---

## Steg 2 — Skärp frågeställningen (endast vid vag input)

Ställ **en** fråga som snävar in fokuset:

> "Vad är det viktigaste du vill förstå om [ämnet] — praktisk implementation, teoretisk grund, eller jämförelse med alternativ?"

Använd svaret för att formulera en precis sökfråga. Dokumentera den:

```
Forskningsfråga: [precis formulering]
```

→ Fortsätt till Steg 3 med den formulerade forskningsfrågan.

---

## Steg 3 — Sök

**Kör ALLTID parallellt** — starta alla sökningar samtidigt utan att vänta på resultat.
**Starta brett, smalna sedan** — börja med korta generella queries (2–4 ord), utvärdera
vad som finns, smalna sedan med mer specifika termer baserat på vad du hittat.

**Webb (kör alltid):**
```
WebSearch("[nyckelord] 2025 2026")                  ← kort, bred query
WebSearch("[nyckelord] implementation example")
```

**Community (kör om ämnet rör teknik, verktyg eller erfarenheter):**

hn-radar och reddit-radar finns som dedicerade skills med optimerade sökprofiler —
använd dem om de passar bättre som fristående sökningar. Annars, direkt:
```
WebSearch("site:news.ycombinator.com \"[nyckelord]\"")
WebSearch("reddit [nyckelord] 2025 2026")
```

Hämta de 2–4 mest relevanta källorna med WebFetch. Prioritera djup över bredd.

---

## Steg 4 — Läs stack-kontext

Läs `G:\Obsidian-main\brain\Infrastructure.md` innan du syntetiserar. Använd den för
att göra "Koppling till din stack" konkret — verkliga sökvägar, aktiva MCPs, befintliga
skills. Inga generiska observationer.

---

## Steg 5 — Syntetisera

Komponera en strukturerad not. Var direkt och opinionerad — skilj tydligt på vad som
är bekräftat, vad som är sannolikt, och vad som är okänt.

```markdown
## Forskningsfråga
[Den preciserade frågan från Steg 1 eller 2]

## Kärnsvar
[2–4 stycken. Det viktigaste — direkt och opinionerat.]

## Detaljer
[Djupare analys. Använd H3 för delområden vid behov.]

## Koppling till din stack
[Konkret mappning mot Niclas verktyg och vault. Hämtat från Infrastructure.md.]

## Vad som är okänt
[Luckor, osäkerheter, vad som kräver vidare undersökning.]

## Källor
- [Titel](URL) — en mening om relevans
```

Sikta på 80–200 rader. Längd styrs av ämnet.

---

## Steg 6 — Skriv

**Write-protokoll (obligatoriskt före varje vault-skrivning):**
1. Beskriv: "Jag planerar att skriva `G:\Obsidian-main\12-KUNSKAP\[topic-slug].md`"
2. Vänta på "OK" från Niclas
3. Skriv filen
4. Verifiera: läs filen, bekräfta att den inte är tom och har korrekt frontmatter
5. Rapportera: `✅ Research klart: [sökväg] ([N] rader)`

**Frontmatter:**
```yaml
---
title: [Läsbart namn]
created: [YYYY-MM-DD]
updated: [YYYY-MM-DD]
type: research
status: growing
source_url: [primärkälla om URL-input, annars utelämna]
tags: [relevanta, taggar]
intent: none
---
```

---

## Specialisering

Denna skill är grunden. Specialiserade skills ärver logiken och lägger till domänspecifika
sökkanaler, anpassade outputformat, eller specifik vault-placering.

En specialiserad skill duplicerar inte denna logik — den utgår från den.

---

## Felhantering

| Situation | Hantering |
|-----------|-----------|
| WebFetch returnerar tomt | Prova alternativ URL, notera begränsning i noten |
| Ämnet för brett för ett svar | Genomför Steg 2 även om input verkade fokuserad |
| Inga relevanta träffar | Formulera om sökfrågan, prova synonymer, rapportera till Niclas |
