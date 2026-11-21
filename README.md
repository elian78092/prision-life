-- [[ SCRIPTS FREE - PRISON LIFE ]] --

local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local TextChatService = game:GetService("TextChatService")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local CoreGui = game:GetService("CoreGui")

local Camera = Workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer
local IsMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

local DISCORD_INVITE = "https://discord.gg/nCFguw5sW"

local function GetParentGui()
    local ok, res = pcall(function()
        local f = Instance.new("Folder")
        f.Parent = CoreGui
        f:Destroy()
        return CoreGui
    end)
    if ok and res then
        return res
    end
    return LocalPlayer:WaitForChild("PlayerGui")
end

local ParentGui = GetParentGui()
if ParentGui:FindFirstChild("ScripsFreeGui") then
    pcall(function() ParentGui.ScripsFreeGui:Destroy() end)
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ScripsFreeGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = ParentGui

local Theme = {
    BgMain = Color3.fromRGB(11, 13, 18),
    BgCard = Color3.fromRGB(19, 22, 31),
    BgSidebar = Color3.fromRGB(14, 17, 24),
    Accent = Color3.fromRGB(235, 55, 65),
    Discord = Color3.fromRGB(88, 101, 242),
    TextMain = Color3.fromRGB(242, 245, 252),
    TextMuted = Color3.fromRGB(150, 158, 176),
    BorderIdle = Color3.fromRGB(42, 49, 66),
    BorderActive = Color3.fromRGB(255, 115, 125)
}

local function StyleRounded(obj, radiusPx, borderColor, borderThickness)
    pcall(function()
        obj.BorderSizePixel = 0
        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, radiusPx)
        corner.Parent = obj

        local stroke = Instance.new("UIStroke")
        stroke.Color = borderColor or Theme.BorderIdle
        stroke.Thickness = borderThickness or 1.4
        stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
        stroke.LineJoinMode = Enum.LineJoinMode.Round
        stroke.Parent = obj
    end)
end

-- ================================================================= --
--                     BYPASS Y LIMPIEZA DE SEGURIDAD                --
-- ================================================================= --
local function RunAntiCheatBypass()
    pcall(function()
        local mt = getrawmetatable and getrawmetatable(game)
        if mt and setreadonly then
            local oldNamecall = mt.__namecall
            setreadonly(mt, false)
            mt.__namecall = newcclosure(function(self, ...)
                local method = getnamecallmethod and getnamecallmethod() or ""
                local nameLower = string.lower(tostring(self))
                if method == "Kick" or method == "kick" then
                    return nil
                end
                if (method == "FireServer" or method == "InvokeServer") and (
                    string.find(nameLower, "anticheat") or
                    string.find(nameLower, "ban") or
                    string.find(nameLower, "exploit") or
                    string.find(nameLower, "cheat") or
                    string.find(nameLower, "check")
                ) then
                    return nil
                end
                return oldNamecall(self, ...)
            end)
            setreadonly(mt, true)
        end
    end)

    pcall(function()
        for _, conn in ipairs(getconnections and getconnections(LocalPlayer.Idled) or {}) do
            if conn.Disable then conn:Disable() end
        end
    end)
end

RunAntiCheatBypass()

-- ================================================================= --
--                     1. PANTALLA DE CARGA (15s)                    --
-- ================================================================= --
local LoadScreen = Instance.new("Frame")
LoadScreen.Size = IsMobile and UDim2.new(0, 320, 0, 165) or UDim2.new(0, 400, 0, 185)
LoadScreen.Position = UDim2.new(0.5, 0, 0.5, 0)
LoadScreen.AnchorPoint = Vector2.new(0.5, 0.5)
LoadScreen.BackgroundColor3 = Theme.BgMain
LoadScreen.ZIndex = 30
LoadScreen.Parent = ScreenGui
StyleRounded(LoadScreen, 16, Theme.Accent, 2)

local LoadTitle = Instance.new("TextLabel")
LoadTitle.Size = UDim2.new(1, -28, 0, 28)
LoadTitle.Position = UDim2.new(0, 14, 0, 16)
LoadTitle.BackgroundTransparency = 1
LoadTitle.Text = "Scrips Free"
LoadTitle.TextColor3 = Theme.TextMain
LoadTitle.TextSize = IsMobile and 14 or 16
LoadTitle.Font = Enum.Font.GothamBold
LoadTitle.TextXAlignment = Enum.TextXAlignment.Left
LoadTitle.ZIndex = 31
LoadTitle.Parent = LoadScreen

local LoadSub = Instance.new("TextLabel")
LoadSub.Size = UDim2.new(1, -28, 0, 20)
LoadSub.Position = UDim2.new(0, 14, 0, 42)
LoadSub.BackgroundTransparency = 1
LoadSub.Text = "Preparando bypass y módulos de combate..."
LoadSub.TextColor3 = Theme.TextMuted
LoadSub.TextSize = IsMobile and 10.5 or 11.5
LoadSub.Font = Enum.Font.Gotham
LoadSub.TextXAlignment = Enum.TextXAlignment.Left
LoadSub.ZIndex = 31
LoadSub.Parent = LoadScreen

local BarBG = Instance.new("Frame")
BarBG.Size = UDim2.new(1, -28, 0, 12)
BarBG.Position = UDim2.new(0, 14, 0, 82)
BarBG.BackgroundColor3 = Theme.BgCard
BarBG.ZIndex = 31
BarBG.Parent = LoadScreen
StyleRounded(BarBG, 6, Theme.BorderIdle, 1)

local BarFill = Instance.new("Frame")
BarFill.Size = UDim2.new(0, 0, 1, 0)
BarFill.BackgroundColor3 = Theme.Accent
BarFill.BorderSizePixel = 0
BarFill.ZIndex = 32
BarFill.Parent = BarBG
Instance.new("UICorner", BarFill).CornerRadius = UDim.new(0, 6)

local LoadLog = Instance.new("TextLabel")
LoadLog.Size = UDim2.new(1, -28, 0, 42)
LoadLog.Position = UDim2.new(0, 14, 0, 108)
LoadLog.BackgroundTransparency = 1
LoadLog.Text = "Iniciando... (15s)"
LoadLog.TextColor3 = Theme.TextMuted
LoadLog.TextSize = IsMobile and 10 or 11
LoadLog.Font = Enum.Font.Code
LoadLog.TextWrapped = true
LoadLog.TextXAlignment = Enum.TextXAlignment.Left
LoadLog.ZIndex = 31
LoadLog.Parent = LoadScreen

