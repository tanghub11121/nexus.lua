local RandomFps = math.random(60, 120)
setfpscap(RandomFps)
local decalsyeeted = true -- Leaving this on makes games look shitty but the fps goes up by at least 20.
local g = game
local w = g.Workspace
local l = g.Lighting
local t = w.Terrain
t.WaterWaveSize = 0
t.WaterWaveSpeed = 0
t.WaterReflectance = 0
t.WaterTransparency = 0
l.GlobalShadows = false
l.FogEnd = 9e9
l.Brightness = 0
settings().Rendering.QualityLevel = "Level01"
for i, v in pairs(g:GetDescendants()) do
    if v:IsA("Part") or v:IsA("Union") or v:IsA("CornerWedgePart") or v:IsA("TrussPart") then
        v.Material = "Plastic"
        v.Reflectance = 0
    elseif v:IsA("Decal") or v:IsA("Texture") and decalsyeeted then
        v.Transparency = 1
    elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
        v.Lifetime = NumberRange.new(0)
    elseif v:IsA("Explosion") then
        v.BlastPressure = 1
        v.BlastRadius = 1
    elseif v:IsA("Fire") or v:IsA("SpotLight") or v:IsA("Smoke") then
        v.Enabled = false
    elseif v:IsA("MeshPart") then
        v.Material = "Plastic"
        v.Reflectance = 0
        v.TextureID = 10385902758728957
    end
end
for i, e in pairs(l:GetChildren()) do
    if e:IsA("BlurEffect") or e:IsA("SunRaysEffect") or e:IsA("ColorCorrectionEffect") or e:IsA("BloomEffect") or e:IsA("DepthOfFieldEffect") then
        e.Enabled = false
    end
end

local TweenService = game:GetService("TweenService")

if game.CoreGui:FindFirstChild("ImageButton") then
    game.CoreGui:FindFirstChild("ImageButton"):Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
local ImageButton = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")
local ClickSound = Instance.new("Sound")

ScreenGui.Name = "ImageButton"
ScreenGui.Parent = game.CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

ImageButton.Parent = ScreenGui
ImageButton.BackgroundColor3 = Color3.fromRGB(255, 182, 193)  -- 
ImageButton.BorderSizePixel = 10  -- ขอบของปุ่ม
ImageButton.BorderColor3 = Color3.fromRGB(255, 182, 193)  -- 
ImageButton.Position = UDim2.new(0.120833337, 0, 0.0952890813, 0)
ImageButton.Size = UDim2.new(0, 55, 0, 55)  
ImageButton.Draggable = true
ImageButton.Image = "rbxassetid://87567481752287"

UICorner.Parent = ImageButton
UICorner.CornerRadius = UDim.new(1, 0)  -- ทำให้ปุ่มเป็นวงกลม

ClickSound.Parent = ImageButton
ClickSound.SoundId = "rbxassetid://130785805"
ClickSound.Volume = 1

local function playClickAnimation()
    local originalSize = ImageButton.Size
    local TweenSizeUp = TweenService:Create(ImageButton, TweenInfo.new(0.1), {Size = UDim2.new(0, 55, 0, 55)})
    local TweenSizeDown = TweenService:Create(ImageButton, TweenInfo.new(0.1), {Size = originalSize})

    TweenSizeUp:Play()
    TweenSizeUp.Completed:Connect(function()
        TweenSizeDown:Play()
    end)
end

ImageButton.MouseButton1Down:Connect(function()
    ClickSound:Play()    
    playClickAnimation()    
    game:GetService("VirtualInputManager"):SendKeyEvent(true, "LeftControl", false, game)
    game:GetService("VirtualInputManager"):SendKeyEvent(false, "LeftControl", false, game)
end)
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "NEXUS HUB",
    SubTitle = "Fisch Beta By Config And Xesonz",
    TabWidth = 160,
    Size = UDim2.fromOffset(560, 400),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Rose",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

