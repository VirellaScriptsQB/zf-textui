=========================================================
💬 ZF-TEXTUI — README.txt (Client-Only • NUI • Exports)
=========================================================

zf-textui is a lightweight, no-blur **TextUI prompt system** for FiveM.
It’s designed for simple on-screen interaction hints like:
“Press E to interact”, lockers, doors, zones, NPCs, etc.

✅ Client-only (no server.lua)
✅ NUI-based (HTML/CSS/JS)
✅ Clean exports + optional client events
✅ Theme support (accent/background/text)
✅ Optional zf-notify auto-detected for feedback
✅ Built-in test & proximity demo commands

---------------------------------------------------------
📦 Requirements
---------------------------------------------------------
Required:
- None (standalone)

Optional:
- zf-notify (for test/demo feedback notifications)

If zf-notify is NOT running:
- zf-textui safely falls back to console prints
- No errors, no crashes

---------------------------------------------------------
📁 Resource Structure
---------------------------------------------------------
zf-textui/
  fxmanifest.lua
  client/
    main.lua
  html/
    index.html
    style.css
    app.js

---------------------------------------------------------
📄 fxmanifest.lua
---------------------------------------------------------
fx_version 'cerulean'
game 'gta5'
lua54 'yes'

name 'zf-textui'
author 'Virella.Scripts'
description 'Sleek no-blur TextUI for ZF scripts (exports + events)'
version '1.0.0'

ui_page 'html/index.html'

files {
  'html/index.html',
  'html/style.css',
  'html/app.js'
}

client_scripts {
  'client/main.lua'
}

---------------------------------------------------------
⚙️ Installation
---------------------------------------------------------
1) Place `zf-textui` into your resources folder:
   resources/[zf]/zf-textui

2) Ensure it in server.cfg:
   ensure zf-textui

3) (Optional) Ensure zf-notify for nicer feedback:
   ensure zf-notify

---------------------------------------------------------
🧠 What zf-textui Does
---------------------------------------------------------
zf-textui displays a floating on-screen prompt containing:
- Text (instruction)
- Optional key hint (E, F, etc.)
- Position on screen
- Accent/background/text colors

It does NOT handle interactions itself.
You decide what happens when the key is pressed.

---------------------------------------------------------
✨ Public Exports (Primary API)
---------------------------------------------------------
All exports are CLIENT-SIDE.

1) ShowTextUI
-------------
exports['zf-textui']:ShowTextUI(text, opts)

Examples:
exports['zf-textui']:ShowTextUI('Press E to open', { key = 'E' })

-- Shorthand key usage:
exports['zf-textui']:ShowTextUI('Press E to open', 'E')

Options (opts):
{
  key = 'E',                 -- optional key label
  position = 'left',         -- left | right | top | bottom
  accent = '#8b5cf6',        -- accent color
  bg = 'rgba(18,18,26,.92)', -- background color
  fg = '#ffffff'             -- text color
}

2) UpdateTextUI
---------------
exports['zf-textui']:UpdateTextUI(text, opts)

- Updates the current TextUI without hiding it
- If TextUI is not open, it will automatically Show

Example:
exports['zf-textui']:UpdateTextUI('Hold E to search', { key = 'E' })

3) HideTextUI
-------------
exports['zf-textui']:HideTextUI()

4) IsTextUIOpen
---------------
local open = exports['zf-textui']:IsTextUIOpen()
-- returns boolean

5) SetTextUITheme
-----------------
exports['zf-textui']:SetTextUITheme({
  accent = '#8b5cf6',
  bg = 'rgba(18,18,26,.92)',
  fg = '#ffffff'
})

Theme updates live if the UI is already open.

---------------------------------------------------------
📡 Optional Client Events
---------------------------------------------------------
These mirror the exports (useful for simple scripts):

TriggerEvent('zf-textui:client:show', text, opts)
TriggerEvent('zf-textui:client:hide')
TriggerEvent('zf-textui:client:update', text, opts)
TriggerEvent('zf-textui:client:theme', themeTable)

---------------------------------------------------------
🧪 Built-In Test Commands
---------------------------------------------------------
/zftxtcheck
- Shows current state:
  • NUI sanity
  • zf-notify state
  • Whether TextUI is open

/zftxt
- Auto demo:
  Show → Update → Hide (with notifications)

/zftxtshow [key] [text...]
- Example:
  /zftxtshow E Press E to interact

/zftxtupdate [key] [text...]
- Updates current text/key

/zftxthide
- Hides TextUI immediately

/zftxtpos <left|right|top|bottom>
- Changes position live

/zftxttheme <#hex>
- Changes accent color live

---------------------------------------------------------
📍 Proximity Demo (Best Real-World Test)
---------------------------------------------------------
/zftxtprox

What it does:
- Draws a marker in the world
- When you enter the radius:
  ✅ TextUI appears
- When you press E:
  ✅ Interaction feedback shown
- When you leave the radius:
  ✅ TextUI hides

Run the command again to disable the demo.

---------------------------------------------------------
🧩 Example Usage (Typical Pattern)
---------------------------------------------------------
CreateThread(function()
  while true do
    local ped = PlayerPedId()
    local coords = GetEntityCoords(ped)
    local dist = #(coords - vec3(145.94, -1042.85, 29.36))

    if dist < 2.0 then
      exports['zf-textui']:ShowTextUI('Open Bank', { key = 'E' })

      if IsControlJustPressed(0, 38) then -- E
        print('Bank opened!')
      end
    else
      exports['zf-textui']:HideTextUI()
    end

    Wait(0)
  end
end)

---------------------------------------------------------
🛠️ Troubleshooting
---------------------------------------------------------
❌ TextUI doesn’t show
✅ Ensure resource started:
   ensure zf-textui
✅ Check F8 for NUI errors
✅ Confirm ui_page path is correct
✅ Confirm html files exist

❌ Notifications not showing
✅ zf-notify is optional
✅ If not started, output goes to console instead

❌ Position incorrect
✅ Only valid positions:
   left | right | top | bottom
Invalid values default to left.

---------------------------------------------------------
⚡ Performance Notes
---------------------------------------------------------
- Client-only (no server load)
- NUI messages only sent when needed
- Proximity demo uses Wait(0); disable it when not testing

---------------------------------------------------------
📜 Credits / License
---------------------------------------------------------
Author: Virella.Scripts
ZF TextUI is part of the ZF ecosystem.
Free to use & customize for your server branding. ✨
