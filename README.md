local Players = game:GetService(“Players”)
local RunService = game:GetService(“RunService”)
local UserInputService = game:GetService(“UserInputService”)
local TweenService = game:GetService(“TweenService”)
local LocalPlayer = Players.LocalPlayer

local Settings = {Enabled = false, HitboxSize = 10, Allies = {}, ShowHitbox = false, TeamCheck = false}
local OriginalSizes = {}
local ExpandedPlayers = {}
local GUIVisible = false
local HitboxVisuals = {}

local ScreenGui = Instance.new(“ScreenGui”)
ScreenGui.Name = “SapekinhaHitbox”
ScreenGui.ResetOnSpawn = false

local ToggleButton = Instance.new(“TextButton”)
ToggleButton.Size = UDim2.new(0, 70, 0, 70)
ToggleButton.Position = UDim2.new(0, 20, 0.5, -35)
ToggleButton.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
ToggleButton.BorderSizePixel = 0
ToggleButton.Text = “💀”
ToggleButton.TextSize = 35
ToggleButton.TextColor3 = Color3.fromRGB(255, 0, 0)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.Parent = ScreenGui

local ToggleCorner = Instance.new(“UICorner”)
ToggleCorner.CornerRadius = UDim.new(1, 0)
ToggleCorner.Parent = ToggleButton

local ToggleStroke = Instance.new(“UIStroke”)
ToggleStroke.Color = Color3.fromRGB(255, 0, 0)
ToggleStroke.Thickness = 3
ToggleStroke.Parent = ToggleButton

local MainFrame = Instance.new(“Frame”)
MainFrame.Size = UDim2.new(0, 420, 0, 580)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -290)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.BorderSizePixel = 0
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new(“UICorner”)
MainCorner.CornerRadius = UDim.new(0, 16)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new(“UIStroke”)
MainStroke.Color = Color3.fromRGB(255, 0, 0)
MainStroke.Thickness = 2
MainStroke.Parent = MainFrame

local TopBar = Instance.new(“Frame”)
TopBar.Size = UDim2.new(1, 0, 0, 80)
TopBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame

local TopBarCorner = Instance.new(“UICorner”)
TopBarCorner.CornerRadius = UDim.new(0, 16)
TopBarCorner.Parent = TopBar

local TopBarGradient = Instance.new(“UIGradient”)
TopBarGradient.Color = ColorSequence.new{ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 20, 20)), ColorSequenceKeypoint.new(0.5, Color3.fromRGB(150, 0, 0)), ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 20, 20))}
TopBarGradient.Rotation = 90
TopBarGradient.Parent = TopBar

local SkullIcon = Instance.new(“TextLabel”)
SkullIcon.Size = UDim2.new(0, 70, 1, 0)
SkullIcon.Position = UDim2.new(0, 15, 0, 0)
SkullIcon.BackgroundTransparency = 1
SkullIcon.Text = “💀”
SkullIcon.TextSize = 45
SkullIcon.TextColor3 = Color3.fromRGB(10, 10, 10)
SkullIcon.Font = Enum.Font.GothamBold
SkullIcon.Parent = TopBar

local TitleLabel = Instance.new(“TextLabel”)
TitleLabel.Size = UDim2.new(1, -180, 0, 40)
TitleLabel.Position = UDim2.new(0, 90, 0, 10)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = “SAPEKINHA SCRIPTS”
TitleLabel.TextSize = 22
TitleLabel.TextColor3 = Color3.fromRGB(10, 10, 10)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TopBar

local SubtitleLabel = Instance.new(“TextLabel”)
SubtitleLabel.Size = UDim2.new(1, -180, 0, 25)
SubtitleLabel.Position = UDim2.new(0, 90, 0, 50)
SubtitleLabel.BackgroundTransparency = 1
SubtitleLabel.Text = “Hitbox Expander v2.0”
SubtitleLabel.TextSize = 14
SubtitleLabel.TextColor3 = Color3.fromRGB(10, 10, 10)
SubtitleLabel.Font = Enum.Font.Gotham
SubtitleLabel.TextXAlignment = Enum.TextXAlignment.Left
SubtitleLabel.TextTransparency = 0.3
SubtitleLabel.Parent = TopBar

local CloseBtn = Instance.new(“TextButton”)
CloseBtn.Size = UDim2.new(0, 55, 0, 55)
CloseBtn.Position = UDim2.new(1, -70, 0, 12)
CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
CloseBtn.BorderSizePixel = 0
CloseBtn.Text = “✕”
CloseBtn.TextSize = 28
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.Parent = TopBar

