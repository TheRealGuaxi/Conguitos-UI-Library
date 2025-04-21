# Conguitos UI Library - Documentation

## Overview
Conguitos UI Library is a Roblox UI framework designed for client scripts environments.

## Installation

```lua
local ConguitosUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/TheRealGuaxi/Conguitos-UI-Library/refs/heads/main/conguitosofuscado.lua"))()
```

## Basic Usage

```lua
-- Initialize
ConguitosUI:Init()

-- Create window
local window = ConguitosUI:CreateWindow("Window Title", UDim2.new(0, 350, 0, 400))

-- Add elements
window:AddLabel("Text label")
window:AddButton("Button", function() print("Clicked") end)
```

## API Reference

### Main Object

#### ConguitosUI:Init()
Initializes the library and displays a welcome notification.

#### ConguitosUI:Notify(title, text, duration)
Displays a notification.
- `title`: string
- `text`: string
- `duration`: number (seconds, default: 3)

Example:
```lua
ConguitosUI:Notify("Title", "Message text", 5)
```

### Window Creation

#### ConguitosUI:CreateWindow(title, size, position)
Creates and returns a window object.
- `title`: string
- `size`: UDim2 (optional, default: 300x350)
- `position`: UDim2 (optional, centered by default)

Returns a window object with various methods.

Example:
```lua
local window = ConguitosUI:CreateWindow(
    "Main Menu", 
    UDim2.new(0, 400, 0, 300),
    UDim2.new(0.5, -200, 0.5, -150)
)
```

### Window Methods

#### Window Control
- `window:Show()` - Shows the window with animation
- `window:Hide()` - Hides the window with animation
- `window:Toggle()` - Toggles visibility
- `window:Destroy()` - Destroys the window instance
- `window:SetHotkey(keyCode)` - Sets a new hotkey (default: RightShift)

Example:
```lua
window:SetHotkey(Enum.KeyCode.F4)
```

#### UI Elements

##### window:AddLabel(text, textColor)
Adds a text label.
- `text`: string
- `textColor`: Color3 (optional)

Returns a label methods object.

Label methods:
- `labelMethods:SetText(text)` - Updates label text
- `labelMethods:SetColor(color)` - Updates label color

Example:
```lua
local label = window:AddLabel("Status: Ready", Color3.fromRGB(0, 255, 0))
label:SetText("Status: Running")
```

##### window:AddButton(text, callback, color)
Adds a clickable button.
- `text`: string
- `callback`: function
- `color`: Color3 (optional)

Returns a button methods object.

Button methods:
- `buttonMethods:SetText(text)` - Updates button text
- `buttonMethods:SetColor(color)` - Updates button color

Example:
```lua
local button = window:AddButton("Execute", function()
    -- Code to execute
    print("Button clicked")
end, Color3.fromRGB(0, 120, 215))
```

##### window:AddToggle(text, defaultValue, callback)
Adds a toggle switch.
- `text`: string
- `defaultValue`: boolean
- `callback`: function that receives the current value

Returns a toggle methods object.

Toggle methods:
- `toggleMethods:GetValue()` - Returns current boolean value
- `toggleMethods:SetValue(value)` - Sets toggle state and triggers callback

Example:
```lua
local toggle = window:AddToggle("Auto Farm", false, function(value)
    if value then
        -- Start auto farm
        startAutoFarm()
    else
        -- Stop auto farm
        stopAutoFarm()
    end
end)

-- Get toggle state elsewhere in code
if toggle:GetValue() then
    -- Do something
end

-- Set toggle programmatically
toggle:SetValue(true)
```

##### window:AddSection(title)
Adds a section header.
- `title`: string

Example:
```lua
window:AddSection("Player Options")
```

##### window:AddSeparator(height, color)
Adds a horizontal line separator.
- `height`: number (optional, default: 1)
- `color`: Color3 (optional, default: dark gray)

Example:
```lua
window:AddSeparator(2, Color3.fromRGB(60, 60, 60))
```

##### window:SetFooter(text)
Sets a footer text at the bottom of the window.
- `text`: string

Example:
```lua
window:SetFooter("Created by Username | v1.0.0")
```

## Advanced Usage

