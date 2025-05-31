-- Grow a Garden - VERSÃO FREE
-- Interface simples para farm automático

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- RemoteEvents
local Plant_RE = ReplicatedStorage:WaitForChild("Plant_RE")
local HarvestRemote = ReplicatedStorage:WaitForChild("HarvestRemote")

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "GrowFreeUI"
ScreenGui.Parent = PlayerGui
ScreenGui.ResetOnSpawn = false

local minimized = false

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 200)
MainFrame.Position = UDim2.new(0.05, 0, 0.1, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(200, 255, 200)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

local TabHolder = Instance.new("Folder")
TabHolder.Name = "Tabs"
TabHolder.Parent = MainFrame

local function createButton(text, position, callback)
    local button = Instance.new("TextButton")
    button.Text = text
    button.Size = UDim2.new(0, 280, 0, 30)
    button.Position = position
    button.BackgroundColor3 = Color3.fromRGB(180, 220, 255)
    button.TextColor3 = Color3.fromRGB(0, 0, 0)
    button.Font = Enum.Font.Gotham
    button.TextSize = 14
    button.Parent = MainFrame
    button.MouseButton1Click:Connect(callback)
    return button
end

-- Plantar automaticamente
local function autoPlant()
    for _, item in pairs(LocalPlayer.Backpack:GetChildren()) do
        if item:IsA("Tool") and item.Name:lower():find("seed") then
            Plant_RE:FireServer(item.Name)
        end
    end
end

-- Colher tudo
local function autoHarvest()
    HarvestRemote:FireServer("All")
end

-- Minimize button
local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -35, 0, 5)
MinBtn.BackgroundColor3 = Color3.fromRGB(150, 200, 150)
MinBtn.Text = "-"
MinBtn.Parent = MainFrame
MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    for _, child in pairs(MainFrame:GetChildren()) do
        if child ~= MinBtn and child:IsA("TextButton") then
            child.Visible = not minimized
        end
    end
end)

-- Gui Title
local Title = Instance.new("TextLabel")
Title.Text = "🌱 Grow Helper (Free)"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(160, 230, 160)
Title.TextColor3 = Color3.fromRGB(0, 0, 0)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.Parent = MainFrame

-- Botões principais
createButton("🌾 Plantar Automaticamente", UDim2.new(0, 10, 0, 40), autoPlant)
createButton("🍎 Colher Frutas", UDim2.new(0, 10, 0, 80), autoHarvest)
createButton("❓ Como farmar melhor?", UDim2.new(0, 10, 0, 120), function()
    game.StarterGui:SetCore("SendNotification", {
        Title = "Dica de Farm!",
        Text = "Use sprinklers e plante sementes raras para mais lucro.",
        Duration = 5
    })
end)

-- Fim