--Fluent provides Lucide Icons https://lucide.dev/icons/ for the tabs, icons are optional
local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

do
    Fluent:Notify({
        Title = "NEXUS HUB",
        Content = "ติ๋มๆจิ้มหี",
        SubContent = "SubContent", -- Optional
        Duration = 3 -- Set to nil to make the notification not disappear
    })

local Farming = Tabs.Main:AddSection("Farming")

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local savedCFrame = nil

Tabs.Main:AddButton({
    Title = "Save CFrame Position",
    Description = "คัดลอกตำแหน่งปัจจุบันและเก็บค่า",
    Callback = function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
           savedCFrame = LocalPlayer.Character.HumanoidRootPart.CFrame
        end        
    end
})

local Toggle = Tabs.Main:AddToggle("Auto Farm Toggle", {Title = "Enable Auto Farm", Description = "ออโต้ฟาร์มให้บันทึกตำแหน่งที่จะฟาร์มก่อน [ Save CFrame Position ]", Default = false })
Toggle:OnChanged(function(Value)
    _G.Auto_Farm = Value
    autocastposition = Value
end)
_G.EnableFishing = true
local args = {
    [1] = 59.600000000000016,
    [2] = 1
}
local function CastFishingRod()
    while _G.EnableFishing do
        local player = game:GetService("Players").LocalPlayer
        local rod = player.Character:FindFirstChild("Rod")
        if rod and rod:FindFirstChild("events") and rod.events:FindFirstChild("cast") then
            rod.events.cast:FireServer(unpack(args))
        end
        task.wait(1) 
    end
end
task.spawn(CastFishingRod)

spawn(function()
    while task.wait() do
        if _G.Auto_Farm and savedCFrame then
            pcall(function()
                local player = game.Players.LocalPlayer
                if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    player.Character.HumanoidRootPart.CFrame = savedCFrame
                    task.spawn(function()
                        while task.wait() do
                            pcall(function()
                                if autocastposition and Teleposi then
                                    teleportToSavedPosition()
                                end
                                -- ย้ายคันเบ็ดจากกระเป๋าเป้ไปที่ตัวละคร
                                for _, tool in pairs(player.Backpack:GetChildren()) do
                                    if string.find(tool.Name, "Rod") then
                                        tool.Parent = player.Character
                                        task.wait(0.2)
                                    end
                                end
                                for _, equippedTool in pairs(player.Character:GetChildren()) do
                                    if string.find(equippedTool.Name, "Rod") then
                                        if player.PlayerGui:FindFirstChild("reel") then
                                            game:GetService("ReplicatedStorage").events.reelfinished:FireServer(100, true)
                                        elseif player.PlayerGui:FindFirstChild("shakeui") then
                                            for _, button in pairs(player.PlayerGui.shakeui.safezone:GetChildren()) do
                                                if button.Name == "button" then
                                                    button.Size = UDim2.new(0, 10000, 0, 10000)
                                                    button.Position = UDim2.new(-2, 0, -5, 0)
                                                    button.Transparency = 1
                                                    task.wait(math.random(1, 2))
                                                    game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
                                                    task.wait(1)
                                                    game:GetService("VirtualUser"):Button1Up(Vector2.new(1280, 672))
                                                end
                                            end
                                        elseif not equippedTool.values.casted.Value then
                                            local args = {
                                                [1] = 100
                                            }
                                            equippedTool.events.cast:FireServer(unpack(args))
                                        end
                                    end
                                end
                            end)
                        end
                    end)
                end
            end)
        end
    end
end)

    _G.SellAllToggle = false 

local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Enable Auto SellAll", Description = "ขายปลาทั้งหมดในตัว", Default = false })

Toggle:OnChanged(function()
    _G.SellAllToggle = Options.MyToggle.Value
    if _G.SellAllToggle then
        print("Sell All Enabled")
    else
        print("Sell All Disabled")
    end
end)

Options.MyToggle:SetValue(false)