### Multiple Windows

```lua
local mainWindow = ConguitosUI:CreateWindow("Main Menu")
local settingsWindow = ConguitosUI:CreateWindow("Settings", UDim2.new(0, 300, 0, 200), UDim2.new(0.7, 0, 0.5, -100))

-- Hide secondary window initially
settingsWindow:Hide()

-- Add button to open settings
mainWindow:AddButton("Open Settings", function()
    settingsWindow:Show()
end)

-- Add button to close settings
settingsWindow:AddButton("Close", function()
    settingsWindow:Hide()
end, Color3.fromRGB(200, 50, 50))
```

### State Management Example

```lua
-- Global state object
local state = {
    autoFarm = false,
    walkSpeed = 16,
    jumpPower = 50
}

-- Create UI
local window = ConguitosUI:CreateWindow("Player Mods")

-- Add toggles with state management
local autoFarmToggle = window:AddToggle("Auto Farm", state.autoFarm, function(value)
    state.autoFarm = value
    updateAutoFarm()
end)

-- Function to update based on state
function updateAutoFarm()
    if state.autoFarm then
        -- Implementation of auto farm
        ConguitosUI:Notify("Auto Farm", "Started", 2)
        -- Auto farm code...
    else
        -- Stop auto farm
        ConguitosUI:Notify("Auto Farm", "Stopped", 2)
        -- Stop auto farm code...
    end
end

-- Using the state in your main loop
while task.wait(1) do
    if state.autoFarm then
        -- Do farming logic
    end
end
```

### Creating a Tab-Like Interface

```lua
local mainWindow = ConguitosUI:CreateWindow("Game Hub", UDim2.new(0, 450, 0, 350))

-- Create "tab" containers (frames)
local pages = {
    main = {},
    teleport = {},
    settings = {}
}

-- Current active page
local currentPage = "main"

-- Function to switch pages
local function switchPage(page)
    -- Hide all elements from other pages
    for pageName, elements in pairs(pages) do
        for _, element in ipairs(elements) do
            element.Visible = (pageName == page)
        end
    end
    currentPage = page
}

-- Tab buttons
mainWindow:AddSection("Navigation")

mainWindow:AddButton("Main", function()
    switchPage("main")
end)

mainWindow:AddButton("Teleport", function()
    switchPage("teleport")
end)

mainWindow:AddButton("Settings", function()
    switchPage("settings")
end)

mainWindow:AddSeparator(3)

-- Add elements to specific pages
local function addToPage(page, element)
    table.insert(pages[page], element)
    element.Visible = (currentPage == page)
    return element
end

-- Main page elements
local mainLabel = mainWindow:AddLabel("Welcome to the main page")
addToPage("main", mainLabel)

-- Teleport page elements
local teleportLabel = mainWindow:AddLabel("Teleport options")
addToPage("teleport", teleportLabel)

local teleportButton = mainWindow:AddButton("Go to Spawn", function()
    -- Teleport code
end)
addToPage("teleport", teleportButton)

-- Settings page elements
local settingsLabel = mainWindow:AddLabel("Adjust settings")
addToPage("settings", settingsLabel)

local settingsToggle = mainWindow:AddToggle("Dark Mode", false, function() end)
addToPage("settings", settingsToggle)

-- Initially show main page
switchPage("main")
```

## Implementation Examples

### Auto Farm Script

