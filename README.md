# NovaUI

A lightweight Roblox UI library designed for broad compatibility (Studio, live Roblox, and common executors), with an Orion-style workflow and an original implementation.

## Quick Start

```lua
local NovaUI = loadstring(game:HttpGet("<your-raw-url-to-UI-file>"))()

local Window = NovaUI:CreateWindow({
    Title = "NovaUI Demo",
    SaveConfig = true,
    ConfigFolder = "NovaUI",
    ConfigName = "default",
    ToggleKey = Enum.KeyCode.RightShift,
    UseToggleButton = true
})

local MainTab = Window:CreateTab({Name = "Main", Icon = "⭐"})
local MainSection = MainTab:CreateSection({Name = "General"})

MainSection:AddLabel("Hello from NovaUI")
MainSection:AddButton({
    Name = "Notify",
    Callback = function()
        NovaUI:Notify({Title = "NovaUI", Content = "Button clicked", Duration = 3})
    end
})

NovaUI:Init()
```

---

## Core API

### Library

- `NovaUI:CreateWindow(cfg)`
- `NovaUI:Notify(cfg)`
- `NovaUI:Toggle(forceVisible)`
- `NovaUI:SetScale(scale)`
- `NovaUI:SetTheme(themePatch)`
- `NovaUI:SetAccent(color3)`
- `NovaUI:GetFlag(flagName)`
- `NovaUI:SaveConfig(name?)`
- `NovaUI:LoadConfig(name?, silent?)`
- `NovaUI:Init()`
- `NovaUI:Destroy()`

### Window

- `Window:CreateTab(cfg)`
- `Window:SaveConfig(name?)`
- `Window:LoadConfig(name?, silent?)`

### Tab

- `Tab:CreateSection(cfg)`

### Section Controls

- `Section:AddLabel(text)`
- `Section:AddParagraph(cfg)`
- `Section:AddButton(cfg)`
- `Section:AddToggle(cfg)`
- `Section:AddSlider(cfg)`
- `Section:AddDropdown(cfg)`
- `Section:AddTextbox(cfg)`
- `Section:AddBind(cfg)`
- `Section:AddColorPicker(cfg)`

---

## Config Reference

### `CreateWindow(cfg)`

Common fields:

- `Title` / `Name` (string)
- `Size` (UDim2)
- `Position` (UDim2)
- `UIScale` (number)
- `ToggleKey` (Enum.KeyCode)
- `UseToggleButton` (boolean)
- `ToggleButtonText` (string)
- `SaveConfig` (boolean)
- `ConfigFolder` (string)
- `ConfigName` (string)
- `MobileAutoSize` (boolean) — defaults to true
- `MobileSize` (UDim2)

### `CreateTab(cfg)`

- `Name` (string)
- `Icon` (string/emoji)

### `CreateSection(cfg)`

- `Name` (string)
- `Collapsed` (boolean)

---

## Control Examples

### Toggle

```lua
local t = MainSection:AddToggle({
    Name = "Enabled",
    Flag = "enabled",
    Default = false,
    Callback = function(v)
        print("Enabled:", v)
    end
})

print(t:Get())
t:Set(true)         -- fires callback
-- t:Set(true, true) -- silent set (no callback)
```

### Slider

```lua
local s = MainSection:AddSlider({
    Name = "Speed",
    Flag = "speed",
    Min = 0,
    Max = 100,
    Increment = 1,
    Default = 25,
    ValueName = "stud/s",
    Callback = function(v)
        print("Speed:", v)
    end
})

print(s:Get())
```

### Dropdown

```lua
local d = MainSection:AddDropdown({
    Name = "Mode",
    Flag = "mode",
    Options = {"A", "B", "C"},
    Default = "A",
    MaxDropdownHeight = 180,
    Callback = function(v)
        print("Mode:", v)
    end
})

print(d:Get())
d:Refresh({"A", "B", "C", "D"}, true)
```

### Textbox

```lua
MainSection:AddTextbox({
    Name = "Player Name",
    Flag = "player_name",
    Placeholder = "Type here",
    Callback = function(text, enterPressed)
        print("Text:", text, "Enter?", enterPressed)
    end
})
```

### Bind

```lua
MainSection:AddBind({
    Name = "Action Key",
    Flag = "action_key",
    Default = Enum.KeyCode.F,
    Callback = function(newKey)
        print("Bound to", newKey.Name)
    end,
    Pressed = function(key)
        print("Pressed", key.Name)
    end
})
```

### Color Picker

```lua
MainSection:AddColorPicker({
    Name = "Accent",
    Flag = "accent",
    ApplyAccent = true,
    Options = {
        Color3.fromRGB(104, 118, 255),
        Color3.fromRGB(68, 190, 119),
        Color3.fromRGB(255, 160, 80)
    },
    Callback = function(color)
        print("Selected color", color)
    end
})
```

---

## Theme and Flags

```lua
NovaUI:SetTheme({
    Accent = Color3.fromRGB(155, 92, 255),
    Background = Color3.fromRGB(20, 22, 30)
})

print("Enabled flag:", NovaUI:GetFlag("enabled"))
```

---

## Save / Load Config

```lua
-- Save current flags to NovaUI/<ConfigName>.json
local ok, err = Window:SaveConfig()
print("Saved?", ok, err)

-- Load and apply saved flags silently by default
local loaded, loadErr = Window:LoadConfig("default", true)
print("Loaded?", loaded, loadErr)
```

Notes:

- Config files are JSON and include `version`, `savedAt`, and `flags`.
- Config names are sanitized for safer filenames.
- Load applies values through registered control setters when available.

---

## Tips

- Call `NovaUI:Init()` once after building your UI.
- Use `NovaUI:Destroy()` before rebuilding manually if your script does repeated execution.
- Prefer `Flag` values on controls for easy persistence.
