---
name: "Library/LogeshG5/Mobile Toolbar"
tags: meta/library
pageDecoration.prefix: "🧰 "
share.uri: "github:LogeshG5/silverbullet-libraries/Mobile Toolbar.md"
share.hash: 68c43076
share.mode: pull
---

# Mobile Toolbar

```space-lua
-- ============================================================
-- Utility helpers
-- ============================================================

function mbtb_normalize_sel(raw)
    if not raw then return nil end
    local f = math.min(raw.from, raw.to)
    local t = math.max(raw.from, raw.to)
    if f == t then return nil end
    return { from = f, to = t }
end

function mbtb_split_lines(text)
    local lines = {}
    local remaining = text
    while true do
        local nl = remaining:find("\n")
        if nl then
            table.insert(lines, remaining:sub(1, nl - 1))
            remaining = remaining:sub(nl + 1)
        else
            table.insert(lines, remaining)
            break
        end
    end
    return lines
end

function mbtb_get_line_range(full, sel)
    if not sel or sel.from >= sel.to then return nil, nil, nil end
    local doc_len = #full
    local from_lua = sel.from + 1

    local line_from_lua = from_lua
    while line_from_lua > 1 do
        local prev = full:sub(line_from_lua - 1, line_from_lua - 1)
        if prev == "\n" then break end
        line_from_lua = line_from_lua - 1
    end

    local line_to_lua = sel.to
    if line_to_lua > 0 and line_to_lua <= doc_len then
        if full:sub(line_to_lua, line_to_lua) == "\n" then
            line_to_lua = line_to_lua - 1
        end
    end

    while line_to_lua < doc_len do
        local next_char = full:sub(line_to_lua + 1, line_to_lua + 1)
        if next_char == "\n" then break end
        line_to_lua = line_to_lua + 1
    end

    local line_from_cm = line_from_lua - 1
    local line_to_cm   = line_to_lua
    if line_from_cm > line_to_cm then return nil, nil, nil end
    return line_from_cm, line_to_cm, full:sub(line_from_lua, line_to_lua)
end

function mbtb_get_len(text)
    if utf8 and utf8.len then
        local ok, len = pcall(utf8.len, text)
        if ok and len then return len end
    end
    return #text
end

function mbtb_get_current_line_range(full)
    local ok_sel, raw_sel = pcall(editor.getSelection)
    if not ok_sel or not raw_sel then return nil, nil, nil end

    local cursor = raw_sel.from or 0
    local doc_len = #full

    -- Convert to Lua index
    local start_lua = cursor + 1

    -- Walk backward → start of line
    while start_lua > 1 do
        if full:sub(start_lua - 1, start_lua - 1) == "\n" then break end
        start_lua = start_lua - 1
    end

    -- Walk forward → end of line
    local end_lua = cursor
    while end_lua < doc_len do
        if full:sub(end_lua + 1, end_lua + 1) == "\n" then break end
        end_lua = end_lua + 1
    end

    local from_cm = start_lua - 1
    local to_cm   = end_lua

    if from_cm > to_cm then return nil, nil, nil end

    return from_cm, to_cm, full:sub(start_lua, end_lua)
end

command.define {
    name = "Text: Toggle Task Item",
    run = function()
        local ok_txt, full = pcall(editor.getText)
        if not ok_txt then return end

        local ok_sel, raw_sel = pcall(editor.getSelection)
        local sel = mbtb_normalize_sel(raw_sel)

        local lf, lt, block

        if sel then
            lf, lt, block = mbtb_get_line_range(full, sel)
        else
            lf, lt, block = mbtb_get_current_line_range(full)
            -- line is empty, so just insert
            if lf == lt then editor.insertAtCursor("- [ ] ") return end
        end

        if not lf then return end

        local lines = mbtb_split_lines(block)

        local all_are_tasks = true
        for _, l in ipairs(lines) do
            if l ~= "" then
                local is_task = l:match("^%- %[.%] ")
                if not is_task then
                    all_are_tasks = false
                    break
                end
            end
        end

        local nl = {}
        for _, l in ipairs(lines) do
            local s = l
            if all_are_tasks then
                s = (l:gsub("^%- %[.%] ", ""))
            else
                if l ~= "" then
                    s = (l:gsub("^%- %[.%] ", ""))
                    s = (s:gsub("^%- ", ""))
                    s = (s:gsub("^%d+%. ", ""))
                    s = "- [ ] " .. s
                end
            end
            table.insert(nl, s)
        end

        local new_block = table.concat(nl, "\n")
        editor.replaceRange(lf, lt, new_block)
        if sel then
          -- Retain selection only if text was already selected
          editor.setSelection(lf, lf + mbtb_get_len(new_block))
        end
    end
}

command.define {
    name = "Text: Toggle Bullet List",
    run = function()
        local ok_txt, full = pcall(editor.getText)
        if not ok_txt then return end

        local ok_sel, raw_sel = pcall(editor.getSelection)
        local sel = mbtb_normalize_sel(raw_sel)

        local lf, lt, block

        if sel then
            lf, lt, block = mbtb_get_line_range(full, sel)
        else
            lf, lt, block = mbtb_get_current_line_range(full)
            -- line is empty, so just insert
            if lf == lt then editor.insertAtCursor("- ") return end
        end

        if not lf then return end

        local lines = mbtb_split_lines(block)

        -- Check if all lines are already bullet points
        local all_are_bullets = true
        for _, l in ipairs(lines) do
            if l ~= "" then
                -- Match a hyphen followed by a space
                local is_bullet = l:match("^%- ")
                if not is_bullet then
                    all_are_bullets = false
                    break
                end
            end
        end

        local nl = {}
        for _, l in ipairs(lines) do
            local s = l
            if all_are_bullets then
                s = (l:gsub("^%- ", ""))
            else
                if l ~= "" then
                    s = (l:gsub("^%- ", ""))
                    s = "- " .. s
                end
            end
            table.insert(nl, s)
        end

        local new_block = table.concat(nl, "\n")
        editor.replaceRange(lf, lt, new_block)
        
        if sel then
          -- Retain selection logic
          editor.setSelection(lf, lf + mbtb_get_len(new_block))
        end
    end
}

-- function to toggle a specific header level
local function toggleHead(level)
  local line = editor.getCurrentLine()
  
  local text = line.text
  local currentLevel = string.match(text, "^(#+)%s*")
  currentLevel = currentLevel and #currentLevel or 0
  
  local textC = line.textWithCursor
  local posC = string.find(textC, "|^|", 1, true)
  
  local lineHead
  if posC > currentLevel + 1 then
    local bodyTextC = string.gsub(textC, "^#+%s*", "")
    if currentLevel == level then
      lineHead = bodyTextC
    else
      lineHead = string.rep("#", level) .. " " .. bodyTextC
    end
  else
    local bodyText = string.gsub(text, "^#+%s*", "")
    if currentLevel == level then
      lineHead = "|^|" .. bodyText
    else
      lineHead = string.rep("#", level) .. " |^|" .. bodyText
    end
  end
  editor.replaceRange(line.from, line.to, lineHead, true)
end

-- register commands Ctrl-1 → Ctrl-6
for lvl = 1, 6 do
  command.define {
    name = "Header: Toggle Level " .. lvl,
    key = "Ctrl-" .. lvl,
    run = function() 
      toggleHead(lvl) 
    end
  }
end
```

