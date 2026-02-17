local Cores = {
    Fundo = Color3.fromRGB(15, 15, 15),
    Topo = Color3.fromRGB(25, 25, 25),
    Destaque = Color3.fromRGB(0, 170, 255),
    Texto = Color3.fromRGB(240, 240, 240),
    BotaoOff = Color3.fromRGB(40, 40, 40),
    BotaoOn = Color3.fromRGB(0, 170, 255),
    Input = Color3.fromRGB(20, 20, 20)
}

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

if LocalPlayer.PlayerGui:FindFirstChild("CyberMochila_V15") then LocalPlayer.PlayerGui.CyberMochila_V15:Destroy() end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "CyberMochila_V15"
ScreenGui.Parent = LocalPlayer.PlayerGui
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 260, 0, 360)
MainFrame.Position = UDim2.new(0.5, -130, 0.5, -175)
MainFrame.BackgroundColor3 = Cores.Fundo
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

-- HEADER
local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, 0, 0, 40)
Header.BackgroundColor3 = Cores.Topo
Header.Parent = MainFrame
Instance.new("UICorner", Header).CornerRadius = UDim.new(0, 12)

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -70, 1, 0)
Title.Position = UDim2.new(0, 15, 0, 0)
Title.Text = "CYBER V15 - ANTI-VOID"
Title.TextColor3 = Cores.Destaque
Title.Font = Enum.Font.GothamBold
Title.TextSize = 12
Title.BackgroundTransparency = 1
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Header

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 25, 0, 25)
CloseBtn.Position = UDim2.new(1, -30, 0, 7)
CloseBtn.Text = "×"
CloseBtn.TextColor3 = Color3.fromRGB(255, 80, 80)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.Parent = Header

local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 25, 0, 25)
MinBtn.Position = UDim2.new(1, -55, 0, 7)
MinBtn.Text = "−"
MinBtn.TextColor3 = Cores.Texto
MinBtn.BackgroundTransparency = 1
MinBtn.Font = Enum.Font.GothamBold
MinBtn.Parent = Header

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, 0, 1, -40)
Content.Position = UDim2.new(0, 0, 0, 40)
Content.BackgroundTransparency = 1
Content.Parent = MainFrame

local Signature = Instance.new("TextLabel")
Signature.Size = UDim2.new(0, 100, 0, 20)
Signature.Position = UDim2.new(1, -105, 1, -20)
Signature.Text = "by: cybernodry"
Signature.TextColor3 = Color3.fromRGB(80, 80, 80)
Signature.Font = Enum.Font.Code
Signature.TextSize = 11
Signature.BackgroundTransparency = 1
Signature.Parent = Content

-- BOTÕES DE MODO
local ModeContainer = Instance.new("Frame")
ModeContainer.Size = UDim2.new(0.9, 0, 0, 30)
ModeContainer.Position = UDim2.new(0.05, 0, 0.03, 0)
ModeContainer.BackgroundTransparency = 1
ModeContainer.Parent = Content

local function createModeBtn(txt, pos)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.31, 0, 1, 0)
    btn.Position = pos
    btn.Text = txt
    btn.BackgroundColor3 = Cores.BotaoOff
    btn.TextColor3 = Cores.Texto
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 8
    btn.Parent = ModeContainer
    Instance.new("UICorner", btn)
    return btn
end

local btnHead = createModeBtn("CABEÇA", UDim2.new(0, 0, 0, 0))
local btnBackF = createModeBtn("MOCHILA (F)", UDim2.new(0.345, 0, 0, 0))
local btnBackT = createModeBtn("MOCHILA (T)", UDim2.new(0.69, 0, 0, 0))
btnHead.BackgroundColor3 = Cores.BotaoOn

-- CONTROLE
local SelectedPlayer = nil
local IsFollowing = false
local CurrentMode = "Head"
local Minimized = false

local function stopAnims()
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("Humanoid") then
        for _, anim in pairs(char.Humanoid:GetPlayingAnimationTracks()) do anim:Stop() end
    end
end

-- INTERFACE
local function setMode(mode, btn)
    CurrentMode = mode
    btnHead.BackgroundColor3 = Cores.BotaoOff
    btnBackF.BackgroundColor3 = Cores.BotaoOff
    btnBackT.BackgroundColor3 = Cores.BotaoOff
    btn.BackgroundColor3 = Cores.BotaoOn
end

btnHead.MouseButton1Click:Connect(function() setMode("Head", btnHead) end)
btnBackF.MouseButton1Click:Connect(function() setMode("BackFront", btnBackF) end)
btnBackT.MouseButton1Click:Connect(function() setMode("BackBack", btnBackT) end)

MinBtn.MouseButton1Click:Connect(function()
    Minimized = not Minimized
    Content.Visible = not Minimized
    MainFrame:TweenSize(Minimized and UDim2.new(0, 260, 0, 40) or UDim2.new(0, 260, 0, 360), "Out", "Quad", 0.3, true)
    MinBtn.Text = Minimized and "+" or "−"
end)
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