-- ================================================================= --
--                    2. TARJETA DE NOVEDADES                        --
-- ================================================================= --
local NewsFrame = Instance.new("Frame")
NewsFrame.Size = IsMobile and UDim2.new(0, 355, 0, 245) or UDim2.new(0, 430, 0, 265)
NewsFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
NewsFrame.AnchorPoint = Vector2.new(0.5, 0.5)
NewsFrame.BackgroundColor3 = Theme.BgMain
NewsFrame.Visible = false
NewsFrame.ZIndex = 25
NewsFrame.Parent = ScreenGui
StyleRounded(NewsFrame, 16, Theme.Accent, 2)

local NewsHeader = Instance.new("TextLabel")
NewsHeader.Size = UDim2.new(1, -32, 0, 36)
NewsHeader.Position = UDim2.new(0, 16, 0, 14)
NewsHeader.BackgroundTransparency = 1
NewsHeader.Text = "Scrips Free - Novedades"
NewsHeader.TextColor3 = Theme.TextMain
NewsHeader.TextSize = IsMobile and 14 or 16
NewsHeader.Font = Enum.Font.GothamBold
NewsHeader.TextXAlignment = Enum.TextXAlignment.Left
NewsHeader.ZIndex = 26
NewsHeader.Parent = NewsFrame

local NewsBody = Instance.new("TextLabel")
NewsBody.Size = UDim2.new(1, -32, 0, 125)
NewsBody.Position = UDim2.new(0, 16, 0, 56)
NewsBody.BackgroundTransparency = 1
NewsBody.Text = "• Aimbot optimizado listo en Combat.\n• Luz brillante sobre el nombre seguida de RGB suave.\n• Rayo animado en el menú cada 20s.\n• Botón directo para copiar el enlace de Discord."
NewsBody.TextColor3 = Theme.TextMuted
NewsBody.TextSize = IsMobile and 11.5 or 13
NewsBody.Font = Enum.Font.GothamMedium
NewsBody.TextWrapped = true
NewsBody.TextXAlignment = Enum.TextXAlignment.Left
NewsBody.TextYAlignment = Enum.TextYAlignment.Top
NewsBody.ZIndex = 26
NewsBody.Parent = NewsFrame

local AcceptBtn = Instance.new("TextButton")
AcceptBtn.Size = UDim2.new(1, -32, 0, 42)
AcceptBtn.Position = UDim2.new(0, 16, 1, -56)
AcceptBtn.BackgroundColor3 = Theme.Accent
AcceptBtn.Text = "ENTRAR AL MENÚ"
AcceptBtn.TextColor3 = Theme.TextMain
AcceptBtn.TextSize = IsMobile and 12 or 13.5
AcceptBtn.Font = Enum.Font.GothamBold
AcceptBtn.ZIndex = 26
AcceptBtn.Parent = NewsFrame
StyleRounded(AcceptBtn, 10, Theme.BorderActive, 1.4)

-- ================================================================= --
--                      3. MENÚ PRINCIPAL                             --
-- ================================================================= --
local MainSize = IsMobile and UDim2.new(0, 460, 0, 305) or UDim2.new(0, 820, 0, 540)
local HeaderH = IsMobile and 42 or 56
local SidebarW = IsMobile and 116 or 180

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = MainSize
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.BackgroundColor3 = Theme.BgMain
MainFrame.ClipsDescendants = true
MainFrame.Visible = false
MainFrame.ZIndex = 10
MainFrame.Parent = ScreenGui
StyleRounded(MainFrame, 16, Theme.Accent, 2)

-- Rayo que cruza el menú cada 20 segundos
local LightningBeam = Instance.new("Frame")
LightningBeam.Name = "MenuLightning"
LightningBeam.Size = UDim2.new(0, 14, 1.8, 0)
LightningBeam.Position = UDim2.new(-0.25, 0, -0.4, 0)
LightningBeam.Rotation = 24
LightningBeam.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
LightningBeam.BackgroundTransparency = 1
LightningBeam.BorderSizePixel = 0
LightningBeam.ZIndex = 45
LightningBeam.Parent = MainFrame

local LightningCore = Instance.new("Frame")
LightningCore.Size = UDim2.new(0.35, 0, 1, 0)
LightningCore.Position = UDim2.new(0.325, 0, 0, 0)
LightningCore.BackgroundColor3 = Color3.fromRGB(140, 215, 255)
LightningCore.BackgroundTransparency = 1
LightningCore.BorderSizePixel = 0
LightningCore.ZIndex = 46
LightningCore.Parent = LightningBeam

local function TriggerMenuLightning()
    pcall(function()
        if not MainFrame.Visible or not MainFrame.Parent then return end
        LightningBeam.Position = UDim2.new(-0.2, 0, -0.4, 0)
        LightningBeam.BackgroundTransparency = 0.15
        LightningCore.BackgroundTransparency = 0.1

        local sweep = TweenService:Create(
            LightningBeam,
            TweenInfo.new(0.35, Enum.EasingStyle.Sine, Enum.EasingDirection.Out),
            {Position = UDim2.new(1.2, 0, -0.4, 0)}
        )
        sweep:Play()
        sweep.Completed:Wait()

        LightningBeam.BackgroundTransparency = 1
        LightningCore.BackgroundTransparency = 1
    end)
end

task.spawn(function()
    while ScreenGui.Parent do
        task.wait(20)
        if ScreenGui.Parent and MainFrame.Visible then
            TriggerMenuLightning()
        end
    end
end)

local function OpenMainMenu()
    pcall(function()
        MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
        MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
        MainFrame.Size = UDim2.new(
            MainSize.X.Scale,
            math.floor(MainSize.X.Offset * 0.9),
            MainSize.Y.Scale,
            math.floor(MainSize.Y.Offset * 0.9)
        )
        MainFrame.Visible = true
        TweenService:Create(
            MainFrame,
            TweenInfo.new(0.26, Enum.EasingStyle.Back, Enum.EasingDirection.Out),
            {Size = MainSize}
        ):Play()
    end)
end

AcceptBtn.MouseButton1Click:Connect(function()
    if NewsFrame and NewsFrame.Parent then
        NewsFrame.Visible = false
        pcall(function() NewsFrame:Destroy() end)
    end
    OpenMainMenu()
end)

local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, -12, 0, HeaderH - 6)
Header.Position = UDim2.new(0, 6, 0, 6)
Header.BackgroundColor3 = Theme.BgSidebar
Header.ZIndex = 12
Header.Parent = MainFrame
StyleRounded(Header, 12, Theme.BorderIdle, 1.2)

