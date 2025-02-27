-- Criar a GUI
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player.PlayerGui

-- Criar o Frame (Painel da GUI)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 150, 0, 100)
frame.Position = UDim2.new(0.1, 0, 0.1, 0)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

-- Criar o botão ON
local buttonOn = Instance.new("TextButton")
buttonOn.Size = UDim2.new(1, 0, 0.5, 0)
buttonOn.Position = UDim2.new(0, 0, 0, 0)
buttonOn.Text = "Anti-Sit ON"
buttonOn.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
buttonOn.Parent = frame

-- Criar o botão OFF
local buttonOff = Instance.new("TextButton")
buttonOff.Size = UDim2.new(1, 0, 0.5, 0)
buttonOff.Position = UDim2.new(0, 0, 0.5, 0)
buttonOff.Text = "Anti-Sit OFF"
buttonOff.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
buttonOff.Parent = frame

-- Criar função para impedir que o jogador sente
local function preventSit()
    local character = player.Character
    if character then
        for _, part in pairs(character:GetDescendants()) do
            if part:IsA("Seat") or part:IsA("VehicleSeat") then
                part.Disabled = true
            end
        end
    end
end

-- Ativar o Anti-Sit
buttonOn.MouseButton1Click:Connect(function()
    if player.Character then
        local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.SeatPart = nil -- Desgruda de qualquer assento
            humanoid:GetPropertyChangedSignal("SeatPart"):Connect(function()
                humanoid.SeatPart = nil -- Impede de sentar
            end)
        end
    end
end)

-- Desativar o Anti-Sit
buttonOff.MouseButton1Click:Connect(function()
    if player.Character then
        local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:GetPropertyChangedSignal("SeatPart"):Disconnect() -- Permite sentar novamente
        end
    end
end)
