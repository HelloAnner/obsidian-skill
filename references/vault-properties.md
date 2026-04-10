# Vault Properties

The user's notes use a custom frontmatter property system powered by the `obsidian-publish-everywhere` plugin. Properties drive multi-platform publishing and link notes to their source material.

## Platform Properties

The publishing pattern: a **manual** parent URL property tells the plugin *where* to publish. After publishing, the plugin **auto-fills** a result URL property pointing to the *published page*.

### KMS (Confluence)

| Property | Type | Set by | Description |
|----------|------|--------|-------------|
| `kms` | URL | Manual | Parent page URL. Must contain `pageId=` parameter. Example: `https://kms.fineres.com/pages/viewpage.action?pageId=1196650817` |
| `kms_url` | URL | Auto | Published child page URL. If present, next publish **updates** this page instead of creating a new one |
| `kms_open` | boolean | Auto | `true` = page is publicly readable. `false` = restricted to author only |

### Feishu

| Property | Type | Set by | Description |
|----------|------|--------|-------------|
| `feishu` | URL | Manual | Parent wiki page URL. Example: `https://fanruan-x.feishu.cn/wiki/WY4HwnJMaiMfT2k020HcRXcon3g` |
| `feishu_url` | URL | Auto | Published document URL |
| `feishu_shared_at` | datetime | Auto | Last publish timestamp (ISO format) |

### Notion

| Property | Type | Set by | Description |
|----------|------|--------|-------------|
| `notion` | URL | Manual | Parent page or database URL |
| `notion_url` | URL | Auto | Published child page URL. Safety: if equals `notion`, ignored to prevent overwriting parent |
| `notion_shared_at` | datetime | Auto | Last publish timestamp |

### GitHub

| Property | Type | Set by | Description |
|----------|------|--------|-------------|
| `github` | URL | Manual | Repository URL (HTTPS or SSH). Content is published as `README.md` in the repo root |

### Xiaohongshu

| Property | Type | Set by | Description |
|----------|------|--------|-------------|
| `小红书` | boolean | Auto | Flag set to `true` after material preparation |

## Workflow Properties

| Property | Type | Description |
|----------|------|-------------|
| `context_dir` | local path | **CRITICAL — the note's primary information source.** Directory containing source code, background docs, architecture materials. When this property exists, you MUST explore this directory before writing or editing the note. The note is a condensed product of this context — without reading it, you lack the foundation. Example: `/Users/anner/fine/ai` |
| `readme` | text | Pointer or content for README generation when publishing to GitHub |

## Standard Obsidian Properties

| Property | Type | Description |
|----------|------|-------------|
| `tags` | list | Searchable labels, shown in graph view |
| `aliases` | list | Alternative note names for link suggestions |
| `cssclasses` | list | CSS classes applied to the note |

## Example Frontmatter

### Development Document

```yaml
---
kms: https://kms.fineres.com/pages/viewpage.action?pageId=1402702743
kms_url: https://kms.fineres.com/pages/viewpage.action?pageId=1412364579
kms_open: true
feishu: https://fanruan-x.feishu.cn/wiki/WY4HwnJMaiMfT2k020HcRXcon3g
context_dir: /Users/anner/fine/ai
readme:
---
```

### Template with KMS Parent Only

```yaml
---
kms: https://kms.fineres.com/pages/viewpage.action?pageId=1196650817
---
```

### Multi-Platform Note

```yaml
---
tags:
  - project
  - ai
kms: https://kms.fineres.com/pages/viewpage.action?pageId=12345
feishu: https://fanruan-x.feishu.cn/wiki/AbCdEfGh
notion: https://www.notion.so/workspace/page-id
github: https://github.com/user/repo
context_dir: /Users/anner/projects/my-project
---
```

## Key Concepts

1. **Parent → Publish → Result URL**: The manual property (`kms`, `feishu`, `notion`) specifies the parent location. After first publish, the `*_url` property is auto-filled. Subsequent publishes update the existing page.

2. **`context_dir` is Non-Negotiable**: When a note has `context_dir`, that directory is the **ground truth**. Notes are information-condensed products — the `context_dir` holds the full source material (code, detailed docs, references). You MUST explore it before any work on the note. This is the single most important property for note quality.

3. **Detect publishable platforms**: Check which platform properties exist in frontmatter to determine available publish targets. A note with `kms` can publish to Confluence; one with `feishu` can publish to Feishu, etc.