local TitleHolder = Instance.new("Frame")
TitleHolder.Size = UDim2.new(0.45, 0, 1, 0)
TitleHolder.Position = UDim2.new(0, 14, 0, 0)
TitleHolder.BackgroundTransparency = 1
TitleHolder.ClipsDescendants = true
TitleHolder.ZIndex = 13
TitleHolder.Parent = Header

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, 0, 1, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "Scrips Free"
TitleLabel.TextColor3 = Theme.TextMain
TitleLabel.TextSize = IsMobile and 13 or 17
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.ZIndex = 13
TitleLabel.Parent = TitleHolder

local TitleGlowStroke = Instance.new("UIStroke")
TitleGlowStroke.Color = Color3.fromRGB(255, 255, 255)
TitleGlowStroke.Thickness = 0
TitleGlowStroke.Transparency = 1
TitleGlowStroke.Parent = TitleLabel

local NameLightStreak = Instance.new("Frame")
NameLightStreak.Size = UDim2.new(0, 26, 1.6, 0)
NameLightStreak.Position = UDim2.new(-0.25, 0, -0.3, 0)
NameLightStreak.Rotation = 18
NameLightStreak.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
NameLightStreak.BackgroundTransparency = 1
NameLightStreak.BorderSizePixel = 0
NameLightStreak.ZIndex = 14
NameLightStreak.Parent = TitleHolder

-- Secuencia: 1) luz pasando por el nombre -> 2) 5s después RGB suave -> vuelve normal
task.spawn(function()
    while ScreenGui.Parent do
        task.wait(10)
        if not ScreenGui.Parent then break end
        pcall(function()
            -- Paso 1: luz brillante pasando por el nombre
            NameLightStreak.Position = UDim2.new(-0.2, 0, -0.3, 0)
            NameLightStreak.BackgroundTransparency = 0.1
            TitleGlowStroke.Color = Color3.fromRGB(255, 255, 255)
            TitleGlowStroke.Thickness = 2.2
            TitleGlowStroke.Transparency = 0.05

            local lightTween = TweenService:Create(
                NameLightStreak,
                TweenInfo.new(0.65, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),
                {Position = UDim2.new(1.15, 0, -0.3, 0)}
            )
            lightTween:Play()
            lightTween.Completed:Wait()

            NameLightStreak.BackgroundTransparency = 1
            TitleGlowStroke.Thickness = 0
            TitleGlowStroke.Transparency = 1

            -- Espera 5 segundos antes del RGB suave
            task.wait(5)
            if not ScreenGui.Parent then return end

            -- Paso 2: RGB suave y vuelve normal
            local baseColor = Theme.TextMain
            local rgbStart = os.clock()
            local rgbDuration = 2.6
            while os.clock() - rgbStart < rgbDuration and ScreenGui.Parent do
                local t = (os.clock() - rgbStart) / rgbDuration
                local hue = (t * 0.65) % 1
                local smoothRgb = Color3.fromHSV(hue, 0.72, 1)
                local fade = math.sin(t * math.pi)
                TitleLabel.TextColor3 = baseColor:Lerp(smoothRgb, fade)
                TitleGlowStroke.Color = smoothRgb
                TitleGlowStroke.Thickness = 1.4 * fade
                TitleGlowStroke.Transparency = 1 - (0.75 * fade)
                RunService.RenderStepped:Wait()
            end

            TitleLabel.TextColor3 = baseColor
            TitleGlowStroke.Thickness = 0
            TitleGlowStroke.Transparency = 1
        end)
    end
end)

local SubBadge = Instance.new("TextLabel")
SubBadge.Size = UDim2.new(0, IsMobile and 82 or 102, 0, IsMobile and 20 or 24)
SubBadge.Position = UDim2.new(0, IsMobile and 94 or 122, 0.5, IsMobile and -10 or -12)
SubBadge.BackgroundColor3 = Theme.BgCard
SubBadge.Text = "PRISON LIFE"
SubBadge.TextColor3 = Theme.Accent
SubBadge.TextSize = IsMobile and 9 or 11
SubBadge.Font = Enum.Font.GothamBold
SubBadge.ZIndex = 13
SubBadge.Parent = Header
StyleRounded(SubBadge, 8, Theme.BorderIdle, 1)

local BtnW = IsMobile and 26 or 32
local BtnH = IsMobile and 26 or 32
local DiscordW = IsMobile and 66 or 84

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, BtnW, 0, BtnH)
CloseBtn.Position = UDim2.new(1, -(BtnW + 8), 0.5, -BtnH / 2)
CloseBtn.BackgroundColor3 = Theme.Accent
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Theme.TextMain
CloseBtn.TextSize = IsMobile and 11 or 13
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.ZIndex = 13
CloseBtn.Parent = Header
StyleRounded(CloseBtn, 8, Theme.BorderActive, 1.2)

local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, BtnW, 0, BtnH)
MinimizeBtn.Position = UDim2.new(1, -(BtnW * 2 + 14), 0.5, -BtnH / 2)
MinimizeBtn.BackgroundColor3 = Theme.BgCard
MinimizeBtn.Text = "—"
MinimizeBtn.TextColor3 = Theme.TextMain
MinimizeBtn.TextSize = IsMobile and 11 or 13
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.ZIndex = 13
MinimizeBtn.Parent = Header
StyleRounded(MinimizeBtn, 8, Theme.BorderIdle, 1.2)

local DiscordBtn = Instance.new("TextButton")
DiscordBtn.Size = UDim2.new(0, DiscordW, 0, BtnH)
DiscordBtn.Position = UDim2.new(1, -(BtnW * 2 + DiscordW + 20), 0.5, -BtnH / 2)
DiscordBtn.BackgroundColor3 = Theme.Discord
DiscordBtn.Text = "Discord"
DiscordBtn.TextColor3 = Theme.TextMain
DiscordBtn.TextSize = IsMobile and 10 or 12
DiscordBtn.Font = Enum.Font.GothamBold
DiscordBtn.ZIndex = 13
DiscordBtn.Parent = Header
StyleRounded(DiscordBtn, 8, Theme.TextMain, 1)

DiscordBtn.MouseButton1Click:Connect(function()
    pcall(function()
        if setclipboard then
            setclipboard(DISCORD_INVITE)
        elseif toclipboard then
            toclipboard(DISCORD_INVITE)
        end
    end)
    DiscordBtn.Text = "Copiado"
    task.wait(1.4)
    if DiscordBtn and DiscordBtn.Parent then
        DiscordBtn.Text = "Discord"
    end
end)

local FloatingToggle = Instance.new("TextButton")
FloatingToggle.Size = UDim2.new(0, 54, 0, 54)
FloatingToggle.Position = UDim2.new(0, 20, 0.42, 0)
FloatingToggle.BackgroundColor3 = Theme.BgMain
FloatingToggle.Text = "SF"
FloatingToggle.TextColor3 = Theme.Accent
FloatingToggle.TextSize = 14
FloatingToggle.Font = Enum.Font.GothamBold
FloatingToggle.Visible = false
FloatingToggle.Active = true
FloatingToggle.Draggable = true
FloatingToggle.ZIndex = 50
FloatingToggle.Parent = ScreenGui
StyleRounded(FloatingToggle, 27, Theme.Accent, 2)

