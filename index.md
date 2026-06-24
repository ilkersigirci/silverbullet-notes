---
title: index
date: 2026-06-24
draf: false
---

> **note** Info
> This is my Silverbullet Notes.
>
> It is used to showcase Silverbullet Features.

${widgets.commandButton("System: Reload")}

Builtin [[^Library/Std/Pages/Maintenance]] Page

**Total pages:** ${#query[[from index.tag "page" select name]]}
**Total documents:** ${#query[[from index.tag "document" select name]]}

- SilverBullet uses [markdown](https://www.markdownguide.org/)
- While SilverBullet implements (most) of [CommonMark](https://commonmark.org/), it also adds a few extensions that are SilverBullet-specific. Most notably, it adds the `${Lua expression}` syntax to render Lua expressions inline.

The last 5 opened pages.

${query[[
  from p = editor.getRecentlyOpenedPages "page"
  where p.lastOpened
  select {ref= "[[".. p.ref .."]]", lastVisit=os.date("%Y-%m-%d %H:%M:%S", p.lastOpened/1000)} 
  order by p.lastOpened desc
  limit 7
]]}
