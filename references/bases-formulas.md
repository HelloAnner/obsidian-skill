# Bases & Formulas Reference

Create and edit Obsidian Bases (`.base` files) with views, filters, formulas, and summaries.

## Schema

Base files use the `.base` extension and contain valid YAML.

```yaml
filters:
  and: []           # Global filters apply to ALL views

formulas:
  formula_name: 'expression'

properties:
  property_name:
    displayName: "Display Name"

summaries:
  custom_summary_name: 'values.mean().round(3)'

views:
  - type: table | cards | list | map
    name: "View Name"
    limit: 10
    groupBy:
      property: property_name
      direction: ASC | DESC
    filters:
      and: []
    order:
      - file.name
      - property_name
      - formula.formula_name
    summaries:
      property_name: Average
```

## Filter Syntax

```yaml
# Single filter
filters: 'status == "done"'

# AND - all conditions must be true
filters:
  and:
    - 'status == "done"'
    - 'priority > 3'

# OR - any condition can be true
filters:
  or:
    - 'file.hasTag("book")'
    - 'file.hasTag("article")'

# NOT - exclude matching items
filters:
  not:
    - 'file.hasTag("archived")'
```

Operators: `==`, `!=`, `>`, `<`, `>=`, `<=`, `&&`, `||`, `!`

## File Properties

| Property | Type | Description |
|----------|------|-------------|
| `file.name` | String | File name |
| `file.basename` | String | Name without extension |
| `file.path` | String | Full path |
| `file.folder` | String | Parent folder |
| `file.ext` | String | Extension |
| `file.size` | Number | Size in bytes |
| `file.ctime` | Date | Created time |
| `file.mtime` | Date | Modified time |
| `file.tags` | List | All tags |
| `file.links` | List | Internal links |
| `file.backlinks` | List | Files linking to this |

## Formula Syntax

```yaml
formulas:
  total: "price * quantity"
  status_icon: 'if(done, "✅", "⏳")'
  created: 'file.ctime.format("YYYY-MM-DD")'
  days_old: '(now() - file.ctime).days'
  days_until_due: 'if(due_date, (date(due_date) - today()).days, "")'
```

**Important**: Subtracting dates returns a **Duration** (not a number). Always access `.days`, `.hours`, etc. before using number functions.

## Key Functions

| Function | Description |
|----------|-------------|
| `date(string)` | Parse string to date |
| `now()` | Current date and time |
| `today()` | Current date (time = 00:00:00) |
| `if(cond, true, false?)` | Conditional |
| `duration(string)` | Parse duration |
| `file(path)` | Get file object |
| `link(path, display?)` | Create a link |

**String**: `.contains()`, `.startsWith()`, `.endsWith()`, `.lower()`, `.replace()`, `.split()`, `.length`

**Number**: `.abs()`, `.ceil()`, `.floor()`, `.round(digits?)`, `.toFixed(precision)`

**List**: `.contains()`, `.filter(expr)`, `.map(expr)`, `.join(sep)`, `.sort()`, `.unique()`, `.length`

**File**: `.hasTag(...)`, `.hasLink(file)`, `.inFolder(folder)`, `.hasProperty(name)`

## Summary Formulas

`Average`, `Min`, `Max`, `Sum`, `Range`, `Median`, `Stddev`, `Earliest`, `Latest`, `Checked`, `Unchecked`, `Empty`, `Filled`, `Unique`

## Example: Task Tracker

```yaml
filters:
  and:
    - file.hasTag("task")
    - 'file.ext == "md"'

formulas:
  days_until_due: 'if(due, (date(due) - today()).days, "")'
  priority_label: 'if(priority == 1, "🔴 High", if(priority == 2, "🟡 Medium", "🟢 Low"))'

views:
  - type: table
    name: "Active Tasks"
    filters:
      and:
        - 'status != "done"'
    order:
      - file.name
      - status
      - formula.priority_label
      - due
      - formula.days_until_due
    groupBy:
      property: status
      direction: ASC
```

## YAML Quoting Rules

- Single quotes for formulas with double quotes: `'if(done, "Yes", "No")'`
- Double quotes for simple strings: `"My View Name"`
- Strings with `:`, `{`, `}`, `[`, `]`, `#`, `!`, etc. must be quoted

## Embedding Bases

```markdown
![[MyBase.base]]
![[MyBase.base#View Name]]
```

## References

- [Bases Syntax](https://help.obsidian.md/bases/syntax)
- [Functions](https://help.obsidian.md/bases/functions)