spawn(function()
    while task.wait() do
        if _G.SellAllToggle then
            local merchant = workspace.world.npcs:FindFirstChild("Marc Merchant")
            if merchant and merchant:FindFirstChild("merchant") and merchant.merchant:FindFirstChild("sellall") then
                merchant.merchant.sellall:InvokeServer()
                print("Sell all invoked.")
            else
                warn("Marc Merchant or sellall not found!")
            end
        end
    end
end)

local Teleport = Tabs.Main:AddSection("Teleport")
    
local Islandlist = {
    "Mousewood",
    "Gamma Grotto",
    "Keepet Altar",
    "Enchant",
    "Mushgrove Elevator",
    "Roslit",
    "Snowcap",
    "Statue",
    "Sunstone",
    "Vertigo",
    "Deep",
    "Forsaken",
    "The Depths",
    "Volcano"
}
local SelectIsland = Tabs.Main:AddDropdown("Dropdown", {
    Title = "Select Island",
    Description = "เลือกเกาะ",
    Values = Islandlist,
    Multi = false,
    Default = nil,
})
SelectIsland:OnChanged(function(Value)
    _G.SelectIsland = Value
end)
Tabs.Main:AddButton({
    Title = "Teleport To Island",
    Description = "วาร์ปไปยังเกาะ [ Select Teleport Rod ]",
    Callback = function()
        if _G.SelectIsland == "Mousewood" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(409.2666931152344, 152.432373046875, 251.96022033691406)
        elseif _G.SelectIsland == "Gamma Grotto" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2231.98828125, -804.1823120117188, 1050.376708984375)
        elseif _G.SelectIsland == "Keepet Altar" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1258.4993896484375, -805.26171875, -285.1141357421875)
        elseif _G.SelectIsland == "Enchant" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1308.7430419921875, -802.427001953125, -81.36365509033203)
        elseif _G.SelectIsland == "Mushgrove" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2735.152099609375, 139.45065307617188, -848.3896484375)
        elseif _G.SelectIsland == "Roslit" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1446.406005859375, 134.9540557861328, 702.18408203125)
        elseif _G.SelectIsland == "Snowcap" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(2622.982666015625, 149.70736694335938, 2406.6318359375)
        elseif _G.SelectIsland == "Statue" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-21.4614315032959, 136.5426025390625, -1128.92724609375)
        elseif _G.SelectIsland == "Sunstone" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-950.8275146484375, 141.89602661132812, -1110.8980712890625)
        elseif _G.SelectIsland == "Vertigo" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-125.87995147705078, -492.8369140625, 1003.998046875)
        elseif _G.SelectIsland == "Deep" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1652.1475830078125, -185.40029907226562, -2878.5908203125)
        elseif _G.SelectIsland == "Forsaken" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2482.68603515625, 135.99893188476562, 1580.337646484375)
        elseif _G.SelectIsland == "The Depths" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(985.5209350585938, -700.447998046875, 1254.260986328125)
        elseif _G.SelectIsland == "Volcano" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1922.143310546875, 161.11578369140625, 329.6386413574219)
        end
    end
})
local Rodlist = {
    "Trident Rod",
    "Steady Rod / Rapid Rod / Fortune Rod",
    "King Rod",
    "Magnet Rod",
    "Nocturnal Rod",
    "Lucky Rod / Plastic Rod / Training Rod / Long Rod",
    "Reinforced Rod"
}
local SelectRod = Tabs.Main:AddDropdown("Dropdown", {
    Title = "Select Teleport Rod",
    Description = "เลือกร้านจุดขายเบ็ด",
    Values = Rodlist,
    Multi = false,
    Default = nil,
})
SelectRod:OnChanged(function(Value)
    _G.SelectRod = Value
end)
Tabs.Main:AddButton({
    Title = "Teleport To Rod",
    Description = "วาร์ปไปจุดขายเบ็ด [ Select Teleport Rod ]",
    Callback = function()
                if _G.SelectRod == "Trident Rod" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1482.7115478515625, -226.02447509765625, -2201.41015625)
        elseif _G.SelectRod == "Steady Rod / Rapid Rod / Fortune Rod" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1505.1763916015625, 141.85292053222656, 762.0928344726562)
        elseif _G.SelectRod == "King Rod" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1372.800537109375, -803.4769897460938, -299.8193359375)
        elseif _G.SelectRod == "Magnet Rod" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-197.21255493164062, 132.50001525878906, 1929.2528076171875)
        elseif _G.SelectRod == "Nocturnal Rod" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-148.67605590820312, -515.2993774414062, 1143.0567626953125)
        elseif _G.SelectRod == "Lucky Rod / Plastic Rod / Training Rod / Long Rod" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(449.2977600097656, 150.50001525878906, 225.24842834472656)
        elseif _G.SelectRod == "Reinforced Rod" then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-981.8455810546875, -244.9109344482422, -2697.35595703125)
        end
    end
})
    local Other = Tabs.Main:AddSection("Other")

    
    


    local Colorpicker = Tabs.Main:AddColorpicker("Colorpicker", {
        Title = "Colorpicker",
        Default = Color3.fromRGB(96, 205, 255)
    })

    Colorpicker:OnChanged(function()
        print("Colorpicker changed:", Colorpicker.Value)
    end)
    
    Colorpicker:SetValueRGB(Color3.fromRGB(0, 255, 140))



    local TColorpicker = Tabs.Main:AddColorpicker("TransparencyColorpicker", {
        Title = "Colorpicker",
        Description = "but you can change the transparency.",
        Transparency = 0,
        Default = Color3.fromRGB(96, 205, 255)
    })

    TColorpicker:OnChanged(function()
        print(
            "TColorpicker changed:", TColorpicker.Value,
            "Transparency:", TColorpicker.Transparency
        )
    end)



    local Keybind = Tabs.Main:AddKeybind("Keybind", {
        Title = "KeyBind",
        Mode = "Toggle", -- Always, Toggle, Hold
        Default = "LeftControl", -- String as the name of the keybind (MB1, MB2 for mouse buttons)

        -- Occurs when the keybind is clicked, Value is `true`/`false`
        Callback = function(Value)
            print("Keybind clicked!", Value)
        end,

        -- Occurs when the keybind itself is changed, `New` is a KeyCode Enum OR a UserInputType Enum
        ChangedCallback = function(New)
            print("Keybind changed!", New)
        end
    })

    -- OnClick is only fired when you press the keybind and the mode is Toggle
    -- Otherwise, you will have to use Keybind:GetState()
    Keybind:OnClick(function()
        print("Keybind clicked:", Keybind:GetState())
    end)

    Keybind:OnChanged(function()
        print("Keybind changed:", Keybind.Value)
    end)

    task.spawn(function()
        while true do
            wait(1)

            -- example for checking if a keybind is being pressed
            local state = Keybind:GetState()
            if state then
                print("Keybind is being held down")
            end

            if Fluent.Unloaded then break end
        end
    end)

    Keybind:SetValue("MB2", "Toggle") -- Sets keybind to MB2, mode to Hold


    local Input = Tabs.Main:AddInput("Input", {
        Title = "Input",
        Default = "Default",
        Placeholder = "Placeholder",
        Numeric = false, -- Only allows numbers
        Finished = false, -- Only calls callback when you press enter
        Callback = function(Value)
            print("Input changed:", Value)
        end
    })

    Input:OnChanged(function()
        print("Input updated:", Input.Value)
    end)
end

SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

SaveManager:IgnoreThemeSettings()

SaveManager:SetIgnoreIndexes({})

InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)


Window:SelectTab(1)

Fluent:Notify({
    Title = "NEXUS HUB",
    Content = "สำเร็จแล้วครับ!",
    Duration = 8
})

SaveManager:LoadAutoloadConfig()
