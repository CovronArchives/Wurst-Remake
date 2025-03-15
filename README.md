![Wurst UI Screenshot](https://raw.githubusercontent.com/CovronArchives/Wurst-Remake/main/IMG_3915.png)

A Roblox UI Library by me.
# What is Wurst?
Wurst is an minecraft hack, made by 7grandad

I made this UI Library cause nobody has about wurst.

Since im bad at doing GUI's, this might not look good, nor look like Wurst at all,

Although, it does serve some accuracy.

Feel free to change the source or take a look at it if you would like to.
# Credits
All credits go to 7grandad for making wurst and other people who had helped or was in the making of wurst.
# Script
The script goes under here. You can add toggles.
```lua
local borderbutton = 2

makefolder("wurstfolder")
makefolder("wurstfolder/assets")
writefile("wurstfolder/assets/wurstlogo.png", game:HttpGet("https://raw.githubusercontent.com/CovronArchives/Wurst-Remake/refs/heads/main/IMG_3915.png", true))

local players = game:GetService("Players")
local player = players.LocalPlayer
local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "WurstUILib"

local gui = Instance.new("ScreenGui")
gui.Parent = game.CoreGui

local wurstlogo = Instance.new("ImageLabel")
wurstlogo.Parent = gui
wurstlogo.Size = UDim2.new(0, 150, 0, 150)
wurstlogo.Position = UDim2.new(0, 500, 0, -50)
wurstlogo.BackgroundTransparency = 1
wurstlogo.Image = getcustomasset("wurstfolder/assets/wurstlogo.png")
wurstlogo.Active = true
wurstlogo.Visible = false
wurstlogo.Draggable = true

local mobilemode = Instance.new("TextButton")
mobilemode.Parent = gui
mobilemode.Size = UDim2.new(0, 70, 0, 20)
mobilemode.Position = UDim2.new(0, 30, 0, 30)
mobilemode.Active = true
mobilemode.Draggable = false
mobilemode.Text = "close"
mobilemode.Font = Enum.Font.SourceSansBold
mobilemode.TextSize = 20
mobilemode.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
mobilemode.MouseButton1Click:Connect(function()
    gui:Destroy()
end)
local k = Instance.new("UICorner")
k.Parent = mobilemode

local destroy = Instance.new("TextButton")
destroy.Parent = gui
destroy.Size = UDim2.new(0, 70, 0, 20)
destroy.Position = UDim2.new(0, 30, 0, 60)
destroy.Active = true
destroy.Draggable = false
destroy.Text = "disable logo"
destroy.Font = Enum.Font.SourceSansBold
destroy.TextSize = 15
destroy.BackgroundColor3 = Color3.fromRGB(255, 100, 0)
destroy.MouseButton1Click:Connect(function()
    destroy:Destroy()
    wurstlogo:Destroy()
end)
local d = Instance.new("UICorner")
d.Parent = destroy

local main_frame = Instance.new("Frame", gui)
main_frame.Size = UDim2.new(0.4, 0, 0.85, 0)
main_frame.Position = UDim2.new(0.3, 0, 0.075, 0)
main_frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
main_frame.Transparency = 1
main_frame.BorderSizePixel = 2
main_frame.BorderColor3 = Color3.fromRGB(60, 60, 60)

local search_box = Instance.new("TextBox", main_frame)
search_box.Size = UDim2.new(1, 0, 0.1, 0)
search_box.Position = UDim2.new(0, 7, 0, 0)
search_box.BackgroundTransparency = 1
search_box.PlaceholderText = "Search"
search_box.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
search_box.TextColor3 = Color3.fromRGB(255, 255, 255)
search_box.Font = Enum.Font.Arcade
search_box.TextSize = 18
search_box.TextXAlignment = Enum.TextXAlignment.Left

local scroll_frame = Instance.new("ScrollingFrame", main_frame)
scroll_frame.Size = UDim2.new(1, 0, 0.9, 0)
scroll_frame.Position = UDim2.new(0, 0, 0.1, 0)
scroll_frame.CanvasSize = UDim2.new(0, 0, 10, 0)
scroll_frame.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
scroll_frame.BackgroundTransparency = 1
scroll_frame.BorderSizePixel = 0
scroll_frame.ScrollBarThickness = 6

local function create_toggle(options)
    local button = Instance.new("TextButton")
    button.TextXAlignment = Enum.TextXAlignment.Left
    button.BackgroundTransparency = 0.5
    button.Size = UDim2.new(0.3, 0, 0, 30)
    button.Text = options.Name
    button.Font = Enum.Font.Arcade
    button.TextSize = 16
    button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.BorderSizePixel = borderbutton
    button.BorderColor3 = Color3.fromRGB(40, 40, 40)

    local active = options.CurrentValue
    button.BackgroundColor3 = active and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(70, 70, 70)

    button.MouseButton1Click:Connect(function()
        active = not active
        button.BackgroundColor3 = active and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(70, 70, 70)
        
        if options.Callback then
            if active then
                options.Callback(true)
            else
                options.Callback(false)
            end
        end
    end)

    return button
end

local function organize_toggles(options)
    local row = 0
    local column = 0
    local button_spacing = 10
    local button_width = (main_frame.AbsoluteSize.X - 4 * button_spacing) / 3
    local position_y = 0

    for _, toggle_data in ipairs(options) do
        if column == 3 then
            row = row + 1
            column = 0
            position_y = position_y + 30
        end

        local position_x = column * (button_width + button_spacing) + button_spacing
        local button = create_toggle(toggle_data)
        button.Position = UDim2.new(0, position_x, 0, position_y)
        button.Size = UDim2.new(0, button_width, 0, 20)
        button.Parent = scroll_frame

        column = column + 1
    end
    
    scroll_frame.CanvasSize = UDim2.new(0, 0, 0, position_y + 50)
end

local toggles = {}

local function add_toggle(name, current_value, flag, callback)
    table.insert(toggles, {
        Name = name,
        CurrentValue = current_value,
        Flag = flag,
        Callback = callback
    })
end

add_toggle("AutoFarm", false, "AutoFarmFlag", function(value)
    if value then
        print("AutoFarm is Enabled")
    else
        print("AutoFarm is Disabled")
    end
end)

add_toggle("AutoMine", false, "AutoMineFlag", function(value)
    if value then
        print("AutoMine is Enabled")
    else
        print("AutoMine is Disabled")
    end
end)

add_toggle("FastLadder", false, "FastLadderFlag", function(value)
    if value then
        print("FastLadder is Enabled")
    else
        print("FastLadder is Disabled")
    end
end)

add_toggle("Flight", false, "FlightFlag", function(value)
    if value then
        print("Flight is Enabled")
    else
        print("Flight is Disabled")
    end
end)

add_toggle("ChestESP", false, "ChestESPFlag", function(value)
    if value then
        print("ChestESP is Enabled")
    else
        print("ChestESP is Disabled")
    end
end)

add_toggle("AutoFarm", false, "AutoFarmFlag", function(value)
    if value then
        print("AutoFarm is Enabled")
    else
        print("AutoFarm is Disabled")
    end
end)

add_toggle("AutoMine", false, "AutoMineFlag", function(value)
    if value then
        print("AutoMine is Enabled")
    else
        print("AutoMine is Disabled")
    end
end)

add_toggle("FastLadder", false, "FastLadderFlag", function(value)
    if value then
        print("FastLadder is Enabled")
    else
        print("FastLadder is Disabled")
    end
end)

add_toggle("Wurst", false, "Logo", function(value)
    wurstlogo.Visible = value
end)

organize_toggles(toggles)

search_box:GetPropertyChangedSignal("Text"):Connect(function()
    local search_text = search_box.Text:lower()
    for _, button in ipairs(scroll_frame:GetChildren()) do
        if button:IsA("TextButton") then
            button.Visible = search_text == "" or string.find(button.Text:lower(), search_text) ~= nil
        end
    end
end)
```
