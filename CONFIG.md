#meta

This is where you configure SilverBullet to your liking. See [[^Library/Std/Config]] for a full list of configuration options.

## General

```space-lua
-- priority: 90
-- Apply preferences after the built-in defaults (priority 99), before libraries.
-- Core configuration settings
config.set {
  -- Keep attachments on demand to reduce local storage and background sync.
  -- Set to true if you need documents available offline.
  ["sync.documents"] = false,
}
```

## Native navigation

Use the built-in Space tree and Table of Contents. SilverBullet handles resizing, following the current page, and remembering panel state.

```space-lua
-- priority: 90
config.set("view.defaults", {
  ["std.spaceTree"] = { dock = "lhs", open = true, width = 280 },
  ["std.toc"] = { dock = "rhs", open = true, width = 240 },
})
```

Saved choices on each device override these defaults. To apply this layout on a device where you have already moved or closed views, run **Navigate: Reset All Views** once. This also resets other views' saved positions and sizes.

- **Navigate: Tree** (`Ctrl-o` / `Cmd-o`) opens or focuses the explorer. Use its **Pages**, **Documents**, and **Meta** segments to filter content; library pages live under Meta.
- **Navigate: Table of Contents** opens the current page's headings. Use its dock menu to move it.
- On narrow screens, sidebars become drawers and stay closed at startup. After opening one from the menu, its native top-bar button keeps it accessible.
- In the tree, press `Space` to open the selected page while keeping keyboard focus in the tree. Use **Editor: New Window** to open a separate editor window.

## Action buttons and mobile editing

Keep the built-in Home, Page Picker, Command Palette, account menu, PWA history controls, and mobile read-only button. Add shortcuts for the native views and Silversearch; on phones these extra shortcuts live in the menu to leave room for the page title and drawer buttons.

```space-lua
-- Action buttons configuration
-- Additional action buttons defined separately
actionButton.define {
  icon = "sidebar",
  description = "Open Space tree",
  command = "Navigate: Tree",
  priority = 6,
}

actionButton.define {
  icon = "search",
  description = "Search with Silversearch",
  command = "Silversearch: Search",
  priority = 5,
}

actionButton.define {
  icon = "list",
  description = "Open table of contents",
  command = "Navigate: Table of Contents",
  priority = 4,
}
```

The [native keyboard bar](https://docs.silverbullet.md/API/keyboardBar) already provides context-aware list/task editing, links, attachments, formatting, undo/redo, and commands. Keep its defaults; **Editor: Toggle Keyboard Bar** remembers visibility per device. **Editor: Toggle Read Only Mode** and the built-in mobile lock button replace the custom read-only toggle.

## Appearance

The theme follows the system until you use **Editor: Toggle Dark Mode**. Use this command to return to the system theme; preferences are no longer cleared every time Space Lua reloads.

```space-lua
command.define {
  name = "Editor: Use System Theme",
  run = function()
    -- Reset the dark mode toggle to follow the system theme
    clientStore.delete("darkMode")
    editor.reloadUI()
  end,
}
```

### Page width

Wider pages than the [[Library/ilkersigirci/styles/baseline|Baseline]] theme's 740px default. The theme loads at `priority: -10`, so this block uses `-20` to load after it.

```space-style
/* priority: -20 */
html[data-theme]:root {
  --bl-line-width: 880px;
}
```

## Libraries

### Silversearch

Preserve filename ranking, recent-note boosting, and developer-friendly tokenization. Library pages remain searchable but rank lower.

```space-lua
-- priority: 90
-- Library configurations
config.set("silversearch", {
  -- Weighs specific fields more
  weights = {
    basename = 15,
    -- Also available: tags, aliases, directory, displayName, content
  },
  -- Weighs pages with specific attributes set through frontmatter more if that attribute is included in the search
  weightCustomProperties = {
    books = 10,
  },
  -- Files that have been edited more recently than, will be weighed more. Options are "day", "week", "month" or "disabled"
  recencyBoost = "week",
  -- Rank specific folders down
  downrankedFoldersFilters = {"Library/"},
  -- Normalize diatrics in queries and search terms. Words like "brûlée" or "žluťoučký" will be indexed as "brulee" and "zlutoucky".
  ignoreDiacritics = true,
  -- Similar to `ignoreDiacritics` but for arabic diatritics
  ignoreArabicDiacritics = false,
  -- Breaks urls down into searchable words
  tokenizeUrls = true,
  -- Breaks words seperated with camel case into searchable words
  splitCamelCase = true,
  -- Increases the fuzziness of the full-text search, options are "0", "1", "2"
  fuzziness = "1",
  -- Puts newlines into the excerpts as opposed to rendering it as one continous string
  renderLineReturnInExcerpts = true,
})
```

### Mermaid

[[Library/silverbullet-diagram/Mermaid]] renders both fenced diagrams and Lua-generated diagrams. These are the new library's supported settings; the old plug's `version`, `integrity`, and `icon_packs` options no longer apply.

```space-lua
-- priority: 90
config.set("mermaid", {
  -- ESM URL used when no local bundle is installed, and by the download command.
  cdnUrl = "https://esm.sh/mermaid@11.4.0?bundle",
  -- Space path used to save and load the optional local bundle.
  bundlePath = "Library/silverbullet-diagram/mermaid.bundle.js",
})
```

Run **Mermaid: Download bundle for offline use** to save a local renderer instead of loading it from the CDN. To make documents available offline on other devices too, enable `sync.documents` above.

### AI gateway

Keep the existing gateway preferences for use with an AI library; no AI library is currently installed in this space. Embedding indexing stays disabled.

```space-lua
-- priority: 90
local aigateway_api_key = "DUMMY"

-- NOTE: When `useProxy: false`, requests bypass SilverBullet's server proxy and go directly from the browser. Useful for local services like Ollama running on the same machine as the browser

config.set("ai", {
  providers = {
    aigateway = {
      provider = "openai",
      apiKey = aigateway_api_key,
      baseUrl = "https://aigateway.home.ilkerflix.com/v1",
      preferredModels = {"openai/gpt-5.4-mini", "vllm/Qwen3.5-35B-A3B"},
    },
  },
  -- Optional: auto-select a default model on startup
  defaultTextModel = "aigateway:vllm-qwen/Qwen3.5-35B-A3B",
  -- Optional: enable embeddings for semantic search
  defaultEmbeddingModel = "aigateway:text-embedding-3-small",
  indexEmbeddings = false,
  -- Optional: configure DALL-E for image generation
  defaultImageModel = "aigateway:dall-e-3",
  -- Optional: customize chat behavior
  chat = {
    userInformation = "I'm a software developer who likes taking notes.",
    userInstructions = "Please give short and concise responses.",
  },
})
```