```space-style
/* -- Fixed Bottom Container ---------------------------------------------- */
#sb-mobile-toolbar {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    z-index: 1000;
    background: var(--root-background-color);
    border-top: 1px solid var(--top-border-color);
    display: flex;
    flex-direction: column; /* Stack for sub-menus if needed, or just single row */
    padding-bottom: env(safe-area-inset-bottom); /* iOS support */
    user-select: none;
    touch-action: manipulation;
}

.sb-toolbar-row {
    display: flex;
    overflow-x: auto;
    scrollbar-width: none;
    padding: 6px;
    gap: 8px;
    align-items: center;
}

.sb-toolbar-row::-webkit-scrollbar { display: none; }

/* -- Buttons ------------------------------------------------------------ */
.sb-mbtb-btn {
    flex-shrink: 0;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    border: none;
    background: transparent;
    color: var(--ui-accent-color);
}

.sb-mbtb-btn:active {
    background: var(--ui-accent-color);
    transform: scale(0.9);
}

.sb-mbtb-btn svg {
    width: 20px;
    height: 20px;
}

.sb-mbtb-separator {
    width: 2px;
    height: 24px;
    background-color: var(--top-border-color);
    /*margin: 0 8px;*/
    align-self: center;
    flex-shrink: 0;
}

.sb-toolbar-row {
    display: flex;
    align-items: center;
    /* ensure the row handles the layout correctly */
}

/* Move the find panel above the toolbar*/
.cm-panels.cm-panels-bottom {
    bottom: 52px !important;
}
```

