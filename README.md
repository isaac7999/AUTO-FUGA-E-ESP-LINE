local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")
local runService = game:GetService("RunService")

-- Criando o GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.CoreGui

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 200, 0, 150)
mainFrame.Position = UDim2.new(0.1, 0, 0.1, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
mainFrame.Parent = screenGui

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 30)
titleLabel.Text = "Painel Admin"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
titleLabel.Parent = mainFrame

-- Botão de minimizar/maximizar
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 30, 0, 30)
toggleButton.Position = UDim2.new(1, -30, 0, 0)
toggleButton.Text = "-"
toggleButton.Parent = mainFrame

local minimized = false
toggleButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        mainFrame.Size = UDim2.new(0, 200, 0, 30)
        toggleButton.Text = "+"
    else
        mainFrame.Size = UDim2.new(0, 200, 0, 150)
        toggleButton.Text = "-"
    end
end)

-- Botão Auto Fugir
local autoFugirButton = Instance.new("TextButton")
autoFugirButton.Size = UDim2.new(1, 0, 0, 30)
autoFugirButton.Position = UDim2.new(0, 0, 0, 40)
autoFugirButton.Text = "Auto Fugir (OFF)"
autoFugirButton.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
autoFugirButton.Parent = mainFrame

local autoFugirAtivo = false
autoFugirButton.MouseButton1Click:Connect(function()
    autoFugirAtivo = not autoFugirAtivo
    autoFugirButton.Text = autoFugirAtivo and "Auto Fugir (ON)" or "Auto Fugir (OFF)"
    autoFugirButton.BackgroundColor3 = autoFugirAtivo and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(100, 0, 0)
end)

-- Botão ESP Line
local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(1, 0, 0, 30)
espButton.Position = UDim2.new(0, 0, 0, 80)
espButton.Text = "ESP Line (OFF)"
espButton.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
espButton.Parent = mainFrame

local espAtivo = false
espButton.MouseButton1Click:Connect(function()
    espAtivo = not espAtivo
    espButton.Text = espAtivo and "ESP Line (ON)" or "ESP Line (OFF)"
    espButton.BackgroundColor3 = espAtivo and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(100, 0, 0)
end)

-- Função de Auto Fugir
runService.Heartbeat:Connect(function()
    if autoFugirAtivo and humanoid.Health < 70 then
        char:SetPrimaryPartCFrame(CFrame.new(-297.6, 5.0, 379.6))
    end
end)

-- Função de ESP Line
local espLines = {}

runService.RenderStepped:Connect(function()
    -- Removendo linhas antigas
    for _, line in pairs(espLines) do
        line:Destroy()
    end
    table.clear(espLines)

    if espAtivo then
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                local hrp = plr.Character.HumanoidRootPart
                local myHrp = char:FindFirstChild("HumanoidRootPart")

                if myHrp then
                    local beam = Instance.new("Beam")
                    local attachment1 = Instance.new("Attachment", myHrp)
                    local attachment2 = Instance.new("Attachment", hrp)

                    beam.Attachment0 = attachment1
                    beam.Attachment1 = attachment2
                    beam.Color = ColorSequence.new(Color3.fromRGB(0, 255, 0))
                    beam.Width0 = 0.1
                    beam.Width1 = 0.1
                    beam.Parent = myHrp

                    table.insert(espLines, beam)
                end
            end
        end
    end
end)