local FOVCircle = nil
pcall(function()
    if Drawing and Drawing.new then
        FOVCircle = Drawing.new("Circle")
        FOVCircle.Transparency = 0.75
        FOVCircle.Thickness = 1.5
        FOVCircle.Color = Theme.Accent
        FOVCircle.Filled = false
        FOVCircle.Visible = false
    end
end)

local tracerLines = {}

local function CleanupVisuals()
    if FOVCircle then
        pcall(function()
            FOVCircle.Visible = false
            FOVCircle:Remove()
        end)
        FOVCircle = nil
    end
    for _, line in pairs(tracerLines) do
        pcall(function()
            line.Visible = false
            line:Remove()
        end)
    end
    table.clear(tracerLines)
    for _, p in ipairs(Players:GetPlayers()) do
        if p.Character and p.Character:FindFirstChild("ScripsFreeESP") then
            pcall(function() p.Character.ScripsFreeESP:Destroy() end)
        end
    end
end

CloseBtn.MouseButton1Click:Connect(function()
    CleanupVisuals()
    pcall(function() ScreenGui:Destroy() end)
end)

MinimizeBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    FloatingToggle.Visible = true
end)

FloatingToggle.MouseButton1Click:Connect(function()
    FloatingToggle.Visible = false
    OpenMainMenu()
end)

local dragging, dragStart, startPos
Header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

-- ================================================================= --
--               4. BARRA LATERAL Y PESTAÑAS                         --
-- ================================================================= --
local BodyTop = HeaderH + 6
local BodyHeight = -(HeaderH + 12)

local SidebarBox = Instance.new("Frame")
SidebarBox.Size = UDim2.new(0, SidebarW - 6, 1, BodyHeight)
SidebarBox.Position = UDim2.new(0, 6, 0, BodyTop)
SidebarBox.BackgroundColor3 = Theme.BgSidebar
SidebarBox.ZIndex = 11
SidebarBox.Parent = MainFrame
StyleRounded(SidebarBox, 12, Theme.BorderIdle, 1.2)

local SidebarScroll = Instance.new("ScrollingFrame")
SidebarScroll.Size = UDim2.new(1, -12, 1, -12)
SidebarScroll.Position = UDim2.new(0, 6, 0, 6)
SidebarScroll.BackgroundTransparency = 1
SidebarScroll.ScrollBarThickness = 2
SidebarScroll.ScrollBarImageColor3 = Theme.Accent
SidebarScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
SidebarScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
SidebarScroll.ZIndex = 12
SidebarScroll.Parent = SidebarBox

local SidebarLayout = Instance.new("UIListLayout")
SidebarLayout.SortOrder = Enum.SortOrder.LayoutOrder
SidebarLayout.Padding = UDim.new(0, IsMobile and 6 or 8)
SidebarLayout.Parent = SidebarScroll

local SidebarPad = Instance.new("UIPadding")
SidebarPad.PaddingTop = UDim.new(0, 2)
SidebarPad.PaddingBottom = UDim.new(0, 2)
SidebarPad.PaddingLeft = UDim.new(0, 2)
SidebarPad.PaddingRight = UDim.new(0, 2)
SidebarPad.Parent = SidebarScroll

local ContentBox = Instance.new("Frame")
ContentBox.Size = UDim2.new(1, -(SidebarW + 12), 1, BodyHeight)
ContentBox.Position = UDim2.new(0, SidebarW + 6, 0, BodyTop)
ContentBox.BackgroundColor3 = Theme.BgSidebar
ContentBox.ZIndex = 11
ContentBox.Parent = MainFrame
StyleRounded(ContentBox, 12, Theme.BorderIdle, 1.2)

local PagesHolder = Instance.new("Frame")
PagesHolder.Size = UDim2.new(1, 0, 1, 0)
PagesHolder.BackgroundTransparency = 1
PagesHolder.ZIndex = 12
PagesHolder.Parent = ContentBox

local tabRegistry = {}

local function CreateTab(labelText, orderIndex)
    local btn = Instance.new("TextButton")
    btn.Name = "Tab_" .. labelText
    btn.LayoutOrder = orderIndex or 1
    btn.Size = UDim2.new(1, 0, 0, IsMobile and 34 or 42)
    btn.BackgroundColor3 = Theme.BgCard
    btn.Text = labelText
    btn.TextColor3 = Theme.TextMuted
    btn.TextSize = IsMobile and 11 or 13
    btn.Font = Enum.Font.GothamBold
    btn.ZIndex = 13
    btn.Parent = SidebarScroll
    StyleRounded(btn, 10, Theme.BorderIdle, 1.3)
    local stroke = btn:FindFirstChildOfClass("UIStroke")

    local page = Instance.new("ScrollingFrame")
    page.Name = "Page_" .. labelText
    page.Size = UDim2.new(1, -16, 1, -16)
    page.Position = UDim2.new(0, 8, 0, 8)
    page.BackgroundTransparency = 1
    page.ScrollBarThickness = IsMobile and 3 or 5
    page.ScrollBarImageColor3 = Theme.Accent
    page.AutomaticCanvasSize = Enum.AutomaticSize.Y
    page.CanvasSize = UDim2.new(0, 0, 0, 0)
    page.Visible = false
    page.ZIndex = 13
    page.Parent = PagesHolder

    local layout = Instance.new("UIListLayout")
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Padding = UDim.new(0, IsMobile and 7 or 10)
    layout.Parent = page

    local pad = Instance.new("UIPadding")
    pad.PaddingTop = UDim.new(0, 2)
    pad.PaddingBottom = UDim.new(0, 4)
    pad.PaddingLeft = UDim.new(0, 2)
    pad.PaddingRight = UDim.new(0, 4)
    pad.Parent = page

    table.insert(tabRegistry, {button = btn, page = page, stroke = stroke})
    return btn, page, stroke
end

local Tab1Btn, Tab1Page, Tab1Stroke = CreateTab("Combat", 1)
local Tab2Btn, Tab2Page = CreateTab("ESP", 2)
local Tab3Btn, Tab3Page = CreateTab("Etc", 3)

local DynamicRoleBtn, DynamicRolePage = nil, nil

