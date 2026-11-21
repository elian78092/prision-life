-- [[ ELIAN HUB - PRISON LIFE FIXED UI & ROUNDED BORDERS ]] --

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

-- Limpiar instancia previa
if CoreGui:FindFirstChild("ElianHub") then
    CoreGui.ElianHub:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ElianHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = CoreGui

local Theme = {
    BgMain = Color3.fromRGB(11, 13, 18),
    BgCard = Color3.fromRGB(19, 22, 31),
    BgSidebar = Color3.fromRGB(14, 17, 24),
    Accent = Color3.fromRGB(235, 55, 65),
    TextMain = Color3.fromRGB(242, 245, 252),
    TextMuted = Color3.fromRGB(150, 158, 176),
    BorderIdle = Color3.fromRGB(42, 49, 66),
    BorderActive = Color3.fromRGB(255, 115, 125)
}

-- Función para aplicar bordes realmente redondeados y limpios
local function StyleRounded(obj, radiusPx, borderColor, borderThickness)
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

    return stroke
end

-- ================================================================= --
--                     1. PANTALLA DE CARGA                          --
-- ================================================================= --
local LoadScreen = Instance.new("Frame")
LoadScreen.Size = IsMobile and UDim2.new(0, 320, 0, 160) or UDim2.new(0, 390, 0, 180)
LoadScreen.Position = UDim2.new(0.5, 0, 0.5, 0)
LoadScreen.AnchorPoint = Vector2.new(0.5, 0.5)
LoadScreen.BackgroundColor3 = Theme.BgMain
LoadScreen.Parent = ScreenGui
StyleRounded(LoadScreen, 16, Theme.Accent, 2)

local LoadTitle = Instance.new("TextLabel")
LoadTitle.Size = UDim2.new(1, -28, 0, 28)
LoadTitle.Position = UDim2.new(0, 14, 0, 16)
LoadTitle.BackgroundTransparency = 1
LoadTitle.Text = "ELIAN HUB  |  PRISON LIFE"
LoadTitle.TextColor3 = Theme.TextMain
LoadTitle.TextSize = IsMobile and 13 or 15
LoadTitle.Font = Enum.Font.GothamBold
LoadTitle.TextXAlignment = Enum.TextXAlignment.Left
LoadTitle.Parent = LoadScreen

local LoadSub = Instance.new("TextLabel")
LoadSub.Size = UDim2.new(1, -28, 0, 20)
LoadSub.Position = UDim2.new(0, 14, 0, 42)
LoadSub.BackgroundTransparency = 1
LoadSub.Text = "Cargando módulos y reparando interfaz..."
LoadSub.TextColor3 = Theme.TextMuted
LoadSub.TextSize = IsMobile and 10.5 or 11.5
LoadSub.Font = Enum.Font.Gotham
LoadSub.TextXAlignment = Enum.TextXAlignment.Left
LoadSub.Parent = LoadScreen

local BarBG = Instance.new("Frame")
BarBG.Size = UDim2.new(1, -28, 0, 12)
BarBG.Position = UDim2.new(0, 14, 0, 80)
BarBG.BackgroundColor3 = Theme.BgCard
BarBG.Parent = LoadScreen
StyleRounded(BarBG, 6, Theme.BorderIdle, 1)

local BarFill = Instance.new("Frame")
BarFill.Size = UDim2.new(0, 0, 1, 0)
BarFill.BackgroundColor3 = Theme.Accent
BarFill.BorderSizePixel = 0
BarFill.Parent = BarBG
Instance.new("UICorner", BarFill).CornerRadius = UDim.new(0, 6)

local LoadLog = Instance.new("TextLabel")
LoadLog.Size = UDim2.new(1, -28, 0, 38)
LoadLog.Position = UDim2.new(0, 14, 0, 106)
LoadLog.BackgroundTransparency = 1
LoadLog.Text = "Iniciando..."
LoadLog.TextColor3 = Theme.TextMuted
LoadLog.TextSize = IsMobile and 10 or 11
LoadLog.Font = Enum.Font.Code
LoadLog.TextWrapped = true
LoadLog.TextXAlignment = Enum.TextXAlignment.Left
LoadLog.Parent = LoadScreen

