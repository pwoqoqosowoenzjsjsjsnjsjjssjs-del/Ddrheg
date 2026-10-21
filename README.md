local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local TeleportService = game:GetService("TeleportService")
local VirtualUser = game:GetService("VirtualUser")
local player = Players.LocalPlayer
local camera = workspace.CurrentCamera

local alive = true
local conns = {}
local function bind(c) table.insert(conns, c) return c end

local function char() return player.Character end
local function hum() local c = char() return c and c:FindFirstChildOfClass("Humanoid") end
local function root() local c = char() return c and c:FindFirstChild("HumanoidRootPart") end

local function tween(o, t, p, s, d)
	local tw = TweenService:Create(o, TweenInfo.new(t, s or Enum.EasingStyle.Quint, d or Enum.EasingDirection.Out), p)
	tw:Play()
	return tw
end
local function corner(p, r) local c = Instance.new("UICorner") c.CornerRadius = UDim.new(0, r or 10) c.Parent = p return c end
local function stroke(p, col, tr, th) local s = Instance.new("UIStroke") s.Color = col s.Transparency = tr or 0.92 s.Thickness = th or 1 s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border s.Parent = p return s end
local function pad(p, l, r, t, b) local u = Instance.new("UIPadding") u.PaddingLeft = UDim.new(0, l or 0) u.PaddingRight = UDim.new(0, r or 0) u.PaddingTop = UDim.new(0, t or 0) u.PaddingBottom = UDim.new(0, b or 0) u.Parent = p return u end
local function C3(r, g, b) return Color3.fromRGB(r, g, b) end

local T = {
	win = C3(20, 20, 22), card = C3(30, 30, 33), cardHover = C3(37, 37, 41),
	text = C3(236, 236, 240), sub = C3(128, 128, 136), white = C3(255, 255, 255),
	tabOn = C3(46, 46, 50), tabOff = C3(27, 27, 30), track = C3(17, 17, 19),
	knobOff = C3(64, 64, 68), accent = C3(38, 172, 102), accent2 = C3(52, 214, 130),
	badge = C3(250, 182, 35), gradA = C3(16, 58, 41), gradB = C3(26, 168, 102),
}

