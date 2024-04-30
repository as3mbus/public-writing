---
cssclasses:
  - academia
  - wideTable
State: On-Going (Draft)
type: Map Of Content
---

```dataview
TABLE tags,
"![anyName|50](" + cover-img + ")" AS "Cover"

WHERE contains(tags, "Game") and !contains(tags, "Development")
SORT file.name ASC
```