local function SwitchTab(activeBtn, activePage)
    for _, item in ipairs(tabRegistry) do
        local isSelected = (item.button == activeBtn)
        item.page.Visible = isSelected
        pcall(function()
            TweenService:Create(item.button, TweenInfo.new(0.18, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
                BackgroundColor3 = isSelected and Theme.Accent or Theme.BgCard,
                TextColor3 = isSelected and Theme.TextMain or Theme.TextMuted
            }):Play()
            if item.stroke then
                TweenService:Create(item.stroke, TweenInfo.new(0.18), {
                    Color = isSelected and Theme.BorderActive or Theme.BorderIdle,
                    Thickness = isSelected and 1.6 or 1.3
                }):Play()
            end
        end)
    end
end

Tab1Page.Visible = true
Tab1Btn.BackgroundColor3 = Theme.Accent
Tab1Btn.TextColor3 = Theme.TextMain
if Tab1Stroke then
    Tab1Stroke.Color = Theme.BorderActive
    Tab1Stroke.Thickness = 1.6
end

Tab1Btn.MouseButton1Click:Connect(function() SwitchTab(Tab1Btn, Tab1Page) end)
Tab2Btn.MouseButton1Click:Connect(function() SwitchTab(Tab2Btn, Tab2Page) end)
Tab3Btn.MouseButton1Click:Connect(function() SwitchTab(Tab3Btn, Tab3Page) end)

local function AddAction(page, text, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, IsMobile and 34 or 44)
    btn.BackgroundColor3 = Theme.BgCard
    btn.Text = text
    btn.TextColor3 = Theme.TextMain
    btn.TextSize = IsMobile and 11 or 13
    btn.Font = Enum.Font.GothamMedium
    btn.ZIndex = 14
    btn.Parent = page
    StyleRounded(btn, 10, Theme.BorderIdle, 1.3)
    local stroke = btn:FindFirstChildOfClass("UIStroke")

    btn.MouseButton1Click:Connect(function()
        pcall(function()
            TweenService:Create(btn, TweenInfo.new(0.12), {BackgroundColor3 = Theme.Accent}):Play()
            if stroke then TweenService:Create(stroke, TweenInfo.new(0.12), {Color = Theme.BorderActive}):Play() end
        end)
        task.wait(0.12)
        pcall(function()
            TweenService:Create(btn, TweenInfo.new(0.22), {BackgroundColor3 = Theme.BgCard}):Play()
            if stroke then TweenService:Create(stroke, TweenInfo.new(0.22), {Color = Theme.BorderIdle}):Play() end
        end)
        pcall(callback)
    end)
end

local function AddToggle(page, label, callback)
    local state = false
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, IsMobile and 34 or 44)
    btn.BackgroundColor3 = Theme.BgCard
    btn.Text = label .. "   [OFF]"
    btn.TextColor3 = Theme.TextMuted
    btn.TextSize = IsMobile and 11 or 13
    btn.Font = Enum.Font.GothamMedium
    btn.ZIndex = 14
    btn.Parent = page
    StyleRounded(btn, 10, Theme.BorderIdle, 1.3)
    local stroke = btn:FindFirstChildOfClass("UIStroke")

    btn.MouseButton1Click:Connect(function()
        state = not state
        pcall(function()
            if state then
                btn.Text = label .. "   [ON]"
                TweenService:Create(btn, TweenInfo.new(0.16), {
                    BackgroundColor3 = Theme.Accent,
                    TextColor3 = Theme.TextMain
                }):Play()
                if stroke then
                    TweenService:Create(stroke, TweenInfo.new(0.16), {
                        Color = Theme.BorderActive,
                        Thickness = 1.6
                    }):Play()
                end
            else
                btn.Text = label .. "   [OFF]"
                TweenService:Create(btn, TweenInfo.new(0.16), {
                    BackgroundColor3 = Theme.BgCard,
                    TextColor3 = Theme.TextMuted
                }):Play()
                if stroke then
                    TweenService:Create(stroke, TweenInfo.new(0.16), {
                        Color = Theme.BorderIdle,
                        Thickness = 1.3
                    }):Play()
                end
            end
        end)
        pcall(function() callback(state) end)
    end)
end

local function AddSlider(page, title, minVal, maxVal, defaultVal, callback)
    local card = Instance.new("Frame")
    card.Size = UDim2.new(1, 0, 0, IsMobile and 48 or 62)
    card.BackgroundColor3 = Theme.BgCard
    card.ZIndex = 14
    card.Parent = page
    StyleRounded(card, 10, Theme.BorderIdle, 1.3)

    local titleLbl = Instance.new("TextLabel")
    titleLbl.Size = UDim2.new(1, -18, 0, 20)
    titleLbl.Position = UDim2.new(0, 9, 0, IsMobile and 4 or 8)
    titleLbl.BackgroundTransparency = 1
    titleLbl.Text = title .. ": " .. defaultVal
    titleLbl.TextColor3 = Theme.TextMain
    titleLbl.TextSize = IsMobile and 10.5 or 13
    titleLbl.Font = Enum.Font.GothamMedium
    titleLbl.TextXAlignment = Enum.TextXAlignment.Left
    titleLbl.ZIndex = 15
    titleLbl.Parent = card

    local track = Instance.new("Frame")
    track.Size = UDim2.new(1, -18, 0, IsMobile and 8 or 10)
    track.Position = UDim2.new(0, 9, 0, IsMobile and 30 or 40)
    track.BackgroundColor3 = Theme.BgSidebar
    track.ZIndex = 15
    track.Parent = card
    StyleRounded(track, 5, Theme.BorderIdle, 1)

    local fill = Instance.new("Frame")
    fill.Size = UDim2.new((defaultVal - minVal) / math.max(maxVal - minVal, 1), 0, 1, 0)
    fill.BackgroundColor3 = Theme.Accent
    fill.BorderSizePixel = 0
    fill.ZIndex = 16
    fill.Parent = track
    Instance.new("UICorner", fill).CornerRadius = UDim.new(0, 5)

    local sliding = false
    local function applyPos(input)
        local ratio = math.clamp((input.Position.X - track.AbsolutePosition.X) / math.max(track.AbsoluteSize.X, 1), 0, 1)
        fill.Size = UDim2.new(ratio, 0, 1, 0)
        local value = math.floor(minVal + ((maxVal - minVal) * ratio))
        titleLbl.Text = title .. ": " .. value
        pcall(function() callback(value) end)
    end

    track.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            sliding = true
            applyPos(input)
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            sliding = false
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if sliding and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            applyPos(input)
        end
    end)
end

-- ================================================================= --
--                     5. COMBAT (SOLO AIMBOT)                       --
-- ================================================================= --
local function IsEnemy(player)
    if not player or player == LocalPlayer then return false end
    local ok, res = pcall(function()
        if player.Team and LocalPlayer.Team then
            return player.Team ~= LocalPlayer.Team
        end
        return true
    end)
    return ok and res or false