local CloseBtnCorner = Instance.new(“UICorner”)
CloseBtnCorner.CornerRadius = UDim.new(1, 0)
CloseBtnCorner.Parent = CloseBtn

local ContentFrame = Instance.new(“ScrollingFrame”)
ContentFrame.Size = UDim2.new(1, -30, 1, -100)
ContentFrame.Position = UDim2.new(0, 15, 0, 90)
ContentFrame.BackgroundTransparency = 1
ContentFrame.BorderSizePixel = 0
ContentFrame.ScrollBarThickness = 5
ContentFrame.ScrollBarImageColor3 = Color3.fromRGB(255, 0, 0)
ContentFrame.CanvasSize = UDim2.new(0, 0, 0, 800)
ContentFrame.Parent = MainFrame

local StatusCard = Instance.new(“Frame”)
StatusCard.Size = UDim2.new(1, 0, 0, 60)
StatusCard.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
StatusCard.BorderSizePixel = 0
StatusCard.Parent = ContentFrame

local StatusCorner = Instance.new(“UICorner”)
StatusCorner.CornerRadius = UDim.new(0, 12)
StatusCorner.Parent = StatusCard

local StatusIcon = Instance.new(“TextLabel”)
StatusIcon.Size = UDim2.new(0, 50, 1, 0)
StatusIcon.Position = UDim2.new(0, 10, 0, 0)
StatusIcon.BackgroundTransparency = 1
StatusIcon.Text = “⚡”
StatusIcon.TextSize = 30
StatusIcon.TextColor3 = Color3.fromRGB(255, 100, 100)
StatusIcon.Font = Enum.Font.GothamBold
StatusIcon.Parent = StatusCard

local StatusText = Instance.new(“TextLabel”)
StatusText.Size = UDim2.new(1, -70, 1, 0)
StatusText.Position = UDim2.new(0, 60, 0, 0)
StatusText.BackgroundTransparency = 1
StatusText.Text = “Status: Desativado”
StatusText.TextSize = 18
StatusText.TextColor3 = Color3.fromRGB(255, 100, 100)
StatusText.Font = Enum.Font.GothamBold
StatusText.TextXAlignment = Enum.TextXAlignment.Left
StatusText.Parent = StatusCard

local function createButton(parent, yPos, text, icon)
local btn = Instance.new(“TextButton”)
btn.Size = UDim2.new(1, 0, 0, 55)
btn.Position = UDim2.new(0, 0, 0, yPos)
btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
btn.BorderSizePixel = 0
btn.Text = “”
btn.Parent = parent

```
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = btn

local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(255, 0, 0)
stroke.Thickness = 2
stroke.Transparency = 0.5
stroke.Parent = btn

local iconLabel = Instance.new("TextLabel")
iconLabel.Size = UDim2.new(0, 50, 1, 0)
iconLabel.Position = UDim2.new(0, 10, 0, 0)
iconLabel.BackgroundTransparency = 1
iconLabel.Text = icon
iconLabel.TextSize = 28
iconLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
iconLabel.Font = Enum.Font.GothamBold
iconLabel.Parent = btn

local textLabel = Instance.new("TextLabel")
textLabel.Size = UDim2.new(1, -70, 1, 0)
textLabel.Position = UDim2.new(0, 60, 0, 0)
textLabel.BackgroundTransparency = 1
textLabel.Text = text
textLabel.TextSize = 16
textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
textLabel.Font = Enum.Font.GothamBold
textLabel.TextXAlignment = Enum.TextXAlignment.Left
textLabel.Parent = btn

return btn, textLabel, stroke
```

end

local ActivateBtn, ActivateText, ActivateStroke = createButton(ContentFrame, 70, “ATIVAR HITBOX”, “🎯”)
local ShowBoxBtn, ShowBoxText = createButton(ContentFrame, 135, “Mostrar Hitbox: OFF”, “👁️”)
local TeamCheckBtn, TeamCheckText = createButton(ContentFrame, 200, “Verificar Time: OFF”, “👥”)

local SizeCard = Instance.new(“Frame”)
SizeCard.Size = UDim2.new(1, 0, 0, 110)
SizeCard.Position = UDim2.new(0, 0, 0, 265)
SizeCard.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
SizeCard.BorderSizePixel = 0
SizeCard.Parent = ContentFrame

local SizeCardCorner = Instance.new(“UICorner”)
SizeCardCorner.CornerRadius = UDim.new(0, 12)
SizeCardCorner.Parent = SizeCard

