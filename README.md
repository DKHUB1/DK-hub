-- DK | HUB
-- Interface visual com minimizar
-- Compatível com PC e MOBILE
-- LocalScript

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- =========================
-- POSIÇÕES SALVAS
-- =========================
local lastMainPosition = UDim2.new(0.5, -325, 0.5, -190)
local lastMiniPosition = UDim2.new(0.5, -30, 0.5, -30)

-- =========================
-- SCREEN GUI
-- =========================
local gui = Instance.new("ScreenGui")
gui.Name = "DK_HUB"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = player:WaitForChild("PlayerGui")

-- =========================
-- HUB PRINCIPAL
-- =========================
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 650, 0, 380)
main.Position = lastMainPosition
main.BackgroundColor3 = Color3.fromRGB(18,18,18)
main.BorderSizePixel = 0
main.Active = true
main.Parent = gui
Instance.new("UICorner", main).CornerRadius = UDim.new(0,14)

-- Drag universal (PC + Mobile)
main.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		local startPos = input.Position
		local framePos = main.Position

		local conn
		conn = UIS.InputChanged:Connect(function(move)
			if move.UserInputType == Enum.UserInputType.MouseMovement
			or move.UserInputType == Enum.UserInputType.Touch then
				local delta = move.Position - startPos
				main.Position = UDim2.new(
					framePos.X.Scale,
					framePos.X.Offset + delta.X,
					framePos.Y.Scale,
					framePos.Y.Offset + delta.Y
				)
				lastMainPosition = main.Position
			end
		end)

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				conn:Disconnect()
			end
		end)
	end
end)

-- =========================
-- SIDEBAR
-- =========================
local sidebar = Instance.new("Frame")
sidebar.Size = UDim2.new(0, 70, 1, 0)
sidebar.BackgroundColor3 = Color3.fromRGB(14,14,14)
sidebar.BorderSizePixel = 0
sidebar.Parent = main
Instance.new("UICorner", sidebar).CornerRadius = UDim.new(0,14)

-- =========================
-- TOP BAR
-- =========================
local top = Instance.new("Frame")
top.Size = UDim2.new(1, -70, 0, 60)
top.Position = UDim2.new(0, 70, 0, 0)
top.BackgroundColor3 = Color3.fromRGB(22,22,22)
top.BorderSizePixel = 0
top.Parent = main

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -120, 1, 0)
title.Position = UDim2.new(0, 15, 0, 0)
title.BackgroundTransparency = 1
title.Text = "DK | HUB"
title.TextColor3 = Color3.fromRGB(255,255,255)
title.Font = Enum.Font.GothamBold
title.TextSize = 22
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = top

-- Botão minimizar
local minimize = Instance.new("TextButton")
minimize.Size = UDim2.new(0, 40, 0, 40)
minimize.Position = UDim2.new(1, -50, 0.5, -20)
minimize.Text = "—"
minimize.Font = Enum.Font.GothamBold
minimize.TextSize = 22
minimize.TextColor3 = Color3.fromRGB(255,255,255)
minimize.BackgroundColor3 = Color3.fromRGB(40,40,40)
minimize.BorderSizePixel = 0
minimize.Parent = top
Instance.new("UICorner", minimize).CornerRadius = UDim.new(1,0)

-- =========================
-- CONTEÚDO
-- =========================
local content = Instance.new("Frame")
content.Size = UDim2.new(1, -90, 1, -80)
content.Position = UDim2.new(0, 80, 0, 70)
content.BackgroundColor3 = Color3.fromRGB(20,20,20)
content.BorderSizePixel = 0
content.Parent = main
Instance.new("UICorner", content).CornerRadius = UDim.new(0,12)

-- =========================
-- MINI DK
-- =========================
local mini = Instance.new("Frame")
mini.Size = UDim2.new(0, 60, 0, 60)
mini.Position = lastMiniPosition
mini.BackgroundColor3 = Color3.fromRGB(20,20,20)
mini.BorderSizePixel = 0
mini.Visible = false
mini.Active = true
mini.Parent = gui
Instance.new("UICorner", mini).CornerRadius = UDim.new(1,0)

-- Drag do mini (mobile + PC)
mini.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		local startPos = input.Position
		local framePos = mini.Position

		local conn
		conn = UIS.InputChanged:Connect(function(move)
			if move.UserInputType == Enum.UserInputType.MouseMovement
			or move.UserInputType == Enum.UserInputType.Touch then
				local delta = move.Position - startPos
				mini.Position = UDim2.new(
					framePos.X.Scale,
					framePos.X.Offset + delta.X,
					framePos.Y.Scale,
					framePos.Y.Offset + delta.Y
				)
				lastMiniPosition = mini.Position
			end
		end)

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				conn:Disconnect()
			end
		end)
	end
end)

local dk = Instance.new("TextLabel")
dk.Size = UDim2.new(1, 0, 1, 0)
dk.BackgroundTransparency = 1
dk.Text = "DK"
dk.TextColor3 = Color3.fromRGB(255,255,255)
dk.Font = Enum.Font.GothamBlack
dk.TextSize = 26
dk.Parent = mini

-- =========================
-- CONTROLES
-- =========================
minimize.Activated:Connect(function()
	main.Visible = false
	mini.Position = lastMiniPosition
	mini.Visible = true
end)

mini.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		mini.Visible = false
		main.Position = lastMainPosition
		main.Visible = true
	end
end)
