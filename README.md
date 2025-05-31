-- Grow a Garden - Versão FREE
-- Interface leve e funcional com plantio, colheita, dicas e minimização/movimentação

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- RemoteEvents conhecidos\local Plant_RE = ReplicatedStorage:WaitForChild("Plant_RE")
local HarvestRemote = ReplicatedStorage:WaitForChild("HarvestRemote")

-- GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "GrowFreeUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

-- Janela principal
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 230)
MainFrame.Position = UDim2.new(0.05, 0, 0.2, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(180, 240, 200)
MainFrame.BorderSizePixel = 0
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.Active = true
MainFrame.Draggable = true

-- Minimizar
local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 25, 0, 25)
MinBtn.Position = UDim2.new(1, -30, 0, 5)
MinBtn.Text = "-"
MinBtn.BackgroundColor3 = Color3.fromRGB(150, 200, 255)
MinBtn.Parent = MainFrame

local isMinimized = false
MinBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    for _, v in ipairs(MainFrame:GetChildren()) do
        if v:IsA("TextButton") or v:IsA("TextLabel") then
            if v ~= MinBtn then
                v.Visible = not isMinimized
            end
        end
    end
    MinBtn.Text = isMinimized and "+" or "-"
end)

-- Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -35, 0, 30)
Title.Position = UDim2.new(0, 5, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "🌱 Grow a Garden - Free UI"
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.fromRGB(0, 120, 0)
Title.TextSize = 18
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = MainFrame

-- Aba: Home
local PlantBtn = Instance.new("TextButton")
PlantBtn.Size = UDim2.new(1, -20, 0, 40)
PlantBtn.Position = UDim2.new(0, 10, 0, 40)
PlantBtn.BackgroundColor3 = Color3.fromRGB(200, 255, 200)
PlantBtn.Text = "🌾 Plantar Sementes"
PlantBtn.Font = Enum.Font.GothamBold
PlantBtn.TextColor3 = Color3.fromRGB(0, 100, 0)
PlantBtn.TextSize = 16
PlantBtn.Parent = MainFrame

PlantBtn.MouseButton1Click:Connect(function()
    local backpack = LocalPlayer:FindFirstChild("Backpack")
    if not backpack then return end
    for _, item in ipairs(backpack:GetChildren()) do
        if item.Name:lower():find("seed") then
            Plant_RE:FireServer(item)
        end
    end
end)

local HarvestBtn = Instance.new("TextButton")
HarvestBtn.Size = UDim2.new(1, -20, 0, 40)
HarvestBtn.Position = UDim2.new(0, 10, 0, 90)
HarvestBtn.BackgroundColor3 = Color3.fromRGB(200, 255, 200)
HarvestBtn.Text = "🍎 Colher Frutas"
HarvestBtn.Font = Enum.Font.GothamBold
HarvestBtn.TextColor3 = Color3.fromRGB(100, 0, 0)
HarvestBtn.TextSize = 16
HarvestBtn.Parent = MainFrame

HarvestBtn.MouseButton1Click:Connect(function()
    HarvestRemote:FireServer()
end)

-- Aba: Ajuda
local HelpLabel = Instance.new("TextLabel")
HelpLabel.Size = UDim2.new(1, -20, 0, 60)
HelpLabel.Position = UDim2.new(0, 10, 0, 140)
HelpLabel.BackgroundTransparency = 1
HelpLabel.TextWrapped = true
HelpLabel.Text = "📘 Dica: Plante as melhores sementes que você tiver e colha rápido! Use sprinklers para acelerar o crescimento."
HelpLabel.Font = Enum.Font.Gotham
HelpLabel.TextColor3 = Color3.fromRGB(0, 70, 130)
HelpLabel.TextSize = 14
HelpLabel.Parent = MainFrame