-- ARRASTE
local dragging, dragStart, startPos
Header.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = true dragStart = i.Position startPos = MainFrame.Position end end)
UserInputService.InputChanged:Connect(function(i) if dragging and i.UserInputType == Enum.UserInputType.MouseMovement then local d = i.Position - dragStart MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X, startPos.Y.Scale, startPos.Y.Offset + d.Y) end end)
UserInputService.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end end)

-- LISTA
local SearchBox = Instance.new("TextBox")
SearchBox.Size = UDim2.new(0.9, 0, 0, 30)
SearchBox.Position = UDim2.new(0.05, 0, 0.15, 0)
SearchBox.BackgroundColor3 = Cores.Input
SearchBox.PlaceholderText = "Nick..."
SearchBox.TextColor3 = Cores.Texto
SearchBox.Parent = Content
Instance.new("UICorner", SearchBox)

local Scroll = Instance.new("ScrollingFrame")
Scroll.Size = UDim2.new(0.9, 0, 0, 120)
Scroll.Position = UDim2.new(0.05, 0, 0.28, 0)
Scroll.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Scroll.Parent = Content
Instance.new("UIListLayout", Scroll).Padding = UDim.new(0, 5)

local ActionBtn = Instance.new("TextButton")
ActionBtn.Size = UDim2.new(0.85, 0, 0, 45)
ActionBtn.Position = UDim2.new(0.075, 0, 0.7, 0)
ActionBtn.BackgroundColor3 = Cores.BotaoOff
ActionBtn.Text = "SELECIONAR ALVO"
ActionBtn.TextColor3 = Cores.Texto
ActionBtn.Font = Enum.Font.GothamBold
ActionBtn.Parent = Content
Instance.new("UICorner", ActionBtn)

local function updateList(filter)
    for _, v in pairs(Scroll:GetChildren()) do if v:IsA("TextButton") then v:Destroy() end end
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and (p.Name:lower():find(filter:lower()) or p.DisplayName:lower():find(filter:lower())) then
            local pBtn = Instance.new("TextButton")
            pBtn.Size = UDim2.new(1, 0, 0, 25)
            pBtn.Text = p.DisplayName
            pBtn.BackgroundColor3 = Color3.fromRGB(30,30,30)
            pBtn.TextColor3 = Cores.Texto
            pBtn.Parent = Scroll
            Instance.new("UICorner", pBtn)
            pBtn.MouseButton1Click:Connect(function()
                SelectedPlayer = p
                ActionBtn.Text = "GRUDAR EM: " .. p.DisplayName:upper()
                ActionBtn.BackgroundColor3 = Cores.BotaoOn
            end)
        end
    end
end
SearchBox:GetPropertyChangedSignal("Text"):Connect(function() updateList(SearchBox.Text) end)
updateList("")

-- FUNÇÃO PARA PARAR DE SEGUIR SEGURO
local function detach()
    IsFollowing = false
    local char = LocalPlayer.Character
    if char then
        char.Humanoid.Sit = false
        for _, p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = true end end
        -- Teleporta um pouco pra cima pra não cair
        char.HumanoidRootPart.CFrame = char.HumanoidRootPart.CFrame * CFrame.new(0, 3, 0)
    end
    ActionBtn.Text = "SELECIONAR ALVO"
    ActionBtn.BackgroundColor3 = Cores.BotaoOff
    SelectedPlayer = nil
end

ActionBtn.MouseButton1Click:Connect(function()
    if SelectedPlayer then
        IsFollowing = not IsFollowing
        if not IsFollowing then
            detach()
        else
            ActionBtn.Text = "PARAR"
            ActionBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
            stopAnims()
        end
    end
end)

-- LOOP PRINCIPAL COM PROTEÇÃO ANTI-VOID
RunService.Stepped:Connect(function()
    if IsFollowing then
        -- VERIFICAÇÃO SE O JOGADOR AINDA EXISTE E TEM PERSONAGEM
        if not SelectedPlayer or not SelectedPlayer.Parent or not SelectedPlayer.Character or not SelectedPlayer.Character:FindFirstChild("HumanoidRootPart") then
            detach() -- Se ele saiu ou morreu, solta automaticamente
            return
        end

        local myChar = LocalPlayer.Character
        local targetChar = SelectedPlayer.Character
        local myRoot = myChar and myChar:FindFirstChild("HumanoidRootPart")
        local tTorso = targetChar:FindFirstChild("UpperTorso") or targetChar:FindFirstChild("Torso")
        local tHead = targetChar:FindFirstChild("Head")

        if myRoot and (tTorso or tHead) then
            for _, p in pairs(myChar:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end
            myRoot.Velocity = Vector3.new(0,0,0)
            stopAnims()

            if CurrentMode == "Head" and tHead then
                myRoot.CFrame = tHead.CFrame * CFrame.new(0, 1.2, 0)
            elseif CurrentMode == "BackFront" and tTorso then
                myRoot.CFrame = tTorso.CFrame * CFrame.new(0, 0.5, 0.7)
            elseif CurrentMode == "BackBack" and tTorso then
                myRoot.CFrame = tTorso.CFrame * CFrame.new(0, 0.5, 0.7) * CFrame.Angles(0, math.rad(180), 0)
            end
        end
    end
end)
