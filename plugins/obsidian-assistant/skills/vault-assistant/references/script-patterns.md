# Script-mönster för vault-scripts

## Läsa vault-manifest

```typescript
import { readFileSync } from 'fs'
import { join } from 'path'

const manifestPath = join(process.cwd(), '00-SYSTEM', 'vault-manifest.json')
const manifest = JSON.parse(readFileSync(manifestPath, 'utf-8'))

const section = manifest.sections['12-KUNSKAP']
const canWrite = section?.write === true
```

## Läsa frontmatter från .md-fil

```typescript
import { readFileSync } from 'fs'

function parseFrontmatter(content: string): Record<string, unknown> {
  const match = content.match(/^---\n([\s\S]*?)\n---/)
  if (!match) return {}
  // Enkel YAML-parsing utan beroenden
  const lines = match[1].split('\n')
  const result: Record<string, unknown> = {}
  for (const line of lines) {
    const [key, ...rest] = line.split(':')
    if (key && rest.length) result[key.trim()] = rest.join(':').trim()
  }
  return result
}
```

## Skriva till dirty-sections

```typescript
import { appendFileSync } from 'fs'
import { join } from 'path'

const dirtyFile = join(process.cwd(), '.claude', 'dirty-sections.tmp')
appendFileSync(dirtyFile, '12-KUNSKAP\n')
```

## Hook-konfiguration i settings.json

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "node --experimental-strip-types .claude/scripts/validate-write.ts"
      }]
    }]
  }
}
```