-- ================================================================= --
--                    2. TARJETA DE NOVEDADES                        --
-- ================================================================= --
local NewsFrame = Instance.new("Frame")
NewsFrame.Size = IsMobile and UDim2.new(0, 320, 0, 210) or UDim2.new(0, 370, 0, 225)
NewsFrame.Position = UDim2.new(0.5, 0, -0.5, 0)
NewsFrame.AnchorPoint = Vector2.new(0.5, 0.5)
NewsFrame.BackgroundColor3 = Theme.BgMain
NewsFrame.Visible = false
NewsFrame.ZIndex = 20
NewsFrame.Parent = ScreenGui
StyleRounded(NewsFrame, 16, Theme.Accent, 2)

local NewsHeader = Instance.new("TextLabel")
NewsHeader.Size = UDim2.new(1, -28, 0, 34)
NewsHeader.Position = UDim2.new(0, 14, 0, 12)
NewsHeader.BackgroundTransparency = 1
NewsHeader.Text = "NOVEDADES DEL SISTEMA"
NewsHeader.TextColor3 = Theme.TextMain
NewsHeader.TextSize = IsMobile and 13 or 15
NewsHeader.Font = Enum.Font.GothamBold
NewsHeader.TextXAlignment = Enum.TextXAlignment.Left
NewsHeader.ZIndex = 21
NewsHeader.Parent = NewsFrame

local NewsBody = Instance.new("TextLabel")
NewsBody.Size = UDim2.new(1, -28, 0, 100)
NewsBody.Position = UDim2.new(0, 14, 0, 50)
NewsBody.BackgroundTransparency = 1
NewsBody.Text = "• Pestañas Combat, ESP, Etc y Rol 100% visibles.\n• Esquinas y bordes suavizados en cada botón y tarjeta.\n• Aimbot, Hitbox Head, ESP, Fly, Speed y Noclip listos.\n• Comando /copiar activo en el chat."
NewsBody.TextColor3 = Theme.TextMuted
NewsBody.TextSize = IsMobile and 10.5 or 12
NewsBody.Font = Enum.Font.GothamMedium
NewsBody.TextWrapped = true
NewsBody.TextXAlignment = Enum.TextXAlignment.Left
NewsBody.TextYAlignment = Enum.TextYAlignment.Top
NewsBody.ZIndex = 21
NewsBody.Parent = NewsFrame

local AcceptBtn = Instance.new("TextButton")
AcceptBtn.Size = UDim2.new(1, -28, 0, 38)
AcceptBtn.Position = UDim2.new(0, 14, 1, -50)
AcceptBtn.BackgroundColor3 = Theme.Accent
AcceptBtn.Text = "ENTRAR AL MENÚ"
AcceptBtn.TextColor3 = Theme.TextMain
AcceptBtn.TextSize = IsMobile and 11.5 or 13
AcceptBtn.Font = Enum.Font.GothamBold
AcceptBtn.ZIndex = 21
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
MainFrame.Visible = false
MainFrame.ZIndex = 10
MainFrame.Parent = ScreenGui
StyleRounded(MainFrame, 16, Theme.Accent, 2)

local function OpenMainMenu()
    MainFrame.Visible = true
    MainFrame.Size = UDim2.new(0, 10, 0, 10)
    TweenService:Create(MainFrame, TweenInfo.new(0.32, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = MainSize
    }):Play()
end

AcceptBtn.MouseButton1Click:Connect(function()
    TweenService:Create(NewsFrame, TweenInfo.new(0.28, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
        Position = UDim2.new(0.5, 0, 1.4, 0)
    }):Play()
    task.wait(0.28)
    NewsFrame:Destroy()
    OpenMainMenu()
end)