```lua
-- Load the UI library
local ConguitosUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/TheRealGuaxi/Conguitos-UI-Library/refs/heads/main/conguitosofuscado.lua"))()
ConguitosUI:Init()

-- Create window
local mainWindow = ConguitosUI:CreateWindow("Farm Script")

-- Variables
local farming = false
local farmConnection = nil

-- Functions
local function startFarming()
    if farmConnection then return end
    
    farming = true
    farmConnection = game:GetService("RunService").Heartbeat:Connect(function()
        -- Farm logic here
        for _, v in pairs(workspace.Collectibles:GetChildren()) do
            if v:IsA("Part") then
                v.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
            end
        end
    end)
    
    ConguitosUI:Notify("Farm", "Auto farm started", 2)
end

local function stopFarming()
    if farmConnection then
        farmConnection:Disconnect()
        farmConnection = nil
    end
    
    farming = false
    ConguitosUI:Notify("Farm", "Auto farm stopped", 2)
end

-- Create UI elements
mainWindow:AddLabel("Auto Farm Configuration")

mainWindow:AddSection("Farm Options")

local farmToggle = mainWindow:AddToggle("Enable Auto Farm", false, function(value)
    if value then
        startFarming()
    else
        stopFarming()
    end
end)

mainWindow:AddButton("Collect All Items Now", function()
    -- Collect all items once
    for _, v in pairs(workspace.Collectibles:GetChildren()) do
        if v:IsA("Part") then
            v.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
        end
    end
    
    ConguitosUI:Notify("Collect", "Collected all items", 2)
end)

-- Clean up on script end
game.Players.LocalPlayer.Character.Humanoid.Died:Connect(function()
    farmToggle:SetValue(false)
end)

mainWindow:SetFooter("Farm Script v1.0")
```

### ESP Script Example

```lua
local ConguitosUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/TheRealGuaxi/Conguitos-UI-Library/refs/heads/main/conguitosofuscado.lua"))()
ConguitosUI:Init()

-- Create UI
local window = ConguitosUI:CreateWindow("ESP Settings")

-- ESP Settings
local espSettings = {
    enabled = false,
    showNames = true,
    showDistance = true,
    showHealth = true,
    teamCheck = true,
    textSize = 14,
    textColor = Color3.fromRGB(255, 255, 255),
    boxColor = Color3.fromRGB(255, 0, 0)
}

-- ESP Functions
local espObjects = {}

local function createESP(player)
    if player == game.Players.LocalPlayer then return end
    
    local esp = Drawing.new("Text")
    esp.Visible = espSettings.enabled
    esp.Size = espSettings.textSize
    esp.Color = espSettings.textColor
    esp.Center = true
    esp.Outline = true
    
    espObjects[player.Name] = esp
end

local function updateESP()
    for playerName, esp in pairs(espObjects) do
        local player = game.Players:FindFirstChild(playerName)
        if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local character = player.Character
            local rootPart = character.HumanoidRootPart
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            
            local screenPos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(rootPart.Position)
            
            if onScreen then
                esp.Position = Vector2.new(screenPos.X, screenPos.Y - 30)
                
                local text = ""
                if espSettings.showNames then
                    text = text .. player.Name .. "\n"
                end
                
                if espSettings.showDistance then
                    local distance = math.floor((rootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude)
                    text = text .. "[" .. distance .. " studs]\n"
                end
                
                if espSettings.showHealth and humanoid then
                    text = text .. "HP: " .. math.floor(humanoid.Health) .. "/" .. math.floor(humanoid.MaxHealth)
                end
                
                esp.Text = text
                esp.Visible = espSettings.enabled
            else
                esp.Visible = false
            end
        else
            esp.Visible = false
            if not player then
                esp:Remove()
                espObjects[playerName] = nil
            end
        end
    end
end

-- Add UI elements
window:AddSection("ESP Options")

window:AddToggle("Enable ESP", espSettings.enabled, function(value)
    espSettings.enabled = value
    for _, esp in pairs(espObjects) do
        esp.Visible = value
    end
end)

window:AddToggle("Show Names", espSettings.showNames, function(value)
    espSettings.showNames = value
end)

window:AddToggle("Show Distance", espSettings.showDistance, function(value)
    espSettings.showDistance = value
end)

window:AddToggle("Show Health", espSettings.showHealth, function(value)
    espSettings.showHealth = value
end)

window:AddToggle("Team Check", espSettings.teamCheck, function(value)
    espSettings.teamCheck = value
end)

-- Initialize ESP for existing players
for _, player in pairs(game.Players:GetPlayers()) do
    createESP(player)
end

-- Handle player joining
game.Players.PlayerAdded:Connect(function(player)
    createESP(player)
end)

-- Update loop
game:GetService("RunService").RenderStepped:Connect(updateESP)

-- Clean up on script end
game.Players.LocalPlayer.Character.Humanoid.Died:Connect(function()
    for _, esp in pairs(espObjects) do
        esp:Remove()
    end
    espObjects = {}
end)
``` 
