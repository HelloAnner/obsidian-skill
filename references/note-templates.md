# Note Templates

Templates are stored in `Work/Templates/`. When creating new notes, use the appropriate template structure.

## Available Templates

### Fine 开发文档模板 (Development Document)

For technical design documents. Has `kms` property for Confluence publishing.

**Structure:**
1. **基本信息** — Task link, product doc
   - 任务背景 (Task background)
   - 问题现状 (Current problems)
   - 设计目标 (Design goals)
2. **详细设计** — Technical deep-dive
   - 功能拆解 (Function breakdown) — mind map style
   - 详细设计图 (Design diagrams) — flow charts, architecture, data flow
   - 实现方案 (Implementation plan) — with alternatives
   - 细分功能/难点设计 (Detailed feature/difficulty design)
   - 其他设计 (Other design) — compatibility, performance, cluster, integration
   - 代码设计 (Code design) — data structures, interfaces, key code
3. **测试建议** — Test suggestions for QA
4. **其他** — Security checklist, API docs, tech debt
5. **开发计划** — Timeline with effort estimation

**Frontmatter:**
```yaml
---
kms: https://kms.fineres.com/pages/viewpage.action?pageId=<parent-id>
---
```

### Fine 日工作记录模板 (Daily Work Log)

For daily work tracking with structured categories.

**Structure:**
- **任务分类** (Task categories):
  - 目标 (Goals) — linked to annual goals
  - 迭代 (Iterations) — sprint work
  - 维护 (Maintenance) — bug fixes, support
  - 突破 (Breakthroughs) — innovation, research
  - 参与度 (Participation)
- **思考** (Reflections):
  - 流程思考 (Process reflection)
  - 价值思考 (Value reflection)
  - 优化思考 (Optimization reflection)
- **检查清单** (Checklist):
  - All tasks completed or blockers documented
  - Customer issues tagged properly
  - Work log >= 8 hours
  - PR comments >= 3 per day
  - Group messages >= 3 per day
  - Meeting contributions >= 3 opinions

### Fine 周工作记录模板 (Weekly Work Summary)

Weekly summary and planning template.

### Fine 客户问题记录模版 (Customer Issue Template)

For tracking and documenting customer-reported issues.

### Fine 工作记录模板 (General Work Record)

Generic work record template.

## Using Templates

### Create Note from Template via CLI

```bash
obsidian create name="20260410 Moss Tool Registry Design" template="Fine 开发文档模板" path="Work/Fine AI/Moss/开发文档/" silent
```

### Set Properties After Creation

```bash
obsidian property:set name="kms" value="https://kms.fineres.com/pages/viewpage.action?pageId=12345" file="20260410 Moss Tool Registry Design"
obsidian property:set name="context_dir" value="/Users/anner/fine/ai" file="20260410 Moss Tool Registry Design"
```

## Conventions

- **File naming**: Date prefix for time-sensitive docs (e.g., `20260410 Topic Name.md`)
- **Location**: Place notes in the appropriate subfolder under `Work/`
- **Language**: Work notes are in Chinese
- **Links**: Use `[[wikilinks]]` to connect related notes, meeting records, and reference materials
- **Diagrams**: Include architecture/flow diagrams in the design section using Excalidraw or Mermaid