local SizeLabel = Instance.new(“TextLabel”)
SizeLabel.Size = UDim2.new(1, -20, 0, 35)
SizeLabel.Position = UDim2.new(0, 10, 0, 10)
SizeLabel.BackgroundTransparency = 1
SizeLabel.Text = “📏 Tamanho da Hitbox: “ .. Settings.HitboxSize
SizeLabel.TextSize = 16
SizeLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
SizeLabel.Font = Enum.Font.GothamBold
SizeLabel.TextXAlignment = Enum.TextXAlignment.Left
SizeLabel.Parent = SizeCard

local SizeInput = Instance.new(“TextBox”)
SizeInput.Size = UDim2.new(1, -20, 0, 50)
SizeInput.Position = UDim2.new(0, 10, 0, 50)
SizeInput.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
SizeInput.BorderSizePixel = 0
SizeInput.PlaceholderText = “Digite (5-100)”
SizeInput.Text = tostring(Settings.HitboxSize)
SizeInput.TextSize = 16
SizeInput.TextColor3 = Color3.fromRGB(255, 255, 255)
SizeInput.Font = Enum.Font.Gotham
SizeInput.Parent = SizeCard

local SizeInputCorner = Instance.new(“UICorner”)
SizeInputCorner.CornerRadius = UDim.new(0, 10)
SizeInputCorner.Parent = SizeInput

local SizeInputStroke = Instance.new(“UIStroke”)
SizeInputStroke.Color = Color3.fromRGB(255, 0, 0)
SizeInputStroke.Thickness = 2
SizeInputStroke.Parent = SizeInput

local AlliesTitle = Instance.new(“TextLabel”)
AlliesTitle.Size = UDim2.new(1, 0, 0, 40)
AlliesTitle.Position = UDim2.new(0, 0, 0, 395)
AlliesTitle.BackgroundTransparency = 1
AlliesTitle.Text = “👥 GERENCIAR ALIADOS”
AlliesTitle.TextSize = 18
AlliesTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
AlliesTitle.Font = Enum.Font.GothamBold
AlliesTitle.TextXAlignment = Enum.TextXAlignment.Left
AlliesTitle.Parent = ContentFrame

local AlliesFrame = Instance.new(“Frame”)
AlliesFrame.Size = UDim2.new(1, 0, 0, 200)
AlliesFrame.Position = UDim2.new(0, 0, 0, 445)
AlliesFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
AlliesFrame.BorderSizePixel = 0
AlliesFrame.Parent = ContentFrame

local AlliesCorner = Instance.new(“UICorner”)
AlliesCorner.CornerRadius = UDim.new(0, 12)
AlliesCorner.Parent = AlliesFrame

local AlliesStroke = Instance.new(“UIStroke”)
AlliesStroke.Color = Color3.fromRGB(255, 0, 0)
AlliesStroke.Thickness = 2
AlliesStroke.Parent = AlliesFrame

local PlayerScroll = Instance.new(“ScrollingFrame”)
PlayerScroll.Size = UDim2.new(1, -10, 1, -10)
PlayerScroll.Position = UDim2.new(0, 5, 0, 5)
PlayerScroll.BackgroundTransparency = 1
PlayerScroll.BorderSizePixel = 0
PlayerScroll.ScrollBarThickness = 5
PlayerScroll.ScrollBarImageColor3 = Color3.fromRGB(255, 0, 0)
PlayerScroll.Parent = AlliesFrame

local PlayerLayout = Instance.new(“UIListLayout”)
PlayerLayout.Padding = UDim.new(0, 8)
PlayerLayout.Parent = PlayerScroll

local function animateHover(button, normalColor, hoverColor)
button.MouseEnter:Connect(function()
TweenService:Create(button, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {BackgroundColor3 = hoverColor}):Play()
end)
button.MouseLeave:Connect(function()
TweenService:Create(button, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {BackgroundColor3 = normalColor}):Play()
end)
end

animateHover(ToggleButton, Color3.fromRGB(10, 10, 10), Color3.fromRGB(20, 20, 20))
animateHover(CloseBtn, Color3.fromRGB(200, 0, 0), Color3.fromRGB(255, 50, 50))
animateHover(ActivateBtn, Color3.fromRGB(30, 30, 30), Color3.fromRGB(50, 50, 50))
animateHover(ShowBoxBtn, Color3.fromRGB(30, 30, 30), Color3.fromRGB(50, 50, 50))
animateHover(TeamCheckBtn, Color3.fromRGB(30, 30, 30), Color3.fromRGB(50, 50, 50))

TweenService:Create(SkullIcon, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.InOut, -1, true), {TextSize = 50}):Play()

