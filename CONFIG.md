#meta

This is where you configure SilverBullet to your liking. See [[^Library/Std/Config]] for a full list of configuration options.

## General

```space-lua
--priority: 9

-- Disable TOC widget and use TOC Sidepanel instead
function widgets.toc(options)
    return
end
```

```space-lua
--priority 100
-- Core configuration settings
config.set {
  ["sync.documents"] = false,
}
```

```space-lua
-- Reset the dark mode toggle to follow the system theme
clientStore.delete("darkMode")
```

## Libraries

```space-lua
-- Library configurations

local aigateway_api_key = "DUMMY"

config.set {
  explorer = {
    position = "lhs",
    homeDirName = "🏠 Home",
    goToCurrentDir = true,
    tileSize = "80px",
    enableContextMenu = true,
    listHeight = "24px",
    negativeFilter = {"Library/Std","*.js", "*test*"},
    treeFolderFirst = false,
    recoverAfterRefresh = true
  },

  AdvancedPanelControl = {
    mode = "auto",  -- "auto" | "overlay" | "dock"
    gestures = true,  -- true (enabled), false (disabled)
    minWidth = "300", -- min Width Constraints for LHS and RHS
    maxWidth = "1000", -- max Width Constraints for LHS and RHS
    minHeight = "100", -- min Height Constraints for BHS
    maxHeight = "500", -- max Height Constraints for BHS
  },

  silversearch = {
    -- Weighs specific fields more
    weights = {
      basename = 15
      -- Also available: tags, aliases, directory, displayName, content
    },
    -- Weighs pages with specific attributes set through frontmatter more if that attribute is included in the search
    weightCustomProperties = {
      books = 10
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
    renderLineReturnInExcerpts = true
  },

  -- NOTE: When `useProxy: false`, requests bypass SilverBullet's server proxy and go directly from the browser. Useful for local services like Ollama running on the same machine as the browser


  ai = {
    providers = {
      aigateway = {
        provider = "openai",
        apiKey = aigateway_api_key,
        baseUrl = "https://aigateway.gpu.ilkerflix.com/v1",
        preferredModels = {"openai/gpt-5.4-mini", "vllm-qwen/Qwen3.5-35B-A3B"}
      }
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
      userInstructions = "Please give short and concise responses."
    }
  }
}
```

### Mermaid

```bash
config.set("mermaid", {
  version = "11.4.0",
  integrity = "new integrity hash",
  -- or disable integrity checking
  integrity_disabled = true
  -- optional: register icon packs 
  icon_packs = {
    {
      name = "logos",
      url = "https://unpkg.com/@iconify-json/logos@1/icons.json",
    },
  },
})
```

## Action Buttons

```space-lua
-- Action buttons configuration
config.set {
  actionButtons = {
    {
      icon = "terminal",
      description = "Run command",
      priority = 999,
      run = function()
        editor.invokeCommand("Open Command Palette")
      end,
    }
  }
}

-- Additional action buttons defined separately
actionButton.define {
  icon = "sidebar",
  priority = 0,
  description = "Toggle Document Explorer",
  run = function()
    editor.invokeCommand("Navigate: Toggle Document Explorer")
  end
}

actionButton.define {
  icon = "search",
  description = "Search",
  priority = 998,
  run = function()
    editor.invokeCommand("Silversearch: Search")
  end
}

-- Defines a new button that forces your UI into read-only mode
actionButton.define {
  icon = (editor.getUiOption("forcedROMode") and "edit") or "eye",
  description = "Toggle Read-Only",
  priority = 1000,
  mobile = true,
  run = function()
    local mode = editor.getUiOption("forcedROMode")
    editor.setUiOption("forcedROMode", not mode)
    editor.rebuildEditorState()
    editor.reloadConfigAndCommands()
  end
}
```

## Commands

```space-lua
-- Sidebar Open Page Command

command.define {
  name = "Sidebar: Open Page",
  key = "Ctrl-Alt-ArrowRight",
  run = function()
    local allPages = query[[
      from index.tag "page"
      order by _.lastModified desc
    ]]
    local page = editor.filterBox('➡️', allPages, "Select the page to open aside")
    if page != nil then
      editor.showPanel("rhs", 2, [[<iframe src = ]] .. page.name .. [[ style = "height: 98vh; width: 100%; border: 0;" />]])
    end
  end
}

command.define {
  name = "Sidebar: Close",
  key = "Ctrl-Alt-ArrowLeft",
  run = function()
    editor.hidePanel("rhs")
  end
}
```
