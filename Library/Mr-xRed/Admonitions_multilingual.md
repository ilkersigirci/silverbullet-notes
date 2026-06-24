---
name: "Library/Mr-xRed/Admonitions_multilingual"
tags: meta/library
pageDecoration.prefix: "🛠️ "
share.uri: "github:Mr-xRed/silverbullet-libraries/Admonitions_multilingual.md"
share.hash: bc58531f
share.mode: pull
---
# Multilingual Custom Admonitions

Shoutout to [paletochen](https://community.silverbullet.md/u/paletochen/summary), [mjf](https://community.silverbullet.md/u/mjf), [malys](https://community.silverbullet.md/u/malys/summary) and [i\_am\_dangry](https://community.silverbullet.md/u/i_am_dangry/summary) for making this admonitions possible.
I wanted to translate the admonition texts to german for my use case, and tinkered around until I made it multilingual😜.

## Currently supported languages
🇬🇧  🇩🇪  🇫🇷  🇮🇹  🇨🇳  🇪🇸  🇭🇺  🇷🇴  🇨🇿  🇵🇱  🇵🇹/🇧🇷
## Configuration

### Language
- add following space-lua to your configuration with your desired language. 
- available languages: "en", "de", "fr", "it", "zh", "es", "hu", "ro", “cs”, “pl”, “pt”
  
```lua
config.set("admonLang","de")
```

### Customize the look of your admonitions (paddings,font size , border-width, style & radius)

```space-style
:root {
  --admonition-title-font-size: 1em;
  --admonition-title-font-weight: 800;
  --admonition-text-font-size: 0.9em;
  --admonition-text-color: var(--root-color);
  --admonition-width: 100%;
  --admonition-padding-title: 10px;
  --admonition-padding-text: 10px;
  --admonition-border-width: 2px;
  --admonition-border-style: solid;
  /* dotted, dashed, solid, double, groove, ridge, inset, outset, none, hidden */
  --admonition-border-radius: 10px;
}
```

## Implementation

To not have multiple space-styles for the different languages I left the admonition types in english, and handle the multi language support only in the SlashCommand definitions.

### Styling: Colors
(jump to [[#Examples:| Examples]])

```space-style

html[data-theme="dark"] {
  --admonition-color-danger:   oklch(90% 0.40 25);    /* deep red */
  --admonition-color-failure:  oklch(90% 0.30 40);    /* red-orange */
  --admonition-color-warning:  oklch(90% 0.30 70);    /* orange-yellow */
  --admonition-color-question: oklch(90% 0.30 100);   /* yellow-green */
  --admonition-color-success:  oklch(90% 0.40 125);   /* green */
  --admonition-color-tip:      oklch(90% 0.30 150);   /* mint-green */
  --admonition-color-note:     oklch(90% 0.30 180);   /* cyan-green */
  --admonition-color-info:     oklch(90% 0.30 210);   /* sky blue */
  --admonition-color-seealso:  oklch(90% 0.30 240);   /* deep blue */
  --admonition-color-abstract: oklch(90% 0.30 270);   /* violet */
  --admonition-color-example:  oklch(90% 0.30 300);   /* purple */
  --admonition-color-bug:      oklch(90% 0.40 350);   /* magenta */
  --admonition-color-quote:    oklch(90% 0 0);        /* neutral gray */
}

html[data-theme="light"] {
  --admonition-color-danger:   oklch(60% 0.40 25);    /* deep red */
  --admonition-color-failure:  oklch(70% 0.40 55);    /* red-orange */
  --admonition-color-warning:  oklch(70% 0.40 80);    /* orange-yellow */
  --admonition-color-question: oklch(70% 0.40 105);   /* yellow-green */
  --admonition-color-success:  oklch(60% 0.50 125);   /* green */
  --admonition-color-tip:      oklch(60% 0.40 150);   /* mint-green */
  --admonition-color-note:     oklch(60% 0.40 180);   /* cyan-green */
  --admonition-color-info:     oklch(60% 0.40 210);   /* sky blue */
  --admonition-color-seealso:  oklch(60% 0.40 240);   /* deep blue */
  --admonition-color-abstract: oklch(60% 0.40 270);   /* violet */
  --admonition-color-example:  oklch(60% 0.40 300);   /* purple */
  --admonition-color-bug:      oklch(60% 0.40 350);   /* magenta */
  --admonition-color-quote:    oklch(50% 0 0);        /* neutral gray */
}
```

### Styling: Icons

```
Icons from Lucide (https://lucide.dev)
Copyright (c) Lucide Contributors 2025
Portions Copyright (c) 2013–2023 Cole Bemis (Feather)
Licensed under the ISC and MIT licenses.
```

```space-style
:root {

--admonition-icon-abstract: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-notepad-text-icon lucide-notepad-text"><path d="M8 2v4"/><path d="M12 2v4"/><path d="M16 2v4"/><rect width="16" height="18" x="4" y="4" rx="2"/><path d="M8 10h6"/><path d="M8 14h8"/><path d="M8 18h5"/></svg>'); 
  
--admonition-icon-note: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-notebook-pen-icon lucide-notebook-pen"><path d="M13.4 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-7.4"/><path d="M2 6h4"/><path d="M2 10h4"/><path d="M2 14h4"/><path d="M2 18h4"/><path d="M21.378 5.626a1 1 0 1 0-3.004-3.004l-5.01 5.012a2 2 0 0 0-.506.854l-.837 2.87a.5.5 0 0 0 .62.62l2.87-.837a2 2 0 0 0 .854-.506z"/></svg>');

--admonition-icon-info: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-info-icon lucide-info"><circle cx="12" cy="12" r="10"/><path d="M12 16v-4"/><path d="M12 8h.01"/></svg>');
  
--admonition-icon-tip: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-flame-icon lucide-flame"><path d="M12 3q1 4 4 6.5t3 5.5a1 1 0 0 1-14 0 5 5 0 0 1 1-3 1 1 0 0 0 5 0c0-2-1.5-3-1.5-5q0-2 2.5-4"/></svg>'); 
  
--admonition-icon-success: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-circle-check-big-icon lucide-circle-check-big"><path d="M21.801 10A10 10 0 1 1 17 3.335"/><path d="m9 11 3 3L22 4"/></svg>'); 

--admonition-icon-question: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-circle-question-mark-icon lucide-circle-question-mark"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><path d="M12 17h.01"/></svg>'); 

--admonition-icon-failure: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-circle-x-icon lucide-circle-x"><circle cx="12" cy="12" r="10"/><path d="m15 9-6 6"/><path d="m9 9 6 6"/></svg>'); 

--admonition-icon-warning: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-triangle-alert-icon lucide-triangle-alert"><path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3"/><path d="M12 9v4"/><path d="M12 17h.01"/></svg>'); 

--admonition-icon-danger: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-zap-icon lucide-zap"><path d="M4 14a1 1 0 0 1-.78-1.63l9.9-10.2a.5.5 0 0 1 .86.46l-1.92 6.02A1 1 0 0 0 13 10h7a1 1 0 0 1 .78 1.63l-9.9 10.2a.5.5 0 0 1-.86-.46l1.92-6.02A1 1 0 0 0 11 14z"/></svg>'); 

--admonition-icon-bug: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-bug-icon lucide-bug"><path d="M12 20v-9"/><path d="M14 7a4 4 0 0 1 4 4v3a6 6 0 0 1-12 0v-3a4 4 0 0 1 4-4z"/><path d="M14.12 3.88 16 2"/><path d="M21 21a4 4 0 0 0-3.81-4"/><path d="M21 5a4 4 0 0 1-3.55 3.97"/><path d="M22 13h-4"/><path d="M3 21a4 4 0 0 1 3.81-4"/><path d="M3 5a4 4 0 0 0 3.55 3.97"/><path d="M6 13H2"/><path d="m8 2 1.88 1.88"/><path d="M9 7.13V6a3 3 0 1 1 6 0v1.13"/></svg>'); 

--admonition-icon-example: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-test-tube-diagonal-icon lucide-test-tube-diagonal"><path d="M21 7 6.82 21.18a2.83 2.83 0 0 1-3.99-.01a2.83 2.83 0 0 1 0-4L17 3"/><path d="m16 2 6 6"/><path d="M12 16H4"/></svg>'); 

--admonition-icon-quote: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-quote-icon lucide-quote"><path d="M16 3a2 2 0 0 0-2 2v6a2 2 0 0 0 2 2 1 1 0 0 1 1 1v1a2 2 0 0 1-2 2 1 1 0 0 0-1 1v2a1 1 0 0 0 1 1 6 6 0 0 0 6-6V5a2 2 0 0 0-2-2z"/><path d="M5 3a2 2 0 0 0-2 2v6a2 2 0 0 0 2 2 1 1 0 0 1 1 1v1a2 2 0 0 1-2 2 1 1 0 0 0-1 1v2a1 1 0 0 0 1 1 6 6 0 0 0 6-6V5a2 2 0 0 0-2-2z"/></svg>'); 

--admonition-icon-seealso: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-reply-icon lucide-reply"><path d="M20 18v-2a4 4 0 0 0-4-4H4"/><path d="m9 17-5-5 5-5"/></svg>'); 
}
```

### Style: Combining the two
```space-style
.sb-admonition[admonition=danger] {
  --admonition-icon: var(--admonition-icon-danger);
  --admonition-color: var(--admonition-color-danger);
}

.sb-admonition[admonition=failure] {
  --admonition-icon: var(--admonition-icon-failure);
  --admonition-color: var(--admonition-color-failure);
}

.sb-admonition[admonition=warning] {
  --admonition-icon: var(--admonition-icon-warning);
  --admonition-color: var(--admonition-color-warning);
}

.sb-admonition[admonition=question] {
  --admonition-icon: var(--admonition-icon-question);
  --admonition-color: var(--admonition-color-question);
}

.sb-admonition[admonition=success] {
  --admonition-icon: var(--admonition-icon-success);
  --admonition-color: var(--admonition-color-success);
}

.sb-admonition[admonition=tip] {
  --admonition-icon: var(--admonition-icon-tip);
  --admonition-color: var(--admonition-color-tip);
}

.sb-admonition[admonition=note] {
  --admonition-icon: var(--admonition-icon-note);
  --admonition-color: var(--admonition-color-note);
}

.sb-admonition[admonition=info] {
  --admonition-icon: var(--admonition-icon-info);
  --admonition-color: var(--admonition-color-info);
}

.sb-admonition[admonition=seealso] {
  --admonition-icon: var(--admonition-icon-seealso);
  --admonition-color: var(--admonition-color-seealso);
}

.sb-admonition[admonition=abstract] {
  --admonition-icon: var(--admonition-icon-abstract);
  --admonition-color: var(--admonition-color-abstract);
}

.sb-admonition[admonition=example] {
  --admonition-icon: var(--admonition-icon-example);
  --admonition-color: var(--admonition-color-example);
}

.sb-admonition[admonition=bug] {
  --admonition-icon: var(--admonition-icon-bug);
  --admonition-color: var(--admonition-color-bug);
}

.sb-admonition[admonition=quote] {
  --admonition-icon: var(--admonition-icon-quote);
  --admonition-color: var(--admonition-color-quote);
}

```

### Styling: Border and Padding implementation
```space-style

.sb-admonition-type * { display: none; }

.sb-admonition-type::before {
    width: var(--admonition-width) !important;
    margin-right: .3em; 
    margin-bottom: .15em; 
}
/* target all admonitions */
#sb-main .cm-editor .sb-admonition {
  font-size: var(--admonition-text-font-size);
/* uncomment next line if you want paragraph padding inside your admonition/*
/*  padding-top: calc(var(--admonition-padding-text)/2); */
  color: var(--admonition-text-color);
  border-left: var(--admonition-border-width)  var(--admonition-border-style) var(--admonition-color) !important;
  border-right: var(--admonition-border-width)  var(--admonition-border-style) var(--admonition-color);}


/* target admonition text */
.sb-admonition:not(.sb-admonition-title){
    width: calc(var(--admonition-width) - ( 2 * var(--admonition-padding-text)));
    padding-inline: var(--admonition-padding-text) !important;
}

/* target the admonition title */
.sb-admonition-title {
  width: calc(var(--admonition-width) - ( 2 * var(--admonition-padding-title)));
  padding-inline: calc(1 * var(--admonition-padding-title)) !important; 
  color: var(--admonition-color) !important;
  font-size: var(--admonition-title-font-size) !important;
  font-weight: var(--admonition-title-font-weight) !important;
  padding-block: var(--admonition-padding-title) !important;
  border-top: var(--admonition-border-width)  var(--admonition-border-style) var(--admonition-color);
  border-top-left-radius: var(--admonition-border-radius);
  border-top-right-radius: var(--admonition-border-radius);
}

/* target the first admonition directly after the title */
.sb-admonition-title + .sb-admonition {
  padding-top: calc(0.7 * var(--admonition-padding-text)) !important;
}



/* target the admonition that is NOT immediately followed by another admonition */
.sb-admonition:not(:has(+ .sb-admonition)){
  padding-bottom: calc(0.7 * var(--admonition-padding-text)) !important;
  border-bottom: var(--admonition-border-width) var(--admonition-border-style) var(--admonition-color);
  border-bottom-left-radius: var(--admonition-border-radius);
  border-bottom-right-radius: var(--admonition-border-radius); 
}

```


### Multilingual SlashCommands

```space-lua
-- prioity: -1
config.define("admonLang", {
    type = "string",
    enum = {"en", "de", "fr", "it", "zh", "es", "hu", "ro", "cs", "pl", "pt"}
})

local admonitions_en = {
  ["Note"] = "note",
  ["Abstract"] = "abstract",
  ["Info"] = "info",
  ["Tip"] = "tip",
  ["Success"] = "success",
  ["Question"] = "question",
  ["Warning"] = "warning",
  ["Failure"] = "failure",
  ["Danger"] = "danger",
  ["Bug"] = "bug",
  ["Example"] = "example",
  ["Quote"] = "quote",
  -- aliases
  ["Hint"] = "tip",
  ["Caution"] = "warning",
  ["Error"] = "failure",
  ["See Also"] = "seealso",
  ["See"] = "seealso",
}

-- German --
local admonitions_de = {
  ["Notiz"] = "note",
  ["Zusammenfassung"] = "abstract",
  ["Information"] = "info",
  ["Tipp"] = "tip",
  ["Erfolg"] = "success",
  ["Frage"] = "question",
  ["Warnung"] = "warning",
  ["Fehler"] = "failure",
  ["Gefahr"] = "danger",
  ["Programmfehler"] = "bug",
  ["Beispiel"] = "example",
  ["Zitat"] = "quote",
  ["Siehe auch"] = "seealso",
  -- aliases
  ["Achtung"] = "warning",
  ["Hinweis"] = "note",
  ["Siehe"] = "seealso",
}

-- French --
local admonitions_fr = {
  ["Remarque"] = "note",
  ["Résumé"] = "abstract",
  ["Information"] = "info",
  ["Astuce"] = "tip",
  ["Succès"] = "success",
  ["Question"] = "question",
  ["Avertissement"] = "warning",
  ["Échec"] = "failure",
  ["Danger"] = "danger",
  ["Bogue"] = "bug",
  ["Exemple"] = "example",
  ["Citation"] = "quote",
  ["Voir aussi"] = "seealso",
  -- aliases
  ["Conseil"] = "tip",
  ["Erreur"] = "failure",
  ["Attention"] = "warning",
  ["Voir"] = "seealso",
}

-- Italian
local admonitions_it = {
  ["Nota"] = "note",
  ["Sommario"] = "abstract",
  ["Informazione"] = "info",
  ["Suggerimento"] = "tip",
  ["Successo"] = "success",
  ["Domanda"] = "question",
  ["Avvertimento"] = "warning",
  ["Errore"] = "failure",
  ["Pericolo"] = "danger",
  ["Bug"] = "bug",
  ["Esempio"] = "example",
  ["Citazione"] = "quote",
  ["Vedi anche"] = "seealso",
  -- aliases --
  ["Attenzione"] = "warning",
  ["Consiglio"] = "tip",
  ["Nota bene"] = "note",
  ["Vedi"] = "seealso",
}

-- Chinese (Simplified)
local admonitions_zh = {
  ["注意"] = "note",
  ["摘要"] = "abstract",
  ["信息"] = "info",
  ["提示"] = "tip",
  ["成功"] = "success",
  ["问题"] = "question",
  ["警告"] = "warning",
  ["失败"] = "failure",
  ["危险"] = "danger",
  ["错误"] = "bug",
  ["示例"] = "example",
  ["引用"] = "quote",
  ["另请参见"] = "seealso",
  -- aliases
  ["注意事项"] = "warning",
  ["错误信息"] = "failure",
  ["提示信息"] = "tip",
}

-- Spanish --
local admonitions_es = {
  ["Nota"] = "note",
  ["Resumen"] = "abstract",
  ["Información"] = "info",
  ["Consejo"] = "tip",
  ["Éxito"] = "success",
  ["Pregunta"] = "question",
  ["Advertencia"] = "warning",
  ["Fallo"] = "failure",
  ["Peligro"] = "danger",
  ["Error"] = "bug",
  ["Ejemplo"] = "example",
  ["Cita"] = "quote",
  ["Ver también"] = "seealso",
  -- aliases
  ["Aviso"] = "warning",
  ["Ver"] = "seealso",
}

-- Hungarian
local admonitions_hu = {
  ["Jegyzet"] = "note",
  ["Kivonat"] = "abstract",
  ["Információ"] = "info",
  ["Tipp"] = "tip",
  ["Siker"] = "success",
  ["Kérdés"] = "question",
  ["Figyelem"] = "warning",
  ["Hiba"] = "failure",
  ["Veszély"] = "danger",
  ["Programhiba"] = "bug",
  ["Példa"] = "example",
  ["Idézet"] = "quote",
  ["Lásd még"] = "seealso",
  -- aliases --
  ["Megjegyzés"] = "note",
  ["Lásd"] = "seealso",
}

-- Romanian
local admonitions_ro = {
  ["Notă"] = "note",
  ["Rezumat"] = "abstract",
  ["Informație"] = "info",
  ["Sfat"] = "tip",
  ["Succes"] = "success",
  ["Întrebare"] = "question",
  ["Atenție"] = "warning",
  ["Eroare"] = "failure",
  ["Pericol"] = "danger",
  ["Bug"] = "bug",
  ["Exemplu"] = "example",
  ["Citat"] = "quote",
  ["Vezi și"] = "seealso",
  -- aliases --
  ["Observație"] = "note",
  ["Vezi"] = "seealso",
}

-- Czech
local admonitions_cs = {
  ["Poznámka"] = "note",
  ["Abstrakt"] = "abstract",
  ["Informace"] = "info",
  ["Tip"] = "tip",
  ["Úspěch"] = "success",
  ["Otázka"] = "question",
  ["Varování"] = "warning",
  ["Neúspěch"] = "failure",
  ["Nebezpečí"] = "danger",
  ["Chyba"] = "bug",
  ["Příklad"] = "example",
  ["Citát"] = "quote",
  ["Viz také"] = "seealso",
  -- aliases --
  ["Upozornění"] = "warning",
  ["Viz"] = "seealso",
}

-- Polish
local admonitions_pl = {
  ["Uwaga"] = "note",
  ["Streszczenie"] = "abstract",
  ["Informacja"] = "info",
  ["Wskazówka"] = "tip",
  ["Sukces"] = "success",
  ["Pytanie"] = "question",
  ["Ostrzeżenie"] = "warning",
  ["Błąd"] = "failure",
  ["Niebezpieczeństwo"] = "danger",
  ["Błąd programu"] = "bug",
  ["Przykład"] = "example",
  ["Cytat"] = "quote",
  ["Zobacz także"] = "seealso",
  -- aliases --
  ["Porada"] = "tip",
  ["Zobacz"] = "seealso",
  ["Uwaga!"] = "warning",
}

-- Português brasileiro --
local admonitions_pt = {
  ["Nota"] = "note",
  ["Resumo"] = "abstract",
  ["Informação"] = "info",
  ["Dica"] = "tip",
  ["Sucesso"] = "success",
  ["Questão"] = "question",
  ["Cuidado"] = "warning",
  ["Falha"] = "failure",
  ["Perigo"] = "danger",
  ["Erro"] = "bug",
  ["Exemplo"] = "example",
  ["Citação"] = "quote",
  ["Ver também"] = "seealso",
  -- aliases
  ["Aviso"] = "warning",
  ["Ver"] = "seealso",
  ["Pergunta"] = "question",
  ["Dúvida"] = "question",
  ["Comentário"] = "quote",	
}

-- Language mapping
local lang_map = {
  en = admonitions_en,
  de = admonitions_de,
  fr = admonitions_fr,
  it = admonitions_it,
  zh = admonitions_zh,
  es = admonitions_es,
  hu = admonitions_hu,
  ro = admonitions_ro,
  cs = admonitions_cs,
  pl = admonitions_pl,
  pt = admonitions_pt
}

-- Select language first
local lang = config.get("admonLang") or ""
--if lang == "" then 
--  editor.flashNotification("⚠️ Admonition lang. not set. Default: English", "info")
--end

local admonitions = lang_map[lang] or admonitions_en

-- Generate slash commands
for displayName, type in pairs(admonitions) do
  slashcommand.define {
    name = "Admonition for " .. displayName,
    description = "Insert admonition type: "..type,
    run = function()
      local currentLine = editor.getCurrentLine()
      local admonitionTemplate = "> **" .. type .. "** " .. displayName .. "\n> " .. currentLine.text .. "|^|\n"
      editor.replaceRange(currentLine.from, currentLine.to, admonitionTemplate, true)
    end
  }
end
```

## Examples:
(jump to [[#Styling: Colors|Colors]])

> **danger** Danger  
> This action is irreversible and could break critical functionality. Execute only if you know what you’re doing. Danger warnings are not for decoration; they mean “one click and you’ll regret everything.” Always assume they’re dead serious — because they usually are.

> **failure** Failure  
> The operation didn’t complete as expected. Review the logs to identify what went wrong. Failures are not the end but a breadcrumb trail back to understanding. If nothing ever failed, you’d never know how impressively creative software can be in breaking.

> **warning** Warning  
> Changing these values could cause unexpected results. Proceed carefully and back up your data first. Warnings exist to stop your future self from swearing at your past self — a small yellow triangle that screams, “Don’t say I didn’t warn you.”

> **question** Question  
> What happens if you modify this setting? Try it and observe the result to understand the behavior. Questions like these encourage exploration and help you build intuition instead of just following recipes. The best learning often starts with curiosity.

> **success** Success
> Your configuration was applied successfully. Everything is running exactly as intended. When success messages appear, take a brief moment to bask in the satisfaction — your system finally listened, and nothing exploded. That’s progress worth celebrating.

> **tip** Tip  
> Use keyboard shortcuts to speed up your workflow. Small optimisations make a big difference over time. Tips are the gentle whispers of experience, saving you hours of trial and error while making you look like you’ve mastered the system from day one.

> **note** Note  
> A small but important reminder. Notes highlight subtle nuances, caveats, or background context that might not be critical but can prevent unnecessary confusion or mistakes. Think of them as gentle nudges from your future self.

> **info** Info  
> Useful details that clarify concepts or processes without being essential. Info blocks provide context, definitions, or explanations that make the main content easier to follow — like a helpful sidekick that doesn’t steal the spotlight.

> **seealso** See Also  
> For further reading and related topics, check these references. “See Also” blocks point you toward deeper insights, neighbouring ideas, or complementary tools — the intellectual equivalent of a friendly librarian handing you one more book you didn’t know you needed.

> **abstract** Abstract  
> This section provides a brief overview of the concept. Abstracts are perfect for giving readers a quick sense of direction, ensuring they know what’s coming before they commit to reading the full explanation.

> **example** Example  
> A concrete illustration of how something works in practice. Examples show the application of a concept, feature, or function, helping you visualize outcomes instead of imagining them. They’re the bridge between theory and hands-on understanding.

> **bug** Bug  
> There’s a known issue with this feature under certain conditions. Bugs are the digital equivalent of ghosts — rarely seen directly, but you feel their presence when something moves that shouldn’t. Developers chase them. Sometimes catching one just to release three more.

> **quote** Quote  
> A distilled thought worth pausing over. Quotes capture experience in a few well-chosen words — fragments of insight that lend perspective, spark reflection, or simply remind you that wisdom often speaks softly but lingers long.


## Discussions to this library
* [SilverBullet Community](https://community.silverbullet.md/t/additional-admonition-types/281?u=mr.red)

## Credits:
- [paletochen](https://community.silverbullet.md/u/paletochen/summary)
- [mjf](https://community.silverbullet.md/u/mjf)
- [malys](https://community.silverbullet.md/u/malys/summary)
- [i\_am\_dangry](https://community.silverbullet.md/u/i_am_dangry/summary)