local function greenGradient(p, rot)
	local g = Instance.new("UIGradient")
	g.Color = ColorSequence.new({ColorSequenceKeypoint.new(0, T.gradA), ColorSequenceKeypoint.new(1, T.gradB)})
	g.Rotation = rot or 0
	g.Offset = Vector2.new(-0.15, 0)
	g.Parent = p
	TweenService:Create(g, TweenInfo.new(3.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {Offset = Vector2.new(0.15, 0)}):Play()
	return g
end

-- ===== ScreenGui =====
local gui = Instance.new("ScreenGui")
gui.Name = "PeidonesHub_FP"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.IgnoreGuiInset = true
local okParent = pcall(function() gui.Parent = (gethui and gethui()) or game:GetService("CoreGui") end)
if not okParent or not gui.Parent then gui.Parent = player:WaitForChild("PlayerGui") end

-- ===== Janela =====
local W, H = 400, 460
local baseScale = 1
local main = Instance.new("CanvasGroup")
main.Size = UDim2.fromOffset(W, H)
main.Position = UDim2.fromScale(0.5, 0.5)
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.BackgroundColor3 = T.win
main.BorderSizePixel = 0
main.GroupTransparency = 1
main.Parent = gui
corner(main, 16)
stroke(main, T.white, 0.9)
local scale = Instance.new("UIScale", main)
scale.Scale = 0.85

-- header
local header = Instance.new("Frame", main)
header.Size = UDim2.new(1, 0, 0, 58)
header.BackgroundTransparency = 1

local titleRow = Instance.new("Frame", header)
titleRow.Size = UDim2.new(0, 260, 0, 24)
titleRow.Position = UDim2.fromOffset(20, 14)
titleRow.BackgroundTransparency = 1
local trl = Instance.new("UIListLayout", titleRow)
trl.FillDirection = Enum.FillDirection.Horizontal
trl.VerticalAlignment = Enum.VerticalAlignment.Center
trl.Padding = UDim.new(0, 8)

local title = Instance.new("TextLabel", titleRow)
title.AutomaticSize = Enum.AutomaticSize.X
title.Size = UDim2.fromOffset(0, 24)
title.BackgroundTransparency = 1
title.Text = "Peidones Hub"
title.Font = Enum.Font.GothamMedium
title.TextSize = 17
title.TextColor3 = T.text

local badge = Instance.new("Frame", titleRow)
badge.AutomaticSize = Enum.AutomaticSize.X
badge.Size = UDim2.fromOffset(0, 22)
badge.BackgroundColor3 = T.badge
corner(badge, 11)
pad(badge, 10, 10)
local badgeStroke = stroke(badge, C3(255, 214, 110), 0.4, 1.5)
local badgeTxt = Instance.new("TextLabel", badge)
badgeTxt.AutomaticSize = Enum.AutomaticSize.X
badgeTxt.Size = UDim2.fromOffset(0, 22)
badgeTxt.BackgroundTransparency = 1
badgeTxt.Text = "updated"
badgeTxt.Font = Enum.Font.GothamBold
badgeTxt.TextSize = 12
badgeTxt.TextColor3 = C3(40, 28, 5)
TweenService:Create(badgeStroke, TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {Transparency = 1}):Play()

local subtitle = Instance.new("TextLabel", header)
subtitle.Size = UDim2.new(0, 200, 0, 14)
subtitle.Position = UDim2.fromOffset(20, 37)
subtitle.BackgroundTransparency = 1
subtitle.Text = "Futebol Proibido"
subtitle.Font = Enum.Font.Gotham
subtitle.TextSize = 11
subtitle.TextColor3 = C3(90, 90, 96)
subtitle.TextXAlignment = Enum.TextXAlignment.Left

local icons = Instance.new("Frame", header)
icons.Size = UDim2.new(0, 140, 0, 28)
icons.Position = UDim2.new(1, -154, 0, 14)
icons.BackgroundTransparency = 1
local il = Instance.new("UIListLayout", icons)
il.FillDirection = Enum.FillDirection.Horizontal
il.HorizontalAlignment = Enum.HorizontalAlignment.Right
il.VerticalAlignment = Enum.VerticalAlignment.Center
il.Padding = UDim.new(0, 4)

local function iconBtn(glyph, size, order)
	local b = Instance.new("TextButton", icons)
	b.Size = UDim2.fromOffset(28, 28)
	b.BackgroundColor3 = T.card
	b.BackgroundTransparency = 1
	b.Text = glyph
	b.Font = Enum.Font.GothamMedium
	b.TextSize = size
	b.TextColor3 = C3(160, 160, 168)
	b.AutoButtonColor = false
	b.LayoutOrder = order
	corner(b, 8)
	b.MouseEnter:Connect(function() tween(b, 0.2, {BackgroundTransparency = 0, TextColor3 = T.white}) end)
	b.MouseLeave:Connect(function() tween(b, 0.2, {BackgroundTransparency = 1, TextColor3 = C3(160, 160, 168)}) end)
	return b
end
local searchBtn = iconBtn("⌕", 18, 1)
local gearBtn = iconBtn("⚙", 17, 2)
local minBtn = iconBtn("—", 13, 3)
local closeBtn = iconBtn("✕", 14, 4)

-- tabs
local tabBar = Instance.new("CanvasGroup", main)
tabBar.Size = UDim2.new(1, -32, 0, 36)
tabBar.Position = UDim2.fromOffset(16, 62)
tabBar.BackgroundTransparency = 1
local tbl = Instance.new("UIListLayout", tabBar)
tbl.FillDirection = Enum.FillDirection.Horizontal
tbl.Padding = UDim.new(0, 6)
tbl.VerticalAlignment = Enum.VerticalAlignment.Center
tbl.SortOrder = Enum.SortOrder.LayoutOrder

-- search
local searchFrame = Instance.new("Frame", main)
searchFrame.Size = UDim2.new(0, 0, 0, 36)
searchFrame.Position = UDim2.fromOffset(16, 62)
searchFrame.BackgroundColor3 = T.tabOff
searchFrame.ClipsDescendants = true
searchFrame.Visible = false
corner(searchFrame, 12)
stroke(searchFrame, T.white, 0.9)
local sIcon = Instance.new("TextLabel", searchFrame)
sIcon.Size = UDim2.fromOffset(36, 36)
sIcon.BackgroundTransparency = 1
sIcon.Text = "⌕"
sIcon.TextSize = 18
sIcon.Font = Enum.Font.GothamMedium
sIcon.TextColor3 = T.sub
local searchBox = Instance.new("TextBox", searchFrame)
searchBox.Size = UDim2.new(1, -46, 1, 0)
searchBox.Position = UDim2.fromOffset(36, 0)
searchBox.BackgroundTransparency = 1
searchBox.PlaceholderText = "Pesquisar..."
searchBox.PlaceholderColor3 = T.sub
searchBox.Text = ""
searchBox.Font = Enum.Font.Gotham
searchBox.TextSize = 14
searchBox.TextColor3 = T.text
searchBox.TextXAlignment = Enum.TextXAlignment.Left
searchBox.ClearTextOnFocus = false

-- conteúdo
local content = Instance.new("CanvasGroup", main)
content.Size = UDim2.new(1, -32, 1, -112)
content.Position = UDim2.fromOffset(16, 106)
content.BackgroundTransparency = 1

-- ===== Notificação / Toast =====
local notifsOn = true
local notifHolder = Instance.new("Frame", gui)
notifHolder.Size = UDim2.fromOffset(280, 500)
notifHolder.Position = UDim2.new(1, -20, 1, -20)
notifHolder.AnchorPoint = Vector2.new(1, 1)
notifHolder.BackgroundTransparency = 1
local nhl = Instance.new("UIListLayout", notifHolder)
nhl.Padding = UDim.new(0, 8)
nhl.VerticalAlignment = Enum.VerticalAlignment.Bottom
nhl.SortOrder = Enum.SortOrder.LayoutOrder

local function notify(t, msg, dur)
	if not notifsOn or not alive then return end
	dur = dur or 3.5
	local wrap = Instance.new("Frame", notifHolder)
	wrap.Size = UDim2.new(1, 0, 0, 66)
	wrap.BackgroundTransparency = 1
	local card = Instance.new("Frame", wrap)
	card.Size = UDim2.fromScale(1, 1)
	card.Position = UDim2.new(1, 40, 0, 0)
	card.BackgroundColor3 = T.win
	card.ClipsDescendants = true
	corner(card, 12)
	stroke(card, T.white, 0.88)
	local ic = Instance.new("TextLabel", card)
	ic.Size = UDim2.fromOffset(32, 32)
	ic.Position = UDim2.fromOffset(12, 14)
	ic.BackgroundColor3 = T.accent
	ic.Text = "✦"
	ic.TextSize = 16
	ic.Font = Enum.Font.GothamBold
	ic.TextColor3 = T.white
	corner(ic, 9)
	greenGradient(ic, 45)
	local a = Instance.new("TextLabel", card)
	a.Size = UDim2.new(1, -66, 0, 18)
	a.Position = UDim2.fromOffset(54, 13)
	a.BackgroundTransparency = 1
	a.Text = t
	a.Font = Enum.Font.GothamBold
	a.TextSize = 13
	a.TextColor3 = T.text
	a.TextXAlignment = Enum.TextXAlignment.Left
	local b = Instance.new("TextLabel", card)
	b.Size = UDim2.new(1, -66, 0, 16)
	b.Position = UDim2.fromOffset(54, 32)
	b.BackgroundTransparency = 1
	b.Text = msg
	b.Font = Enum.Font.Gotham
	b.TextSize = 12
	b.TextColor3 = T.sub
	b.TextXAlignment = Enum.TextXAlignment.Left
	b.TextTruncate = Enum.TextTruncate.AtEnd
	local prog = Instance.new("Frame", card)
	prog.Size = UDim2.new(1, 0, 0, 2)
	prog.Position = UDim2.new(0, 0, 1, -2)
	prog.BackgroundColor3 = T.accent2
	prog.BorderSizePixel = 0
	tween(card, 0.55, {Position = UDim2.new()}, Enum.EasingStyle.Back)
	tween(prog, dur, {Size = UDim2.new(0, 0, 0, 2)}, Enum.EasingStyle.Linear)
	task.delay(dur, function()
		tween(card, 0.35, {Position = UDim2.new(1, 40, 0, 0)}, Enum.EasingStyle.Quint, Enum.EasingDirection.In).Completed:Wait()
		tween(wrap, 0.25, {Size = UDim2.new(1, 0, 0, 0)}).Completed:Wait()
		wrap:Destroy()
	end)
end

local function toast(msg)
	if not notifsOn or not alive then return end
	local f = Instance.new("Frame", gui)
	f.AnchorPoint = Vector2.new(0.5, 0)
	f.Position = UDim2.new(0.5, 0, 0, -60)
	f.Size = UDim2.fromOffset(0, 40)
	f.AutomaticSize = Enum.AutomaticSize.X
	f.BackgroundColor3 = C3(8, 8, 10)
	corner(f, 20)
	stroke(f, T.white, 0.88)
	pad(f, 16, 18)
	local l = Instance.new("TextLabel", f)
	l.AutomaticSize = Enum.AutomaticSize.X
	l.Size = UDim2.fromOffset(0, 40)
	l.BackgroundTransparency = 1
	l.Text = "✦  " .. msg
	l.Font = Enum.Font.GothamMedium
	l.TextSize = 14
	l.TextColor3 = T.text
	tween(f, 0.55, {Position = UDim2.new(0.5, 0, 0, 24)}, Enum.EasingStyle.Back)
	task.delay(2.4, function()
		tween(f, 0.35, {Position = UDim2.new(0.5, 0, 0, -60)}, Enum.EasingStyle.Quint, Enum.EasingDirection.In).Completed:Wait()
		f:Destroy()
	end)
end

-- ===== Pílula minimizada =====
local pill = Instance.new("TextButton", gui)
pill.Size = UDim2.fromOffset(290, 64)
pill.AnchorPoint = Vector2.new(0.5, 0)
pill.Position = UDim2.new(0.5, 0, 0, -90)
pill.BackgroundColor3 = C3(8, 8, 10)
pill.Text = ""
pill.AutoButtonColor = false
pill.Visible = false
corner(pill, 32)
stroke(pill, T.white, 0.9)
local pillScale = Instance.new("UIScale", pill)
local logo = Instance.new("TextLabel", pill)
logo.Size = UDim2.fromOffset(44, 44)
logo.Position = UDim2.fromOffset(14, 10)
logo.BackgroundTransparency = 1
logo.Text = "✦"
logo.TextSize = 34
logo.Font = Enum.Font.GothamBold
logo.TextColor3 = T.white
local pt = Instance.new("TextLabel", pill)
pt.Size = UDim2.new(1, -80, 0, 22)
pt.Position = UDim2.fromOffset(70, 11)
pt.BackgroundTransparency = 1
pt.Text = "Peidones Hub"
pt.Font = Enum.Font.GothamMedium
pt.TextSize = 18
pt.TextColor3 = T.text
pt.TextXAlignment = Enum.TextXAlignment.Left
local ps = Instance.new("TextLabel", pill)
ps.Size = UDim2.new(1, -80, 0, 18)
ps.Position = UDim2.fromOffset(70, 33)
ps.BackgroundTransparency = 1
ps.Text = "Toque pra mostrar"
ps.Font = Enum.Font.Gotham
ps.TextSize = 14
ps.TextColor3 = C3(110, 110, 118)
ps.TextXAlignment = Enum.TextXAlignment.Left
TweenService:Create(logo, TweenInfo.new(6, Enum.EasingStyle.Linear, Enum.EasingDirection.Out, -1), {Rotation = 360}):Play()
pill.MouseEnter:Connect(function() tween(pillScale, 0.25, {Scale = 1.04}, Enum.EasingStyle.Back) end)
pill.MouseLeave:Connect(function() tween(pillScale, 0.25, {Scale = 1}) end)

local minimized = false
local function minimize()
	if minimized then return end
	minimized = true
	tween(scale, 0.3, {Scale = baseScale * 0.88}, Enum.EasingStyle.Quint, Enum.EasingDirection.In)
	tween(main, 0.3, {GroupTransparency = 1}).Completed:Connect(function() if minimized then main.Visible = false end end)
	pill.Visible = true
	pill.Position = UDim2.new(0.5, 0, 0, -90)
	tween(pill, 0.6, {Position = UDim2.new(0.5, 0, 0, 18)}, Enum.EasingStyle.Back)
end
local function restore()
	if not minimized then return end
	minimized = false
	tween(pill, 0.3, {Position = UDim2.new(0.5, 0, 0, -90)}, Enum.EasingStyle.Quint, Enum.EasingDirection.In).Completed:Connect(function() if not minimized then pill.Visible = false end end)
	main.Visible = true
	tween(scale, 0.5, {Scale = baseScale}, Enum.EasingStyle.Back)
	tween(main, 0.35, {GroupTransparency = 0})
end
pill.MouseButton1Click:Connect(restore)
minBtn.MouseButton1Click:Connect(minimize)
bind(UIS.InputBegan:Connect(function(i, gp)
	if not gp and i.KeyCode == Enum.KeyCode.RightControl then if minimized then restore() else minimize() end end
end))

-- arrastar
do
	local dragging, sp, sm
	header.InputBegan:Connect(function(i)
		if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
			dragging, sp, sm = true, main.Position, i.Position
			i.Changed:Connect(function() if i.UserInputState == Enum.UserInputState.End then dragging = false end end)
		end
	end)
	bind(UIS.InputChanged:Connect(function(i)
		if dragging and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
			local d = i.Position - sm
			tween(main, 0.15, {Position = UDim2.new(sp.X.Scale, sp.X.Offset + d.X, sp.Y.Scale, sp.Y.Offset + d.Y)})
		end
	end))
end

-- ===== Sistema de páginas =====
local pages, current = {}, nil

local function pressFx(inner)
	local s = inner:FindFirstChildOfClass("UIScale")
	if not s then return end
	tween(s, 0.08, {Scale = 0.97}).Completed:Connect(function() tween(s, 0.35, {Scale = 1}, Enum.EasingStyle.Back) end)
end

local function makeCard(parent)
	local inner = Instance.new("Frame", parent)
	inner.Size = UDim2.fromScale(1, 1)
	inner.BackgroundColor3 = T.card
	inner.BorderSizePixel = 0
	corner(inner, 12)
	local st = stroke(inner, T.white, 0.95)
	Instance.new("UIScale", inner)
	inner.MouseEnter:Connect(function() tween(inner, 0.2, {BackgroundColor3 = T.cardHover}) tween(st, 0.2, {Transparency = 0.9}) end)
	inner.MouseLeave:Connect(function() tween(inner, 0.2, {BackgroundColor3 = T.card}) tween(st, 0.2, {Transparency = 0.95}) end)
	return inner
end

local function rowLabel(inner, txt, y, h)
	local l = Instance.new("TextLabel", inner)
	l.Size = UDim2.new(1, -90, 0, h or 0)
	l.Position = UDim2.fromOffset(18, y or 0)
	if not h then l.Size = UDim2.new(1, -90, 1, 0) end
	l.BackgroundTransparency = 1
	l.Text = txt
	l.Font = Enum.Font.GothamMedium
	l.TextSize = 14
	l.TextColor3 = T.text
	l.TextXAlignment = Enum.TextXAlignment.Left
	l.TextTruncate = Enum.TextTruncate.AtEnd
	return l
end

local function hitButton(inner)
	local b = Instance.new("TextButton", inner)
	b.Size = UDim2.fromScale(1, 1)
	b.BackgroundTransparency = 1
	b.Text = ""
	b.ZIndex = 5
	return b
end

local function decorateToggle(inner, name, def, cb)
	rowLabel(inner, name)
	local trk = Instance.new("Frame", inner)
	trk.Size = UDim2.fromOffset(52, 28)
	trk.Position = UDim2.new(1, -66, 0.5, -14)
	trk.BackgroundColor3 = T.track
	corner(trk, 14)
	local trkStroke = stroke(trk, T.white, 0.88)
	local knob = Instance.new("Frame", trk)
	knob.Size = UDim2.fromOffset(28, 20)
	knob.Position = UDim2.new(0, 4, 0.5, -10)
	knob.BackgroundColor3 = T.knobOff
	corner(knob, 10)
	local kStroke = stroke(knob, T.accent2, 1, 1.5)
	local state = false
	local function render()
		tween(knob, 0.12, {Size = UDim2.fromOffset(34, 20)}).Completed:Connect(function()
			tween(knob, 0.35, {Size = UDim2.fromOffset(28, 20)}, Enum.EasingStyle.Back)
		end)
		tween(knob, 0.4, {Position = UDim2.new(0, state and 20 or 4, 0.5, -10), BackgroundColor3 = state and T.accent or T.knobOff})
		tween(kStroke, 0.3, {Transparency = state and 0.35 or 1})
		tween(trkStroke, 0.3, {Color = state and T.accent or T.white, Transparency = state and 0.6 or 0.88})
	end
	local function set(v, silent)
		if state == v then return end
		state = v
		render()
		if not silent then task.spawn(cb, v) end
	end
	hitButton(inner).MouseButton1Click:Connect(function() pressFx(inner) set(not state) end)
	if def then set(true) end
	return {Set = set, Get = function() return state end}
end

local function Page(name, glyph)
	local p = {rows = {}, items = {}, sections = {}, order = 0}
	local btn = Instance.new("TextButton", tabBar)
	btn.AutomaticSize = Enum.AutomaticSize.X
	btn.Size = UDim2.fromOffset(0, 34)
	btn.BackgroundColor3 = T.tabOff
	btn.Text = glyph .. "  " .. name
	btn.Font = Enum.Font.GothamMedium
	btn.TextSize = 14
	btn.TextColor3 = T.sub
	btn.AutoButtonColor = false
	btn.LayoutOrder = #pages + 1
	corner(btn, 13)
	pad(btn, 14, 16)
	local bStroke = stroke(btn, T.white, 0.94)
	local bScale = Instance.new("UIScale", btn)
	p.btn, p.bStroke = btn, bStroke

	local page = Instance.new("ScrollingFrame", content)
	page.Size = UDim2.fromScale(1, 1)
	page.BackgroundTransparency = 1
	page.BorderSizePixel = 0
	page.ScrollBarThickness = 2
	page.ScrollBarImageColor3 = T.knobOff
	page.AutomaticCanvasSize = Enum.AutomaticSize.Y
	page.CanvasSize = UDim2.new()
	page.Visible = false
	local pl = Instance.new("UIListLayout", page)
	pl.Padding = UDim.new(0, 8)
	pl.SortOrder = Enum.SortOrder.LayoutOrder
	pad(page, 2, 4, 2, 12)
	p.page = page

	btn.MouseEnter:Connect(function() if current ~= p then tween(btn, 0.2, {TextColor3 = T.text}) end end)
	btn.MouseLeave:Connect(function() if current ~= p then tween(btn, 0.2, {TextColor3 = T.sub}) end end)
	btn.MouseButton1Click:Connect(function()
		tween(bScale, 0.08, {Scale = 0.94}).Completed:Connect(function() tween(bScale, 0.35, {Scale = 1}, Enum.EasingStyle.Back) end)
		p:Select()
	end)

	function p:Select()
		if current == p then return end
		for _, o in ipairs(pages) do
			o.page.Visible = false
			tween(o.btn, 0.25, {BackgroundColor3 = T.tabOff, TextColor3 = T.sub})
			tween(o.bStroke, 0.25, {Transparency = 0.94})
		end
		current = p
		tween(btn, 0.25, {BackgroundColor3 = T.tabOn, TextColor3 = T.white})
		tween(bStroke, 0.25, {Transparency = 0.85})
		page.Visible = true
		page.CanvasPosition = Vector2.zero
		content.GroupTransparency = 1
		tween(content, 0.35, {GroupTransparency = 0})
		for i, inner in ipairs(p.rows) do
			inner.Position = UDim2.fromOffset(0, 16)
			task.delay(math.min(i, 14) * 0.03, function() tween(inner, 0.45, {Position = UDim2.new()}) end)
		end
		searchBox.Text = ""
	end

	local function newWrap(h)
		p.order += 1
		local wrap = Instance.new("Frame", page)
		wrap.Size = UDim2.new(1, 0, 0, h)
		wrap.BackgroundTransparency = 1
		wrap.LayoutOrder = p.order
		return wrap
	end
	local function newRow(h, name)
		local wrap = newWrap(h)
		local inner = makeCard(wrap)
		table.insert(p.rows, inner)
		if name then table.insert(p.items, {wrap = wrap, name = name}) end
		return wrap, inner
	end

	function p:Section(txt)
		local wrap = newWrap(26)
		local ic = Instance.new("TextLabel", wrap)
		ic.Size = UDim2.fromOffset(18, 26)
		ic.Position = UDim2.fromOffset(4, 0)
		ic.BackgroundTransparency = 1
		ic.Text = "⌂"
		ic.TextSize = 15
		ic.Font = Enum.Font.GothamBold
		ic.TextColor3 = C3(100, 100, 108)
		local l = Instance.new("TextLabel", wrap)
		l.Size = UDim2.new(1, -30, 1, 0)
		l.Position = UDim2.fromOffset(28, 0)
		l.BackgroundTransparency = 1
		l.Text = txt
		l.Font = Enum.Font.GothamMedium
		l.TextSize = 13
		l.TextColor3 = C3(120, 120, 128)
		l.TextXAlignment = Enum.TextXAlignment.Left
		table.insert(p.sections, wrap)
	end

	function p:Toggle(name, def, cb)
		local _, inner = newRow(50, name)
		return decorateToggle(inner, name, def, cb)
	end

	function p:Grid()
		p.order += 1
		local wrap = Instance.new("Frame", page)
		wrap.Size = UDim2.new(1, 0, 0, 0)
		wrap.AutomaticSize = Enum.AutomaticSize.Y
		wrap.BackgroundTransparency = 1
		wrap.LayoutOrder = p.order
		local g = Instance.new("UIGridLayout", wrap)
		g.CellSize = UDim2.new(0.5, -4, 0, 48)
		g.CellPadding = UDim2.fromOffset(8, 8)
		g.SortOrder = Enum.SortOrder.LayoutOrder
		local grid, n = {}, 0
		function grid:Toggle(name, def, cb)
			n += 1
			local cell = Instance.new("Frame", wrap)
			cell.BackgroundTransparency = 1
			cell.LayoutOrder = n
			local inner = makeCard(cell)
			table.insert(p.rows, inner)
			table.insert(p.items, {wrap = cell, name = name})
			return decorateToggle(inner, name, def, cb)
		end
		return grid
	end

	function p:Slider(name, min, max, def, suffix, cb)
		local _, inner = newRow(64, name)
		rowLabel(inner, name, 14, 18)
		local val = Instance.new("TextLabel", inner)
		val.Size = UDim2.fromOffset(120, 16)
		val.Position = UDim2.fromOffset(18, 34)
		val.BackgroundTransparency = 1
		val.Font = Enum.Font.Gotham
		val.TextSize = 13
		val.TextColor3 = C3(160, 160, 168)
		val.TextXAlignment = Enum.TextXAlignment.Left
		val.Text = def .. (suffix or "")
		local trk = Instance.new("Frame", inner)
		trk.Size = UDim2.new(0.52, -18, 0, 12)
		trk.Position = UDim2.new(0.48, 0, 0.5, -6)
		trk.BackgroundColor3 = C3(40, 40, 44)
		corner(trk, 6)
		local a0 = (def - min) / (max - min)
		local fill = Instance.new("Frame", trk)
		fill.Size = UDim2.fromScale(a0, 1)
		fill.BackgroundColor3 = T.white
		fill.BorderSizePixel = 0
		corner(fill, 6)
		local fg = Instance.new("UIGradient", fill)
		fg.Color = ColorSequence.new(C3(18, 92, 60), C3(40, 190, 112))
		local knob = Instance.new("Frame", trk)
		knob.Size = UDim2.fromOffset(34, 22)
		knob.AnchorPoint = Vector2.new(0.5, 0.5)
		knob.Position = UDim2.fromScale(a0, 0.5)
		knob.BackgroundColor3 = T.white
		knob.ZIndex = 3
		corner(knob, 11)
		local kStroke = stroke(knob, T.accent2, 1, 2)
		local dragging = false
		local function set(x)
			local a = math.clamp((x - trk.AbsolutePosition.X) / trk.AbsoluteSize.X, 0, 1)
			local v = math.floor(min + (max - min) * a + 0.5)
			val.Text = v .. (suffix or "")
			tween(fill, 0.15, {Size = UDim2.fromScale(a, 1)})
			tween(knob, 0.15, {Position = UDim2.fromScale(a, 0.5)})
			task.spawn(cb, v)
		end
		local hit = Instance.new("TextButton", inner)
		hit.Size = UDim2.new(0.52, 0, 1, 0)
		hit.Position = UDim2.new(0.48, -9, 0, 0)
		hit.BackgroundTransparency = 1
		hit.Text = ""
		hit.ZIndex = 5
		hit.InputBegan:Connect(function(i)
			if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
				dragging = true
				tween(knob, 0.25, {Size = UDim2.fromOffset(40, 24)}, Enum.EasingStyle.Back)
				tween(kStroke, 0.2, {Transparency = 0.3})
				tween(val, 0.2, {TextColor3 = T.accent2})
				set(i.Position.X)
			end
		end)
		bind(UIS.InputChanged:Connect(function(i)
			if dragging and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then set(i.Position.X) end
		end))
		bind(UIS.InputEnded:Connect(function(i)
			if dragging and (i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch) then
				dragging = false
				tween(knob, 0.3, {Size = UDim2.fromOffset(34, 22)}, Enum.EasingStyle.Back)
				tween(kStroke, 0.2, {Transparency = 1})
				tween(val, 0.2, {TextColor3 = C3(160, 160, 168)})
			end
		end))
	end

	function p:Button(name, cb)
		local _, inner = newRow(50, name)
		inner.ClipsDescendants = true
		rowLabel(inner, name)
		local arrow = Instance.new("TextLabel", inner)
		arrow.Size = UDim2.fromOffset(20, 20)
		arrow.Position = UDim2.new(1, -36, 0.5, -10)
		arrow.BackgroundTransparency = 1
		arrow.Text = "›"
		arrow.Font = Enum.Font.GothamBold
		arrow.TextSize = 20
		arrow.TextColor3 = C3(90, 90, 98)
		local hit = hitButton(inner)
		hit.MouseButton1Down:Connect(function(x, y)
			local r = Instance.new("Frame", inner)
			r.AnchorPoint = Vector2.new(0.5, 0.5)
			r.Position = UDim2.fromOffset(x - inner.AbsolutePosition.X, y - inner.AbsolutePosition.Y)
			r.Size = UDim2.fromOffset(0, 0)
			r.BackgroundColor3 = T.white
			r.BackgroundTransparency = 0.88
			r.ZIndex = 2
			corner(r, 999)
			tween(r, 0.6, {Size = UDim2.fromOffset(420, 420), BackgroundTransparency = 1}).Completed:Connect(function() r:Destroy() end)
		end)
		hit.MouseButton1Click:Connect(function()
			pressFx(inner)
			tween(arrow, 0.12, {Position = UDim2.new(1, -28, 0.5, -10), TextColor3 = T.accent2}).Completed:Connect(function()
				tween(arrow, 0.4, {Position = UDim2.new(1, -36, 0.5, -10), TextColor3 = C3(90, 90, 98)}, Enum.EasingStyle.Back)
			end)
			task.spawn(cb)
		end)
	end

	local function counter(lbl, fmt)
		local nv = Instance.new("NumberValue")
		nv.Changed:Connect(function(v) lbl.Text = fmt(v) end)
		return function(n)
			if type(n) ~= "number" then lbl.Text = tostring(n) return end
			tween(nv, 0.6, {Value = n})
		end
	end

	function p:Stats(a, b)
		p.order += 1
		local wrap = Instance.new("Frame", page)
		wrap.Size = UDim2.new(1, 0, 0, 52)
		wrap.BackgroundTransparency = 1
		wrap.LayoutOrder = p.order
		local g = Instance.new("UIGridLayout", wrap)
		g.CellSize = UDim2.new(0.5, -4, 0, 52)
		g.CellPadding = UDim2.fromOffset(8, 0)
		local setters = {}
		for idx, s in ipairs({a, b}) do
			local cell = Instance.new("Frame", wrap)
			cell.BackgroundTransparency = 1
			cell.LayoutOrder = idx
			local inner = Instance.new("Frame", cell)
			inner.Size = UDim2.fromScale(1, 1)
			inner.BackgroundColor3 = T.white
			corner(inner, 12)
			local st = stroke(inner, T.accent2, 0.55, 1.2)
			Instance.new("UIScale", inner)
			greenGradient(inner, 0)
			inner.MouseEnter:Connect(function() tween(st, 0.25, {Transparency = 0.1}) end)
			inner.MouseLeave:Connect(function() tween(st, 0.25, {Transparency = 0.55}) end)
			local l = Instance.new("TextLabel", inner)
			l.Size = UDim2.new(0.5, 0, 1, 0)
			l.Position = UDim2.fromOffset(16, 0)
			l.BackgroundTransparency = 1
			l.Text = s[1]
			l.Font = Enum.Font.GothamMedium
			l.TextSize = 14
			l.TextColor3 = T.white
			l.TextXAlignment = Enum.TextXAlignment.Left
			local v = Instance.new("TextLabel", inner)
			v.Size = UDim2.new(0.5, -16, 1, 0)
			v.Position = UDim2.new(0.5, 0, 0, 0)
			v.BackgroundTransparency = 1
			v.Text = "0"
			v.Font = Enum.Font.GothamBold
			v.TextSize = 15
			v.TextColor3 = T.white
			v.TextXAlignment = Enum.TextXAlignment.Right
			table.insert(p.rows, inner)
			setters[idx] = counter(v, s[2])
		end
		return setters[1], setters[2]
	end

	function p:BigCard(titleTxt, fmt)
		local wrap = newWrap(104)
		local inner = Instance.new("Frame", wrap)
		inner.Size = UDim2.fromScale(1, 1)
		inner.BackgroundColor3 = T.white
		corner(inner, 14)
		local st = stroke(inner, T.accent2, 0.5, 1.2)
		greenGradient(inner, 20)
		table.insert(p.rows, inner)
		TweenService:Create(st, TweenInfo.new(1.6, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {Transparency = 0.15}):Play()
		local t1 = Instance.new("TextLabel", inner)
		t1.Size = UDim2.new(1, -36, 0, 20)
		t1.Position = UDim2.fromOffset(20, 20)
		t1.BackgroundTransparency = 1
		t1.Text = titleTxt
		t1.Font = Enum.Font.GothamMedium
		t1.TextSize = 16
		t1.TextColor3 = T.white
		t1.TextXAlignment = Enum.TextXAlignment.Left
		local v = Instance.new("TextLabel", inner)
		v.Size = UDim2.new(0.6, 0, 0, 26)
		v.Position = UDim2.fromOffset(20, 56)
		v.BackgroundTransparency = 1
		v.Text = "0"
		v.Font = Enum.Font.GothamMedium
		v.TextSize = 22
		v.TextColor3 = T.white
		v.TextXAlignment = Enum.TextXAlignment.Left
		local dlt = Instance.new("TextLabel", inner)
		dlt.Size = UDim2.new(0.4, -20, 0, 20)
		dlt.Position = UDim2.new(0.6, 0, 0, 62)
		dlt.BackgroundTransparency = 1
		dlt.Text = ""
		dlt.Font = Enum.Font.GothamMedium
		dlt.TextSize = 13
		dlt.TextColor3 = C3(220, 255, 235)
		dlt.TextXAlignment = Enum.TextXAlignment.Right
		local setV = counter(v, fmt)
		return function(n, delta) setV(n) if delta then dlt.Text = delta end end
	end

	table.insert(pages, p)
	return p
end

-- busca
local searchOpen = false
searchBtn.MouseButton1Click:Connect(function()
	searchOpen = not searchOpen
	if searchOpen then
		searchFrame.Visible = true
		tween(tabBar, 0.25, {GroupTransparency = 1})
		tween(searchFrame, 0.45, {Size = UDim2.new(1, -32, 0, 36)})
		task.delay(0.15, function() searchBox:CaptureFocus() end)
	else
		searchBox.Text = ""
		tween(searchFrame, 0.3, {Size = UDim2.new(0, 0, 0, 36)}).Completed:Connect(function() if not searchOpen then searchFrame.Visible = false end end)
		tween(tabBar, 0.3, {GroupTransparency = 0})
	end
end)
searchBox:GetPropertyChangedSignal("Text"):Connect(function()
	if not current then return end
	local q = searchBox.Text:lower()
	for _, it in ipairs(current.items) do it.wrap.Visible = q == "" or it.name:lower():find(q, 1, true) ~= nil end
	for _, s in ipairs(current.sections) do s.Visible = q == "" end
end)

-- ===== Lógica do jogo =====
local S = {
	reach = false, reachSize = 8, showReach = false, magnet = false,
	ballEsp = false, tracer = false, esp = false, chams = false, crosshair = false,
	infJump = false, noclip = false, fov = nil, ws = nil, jp = nil,
}
local toques, sessionStart = 0, tick()

local ballCache, lastScan = nil, 0
local function getBall()
	if ballCache and ballCache.Parent then return ballCache end
	if tick() - lastScan < 1.5 then return nil end
	lastScan = tick()
	for _, n in ipairs({"Ball", "Football", "SoccerBall", "Bola", "TPS"}) do
		local f = workspace:FindFirstChild(n, true)
		if f and f:IsA("BasePart") then ballCache = f return f end
	end
	local mine = char()
	for _, d in ipairs(workspace:GetDescendants()) do
		if d:IsA("BasePart") and d.Size.Magnitude < 15 then
			local n = d.Name:lower()
			if (n:find("ball") or n:find("bola")) and not (mine and d:IsDescendantOf(mine)) then ballCache = d return d end
		end
	end
	return nil
end

local limbNames = {["Right Leg"] = true, ["Left Leg"] = true, RightFoot = true, LeftFoot = true, RightLowerLeg = true, LeftLowerLeg = true, Torso = true, UpperTorso = true, HumanoidRootPart = true}
local function limbs()
	local out, c = {}, char()
	if not c then return out end
	for _, p in ipairs(c:GetChildren()) do if p:IsA("BasePart") and limbNames[p.Name] then table.insert(out, p) end end
	return out
end

-- esfera do reach
local sphere = Instance.new("Part")
sphere.Shape = Enum.PartType.Ball
sphere.Anchored = true
sphere.CanCollide = false
sphere.CanTouch = false
sphere.CanQuery = false
sphere.Material = Enum.Material.ForceField
sphere.Color = T.accent2
sphere.Transparency = 1

-- tracer
local att0, att1, beam
local function clearTracer() if beam then beam:Destroy() att0:Destroy() att1:Destroy() beam = nil end end

-- esp bola
local ballHl, ballBb
local function clearBallEsp() if ballHl then ballHl:Destroy() ballHl = nil end if ballBb then ballBb:Destroy() ballBb = nil end end

-- crosshair
local cross = Instance.new("Frame", gui)
cross.Size = UDim2.fromOffset(20, 20)
cross.AnchorPoint = Vector2.new(0.5, 0.5)
cross.Position = UDim2.fromScale(0.5, 0.5)
cross.BackgroundTransparency = 1
cross.Visible = false
for _, sz in ipairs({UDim2.fromOffset(20, 2), UDim2.fromOffset(2, 20)}) do
	local b = Instance.new("Frame", cross)
	b.Size = sz
	b.AnchorPoint = Vector2.new(0.5, 0.5)
	b.Position = UDim2.fromScale(0.5, 0.5)
	b.BackgroundColor3 = T.accent2
	b.BorderSizePixel = 0
end

local warnedFTI = false
local acc = 0
bind(RunService.Heartbeat:Connect(function(dt)
	acc += dt
	if acc < 0.05 then return end
	acc = 0
	local ball, r, h = getBall(), root(), hum()
	if S.reach and ball and r then
		if not firetouchinterest then
			if not warnedFTI then warnedFTI = true notify("Reach", "Teu executor não tem firetouchinterest") end
		elseif (ball.Position - r.Position).Magnitude <= S.reachSize then
			for _, l in ipairs(limbs()) do
				firetouchinterest(l, ball, 0)
				firetouchinterest(l, ball, 1)
			end
			toques += 1
		end
	end
	if S.magnet and ball and h then h:MoveTo(ball.Position) end
end))

bind(RunService.RenderStepped:Connect(function()
	local ball, r = getBall(), root()
	if S.showReach and r then
		sphere.Parent = workspace
		sphere.Size = Vector3.one * S.reachSize * 2
		sphere.CFrame = r.CFrame
		sphere.Transparency = 0.6
	else
		sphere.Parent = nil
	end
	if S.tracer and ball and r then
		if not beam or att1.Parent ~= ball or att0.Parent ~= r then
			clearTracer()
			att0 = Instance.new("Attachment", r)
			att1 = Instance.new("Attachment", ball)
			beam = Instance.new("Beam", r)
			beam.Attachment0, beam.Attachment1 = att0, att1
			beam.Color = ColorSequence.new(T.accent2)
			beam.Width0, beam.Width1 = 0.15, 0.15
			beam.FaceCamera = true
			beam.LightEmission = 1
		end
	elseif beam then clearTracer() end
	if S.ballEsp and ball then
		if not ballHl or ballHl.Parent ~= ball then
			clearBallEsp()
			ballHl = Instance.new("Highlight")
			ballHl.FillColor = T.accent2
			ballHl.OutlineColor = T.white
			ballHl.FillTransparency = 0.4
			ballHl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
			ballHl.Parent = ball
			ballBb = Instance.new("BillboardGui")
			ballBb.Size = UDim2.fromOffset(120, 24)
			ballBb.StudsOffset = Vector3.new(0, 3, 0)
			ballBb.AlwaysOnTop = true
			ballBb.Parent = ball
			local t = Instance.new("TextLabel", ballBb)
			t.Size = UDim2.fromScale(1, 1)
			t.BackgroundTransparency = 1
			t.Font = Enum.Font.GothamBold
			t.TextSize = 14
			t.TextColor3 = T.white
			t.TextStrokeTransparency = 0.4
			t.Name = "D"
		end
		if r and ballBb:FindFirstChild("D") then ballBb.D.Text = "⚽ " .. math.floor((ball.Position - r.Position).Magnitude) .. "m" end
	elseif ballHl then clearBallEsp() end
	if S.fov then camera.FieldOfView = S.fov end
	local h = hum()
	if h then
		if S.ws then h.WalkSpeed = S.ws end
		if S.jp then h.UseJumpPower = true h.JumpPower = S.jp end
	end
end))

bind(RunService.Stepped:Connect(function()
	if S.noclip and char() then
		for _, p in ipairs(char():GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end
	end
end))
bind(UIS.JumpRequest:Connect(function()
	local h = hum()
	if S.infJump and h then h:ChangeState(Enum.HumanoidStateType.Jumping) end
end))

task.spawn(function()
	while alive do
		for _, plr in ipairs(Players:GetPlayers()) do
			if plr ~= player and plr.Character then
				local c = plr.Character
				local e, ch = c:FindFirstChild("PH_ESP"), c:FindFirstChild("PH_CHAM")
				if S.esp and not e then
					e = Instance.new("Highlight")
					e.Name = "PH_ESP"
					e.FillTransparency = 1
					e.OutlineColor = T.accent2
					e.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
					e.Parent = c
				elseif not S.esp and e then e:Destroy() end
				if S.chams and not ch then
					ch = Instance.new("Highlight")
					ch.Name = "PH_CHAM"
					ch.FillColor = T.accent
					ch.FillTransparency = 0.45
					ch.OutlineTransparency = 1
					ch.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
					ch.Parent = c
				elseif not S.chams and ch then ch:Destroy() end
			end
		end
		task.wait(1)
	end
end)

-- ===== Páginas =====
local home = Page("Home", "⌂")
local futebol = Page("Futebol", "◎")
local jogador = Page("Player", "☺")
local settings = Page("Settings", "⚙")

home:Section("Status rápido")
local setDist, setToq = home:Stats({"Bola", function(v) return math.floor(v) .. "m" end}, {"Toques", function(v) return tostring(math.floor(v)) end})
home:Section("Controles principais")
home:Toggle("Reach", false, function(s) S.reach = s notify("Reach", s and "Ligado · alcance " .. S.reachSize or "Desligado") end)
home:Slider("Tamanho do reach", 1, 25, 8, " studs", function(v) S.reachSize = v end)
home:Section("Mais opções")
home:Button("Teleportar até a bola", function()
	local b, r = getBall(), root()
	if b and r then r.CFrame = CFrame.new(b.Position + Vector3.new(0, 3, 2)) toast("Teleportado pra bola") else notify("Erro", "Bola não encontrada") end
end)
home:Button("Puxar a bola", function()
	local b, r = getBall(), root()
	if b and r then b.CFrame = r.CFrame * CFrame.new(0, -2, -3) toast("Tentando puxar a bola") else notify("Erro", "Bola não encontrada") end
end)

futebol:Section("Bola")
local g1 = futebol:Grid()
g1:Toggle("Ímã de bola", false, function(s) S.magnet = s end)
g1:Toggle("ESP bola", false, function(s) S.ballEsp = s end)
g1:Toggle("Tracer bola", false, function(s) S.tracer = s end)
g1:Toggle("Ver reach", false, function(s) S.showReach = s end)
g1:Toggle("ESP players", false, function(s) S.esp = s end)
g1:Toggle("Chams", false, function(s) S.chams = s end)
futebol:Section("Visuais")
futebol:Toggle("Crosshair", false, function(s)
	cross.Visible = true
	cross.Size = s and UDim2.fromOffset(0, 0) or UDim2.fromOffset(20, 20)
	tween(cross, 0.35, {Size = s and UDim2.fromOffset(20, 20) or UDim2.fromOffset(0, 0)}, Enum.EasingStyle.Back).Completed:Connect(function() cross.Visible = s end)
end)
futebol:Slider("Field of View", 50, 120, 70, " °", function(v) S.fov = v end)
local setBig = futebol:BigCard("Toques na bola", function(v) return tostring(math.floor(v)) end)

jogador:Section("Movimento")
jogador:Slider("WalkSpeed", 16, 120, 16, "", function(v) S.ws = v end)
jogador:Slider("JumpPower", 50, 200, 50, "", function(v) S.jp = v end)
local g2 = jogador:Grid()
g2:Toggle("Pulo infinito", false, function(s) S.infJump = s end)
g2:Toggle("Noclip", false, function(s) S.noclip = s end)
jogador:Section("Utilidades")
local afkConn
jogador:Toggle("Anti-AFK", false, function(s)
	if s then
		afkConn = player.Idled:Connect(function() VirtualUser:CaptureController() VirtualUser:ClickButton2(Vector2.new()) end)
	elseif afkConn then afkConn:Disconnect() afkConn = nil end
end)
jogador:Button("Resetar personagem", function() local h = hum() if h then h.Health = 0 end end)

settings:Section("Interface")
settings:Slider("Escala da UI", 70, 130, 100, "%", function(v) baseScale = v / 100 if not minimized then tween(scale, 0.3, {Scale = baseScale}) end end)
settings:Toggle("Notificações", true, function(s) notifsOn = s end)
settings:Button("Mostrar notificação", function() notify("Peidones Hub", "Notificação de teste") end)
settings:Button("Mostrar toast", function() toast("Toast de teste") end)
settings:Section("Servidor")
settings:Button("Rejoin", function() TeleportService:Teleport(game.PlaceId, player) end)
settings:Section("Hub")
settings:Button("Destruir hub", function()
	alive = false
	for _, c in ipairs(conns) do pcall(function() c:Disconnect() end) end
	if afkConn then afkConn:Disconnect() end
	clearTracer() clearBallEsp() sphere:Destroy()
	for _, plr in ipairs(Players:GetPlayers()) do
		if plr.Character then
			for _, n in ipairs({"PH_ESP", "PH_CHAM"}) do local x = plr.Character:FindFirstChild(n) if x then x:Destroy() end end
		end
	end
	tween(scale, 0.3, {Scale = 0.8}, Enum.EasingStyle.Quint, Enum.EasingDirection.In)
	tween(main, 0.3, {GroupTransparency = 1}).Completed:Connect(function() gui:Destroy() end)
end)

gearBtn.MouseButton1Click:Connect(function()
	tween(gearBtn, 0.6, {Rotation = gearBtn.Rotation + 180}, Enum.EasingStyle.Back)
	settings:Select()
end)
closeBtn.MouseButton1Click:Connect(function()
	alive = false
	for _, c in ipairs(conns) do pcall(function() c:Disconnect() end) end
	clearTracer() clearBallEsp() sphere:Destroy()
	tween(scale, 0.3, {Scale = 0.8}, Enum.EasingStyle.Quint, Enum.EasingDirection.In)
	tween(main, 0.3, {GroupTransparency = 1}).Completed:Connect(function() gui:Destroy() end)
end)

-- stats ao vivo
task.spawn(function()
	local last = 0
	while alive do
		local b, r = getBall(), root()
		if b and r then setDist((b.Position - r.Position).Magnitude) else setDist("—") end
		if toques ~= last then
			setToq(toques)
			local mins = math.max((tick() - sessionStart) / 60, 1 / 60)
			setBig(toques, "+" .. string.format("%.1f", toques / mins) .. "/min")
			last = toques
		end
		task.wait(0.3)
	end
end)

-- abertura
home:Select()
tween(scale, 0.6, {Scale = baseScale}, Enum.EasingStyle.Back)
tween(main, 0.4, {GroupTransparency = 0})
setBig(0, "+0.0/min")
task.delay(0.7, function() notify("Peidones Hub", "Carregado · RightControl minimiza") end)