end

local aimbotEnabled = false
local fovVisible = true
local fovRadius = 130
local smoothness = 4

AddToggle(Tab1Page, "Aimbot Assist", function(state) aimbotEnabled = state end)
AddToggle(Tab1Page, "Círculo FOV", function(state) fovVisible = state end)
AddSlider(Tab1Page, "Suavidad Aimbot", 1, 15, 4, function(val) smoothness = val end)

-- ================================================================= --
--                         6. ESP DINÁMICO                           --
-- ================================================================= --
local espBox = false
local espNames = false
local espHealth = false
local espDist = false
local espTracer = false

AddToggle(Tab2Page, "ESP Caja", function(state) espBox = state end)
AddToggle(Tab2Page, "ESP Nombres", function(state) espNames = state end)
AddToggle(Tab2Page, "ESP Vidas", function(state) espHealth = state end)
AddToggle(Tab2Page, "ESP Distancia", function(state) espDist = state end)
AddToggle(Tab2Page, "ESP Tracers", function(state) espTracer = state end)

-- ================================================================= --
--                  7. ETC (ARMERÍA, FLY, SPEED, NOCLIP)             --
-- ================================================================= --
AddAction(Tab3Page, "Ir a la Armería", function()
    local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root and root:IsA("BasePart") then
        root.CFrame = CFrame.new(814.84, 100.95, 2229.09)
    end
end)

local flyActive = false
local flyLevel = 25
local bg, bv

local function StopFly()
    flyActive = false
    if bg then pcall(function() bg:Destroy() end) bg = nil end
    if bv then pcall(function() bv:Destroy() end) bv = nil end
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then pcall(function() hum.PlatformStand = false end) end
end

AddToggle(Tab3Page, "Fly (Vuelo)", function(state)
    flyActive = state
    local char = LocalPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChildOfClass("Humanoid")

    if flyActive and hrp and hrp:IsA("BasePart") then
        pcall(function()
            if hum then hum.PlatformStand = true end

            bg = Instance.new("BodyGyro")
            bg.P = 9e4
            bg.MaxTorque = Vector3.new(9e4, 9e4, 9e4)
            bg.CFrame = hrp.CFrame
            bg.Parent = hrp

            bv = Instance.new("BodyVelocity")
            bv.Velocity = Vector3.zero
            bv.MaxForce = Vector3.new(9e4, 9e4, 9e4)
            bv.Parent = hrp
        end)

        task.spawn(function()
            while flyActive and ScreenGui.Parent and hrp.Parent and bg and bv do
                pcall(function()
                    local camCF = Camera.CFrame
                    local dir = Vector3.zero
                    if UserInputService:IsKeyDown(Enum.KeyCode.W) then dir += camCF.LookVector end
                    if UserInputService:IsKeyDown(Enum.KeyCode.S) then dir -= camCF.LookVector end
                    if UserInputService:IsKeyDown(Enum.KeyCode.A) then dir -= camCF.RightVector end
                    if UserInputService:IsKeyDown(Enum.KeyCode.D) then dir += camCF.RightVector end
                    if UserInputService:IsKeyDown(Enum.KeyCode.Space) then dir += Vector3.yAxis end
                    if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then dir -= Vector3.yAxis end

                    bv.Velocity = dir * (flyLevel * 5)
                    bg.CFrame = camCF
                end)
                RunService.RenderStepped:Wait()
            end
            StopFly()
        end)
    else
        StopFly()
    end
end)

AddSlider(Tab3Page, "Velocidad Fly", 1, 50, 25, function(val) flyLevel = val end)

local speedActive = false
local speedValue = 30

AddToggle(Tab3Page, "Speed (Velocidad)", function(state)
    speedActive = state
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not state and hum then
        pcall(function() hum.WalkSpeed = 16 end)
    end
end)

AddSlider(Tab3Page, "Ajustar Speed", 1, 50, 30, function(val) speedValue = val end)

local noclipActive = false
AddToggle(Tab3Page, "Noclip", function(state) noclipActive = state end)

RunService.Stepped:Connect(function()
    if noclipActive and ScreenGui.Parent and LocalPlayer.Character then
        pcall(function()
            for _, part in ipairs(LocalPlayer.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end)
    end
end)

-- ================================================================= --
--                  8. LOOP PRINCIPAL UNIFICADO (SIN LAG)           --
-- ================================================================= --
local frameCounter = 0

RunService.RenderStepped:Connect(function()
    if not ScreenGui.Parent then return end
    frameCounter += 1

    if speedActive then
        pcall(function()
            local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if hum and hum.WalkSpeed ~= speedValue * 0.85 then
                hum.WalkSpeed = speedValue * 0.85
            end
        end)
    end

    local mousePos = Vector2.zero
    pcall(function() mousePos = UserInputService:GetMouseLocation() end)

    if FOVCircle then
        pcall(function()
            FOVCircle.Position = mousePos
            FOVCircle.Radius = fovRadius
            FOVCircle.Visible = aimbotEnabled and fovVisible
        end)
    end

    local anyEsp = espBox or espNames or espHealth or espDist or espTracer
    local activeTracers = {}
    local bestAimPos = nil
    local bestDist = fovRadius
    local updateVisualsTick = (frameCounter % 2 == 0)

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            pcall(function()
                local char = player.Character
                local head = char and char:FindFirstChild("Head")
                local hrp = char and char:FindFirstChild("HumanoidRootPart")
                local hum = char and char:FindFirstChildOfClass("Humanoid")

                local aliveEnemy = (char and head and head:IsA("BasePart") and hum and hum.Health > 0 and IsEnemy(player))

                if aliveEnemy and aimbotEnabled and UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
                    local screenPt, onScreen = Camera:WorldToViewportPoint(head.Position)
                    if onScreen then
                        local dist = (Vector2.new(screenPt.X, screenPt.Y) - mousePos).Magnitude
                        if dist < bestDist then
                            bestDist = dist
                            bestAimPos = head.Position
                        end
                    end
                end

                if updateVisualsTick and char then
                    if aliveEnemy and hrp and hrp:IsA("BasePart") and anyEsp then
                        local folder = char:FindFirstChild("ScripsFreeESP")
                        if not folder then
                            folder = Instance.new("Folder")
                            folder.Name = "ScripsFreeESP"
                            folder.Parent = char

                            local hl = Instance.new("Highlight")
                            hl.Name = "Box"
                            hl.Adornee = char
                            hl.FillColor = Theme.Accent
                            hl.OutlineColor = Theme.TextMain
                            hl.FillTransparency = 0.45
                            hl.OutlineTransparency = 0
                            hl.Parent = folder

                            local bb = Instance.new("BillboardGui")
                            bb.Name = "Info"
                            bb.Adornee = hrp
                            bb.Size = UDim2.new(0, 160, 0, 56)
                            bb.StudsOffset = Vector3.new(0, 3, 0)
                            bb.AlwaysOnTop = true
                            bb.Parent = folder

                            local txt = Instance.new("TextLabel")
                            txt.Name = "Text"
                            txt.Size = UDim2.new(1, 0, 1, 0)
                            txt.BackgroundTransparency = 1
                            txt.TextColor3 = Theme.TextMain
                            txt.TextStrokeTransparency = 0
                            txt.TextSize = 12
                            txt.Font = Enum.Font.GothamBold
                            txt.Parent = bb
                        end

                        local hl = folder:FindFirstChild("Box")
                        if hl then hl.Enabled = espBox end

                        local bb = folder:FindFirstChild("Info")
                        if bb then
                            local txt = bb:FindFirstChild("Text")
                            if txt then
                                local parts = {}
                                if espNames then table.insert(parts, "[" .. player.Name .. "]") end
                                if espHealth then table.insert(parts, "HP: " .. math.floor(hum.Health)) end
                                if espDist then
                                    local meters = math.floor((hrp.Position - Camera.CFrame.Position).Magnitude)
                                    table.insert(parts, meters .. "m")
                                end
                                txt.Text = table.concat(parts, "  ")
                                bb.Enabled = #parts > 0
                            end
                        end

                        if espTracer and Drawing and Drawing.new then
                            local line = tracerLines[player]
                            if not line then
                                line = Drawing.new("Line")
                                line.Thickness = 1.5
                                line.Color = Theme.Accent
                                line.Transparency = 0.75
                                tracerLines[player] = line
                            end
                            if line then
                                local pt, onScreen = Camera:WorldToViewportPoint(hrp.Position)
                                if onScreen then
                                    line.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                                    line.To = Vector2.new(pt.X, pt.Y)
                                    line.Visible = true
                                    activeTracers[player] = true
                                else
                                    line.Visible = false
                                end
                            end
                        end
                    elseif char:FindFirstChild("ScripsFreeESP") then
                        char.ScripsFreeESP:Destroy()
                    end
                end
            end)
        end
    end

    if bestAimPos then
        pcall(function()
            local factor = math.clamp(smoothness, 1, 20)
            Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, bestAimPos), 1 / factor)
        end)
    end

    if updateVisualsTick then
        for p, line in pairs(tracerLines) do
            if not activeTracers[p] then
                pcall(function() line.Visible = false end)
            end
        end
    end
