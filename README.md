local ReplicatedStorage = game:GetService("ReplicatedStorage")

local Lighting = game:GetService("Lighting")

local Players = game:GetService("Players")

-- Cria RemoteEvent se não existir

local AdminCommand = ReplicatedStorage:FindFirstChild("AdminCommand")

if not AdminCommand then

    AdminCommand = Instance.new("RemoteEvent")

    AdminCommand.Name = "AdminCommand"

    AdminCommand.Parent = ReplicatedStorage

end

-- Função que cria a interface para o player

local function criarGUI(player)

    local playerGui = player:WaitForChild("PlayerGui")

    local screenGui = Instance.new("ScreenGui")

    screenGui.Name = "AdminUI"

    screenGui.ResetOnSpawn = false

    screenGui.Parent = playerGui

    local mainFrame = Instance.new("Frame", screenGui)

    mainFrame.Size = UDim2.new(0, 300, 0, 400)

    mainFrame.Position = UDim2.new(0.01, 0, 0.2, 0)

    mainFrame.BackgroundColor3 = Color3.new(0, 0, 0)

    mainFrame.BorderColor3 = Color3.new(1, 1, 1)

    mainFrame.BorderSizePixel = 2

    local function createLabel(text, position)

        local label = Instance.new("TextLabel")

        label.Size = UDim2.new(0.8, 0, 0.05, 0)

        label.Position = position

        label.BackgroundTransparency = 1

        label.Text = text

        label.TextColor3 = Color3.new(1, 1, 1)

        label.Font = Enum.Font.Code

        label.TextScaled = true

        label.Parent = mainFrame

        return label

    end

    -- Skybox Controls

    createLabel("Skybox Controls", UDim2.new(0.1, 0, 0.05, 0))

    local skyboxInput = Instance.new("TextBox", mainFrame)

    skyboxInput.Size = UDim2.new(0.8, 0, 0.07, 0)

    skyboxInput.Position = UDim2.new(0.1, 0, 0.11, 0)

    skyboxInput.PlaceholderText = "Skybox ID"

    skyboxInput.BackgroundColor3 = Color3.new(1, 1, 1)

    skyboxInput.TextColor3 = Color3.new(0, 0, 0)

    skyboxInput.Font = Enum.Font.Code

    skyboxInput.TextScaled = true

    local skyboxButton = Instance.new("TextButton", mainFrame)

    skyboxButton.Size = UDim2.new(0.8, 0, 0.07, 0)

    skyboxButton.Position = UDim2.new(0.1, 0, 0.19, 0)

    skyboxButton.Text = "Set Skybox"

    skyboxButton.BackgroundColor3 = Color3.new(1, 1, 1)

    skyboxButton.TextColor3 = Color3.new(0, 0, 0)

    skyboxButton.Font = Enum.Font.Code

    skyboxButton.TextScaled = true

    skyboxButton.MouseButton1Click:Connect(function()

        local id = skyboxInput.Text

        if tonumber(id) then

            AdminCommand:FireServer(player, "skybox", id)

        end

    end)

    -- Music Controls

    createLabel("Music Controls", UDim2.new(0.1, 0, 0.30, 0))

    local musicInput = Instance.new("TextBox", mainFrame)

    musicInput.Size = UDim2.new(0.8, 0, 0.07, 0)

    musicInput.Position = UDim2.new(0.1, 0, 0.36, 0)

    musicInput.PlaceholderText = "Music ID"

    musicInput.BackgroundColor3 = Color3.new(1, 1, 1)

    musicInput.TextColor3 = Color3.new(0, 0, 0)

    musicInput.Font = Enum.Font.Code

    musicInput.TextScaled = true

    local musicButton = Instance.new("TextButton", mainFrame)

    musicButton.Size = UDim2.new(0.8, 0, 0.07, 0)

    musicButton.Position = UDim2.new(0.1, 0, 0.44, 0)

    musicButton.Text = "Play Music"

    musicButton.BackgroundColor3 = Color3.new(1, 1, 1)

    musicButton.TextColor3 = Color3.new(0, 0, 0)

    musicButton.Font = Enum.Font.Code

    musicButton.TextScaled = true

    musicButton.MouseButton1Click:Connect(function()

        local id = musicInput.Text

        if tonumber(id) then

            AdminCommand:FireServer(player, "music", id)

        end

    end)

end

-- Quando o player entra, cria a interface

Players.PlayerAdded:Connect(function(player)

    criarGUI(player)

end)

-- Processa os comandos no servidor

AdminCommand.OnServerEvent:Connect(function(player, command, arg)

    if command == "skybox" then

        local skyId = tostring(arg)

        for _, v in pairs(Lighting:GetChildren()) do

            if v:IsA("Sky") then

                v:Destroy()

            end

        end

        local sky = Instance.new("Sky")

        local asset = "rbxassetid://" .. skyId

        sky.SkyboxBk = asset

        sky.SkyboxDn = asset

        sky.SkyboxFt = asset

        sky.SkyboxLf = asset

        sky.SkyboxRt = asset

        sky.SkyboxUp = asset

        sky.Name = "CustomSky"

        sky.Parent = Lighting

    elseif command == "music" then

        local musicId = tostring(arg)

        local asset = "rbxassetid://" .. musicId

        for _, s in pairs(workspace:GetChildren()) do

            if s:IsA("Sound") and s.Name == "GlobalMusic" then

                s:Destroy()

            end

        end

        local sound = Instance.new("Sound")

        sound.Name = "GlobalMusic"

        sound.SoundId = asset

        sound.Looped = true

        sound.Volume = 1

        sound.Parent = workspace

        sound:Play()

    end

end)# Ydheje
