# Publish Workflow

Publish notes to multiple platforms using the `obsidian` CLI and the `obsidian-publish-everywhere` plugin.

## Quick Publish via Plugin (Preferred)

When Obsidian is running with the plugin active, trigger publish commands directly:

```bash
# Publish active note to specific platform
obsidian eval code="app.commands.executeCommandById('obsidian-publish-everywhere:publish-to-confluence')"
obsidian eval code="app.commands.executeCommandById('obsidian-publish-everywhere:publish-to-feishu')"
obsidian eval code="app.commands.executeCommandById('obsidian-publish-everywhere:publish-to-notion')"
obsidian eval code="app.commands.executeCommandById('obsidian-publish-everywhere:publish-to-github')"
obsidian eval code="app.commands.executeCommandById('obsidian-publish-everywhere:publish-to-xiaohongshu')"

# One-click publish to ALL configured platforms
obsidian eval code="app.commands.executeCommandById('obsidian-publish-everywhere:publish-to-all-platforms')"

# Copy rich content to clipboard (HTML with embedded images)
obsidian eval code="app.commands.executeCommandById('obsidian-publish-everywhere:copy-rich-content')"
```

**Important**: These commands operate on the **active file** in Obsidian. To publish a specific note, first open it:

```bash
obsidian open file="My Note"
# Then trigger publish
obsidian eval code="app.commands.executeCommandById('obsidian-publish-everywhere:publish-to-confluence')"
```

## Pre-Publish Checklist

Before publishing, verify the note has the required frontmatter:

```bash
# Check what platforms are configured
obsidian property:get name="kms" file="My Note"
obsidian property:get name="feishu" file="My Note"
obsidian property:get name="notion" file="My Note"
obsidian property:get name="github" file="My Note"
```

## Property Management

```bash
# Read properties
obsidian property:get name="kms_url" file="My Note"
obsidian property:get name="kms_open" file="My Note"

# Set properties
obsidian property:set name="kms" value="https://kms.fineres.com/pages/viewpage.action?pageId=12345" file="My Note"
obsidian property:set name="context_dir" value="/Users/anner/fine/ai" file="My Note"
obsidian property:set name="kms_open" value="true" file="My Note"
```

## Platform-Specific Flows

### KMS (Confluence)

The plugin handles the full flow:
1. Reads `kms` property → extracts `pageId` as parent
2. Reads `kms_url` → if exists, updates that page; otherwise creates a new child page
3. Converts markdown to Confluence storage format (handles images, Excalidraw→PNG, Mermaid→PNG, wikilinks→KMS links)
4. Applies read restriction based on `kms_open`
5. Auto-fills `kms_url` and `kms_open` in frontmatter

### Feishu

1. Reads `feishu` property → parses document token
2. Converts markdown with images to Feishu document format
3. Creates or updates document
4. Auto-fills `feishu_url` and `feishu_shared_at`

### Notion

1. Reads `notion` property → determines parent page or database
2. If database: matches by title, updates if exists, creates if not
3. If page: creates child page
4. Auto-fills `notion_url` and `notion_shared_at`

### GitHub

1. Reads `github` property → gets repo URL
2. Clones repo to temp directory
3. Writes note content as `README.md`
4. Commits and pushes

## Batch Operations

```bash
# Find notes with specific tags
obsidian search query="tag:#publishable" limit=50

# Find notes with KMS configured
obsidian search query="kms:" limit=50

# Count all tags
obsidian tags sort=count counts

# Check daily note
obsidian daily:read
```

## Typical Workflow

1. **Create/edit note** with appropriate template and frontmatter
2. **Set `context_dir`** if the note relates to a code project
3. **Enrich content** — Claude reads context_dir and fills in details
4. **Generate diagrams** — Excalidraw or Mermaid as needed
5. **Review** — read the final note
6. **Publish** — trigger plugin command for target platform(s)
7. **Verify** — check the auto-filled `*_url` properties