end)

-- ================================================================= --
--                  9. ROLES DINÁMICOS (POLICÍA / PRESO / CRIM)     --
-- ================================================================= --
local function GetPlayerRole(player)
    if not player then return "neutral" end
    local ok, role = pcall(function()
        if not player.Team then return "neutral" end
        local tName = string.lower(player.Team.Name)
        if string.find(tName, "pol") or string.find(tName, "guard") or string.find(tName, "security") then
            return "police"
        elseif string.find(tName, "crim") then
            return "criminal"
        elseif string.find(tName, "pris") or string.find(tName, "prision") or string.find(tName, "inmat") then
            return "prisoner"
        end
        return "other"
    end)
    return ok and role or "neutral"
end

local function EquipAndFireCuffs(targetPlayer)
    pcall(function()
        local char = LocalPlayer.Character
        local backpack = LocalPlayer:FindFirstChildOfClass("Backpack")
        local cuffs = nil

        if backpack then
            for _, item in ipairs(backpack:GetChildren()) do
                local n = string.lower(item.Name)
                if string.find(n, "cuff") or string.find(n, "esposa") or string.find(n, "handcuff") then
                    cuffs = item
                    break
                end
            end
        end

        if cuffs and char and char:FindFirstChildOfClass("Humanoid") and cuffs.Parent == backpack then
            char.Humanoid:EquipTool(cuffs)
            task.wait(0.1)
        end

        local targetChar = targetPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") and targetChar and targetChar:FindFirstChild("HumanoidRootPart") then
            local hrp = char.HumanoidRootPart
            local targetHrp = targetChar.HumanoidRootPart
            local savedCF = hrp.CFrame

            hrp.CFrame = targetHrp.CFrame * CFrame.new(0, 0, 2.5)

            local tool = char:FindFirstChildOfClass("Tool")
            if tool then
                for _, desc in ipairs(tool:GetDescendants()) do
                    if desc:IsA("RemoteEvent") then
                        desc:FireServer(targetChar)
                        desc:FireServer(targetPlayer)
                    end
                end
            end
            for _, obj in ipairs(ReplicatedStorage:GetDescendants()) do
                if obj:IsA("RemoteEvent") and (string.find(string.lower(obj.Name), "arrest") or string.find(string.lower(obj.Name), "esposar")) then
                    obj:FireServer(targetPlayer)
                end
            end

            task.wait(0.3)
            if hrp and hrp.Parent then
                hrp.CFrame = savedCF
            end
        end
    end)
end

local function RemoveDynamicTab()
    pcall(function()
        if DynamicRoleBtn then
            for i = #tabRegistry, 1, -1 do
                if tabRegistry[i].button == DynamicRoleBtn then
                    table.remove(tabRegistry, i)
                end
            end
            DynamicRoleBtn:Destroy()
            DynamicRoleBtn = nil
        end
        if DynamicRolePage then
            DynamicRolePage:Destroy()
            DynamicRolePage = nil
        end
    end)
end

