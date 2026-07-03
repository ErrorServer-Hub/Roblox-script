-- [[ ERR_SRV // OMNI-FRAMEWORK V27 // FIXED HITBOX & CLICK-SLOTS ]]
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local Workspace = game:GetService("Workspace")
local LP = Players.LocalPlayer
local Camera = Workspace.CurrentCamera

if CoreGui:FindFirstChild("OmniV27") then CoreGui.OmniV27:Destroy() end

local Config = {
    Speed = 16, Noclip = false, InfJump = false, GodMode = false,
    Fly = false, FlySpeed = 50, Hitbox = false,
    IsRecording = false, IsPlaying = false, CurrentSlot = 1
}
local Slots = {}
for i = 1, 20 do Slots[i] = {} end -- Инициализация 20 слотов

local OldHealth = 100
local TInfo = TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
local SpringInfo = TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.Out)

-- [[ 1. FIXED NATIVE UI SYSTEM ]]
local SG = Instance.new("ScreenGui", CoreGui) SG.Name = "OmniV27"
local Main = Instance.new("Frame", SG)
Main.Size = UDim2.new(0, 500, 0, 310)
Main.Position = UDim2.new(0.5, -250, 0.5, -155)
Main.BackgroundColor3 = Color3.fromRGB(12, 12, 15)
Main.ClipsDescendants = true Main.Active = true Main.Draggable = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 8)

local Bar = Instance.new("Frame", Main) Bar.Size = UDim2.new(1, 0, 0, 38) Bar.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
Instance.new("UICorner", Bar).CornerRadius = UDim.new(0, 8)

local Tl = Instance.new("TextLabel", Bar) Tl.Size = UDim2.new(0.7, 0, 1, 0) Tl.Position = UDim2.new(0, 15, 0, 0)
Tl.BackgroundTransparency = 1 Tl.Text = "ERR_SRV // OMNI V27" Tl.TextColor3 = Color3.fromRGB(255, 45, 45) Tl.Font = Enum.Font.Code Tl.TextSize = 13 Tl.TextXAlignment = Enum.TextXAlignment.Left

local function MakeWindowBtn(txt, pos, color, cb)
    local B = Instance.new("TextButton", Bar) B.Size = UDim2.new(0, 26, 0, 22) B.Position = pos
    B.BackgroundColor3 = color B.Text = txt B.TextColor3 = Color3.fromRGB(255,255,255) B.Font = Enum.Font.SourceSansBold B.TextSize = 12
    Instance.new("UICorner", B).CornerRadius = UDim.new(0, 4)
    B.MouseButton1Click:Connect(cb)
end

MakeWindowBtn("X", UDim2.new(1, -36, 0, 8), Color3.fromRGB(160, 40, 40), function() SG:Destroy() end)
local Mini = false
MakeWindowBtn("_", UDim2.new(1, -68, 0, 8), Color3.fromRGB(45, 45, 55), function()
    Mini = not Mini
    TweenService:Create(Main, TInfo, {Size = Mini and UDim2.new(0, 500, 0, 38) or UDim2.new(0, 500, 0, 310)}):Play()
end)

local SB = Instance.new("Frame", Main) SB.Size = UDim2.new(0, 130, 1, -38) SB.Position = UDim2.new(0, 0, 0, 38) SB.BackgroundColor3 = Color3.fromRGB(15, 15, 19)
local SBL = Instance.new("UIListLayout", SB) SBL.Padding = UDim.new(0, 4) SBL.HorizontalAlignment = Enum.HorizontalAlignment.Center

local Cont = Instance.new("Frame", Main) Cont.Size = UDim2.new(1, -145, 1, -50) Cont.Position = UDim2.new(0, 140, 0, 45) Cont.BackgroundTransparency = 1

