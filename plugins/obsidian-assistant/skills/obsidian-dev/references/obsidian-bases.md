# Obsidian Bases

Bases är ett inbyggt Obsidian-format (introducerat 2025) för att skapa databas-liknande vyer
av noter. Filer sparas med `.base`-ändelse.

## Grundstruktur

En `.base`-fil är JSON med views, filters och formulas.

```json
{
  "views": [
    {
      "id": "view-1",
      "name": "Alla noter",
      "type": "table",
      "filter": {
        "conditions": []
      },
      "order": {
        "field": "created",
        "direction": "desc"
      }
    }
  ]
}
```

## Vytyper

- `table` — tabellvy med kolumner
- `board` — kanban-vy grupperad på ett fält
- `gallery` — kortvy med bild

## Filter

```json
{
  "filter": {
    "operator": "AND",
    "conditions": [
      { "field": "status", "operator": "eq", "value": "growing" },
      { "field": "type", "operator": "in", "value": ["note", "projekt"] }
    ]
  }
}
```

## Formulas

Beräknade fält med `formula`-typ i columns-arrayen.

```json
{
  "id": "age",
  "name": "Ålder (dagar)",
  "type": "formula",
  "formula": "dateDiff(prop('created'), now(), 'days')"
}
```