-- Barra Superior
local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, -12, 0, HeaderH - 6)
Header.Position = UDim2.new(0, 6, 0, 6)
Header.BackgroundColor3 = Theme.BgSidebar
Header.ZIndex = 12
Header.Parent = MainFrame
StyleRounded(Header, 12, Theme.BorderIdle, 1.2)

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(0.6, 0, 1, 0)
TitleLabel.Position = UDim2.new(0, 14, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "ELIAN HUB"
TitleLabel.TextColor3 = Theme.TextMain
TitleLabel.TextSize = IsMobile and 13 or 17
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.ZIndex = 13
TitleLabel.Parent = Header

local SubBadge = Instance.new("TextLabel")
SubBadge.Size = UDim2.new(0, IsMobile and 86 or 106, 0, IsMobile and 20 or 24)
SubBadge.Position = UDim2.new(0, IsMobile and 94 or 126, 0.5, IsMobile and -10 or -12)
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

-- Botón flotante para reabrir
local FloatingToggle = Instance.new("TextButton")
FloatingToggle.Size = UDim2.new(0, 54, 0, 54)
FloatingToggle.Position = UDim2.new(0, 20, 0.42, 0)
FloatingToggle.BackgroundColor3 = Theme.BgMain
FloatingToggle.Text = "HUB"
FloatingToggle.TextColor3 = Theme.Accent
FloatingToggle.TextSize = 13
FloatingToggle.Font = Enum.Font.GothamBold
FloatingToggle.Visible = false
FloatingToggle.Active = true
FloatingToggle.Draggable = true
FloatingToggle.ZIndex = 50
FloatingToggle.Parent = ScreenGui
StyleRounded(FloatingToggle, 27, Theme.Accent, 2)

local FOVCircle = Drawing.new("Circle")
FOVCircle.Transparency = 0.75
FOVCircle.Thickness = 1.5
FOVCircle.Color = Theme.Accent
FOVCircle.Filled = false
FOVCircle.Visible = false

local tracerLines = {}

local function CleanupVisuals()
    FOVCircle.Visible = false
    pcall(function() FOVCircle:Remove() end)
    for _, line in pairs(tracerLines) do
        pcall(function()
            line.Visible = false
            line:Remove()
        end)
    end
    table.clear(tracerLines)
    for _, p in ipairs(Players:GetPlayers()) do
        if p.Character and p.Character:FindFirstChild("ElianESP") then
            p.Character.ElianESP:Destroy()
        end
    end
end

CloseBtn.MouseButton1Click:Connect(function()
    CleanupVisuals()
    ScreenGui:Destroy()
end)

MinimizeBtn.MouseButton1Click:Connect(function()
    TweenService:Create(MainFrame, TweenInfo.new(0.24, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
        Size = UDim2.new(0, 10, 0, 10)
    }):Play()
    task.wait(0.24)
    MainFrame.Visible = false
    MainFrame.Size = MainSize
    FloatingToggle.Visible = true
end)

FloatingToggle.MouseButton1Click:Connect(function()
    FloatingToggle.Visible = false
    OpenMainMenu()
end)

-- Arrastre del menú principal
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
--               4. BARRA LATERAL Y PESTAÑAS (ARREGLADAS)            --
-- ================================================================= --
local BodyTop = HeaderH + 6
local BodyHeight = -(HeaderH + 12)

-- Contenedor redondeado para las pestañas
local SidebarBox = Instance.new("Frame")
SidebarBox.Size = UDim2.new(0, SidebarW - 6, 1, BodyHeight)
SidebarBox.Position = UDim2.new(0, 6, 0, BodyTop)
SidebarBox.BackgroundColor3 = Theme.BgSidebar
SidebarBox.ZIndex = 11
SidebarBox.Parent = MainFrame
StyleRounded(SidebarBox, 12, Theme.BorderIdle, 1.2)

-- ScrollingFrame interno para que TODAS las pestañas se vean siempre
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

-- Contenedor redondeado para el contenido de cada pestaña
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
    local stroke = StyleRounded(btn, 10, Theme.BorderIdle, 1.3)

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
        TweenService:Create(item.button, TweenInfo.new(0.15), {
            BackgroundColor3 = isSelected and Theme.Accent or Theme.BgCard,
            TextColor3 = isSelected and Theme.TextMain or Theme.TextMuted
        }):Play()
        TweenService:Create(item.stroke, TweenInfo.new(0.15), {
            Color = isSelected and Theme.BorderActive or Theme.BorderIdle,
            Thickness = isSelected and 1.6 or 1.3
        }):Play()
    end
end

Tab1Page.Visible = true
Tab1Btn.BackgroundColor3 = Theme.Accent
Tab1Btn.TextColor3 = Theme.TextMain
Tab1Stroke.Color = Theme.BorderActive
Tab1Stroke.Thickness = 1.6

Tab1Btn.MouseButton1Click:Connect(function() SwitchTab(Tab1Btn, Tab1Page) end)
Tab2Btn.MouseButton1Click:Connect(function() SwitchTab(Tab2Btn, Tab2Page) end)
Tab3Btn.MouseButton1Click:Connect(function() SwitchTab(Tab3Btn, Tab3Page) end)

-- Componentes UI redondeados
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
    local stroke = StyleRounded(btn, 10, Theme.BorderIdle, 1.3)

    btn.MouseButton1Click:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.12), {BackgroundColor3 = Theme.Accent}):Play()
        TweenService:Create(stroke, TweenInfo.new(0.12), {Color = Theme.BorderActive}):Play()
        task.wait(0.12)
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Theme.BgCard}):Play()
        TweenService:Create(stroke, TweenInfo.new(0.2), {Color = Theme.BorderIdle}):Play()
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
    local stroke = StyleRounded(btn, 10, Theme.BorderIdle, 1.3)

    btn.MouseButton1Click:Connect(function()
        state = not state
        if state then
            btn.Text = label .. "   [ON]"
            TweenService:Create(btn, TweenInfo.new(0.15), {
                BackgroundColor3 = Theme.Accent,
                TextColor3 = Theme.TextMain
            }):Play()
            TweenService:Create(stroke, TweenInfo.new(0.15), {
                Color = Theme.BorderActive,
                Thickness = 1.6
            }):Play()
        else
            btn.Text = label .. "   [OFF]"
            TweenService:Create(btn, TweenInfo.new(0.15), {
                BackgroundColor3 = Theme.BgCard,
                TextColor3 = Theme.TextMuted
            }):Play()
            TweenService:Create(stroke, TweenInfo.new(0.15), {
                Color = Theme.BorderIdle,
                Thickness = 1.3
            }):Play()
        end
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
    fill.Size = UDim2.new((defaultVal - minVal) / (maxVal - minVal), 0, 1, 0)
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
--                     5. COMBAT (AIMBOT + HITBOX)                   --
-- ================================================================= --
local function IsEnemy(player)
    if player == LocalPlayer then return false end
    if player.Team and LocalPlayer.Team then
        return player.Team ~= LocalPlayer.Team
    end
    return true
end

local aimbotEnabled = false
local fovVisible = true
local fovRadius = 130
local smoothness = 4
local hitboxHeadEnabled = false
local hitboxHeadSize = 2

AddToggle(Tab1Page, "Aimbot Assist", function(state) aimbotEnabled = state end)
AddToggle(Tab1Page, "Círculo FOV", function(state) fovVisible = state end)
AddToggle(Tab1Page, "Hitbox Head", function(state) hitboxHeadEnabled = state end)
AddSlider(Tab1Page, "Suavidad Aimbot", 1, 15, 4, function(val) smoothness = val end)
AddSlider(Tab1Page, "Tamaño Hitbox Head", 1, 15, 2, function(val) hitboxHeadSize = val end)

RunService.RenderStepped:Connect(function()
    if not ScreenGui.Parent then return end

    local mousePos = UserInputService:GetMouseLocation()
    FOVCircle.Position = mousePos
    FOVCircle.Radius = fovRadius
    FOVCircle.Visible = aimbotEnabled and fovVisible

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local head = player.Character:FindFirstChild("Head")
            local hum = player.Character:FindFirstChildOfClass("Humanoid")
            if head and hum and hum.Health > 0 and IsEnemy(player) then
                if hitboxHeadEnabled then
                    head.Size = Vector3.new(hitboxHeadSize, hitboxHeadSize, hitboxHeadSize)
                    head.CanCollide = false
                    head.Massless = true
                elseif head.Size ~= Vector3.new(2, 1, 1) then
                    head.Size = Vector3.new(2, 1, 1)
                end
            end
        end
    end

    if aimbotEnabled and UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
        local bestHead = nil
        local bestDist = fovRadius

        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local head = player.Character:FindFirstChild("Head")
                local hum = player.Character:FindFirstChildOfClass("Humanoid")
                if head and hum and hum.Health > 0 and IsEnemy(player) then
                    local screenPt, onScreen = Camera:WorldToViewportPoint(head.Position)
                    if onScreen then
                        local dist = (Vector2.new(screenPt.X, screenPt.Y) - mousePos).Magnitude
                        if dist < bestDist then
                            bestDist = dist
                            bestHead = head
                        end
                    end
                end
            end
        end

        if bestHead then
            local factor = math.clamp(smoothness, 1, 20)
            Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, bestHead.Position), 1 / factor)
        end
    end