local function RefreshRoleTab()
    local role = GetPlayerRole(LocalPlayer)

    if role == "police" and not DynamicRoleBtn then
        DynamicRoleBtn, DynamicRolePage = CreateTab("Policía", 4)
        DynamicRoleBtn.MouseButton1Click:Connect(function() SwitchTab(DynamicRoleBtn, DynamicRolePage) end)

        local headerLbl = Instance.new("TextLabel")
        headerLbl.Size = UDim2.new(1, 0, 0, 26)
        headerLbl.BackgroundTransparency = 1
        headerLbl.Text = "CRIMINALES ACTIVOS"
        headerLbl.TextColor3 = Theme.TextMain
        headerLbl.TextSize = IsMobile and 11 or 13
        headerLbl.Font = Enum.Font.GothamBold
        headerLbl.ZIndex = 14
        headerLbl.Parent = DynamicRolePage

        local crimList = Instance.new("ScrollingFrame")
        crimList.Size = UDim2.new(1, 0, 1, -32)
        crimList.BackgroundTransparency = 1
        crimList.ScrollBarThickness = 4
        crimList.AutomaticCanvasSize = Enum.AutomaticSize.Y
        crimList.CanvasSize = UDim2.new(0, 0, 0, 0)
        crimList.ZIndex = 14
        crimList.Parent = DynamicRolePage

        local crimLayout = Instance.new("UIListLayout")
        crimLayout.SortOrder = Enum.SortOrder.LayoutOrder
        crimLayout.Padding = UDim.new(0, 6)
        crimLayout.Parent = crimList

        task.spawn(function()
            while ScreenGui.Parent and DynamicRoleBtn and GetPlayerRole(LocalPlayer) == "police" do
                pcall(function()
                    for _, child in ipairs(crimList:GetChildren()) do
                        if child:IsA("Frame") then child:Destroy() end
                    end

                    for _, plr in ipairs(Players:GetPlayers()) do
                        if plr ~= LocalPlayer and GetPlayerRole(plr) == "criminal" then
                            local row = Instance.new("Frame")
                            row.Size = UDim2.new(1, 0, 0, IsMobile and 34 or 42)
                            row.BackgroundTransparency = 1
                            row.ZIndex = 15
                            row.Parent = crimList

                            local rowLayout = Instance.new("UIListLayout")
                            rowLayout.FillDirection = Enum.FillDirection.Horizontal
                            rowLayout.SortOrder = Enum.SortOrder.LayoutOrder
                            rowLayout.Padding = UDim.new(0, 6)
                            rowLayout.Parent = row

                            local tpBtn = Instance.new("TextButton")
                            tpBtn.Size = UDim2.new(0.54, -3, 1, 0)
                            tpBtn.BackgroundColor3 = Theme.BgCard
                            tpBtn.Text = "TP: " .. plr.Name
                            tpBtn.TextColor3 = Theme.TextMain
                            tpBtn.TextSize = IsMobile and 10 or 12
                            tpBtn.Font = Enum.Font.GothamBold
                            tpBtn.ZIndex = 16
                            tpBtn.Parent = row
                            StyleRounded(tpBtn, 9, Theme.BorderIdle, 1.2)

                            tpBtn.MouseButton1Click:Connect(function()
                                local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
                                local tgtRoot = plr.Character and plr.Character:FindFirstChild("HumanoidRootPart")
                                if myRoot and tgtRoot then
                                    myRoot.CFrame = tgtRoot.CFrame * CFrame.new(0, 0, 3)
                                end
                            end)

                            local arrBtn = Instance.new("TextButton")
                            arrBtn.Size = UDim2.new(0.46, -3, 1, 0)
                            arrBtn.BackgroundColor3 = Theme.BgCard
                            arrBtn.Text = "Arrestar"
                            arrBtn.TextColor3 = Theme.Accent
                            arrBtn.TextSize = IsMobile and 10 or 12
                            arrBtn.Font = Enum.Font.GothamBold
                            arrBtn.ZIndex = 16
                            arrBtn.Parent = row
                            StyleRounded(arrBtn, 9, Theme.BorderIdle, 1.2)

                            arrBtn.MouseButton1Click:Connect(function()
                                EquipAndFireCuffs(plr)
                            end)
                        end
                    end
                end)
                task.wait(2)
            end
        end)

    elseif role == "prisoner" and not DynamicRoleBtn then
        DynamicRoleBtn, DynamicRolePage = CreateTab("Prisionero", 4)
        DynamicRoleBtn.MouseButton1Click:Connect(function() SwitchTab(DynamicRoleBtn, DynamicRolePage) end)

        AddAction(DynamicRolePage, "Ir a la Prisión", function()
            local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if root then root.CFrame = CFrame.new(869.17, 114.79, 2488.31) end
        end)

        AddAction(DynamicRolePage, "Ir a Zona Criminal", function()
            local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if root then root.CFrame = CFrame.new(-973.32, 108.32, 2052.10) end
        end)

    elseif role == "criminal" and not DynamicRoleBtn then
        DynamicRoleBtn, DynamicRolePage = CreateTab("Criminal", 4)
        DynamicRoleBtn.MouseButton1Click:Connect(function() SwitchTab(DynamicRoleBtn, DynamicRolePage) end)

        AddAction(DynamicRolePage, "Ir a Base Criminal", function()
            local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if root then root.CFrame = CFrame.new(-973.32, 108.32, 2052.10) end
        end)

    elseif role ~= "police" and role ~= "prisoner" and role ~= "criminal" and DynamicRoleBtn then
        RemoveDynamicTab()
        SwitchTab(Tab1Btn, Tab1Page)
    end
end

task.spawn(function()
    while ScreenGui.Parent do
        pcall(RefreshRoleTab)
        task.wait(1)
    end
end)

-- ================================================================= --
--                     10. COMANDO /COPIAR EN CHAT                   --
-- ================================================================= --
local function OnChatMessage(msg)
    if type(msg) == "string" and string.lower(msg) == "/copiar" then
        pcall(function()
            local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if root then
                local p = root.Position
                local formatted = string.format("Vector3.new(%.2f, %.2f, %.2f)", p.X, p.Y, p.Z)
                if setclipboard then
                    setclipboard(formatted)
                end
            end
        end)
    end
end

pcall(function()
    if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
        TextChatService.MessageReceived:Connect(function(tcMsg)
            if tcMsg.TextSource and tcMsg.TextSource.UserId == LocalPlayer.UserId then
                OnChatMessage(tcMsg.Text)
            end
        end)
    else
        LocalPlayer.Chatted:Connect(OnChatMessage)
    end
end)

-- ================================================================= --
--                     11. SECUENCIA DE INICIO (15 SEGUNDOS)          --
-- ================================================================= --
task.spawn(function()
    local totalDuration = 15
    local startTime = os.clock()
    local messages = {
        "Aplicando bypass y bloqueando comprobaciones...",
        "Cargando aimbot y controles...",
        "Optimizando rendimiento del cliente...",
        "Finalizando configuración de Scrips Free..."
    }

    while ScreenGui.Parent do
        local elapsed = os.clock() - startTime
        local progress = math.clamp(elapsed / totalDuration, 0, 1)
        local remaining = math.max(0, math.ceil(totalDuration - elapsed))
        local msgIndex = math.clamp(math.floor(progress * #messages) + 1, 1, #messages)

        pcall(function()
            BarFill.Size = UDim2.new(progress, 0, 1, 0)
            LoadLog.Text = "> " .. messages[msgIndex] .. " (" .. remaining .. "s)"
        end)

        if progress >= 1 then
            break
        end
        task.wait(0.1)
    end

    if not ScreenGui.Parent then return end
    pcall(function()
        NewsFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
        NewsFrame.Visible = true
        if LoadScreen and LoadScreen.Parent then
            LoadScreen.Visible = false
            LoadScreen:Destroy()
        end
    end)
end)