local function isAlly(userId)
return table.find(Settings.Allies, userId)
end

local function isTeammate(player)
if not Settings.TeamCheck then return false end
if not LocalPlayer.Team then return false end
return player.Team == LocalPlayer.Team
end

local function updatePlayerList()
for _, child in pairs(PlayerScroll:GetChildren()) do
if child:IsA(“Frame”) then child:Destroy() end
end

```
for _, player in pairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        local PlayerCard = Instance.new("Frame")
        PlayerCard.Size = UDim2.new(1, -5, 0, 45)
        PlayerCard.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        PlayerCard.BorderSizePixel = 0
        PlayerCard.Parent = PlayerScroll
        
        local CardCorner = Instance.new("UICorner")
        CardCorner.CornerRadius = UDim.new(0, 10)
        CardCorner.Parent = PlayerCard
        
        local PlayerName = Instance.new("TextLabel")
        PlayerName.Size = UDim2.new(0.45, 0, 1, 0)
        PlayerName.Position = UDim2.new(0, 45, 0, 0)
        PlayerName.BackgroundTransparency = 1
        PlayerName.Text = "👤 " .. player.Name
        PlayerName.TextSize = 14
        PlayerName.TextColor3 = Color3.fromRGB(255, 255, 255)
        PlayerName.Font = Enum.Font.Gotham
        PlayerName.TextXAlignment = Enum.TextXAlignment.Left
        PlayerName.TextTruncate = Enum.TextTruncate.AtEnd
        PlayerName.Parent = PlayerCard
        
        local AllyBtn = Instance.new("TextButton")
        AllyBtn.Size = UDim2.new(0.35, 0, 0, 35)
        AllyBtn.Position = UDim2.new(0.62, 0, 0.5, -17)
        AllyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        AllyBtn.BorderSizePixel = 0
        AllyBtn.Text = "Aliar"
        AllyBtn.TextSize = 13
        AllyBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
        AllyBtn.Font = Enum.Font.GothamBold
        AllyBtn.Parent = PlayerCard
        
        local AllyBtnCorner = Instance.new("UICorner")
        AllyBtnCorner.CornerRadius = UDim.new(0, 8)
        AllyBtnCorner.Parent = AllyBtn
        
        local function updateAllyBtn()
            if isAlly(player.UserId) then
                AllyBtn.Text = "✓ Aliado"
                AllyBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
            else
                AllyBtn.Text = "Aliar"
                AllyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            end
        end
        
        updateAllyBtn()
        animateHover(AllyBtn, AllyBtn.BackgroundColor3, Color3.fromRGB(70, 70, 70))
        
        AllyBtn.MouseButton1Click:Connect(function()
            local idx = table.find(Settings.Allies, player.UserId)
            if idx then table.remove(Settings.Allies, idx) else table.insert(Settings.Allies, player.UserId) end
            updateAllyBtn()
        end)
    end
end

PlayerScroll.CanvasSize = UDim2.new(0, 0, 0, PlayerLayout.AbsoluteContentSize.Y + 10)
```

end

local function toggleGUI()
GUIVisible = not GUIVisible
if GUIVisible then
MainFrame.Visible = true
MainFrame.Size = UDim2.new(0, 0, 0, 0)
TweenService:Create(MainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = UDim2.new(0, 420, 0, 580), Position = UDim2.new(0.5, -210, 0.5, -290)}):Play()
else
TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0)}):Play()
task.wait(0.3)
MainFrame.Visible = false
end
end

