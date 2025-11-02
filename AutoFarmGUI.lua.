local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoFarmGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game:GetService("CoreGui")

local Frame = Instance.new("Frame", ScreenGui)
Frame.Position = UDim2.new(0.5, -120, 0.5, -80)
Frame.Size = UDim2.new(0, 240, 0, 180)
Frame.BackgroundColor3 = Color3.fromRGB(40,40,40)
Frame.Visible = true
Frame.Active = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0,12)

local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1,0,0,30)
Title.BackgroundTransparency = 1
Title.Text = "Auto Farm GUI"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20
Title.TextColor3 = Color3.fromRGB(255,255,255)

local MobToggle = Instance.new("TextButton", Frame)
MobToggle.Position = UDim2.new(0.1,0,0.3,0)
MobToggle.Size = UDim2.new(0.8,0,0.18,0)
MobToggle.BackgroundColor3 = Color3.fromRGB(70,70,70)
MobToggle.Text = "Farm Halloween Mobs: OFF"
MobToggle.Font = Enum.Font.Gotham
MobToggle.TextColor3 = Color3.fromRGB(255,255,255)
MobToggle.TextSize = 18

local CandyToggle = Instance.new("TextButton", Frame)
CandyToggle.Position = UDim2.new(0.1,0,0.53,0)
CandyToggle.Size = UDim2.new(0.8,0,0.18,0)
CandyToggle.BackgroundColor3 = Color3.fromRGB(70,70,70)
CandyToggle.Text = "Farm Candy Corn: OFF"
CandyToggle.Font = Enum.Font.Gotham
CandyToggle.TextColor3 = Color3.fromRGB(255,255,255)
CandyToggle.TextSize = 18

local CloseBtn = Instance.new("TextButton", Frame)
CloseBtn.Position = UDim2.new(0.85,0,0.02,0)
CloseBtn.Size = UDim2.new(0.12,0,0.18,0)
CloseBtn.BackgroundColor3 = Color3.fromRGB(150,50,50)
CloseBtn.Text = "X"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextColor3 = Color3.fromRGB(255,255,255)
CloseBtn.TextSize = 16

local HideBtn = Instance.new("TextButton", Frame)
HideBtn.Position = UDim2.new(0.72,0,0.02,0)
HideBtn.Size = UDim2.new(0.12,0,0.18,0)
HideBtn.BackgroundColor3 = Color3.fromRGB(50,50,150)
HideBtn.Text = "Hide"
HideBtn.Font = Enum.Font.GothamBold
HideBtn.TextColor3 = Color3.fromRGB(255,255,255)
HideBtn.TextSize = 16

local ErrorLabel = Instance.new("TextLabel", Frame)
ErrorLabel.Position = UDim2.new(0, 0, 0.82, 0)
ErrorLabel.Size = UDim2.new(1, 0, 0.15, 0)
ErrorLabel.BackgroundTransparency = 1
ErrorLabel.TextColor3 = Color3.fromRGB(255,80,80)
ErrorLabel.Font = Enum.Font.Gotham
ErrorLabel.TextSize = 14
ErrorLabel.Text = ""
ErrorLabel.Visible = false

-- ShowBtn (botão que aparece quando está oculto)
local ShowBtn = Instance.new("TextButton", ScreenGui)
ShowBtn.Position = UDim2.new(0.5, -40, 0.5, -15)
ShowBtn.Size = UDim2.new(0, 80, 0, 30)
ShowBtn.BackgroundColor3 = Color3.fromRGB(50,50,150)
ShowBtn.Text = "Show GUI"
ShowBtn.Font = Enum.Font.GothamBold
ShowBtn.TextColor3 = Color3.fromRGB(255,255,255)
ShowBtn.TextSize = 18
ShowBtn.Visible = false
ShowBtn.Active = true

-- Dragging manual Frame
local UIS = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos
Frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = Frame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)
Frame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)
UIS.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        Frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X,
                                   startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- Dragging manual ShowBtn
local showDragging, showDragInput, showDragStart, showStartPos
ShowBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        showDragging = true
        showDragStart = input.Position
        showStartPos = ShowBtn.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                showDragging = false
            end
        end)
    end
end)
ShowBtn.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        showDragInput = input
    end
end)
UIS.InputChanged:Connect(function(input)
    if input == showDragInput and showDragging then
        local delta = input.Position - showDragStart
        ShowBtn.Position = UDim2.new(showStartPos.X.Scale, showStartPos.X.Offset + delta.X,
                                     showStartPos.Y.Scale, showStartPos.Y.Offset + delta.Y)
    end
end)

-- Toggle logic
local AutoFarmMobs = false
local AutoFarmCandy = false
local mobsThread, candyThread

local function setMobToggleVisual(state)
    MobToggle.BackgroundColor3 = state and Color3.fromRGB(30,150,30) or Color3.fromRGB(70,70,70)
    MobToggle.Text = "Farm Halloween Mobs: " .. (state and "ON" or "OFF")
end