local Pages = {}
local function CreatePage(id)
    local Sc = Instance.new("ScrollingFrame", Cont) Sc.Size = UDim2.new(1, 0, 1, 0) Sc.BackgroundTransparency = 1 Sc.BorderSizePixel = 0
    Sc.CanvasSize = UDim2.new(0, 0, 0, 380) Sc.ScrollBarThickness = 0 Sc.Visible = false
    Instance.new("UIListLayout", Sc).Padding = UDim.new(0, 6) Pages[id] = Sc return Sc
end
local P1, P2, P3 = CreatePage("Combat"), CreatePage("Move"), CreatePage("Bot")

local function Tab(txt, id)
    local B = Instance.new("TextButton", SB) B.Size = UDim2.new(1, -12, 0, 34) B.BackgroundColor3 = Color3.fromRGB(22, 22, 28)
    B.Text = txt B.TextColor3 = Color3.fromRGB(160,160,160) B.TextSize = 13 B.Font = Enum.Font.SourceSans
    Instance.new("UICorner", B).CornerRadius = UDim.new(0, 5)
    B.MouseButton1Click:Connect(function()
        for i, p in pairs(Pages) do p.Visible = (i == id) end
        for _, btn in pairs(SB:GetChildren()) do if btn:IsA("TextButton") then TweenService:Create(btn, TInfo, {TextColor3 = Color3.fromRGB(160,160,160), BackgroundColor3 = Color3.fromRGB(22, 22, 28)}):Play() end end
        TweenService:Create(B, TInfo, {TextColor3 = Color3.fromRGB(255,50,50), BackgroundColor3 = Color3.fromRGB(28, 22, 22)}):Play()
    end)
end
Tab("Combat & ESP", "Combat") Tab("Movement", "Move") Tab("Route Recorder", "Bot") Pages.Combat.Visible = true

-- [[ 2. UI СТИЛИЗАЦИЯ ]]
local function Toggle(p, name, def, cb)
    local F = Instance.new("Frame", p) F.Size = UDim2.new(1, -4, 0, 36) F.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    Instance.new("UICorner", F).CornerRadius = UDim.new(0, 4)
    local L = Instance.new("TextLabel", F) L.Size = UDim2.new(0.7, 0, 1, 0) L.Position = UDim2.new(0, 10, 0, 0) L.BackgroundTransparency = 1 L.Text = name L.TextColor3 = Color3.fromRGB(210,210,210) L.TextXAlignment = Enum.TextXAlignment.Left L.TextSize = 13
    
    local B = Instance.new("TextButton", F) B.Size = UDim2.new(0, 44, 0, 20) B.Position = UDim2.new(1, -54, 0.5, -10) B.BackgroundColor3 = def and Color3.fromRGB(255,50,50) or Color3.fromRGB(40,40,46) B.Text = ""
    Instance.new("UICorner", B).CornerRadius = UDim.new(0, 10)
    local C = Instance.new("Frame", B) C.Size = UDim2.new(0, 14, 0, 14) C.Position = def and UDim2.new(1, -18, 0.5, -7) or UDim2.new(0, 4, 0.5, -7) C.BackgroundColor3 = Color3.fromRGB(255,255,255) Instance.new("UICorner", C).CornerRadius = UDim.new(0, 7)
    
    local s = def B.MouseButton1Click:Connect(function()
        s = not s
        TweenService:Create(C, SpringInfo, {Position = s and UDim2.new(1, -18, 0.5, -7) or UDim2.new(0, 4, 0.5, -7)}):Play()
        TweenService:Create(B, TInfo, {BackgroundColor3 = s and Color3.fromRGB(255,50,50) or Color3.fromRGB(40,40,46)}):Play()
        cb(s)
    end)
end