local function toggleHitbox()
Settings.Enabled = not Settings.Enabled
if Settings.Enabled then
StatusText.Text = “Status: Ativado”
StatusText.TextColor3 = Color3.fromRGB(100, 255, 100)
StatusIcon.TextColor3 = Color3.fromRGB(100, 255, 100)
ActivateText.Text = “DESATIVAR HITBOX”
TweenService:Create(ActivateBtn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(200, 0, 0)}):Play()
else
StatusText.Text = “Status: Desativado”
StatusText.TextColor3 = Color3.fromRGB(255, 100, 100)
StatusIcon.TextColor3 = Color3.fromRGB(255, 100, 100)
ActivateText.Text = “ATIVAR HITBOX”
TweenService:Create(ActivateBtn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
for userId, _ in pairs(ExpandedPlayers) do
local player = Players:GetPlayerByUserId(userId)
if player and player.Character then
local part = player.Character:FindFirstChild(“Torso”) or player.Character:FindFirstChild(“UpperTorso”)
if part then
local key = userId .. “_torso”
if OriginalSizes[key] then
part.Size = OriginalSizes[key].Size
part.CanCollide = OriginalSizes[key].CanCollide
part.Massless = OriginalSizes[key].Massless
end
if HitboxVisuals[key] then
HitboxVisuals[key]:Destroy()
HitboxVisuals[key] = nil
end
end
end
end
ExpandedPlayers = {}
end
end

local function toggleShowHitbox()
Settings.ShowHitbox = not Settings.ShowHitbox
if Settings.ShowHitbox then
ShowBoxText.Text = “Mostrar Hitbox: ON”
TweenService:Create(ShowBoxBtn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(0, 150, 100)}):Play()
else
ShowBoxText.Text = “Mostrar Hitbox: OFF”
TweenService:Create(ShowBoxBtn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
for _, visual in pairs(HitboxVisuals) do
if visual then visual:Destroy() end
end
HitboxVisuals = {}
end
end

local function toggleTeamCheck()
Settings.TeamCheck = not Settings.TeamCheck
if Settings.TeamCheck then
TeamCheckText.Text = “Verificar Time: ON”
TweenService:Create(TeamCheckBtn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(0, 150, 200)}):Play()
else
TeamCheckText.Text = “Verificar Time: OFF”
TweenService:Create(TeamCheckBtn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
end
end

ToggleButton.MouseButton1Click:Connect(toggleGUI)
CloseBtn.MouseButton1Click:Connect(toggleGUI)
ActivateBtn.MouseButton1Click:Connect(toggleHitbox)
ShowBoxBtn.MouseButton1Click:Connect(toggleShowHitbox)
TeamCheckBtn.MouseButton1Click:Connect(toggleTeamCheck)

SizeInput.FocusLost:Connect(function()
local newSize = tonumber(SizeInput.Text)
if newSize and newSize >= 5 and newSize <= 100 then
Settings.HitboxSize = newSize
SizeLabel.Text = “📏 Tamanho da Hitbox: “ .. newSize
for userId, _ in pairs(ExpandedPlayers) do ExpandedPlayers[userId] = nil end
else
SizeInput.Text = tostring(Settings.HitboxSize)
end
end)

UserInputService.InputBegan:Connect(function(input, gpe)
if gpe then return end
if input.KeyCode == Enum.KeyCode.P then toggleHitbox() end
end)

local function expandHitbox(player)
if player == LocalPlayer or isAlly(player.UserId) or isTeammate(player) or ExpandedPlayers[player.UserId] then return end
local char = player.Character
if not char then return end
local part = char:FindFirstChild(“Torso”) or char:FindFirstChild(“UpperTorso”)
if not part then return end
local key = player.UserId .. “_torso”
if not OriginalSizes[key] then
OriginalSizes[key] = {Size = part.Size, CanCollide = part.CanCollide, Massless = part.Massless}
end
part.Size = Vector3.new(Settings.HitboxSize, Settings.HitboxSize, Settings.HitboxSize)
part.CanCollide = false
part.Massless = true
if Settings.ShowHitbox and not HitboxVisuals[key] then
local box = Instance.new(“BoxHandleAdornment”)
box.Size = Vector3.new(Settings.HitboxSize, Settings.HitboxSize, Settings.HitboxSize)
box.Color3 = Color3.fromRGB(255, 0, 0)
box.Transparency = 0.7
box.AlwaysOnTop = true
box.Adornee = part
box.Parent = part
HitboxVisuals[key] = box
end
ExpandedPlayers[player.UserId] = true
end

RunService.Heartbeat:Connect(function()
if not Settings.Enabled then return end
for _, player in pairs(Players:GetPlayers()) do
if player ~= LocalPlayer and not isAlly(player.UserId) and not isTeammate(player) then
expandHitbox(player)
end
end
end)

Players.PlayerAdded:Connect(function() task.wait(0.5) updatePlayerList() end)
Players.PlayerRemoving:Connect(function(player)
ExpandedPlayers[player.UserId] = nil
OriginalSizes[player.UserId .. “_torso”] = nil
local key = player.UserId .. “_torso”
if HitboxVisuals[key] then HitboxVisuals[key]:Destroy() HitboxVisuals[key] = nil end
local idx = table.find(Settings.Allies, player.UserId)
if idx then table.remove(Settings.Allies, idx) end
task.wait(0.5) updatePlayerList()
end)

task.spawn(function()
while task.wait(5) do updatePlayerList() end
end)

ScreenGui.Parent = gethui and gethui() or game:GetService(“CoreGui”)
updatePlayerList()
print(“💀 SAPEKINHA SCRIPTS carregado!”)