```space-lua
-- priority: 1

-- 1. Configuration & Icon Definitions
local icons = {
    back     = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>]],
    headings = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M6 12h12"/><path d="M6 20V4"/><path d="M18 20V4"/></svg>]],
    bold     = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M6 12h9a4 4 0 0 1 0 8H6a1 1 0 0 1-1-1V5a1 1 0 0 1 1-1h7a4 4 0 0 1 0 8"></path></svg>]],
    italic   = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="19" y1="4" x2="10" y2="4"></line><line x1="14" y1="20" x2="5" y2="20"></line><line x1="15" y1="4" x2="9" y2="20"></line></svg>]],
    list     = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><path d="M3 6h.01"></path><path d="M3 12h.01"></path><path d="M3 18h.01"></path></svg>]],
    h1       = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><text x="2" y="18" font-family="sans-serif" font-size="16" font-weight="bold">H1</text></svg>]],
    h2       = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><text x="2" y="18" font-family="sans-serif" font-size="16" font-weight="bold">H2</text></svg>]],
    h3       = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor"><text x="2" y="18" font-family="sans-serif" font-size="16" font-weight="bold">H3</text></svg>]],
    check    = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="18" x="3" y="3" rx="2"></rect><path d="m9 12 2 2 4-4"></path></svg>]],
    link     = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg>]],
    quote    = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 21c3 0 7-1 7-8V5H4v8h4c0 2-1 3-5 5z"></path><path d="M14 21c3 0 7-1 7-8V5h-6v8h4c0 2-1 3-5 5z"></path></svg>]],
  indent = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-list-indent-increase-icon lucide-list-indent-increase"><path d="M21 5H11"/><path d="M21 12H11"/><path d="M21 19H11"/><path d="m3 8 4 4-4 4"/></svg>]],
  outdent = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-list-indent-decrease-icon lucide-list-indent-decrease"><path d="M21 5H11"/><path d="M21 12H11"/><path d="M21 19H11"/><path d="m7 8-4 4 4 4"/></svg>]],
  moveup = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-arrow-up-from-line-icon lucide-arrow-up-from-line"><path d="m18 9-6-6-6 6"/><path d="M12 3v14"/><path d="M5 21h14"/></svg>]],
  movedown = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-arrow-down-from-line-icon lucide-arrow-down-from-line"><path d="M19 3H5"/><path d="M12 21V7"/><path d="m6 15 6 6 6-6"/></svg>]],
  fold = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-fold-vertical-icon lucide-fold-vertical"><path d="M12 22v-6"/><path d="M12 8V2"/><path d="M4 12H2"/><path d="M10 12H8"/><path d="M16 12h-2"/><path d="M22 12h-2"/><path d="m15 19-3-3-3 3"/><path d="m15 5-3 3-3-3"/></svg>]],
  delete = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-trash-icon lucide-trash"><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/><path d="M3 6h18"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>]],
  undo = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-undo2-icon lucide-undo-2"><path d="M9 14 4 9l5-5"/><path d="M4 9h10.5a5.5 5.5 0 0 1 5.5 5.5a5.5 5.5 0 0 1-5.5 5.5H11"/></svg>]],
  redo = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-redo2-icon lucide-redo-2"><path d="m15 14 5-5-5-5"/><path d="M20 9H9.5A5.5 5.5 0 0 0 4 14.5A5.5 5.5 0 0 0 9.5 20H13"/></svg>]],
  highlight = [[<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-highlighter-icon lucide-highlighter"><path d="m9 11-6 6v3h9l3-3"/><path d="m22 12-4.6 4.6a2 2 0 0 1-2.8 0l-5.2-5.2a2 2 0 0 1 0-2.8L14 4"/></svg>]]
  }

-- 2. Menu Schema
local menus = {
    main = {
        { icon = icons.headings, sub = "headings" },
        { icon = icons.check,    cmd = "Text: Toggle Task Item" },
        { icon = icons.list,     cmd = "Text: Toggle Bullet List" },
        { separator = true },
        { icon = icons.indent,   cmd = "Editor: Indent"},
        { icon = icons.outdent,  cmd = "Editor: Outdent"},
        { icon = icons.moveup,   cmd = "Outline: Move Up"},
        { icon = icons.movedown, cmd = "Outline: Move Down"},
        { separator = true },
        { icon = icons.delete,   cmd = "Delete Line"},
        { icon = icons.undo,     cmd = "Editor: Undo"},
        { icon = icons.redo,     cmd = "Editor: Redo"},
        { icon = icons.fold,     cmd = "Outline: Toggle Fold"},
    },
    headings = {
        { icon = icons.back,     sub = "main" },
        { icon = icons.h1,       cmd = "Header: Toggle Level 1" },
        { icon = icons.h2,       cmd = "Header: Toggle Level 2" },
        { icon = icons.h3,       cmd = "Header: Toggle Level 3" }
    },
    selection_menu = {
        { icon = icons.bold,     cmd = "Text: Bold" },
        { icon = icons.italic,   cmd = "Text: Italic" },
        { icon = icons.quote,    cmd = "Text: Quote" },
        { icon = icons.check,    cmd = "Text: Toggle Task Item" },
        { icon = icons.list,     cmd = "Text: Toggle Bullet List" },
        { icon = icons.highlight,cmd = "Text: Marker" },
        { icon = icons.delete,   cmd = "Delete Line"},
    }
}

local currentMenu = "main"

-- 3. Core Rendering Function
function renderMobileToolbar(menuKey)
    currentMenu = menuKey
    local toolbar = js.window.document.getElementById("sb-mobile-toolbar")
    
    if not toolbar then
        toolbar = js.window.document.createElement("div")
        toolbar.id = "sb-mobile-toolbar"
        js.window.document.body.appendChild(toolbar)

        toolbar.addEventListener("click", function(e)
          editor.focus()
            local btn = e.target.closest(".sb-mbtb-btn")
            if not btn then return end
            
            e.preventDefault()
            e.stopPropagation()

            local action = btn.getAttribute("data-action")
         
            if action:sub(1,5) == "menu:" then
                renderMobileToolbar(action:sub(6))
            elseif action:sub(1,4) == "cmd:" then
                editor.invokeCommand(action:sub(5))
            end
            
        end)
    end

    local menuItems = menus[menuKey] or menus.main
    local html = '<div class="sb-toolbar-row">'
    
    for _, item in ipairs(menuItems) do
        if item.separator then
            -- Render a visual divider instead of a button
            html = html .. '<div class="sb-mbtb-separator"></div>'
        else
        local action = item.sub and ("menu:" .. item.sub) or ("cmd:" .. item.cmd)
        local label  = item.cmd or item.sub
        html = html .. string.format(
            '<button class="sb-mbtb-btn" data-action="%s" aria-label="%s">%s</button>',
            action, label, item.icon
        )
        end
    end
    
    html = html .. '</div>'
    toolbar.innerHTML = html
end

-- 4. Selection Logic
-- This function checks if text is selected and switches the menu
function updateToolbarState()
    local selection = editor.getSelection()
    local hasSelection = selection.from ~= selection.to
    
    if hasSelection and currentMenu ~= "selection_menu" then
        renderMobileToolbar("selection_menu")
    elseif not hasSelection and currentMenu == "selection_menu" then
        renderMobileToolbar("main")
    end
end

-- 5. Initialization & Event Polling
function initToolbar()
    renderMobileToolbar("main")
    
    -- Since SilverBullet doesn't always expose a direct 'onSelectionChange' 
    -- event to Space Lua easily, we can poll or hook into the render loop.
    -- A simple interval is effective for mobile responsiveness.
    js.window.setInterval(function()
        updateToolbarState()
    end, 200)
end

function destroyToolbar()
    local toolbar = js.window.document.getElementById("sb-mobile-toolbar")
    if toolbar then toolbar.remove() end
end

-- Execute
-- Only show on mobile/small screens (optional check)
local isMobile = js.window.innerWidth < 600 or js.window.navigator.userAgent:match("Android|iPhone|iPad")
if isMobile then
   initToolbar()
end
```