local function setCandyToggleVisual(state)
    CandyToggle.BackgroundColor3 = state and Color3.fromRGB(30,150,30) or Color3.fromRGB(70,70,70)
    CandyToggle.Text = "Farm Candy Corn: " .. (state and "ON" or "OFF")
end

MobToggle.MouseButton1Click:Connect(function()
    AutoFarmMobs = not AutoFarmMobs
    setMobToggleVisual(AutoFarmMobs)
    if AutoFarmMobs and not mobsThread then
        mobsThread = task.spawn(function()
            local player = game.Players.LocalPlayer
            local function getChar()
                return player.Character or player.CharacterAdded:Wait()
            end
            local function isAlive()
                local char = getChar()
                local hum = char:FindFirstChildOfClass("Humanoid")
                return hum and hum.Health > 0
            end
            local function teleportTo(target)
                local char = getChar()
                local hrp = char:FindFirstChild("HumanoidRootPart")
                if hrp and target then
                    if target:FindFirstChild("HumanoidRootPart") then
                        hrp.CFrame = target.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0)
                    elseif target:IsA("BasePart") then
                        hrp.CFrame = target.CFrame + Vector3.new(0, 3, 0)
                    end
                end
            end
            while AutoFarmMobs do
                ErrorLabel.Visible = false
                if not isAlive() then task.wait(1) continue end
                local nearestMob, minDist
                local folder = workspace:FindFirstChild("__GAME_CONTENT")
                if folder and folder:FindFirstChild("Mobs") and folder.Mobs:FindFirstChild("Halloween") then
                    for _,mob in pairs(folder.Mobs.Halloween:GetDescendants()) do
                        if mob:FindFirstChild("Humanoid") and mob:FindFirstChild("HumanoidRootPart") and mob.Humanoid.Health > 0 then
                            local dist = (getChar().HumanoidRootPart.Position - mob.HumanoidRootPart.Position).Magnitude
                            if not minDist or dist < minDist then
                                minDist = dist
                                nearestMob = mob
                            end
                        end
                    end
                end
                if nearestMob then
                    teleportTo(nearestMob)
                    task.wait(0.2)
                    local char = getChar()
                    local tool = char:FindFirstChildOfClass("Tool")
                    if tool then pcall(function() tool:Activate() end) end
                else
                    ErrorLabel.Text = "Nenhum mob Halloween encontrado!"
                    ErrorLabel.Visible = true
                end
                for i=1,6 do
                    if not AutoFarmMobs then break end
                    task.wait(0.1)
                end
            end
            mobsThread = nil
            setMobToggleVisual(false)
        end)
    elseif not AutoFarmMobs and mobsThread then
        mobsThread = nil
    end
end)

CandyToggle.MouseButton1Click:Connect(function()
    AutoFarmCandy = not AutoFarmCandy
    setCandyToggleVisual(AutoFarmCandy)
    if AutoFarmCandy and not candyThread then
        candyThread = task.spawn(function()
            local player = game.Players.LocalPlayer
            local function getChar()
                return player.Character or player.CharacterAdded:Wait()
            end
            local function isAlive()
                local char = getChar()
                local hum = char:FindFirstChildOfClass("Humanoid")
                return hum and hum.Health > 0
            end
            local function teleportTo(target)
                local char = getChar()
                local hrp = char:FindFirstChild("HumanoidRootPart")
                if hrp and target and target:IsA("BasePart") then
                    hrp.CFrame = target.CFrame + Vector3.new(0, 3, 0)
                end
            end
            while AutoFarmCandy do
                ErrorLabel.Visible = false
                if not isAlive() then task.wait(1) continue end
                local candiesFolder = workspace:FindFirstChild("__GAME_CONTENT")
                local nearest, mindist
                if candiesFolder and candiesFolder:FindFirstChild("PlayersCandy") then
                    for _,candy in pairs(candiesFolder.PlayersCandy:GetDescendants()) do
                        if candy:IsA("BasePart") then
                            local dist = (getChar().HumanoidRootPart.Position - candy.Position).Magnitude
                            if not mindist or dist < mindist then
                                mindist = dist
                                nearest = candy
                            end
                        end
                    end
                end
                if nearest then
                    teleportTo(nearest)
                else
                    ErrorLabel.Text = "Nenhum Candy Corn encontrado!"
                    ErrorLabel.Visible = true
                end
                for i=1,7 do
                    if not AutoFarmCandy then break end
                    task.wait(0.1)
                end
            end
            candyThread = nil
            setCandyToggleVisual(false)
        end)
    elseif not AutoFarmCandy and candyThread then
        candyThread = nil
    end
end)

local function hideGUI()
    Frame.Visible = false
    ShowBtn.Visible = true
end

CloseBtn.MouseButton1Click:Connect(hideGUI)
HideBtn.MouseButton1Click:Connect(hideGUI)

ShowBtn.MouseButton1Click:Connect(function()
    Frame.Visible = true
    ShowBtn.Visible = false
end)

UIS.InputBegan:Connect(function(input, gp)
    if not gp and input.KeyCode == Enum.KeyCode.RightShift then
        Frame.Visible = not Frame.Visible
        ShowBtn.Visible = not Frame.Visible
    end
end)
