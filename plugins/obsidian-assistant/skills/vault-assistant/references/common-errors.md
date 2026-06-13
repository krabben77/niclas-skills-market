# Vanliga fel i vault-systemet

## validate-write.ts

**"Missing required frontmatter field"**
Filen saknar ett av: `title, created, type, status, tags`.
Lösning: Lägg till saknade fält i YAML-blocket.

**"Write blocked: section has write:false"**
Försök att skriva till sektion 00 eller 50.
Lösning: Kontrollera att målsökvägen är rätt. Skriv aldrig till 00-SYSTEM utan godkännande.

**Hook triggar inte alls**
Kontrollera `.claude/settings.json` — hook måste ha rätt event-namn och script-sökväg.
Kör manuellt: `node --experimental-strip-types .claude/scripts/<script>.ts`

## session-start.ts

**"Cannot find vault-manifest.json"**
Scriptet körs utanför vault-root, eller manifestet saknas.
Kontrollera att cwd är `C:\Users\fremb\Documents\Obsidian-v2`.

## stop-checklist.ts

**Index regenereras inte**
Kontrollera `.claude/dirty-sections.tmp` — om filen är tom eller saknas sker ingen regenerering.
Manuell körning: `node --experimental-strip-types .claude/scripts/generate-index.ts <sektion>`