end)

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

RunService.RenderStepped:Connect(function()
    if not ScreenGui.Parent then return end

    local activeTracers = {}
    local anyEsp = espBox or espNames or espHealth or espDist or espTracer

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local char = player.Character
            local hrp = char:FindFirstChild("HumanoidRootPart")
            local hum = char:FindFirstChildOfClass("Humanoid")

            if hrp and hum and hum.Health > 0 and IsEnemy(player) and anyEsp then
                local folder = char:FindFirstChild("ElianESP")
                if not folder then
                    folder = Instance.new("Folder")
                    folder.Name = "ElianESP"
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

                if espTracer then
                    local line = tracerLines[player]
                    if not line then
                        line = Drawing.new("Line")
                        line.Thickness = 1.5
                        line.Color = Theme.Accent
                        line.Transparency = 0.75
                        tracerLines[player] = line
                    end
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
            elseif char:FindFirstChild("ElianESP") then
                char.ElianESP:Destroy()
            end
        end
    end

    for p, line in pairs(tracerLines) do
        if not activeTracers[p] then
            line.Visible = false
        end
    end
end)

-- ================================================================= --
--                  7. ETC (ARMERÍA, FLY, SPEED, NOCLIP)             --
-- ================================================================= --
AddAction(Tab3Page, "Ir a la Armería", function()
    local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then
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
    if hum then hum.PlatformStand = false end
end

AddToggle(Tab3Page, "Fly (Vuelo)", function(state)
    flyActive = state
    local char = LocalPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChildOfClass("Humanoid")

    if flyActive and hrp then
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

        task.spawn(function()
            while flyActive and ScreenGui.Parent and hrp.Parent do
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
        hum.WalkSpeed = 16
    end
end)

AddSlider(Tab3Page, "Ajustar Speed", 1, 50, 30, function(val) speedValue = val end)

RunService.RenderStepped:Connect(function()
    if speedActive and ScreenGui.Parent then
        local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then
            hum.WalkSpeed = speedValue * 0.85
        end
    end
end)

local noclipActive = false
AddToggle(Tab3Page, "Noclip", function(state) noclipActive = state end)

RunService.Stepped:Connect(function()
    if noclipActive and ScreenGui.Parent and LocalPlayer.Character then
        for _, part in ipairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- ================================================================= --
--                  8. ROLES DINÁMICOS (POLICÍA / PRESO / CRIM)     --
-- ================================================================= --
local function GetPlayerRole(player)
    if not player or not player.Team then return "neutral" end
    local tName = string.lower(player.Team.Name)
    if string.find(tName, "pol") or string.find(tName, "guard") or string.find(tName, "security") then
        return "police"
    elseif string.find(tName, "crim") then
        return "criminal"
    elseif string.find(tName, "pris") or string.find(tName, "prision") or string.find(tName, "inmat") then
        return "prisoner"
    end
    return "other"
end

local function EquipAndFireCuffs(targetPlayer)
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

        pcall(function()
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
        end)

        task.wait(0.3)
        hrp.CFrame = savedCF
    end
end

local function RemoveDynamicTab()
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
        RefreshRoleTab()
        task.wait(1)
    end
end)

-- ================================================================= --
--                     9. COMANDO /COPIAR EN CHAT                    --
-- ================================================================= --
local function OnChatMessage(msg)
    if string.lower(msg) == "/copiar" then
        local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if root then
            local p = root.Position
            local formatted = string.format("Vector3.new(%.2f, %.2f, %.2f)", p.X, p.Y, p.Z)
            if setclipboard then
                setclipboard(formatted)
            end
        end
    end
end

if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
    TextChatService.MessageReceived:Connect(function(tcMsg)
        if tcMsg.TextSource and tcMsg.TextSource.UserId == LocalPlayer.UserId then
            OnChatMessage(tcMsg.Text)
        end
    end)
else
    LocalPlayer.Chatted:Connect(OnChatMessage)
end

-- ================================================================= --
--                     10. SECUENCIA DE INICIO                       --
-- ================================================================= --
task.spawn(function()
    local steps = {
        "Verificando entorno del jugador...",
        "Cargando pestañas Combat, ESP y Etc...",
        "Aplicando bordes redondeados...",
        "Listo para iniciar."
    }

    for i, text in ipairs(steps) do
        if not ScreenGui.Parent then return end
        LoadLog.Text = "> " .. text
        TweenService:Create(BarFill, TweenInfo.new(0.45, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {
            Size = UDim2.new(i / #steps, 0, 1, 0)
        }):Play()
        task.wait(0.55)
    end

    if not ScreenGui.Parent then return end
    task.wait(0.2)
    LoadScreen:Destroy()

    NewsFrame.Visible = true
    TweenService:Create(NewsFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Position = UDim2.new(0.5, 0, 0.5, 0)
    }):Play()
end)