local function Input(p, name, ph, cb)
    local F = Instance.new("Frame", p) F.Size = UDim2.new(1, -4, 0, 36) F.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    Instance.new("UICorner", F).CornerRadius = UDim.new(0, 4)
    local L = Instance.new("TextLabel", F) L.Size = UDim2.new(0.5, 0, 1, 0) L.Position = UDim2.new(0, 10, 0, 0) L.BackgroundTransparency = 1 L.Text = name L.TextColor3 = Color3.fromRGB(210,210,210) L.TextXAlignment = Enum.TextXAlignment.Left L.TextSize = 13
    local Box = Instance.new("TextBox", F) Box.Size = UDim2.new(0, 85, 0, 22) Box.Position = UDim2.new(1, -95, 0.5, -11) Box.BackgroundColor3 = Color3.fromRGB(32,32,38) Box.PlaceholderText = ph Box.Text = "" Box.TextColor3 = Color3.fromRGB(255,255,255) Box.TextSize = 12
    Instance.new("UICorner", Box).CornerRadius = UDim.new(0, 4)
    Box.FocusLost:Connect(function() cb(Box.Text) end)
end

-- [[ 3. ХАРДКОРНОЕ ЯДРО ДВИЖКА ]]
RunService.Heartbeat:Connect(function(dt)
    local char = LP.Character if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hrp or not hum then return end
    
    if Config.Noclip then
        for _, v in pairs(char:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end
    end
    
    if Config.Fly then
        hrp.Velocity = Vector3.zero
        local moveDir = hum.MoveDirection
        local flyVec = Vector3.zero
        if moveDir.Magnitude > 0 then flyVec = Camera.CFrame:VectorToWorldSpace(Vector3.new(moveDir.X, 0, moveDir.Z * 1.5)) end
        if UIS:IsKeyDown(Enum.KeyCode.Space) then flyVec = flyVec + Vector3.new(0, 1, 0) end
        hrp.CFrame = hrp.CFrame + (flyVec * (Config.FlySpeed * dt))
    elseif Config.Speed > 16 and hum.MoveDirection.Magnitude > 0 and not Config.IsPlaying then
        hrp.Velocity = Vector3.new(0, hrp.Velocity.Y, 0)
        hrp.CFrame = hrp.CFrame + (hum.MoveDirection * (Config.Speed * dt))
    end
end)

UIS.JumpRequest:Connect(function()
    if Config.InfJump and LP.Character and LP.Character:FindFirstChildOfClass("Humanoid") then
        LP.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
    end
end)

task.spawn(function()
    while task.wait() do
        if Config.GodMode and LP.Character then
            local hum = LP.Character:FindFirstChildOfClass("Humanoid")
            local hrp = LP.Character:FindFirstChild("HumanoidRootPart")
            if hum and hrp and hum.Health < OldHealth and hum.Health > 0 then
                local c = hrp.CFrame hrp.CFrame = CFrame.new(9e9, 9e9, 9e9)
                task.wait() hum.Health = hum.MaxHealth hrp.CFrame = c
            end
            if hum then OldHealth = hum.Health end
        end
    end
end)

-- Улучшенные Визуальные Хитбоксы (Не ломают физику игры)
task.spawn(function()
    while task.wait(0.5) do
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LP and p.Character then
                local hrp = p.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    local box = hrp:FindFirstChild("OmniBox")
                    if Config.Hitbox then
                        if not box then
                            box = Instance.new("Part", hrp)
                            box.Name = "OmniBox"
                            box.Size = Vector3.new(4.5, 4.5, 4.5)
                            box.Anchored = false
                            box.CanCollide = false
                            box.Massless = true
                            box.Material = Enum.Material.Neon
                            box.Color = Color3.fromRGB(255, 30, 30)
                            box.Transparency = 0.75
                            
                            local weld = Instance.new("Weld", box)
                            weld.Part0 = hrp
                            weld.Part1 = box
                            
                            local b = Instance.new("SelectionBox", box)
                            b.Adornee = box
                            b.Color3 = Color3.fromRGB(255, 0, 0)
                            b.LineThickness = 0.05
                        end
                    else
                        if box then box:Destroy() end
                    end
                end
            end
        end
    end
end)

-- [[ СИСТЕМА КЛИКЕР-СЛОТОВ БОТА ]]
RunService.Heartbeat:Connect(function()
    if Config.IsRecording and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        table.insert(Slots[Config.CurrentSlot], {CF = LP.Character.HumanoidRootPart.CFrame, Jump = UIS:IsKeyDown(Enum.KeyCode.Space)})
    end
end)

local function PlayBot()
    Config.IsPlaying = true
    task.spawn(function()
        local path = Slots[Config.CurrentSlot]
        if not path or #path == 0 then Config.IsPlaying = false return end
        for _, d in pairs(path) do
            if not Config.IsPlaying or not LP.Character then break end
            local hrp = LP.Character:FindFirstChild("HumanoidRootPart")
            local hum = LP.Character:FindFirstChildOfClass("Humanoid")
            if hrp then hrp.CFrame = d.CF end
            if d.Jump and hum then hum:ChangeState("Jumping") end
            task.wait(0.01)
        end
        Config.IsPlaying = false
    end)
end

-- [[ КОНФИГУРАЦИЯ СТРАНИЦ ]]
Toggle(P1, "Visual Enemy Hitboxes (Safe Mode)", false, function(v) Config.Hitbox = v end)
Toggle(P1, "Ultra GodMode (Anti-Damage)", false, function(v) Config.GodMode = v end)

Toggle(P2, "Air-Walk (Infinite Jump)", false, function(v) Config.InfJump = v end)
Toggle(P2, "Noclip (Through Walls)", false, function(v) Config.Noclip = v end)
Toggle(P2, "CFrame Fly Engine", false, function(v) Config.Fly = v end)
Input(P2, "WalkSpeed Override", "16", function(v) Config.Speed = tonumber(v) or 16 end)
Input(P2, "Fly Speed Override", "50", function(v) Config.FlySpeed = tonumber(v) or 50 end)

-- Сборка кастомного Селектора Слотов во вкладке Маршрутизатора
local SlotFrame = Instance.new("Frame", P3) SlotFrame.Size = UDim2.new(1, -4, 0, 45) SlotFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
Instance.new("UICorner", SlotFrame).CornerRadius = UDim.new(0, 4)

local SlotLabel = Instance.new("TextLabel", SlotFrame) SlotLabel.Size = UDim2.new(0.4, 0, 1, 0) SlotLabel.Position = UDim2.new(0, 10, 0, 0) SlotLabel.BackgroundTransparency = 1
SlotLabel.Text = "Active Slot: [ 1 / 20 ]" SlotLabel.TextColor3 = Color3.fromRGB(255, 200, 50) SlotLabel.Font = Enum.Font.SourceSansBold SlotLabel.TextSize = 14 SlotLabel.TextXAlignment = Enum.TextXAlignment.Left

local function CreateSelectorBtn(txt, pos, offset)
    local B = Instance.new("TextButton", SlotFrame) B.Size = UDim2.new(0, 65, 0, 26) B.Position = pos B.BackgroundColor3 = Color3.fromRGB(35, 35, 42)
    B.Text = txt B.TextColor3 = Color3.fromRGB(255,255,255) B.Font = Enum.Font.SourceSansBold B.TextSize = 12 Instance.new("UICorner", B).CornerRadius = UDim.new(0, 4)
    B.MouseButton1Click:Connect(function()
        Config.CurrentSlot = math.clamp(Config.CurrentSlot + offset, 1, 20)
        SlotLabel.Text = "Active Slot: [ " .. tostring(Config.CurrentSlot) .. " / 20 ]"
    end)
end
CreateSelectorBtn("<<< Prev", UDim2.new(1, -145, 0.5, -13), -1)
CreateSelectorBtn("Next >>>", UDim2.new(1, -75, 0.5, -13), 1)

Toggle(P3, "Record Path to Active Slot", false, function(v) Config.IsRecording = v if v then Slots[Config.CurrentSlot] = {} end end)
Toggle(P3, "Execute Route from Active Slot", false, function(v) Config.IsPlaying = v if v then PlayBot() end end)
