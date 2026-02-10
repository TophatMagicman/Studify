# Studify code. Was made in a few minutes if you exclude UI, so don't judge harshly

```lua
local Selection = game:GetService("Selection")
local UIS = game:GetService("UserInputService")

local ChangeHistoryService = game:GetService("ChangeHistoryService")

local MainFrame = script.Parent.UI.CanvasGroup

local toolbar = plugin:CreateToolbar("Studify")
local OpenButton = toolbar:CreateButton("Studify", "Open UI", "rbxassetid://103032922773803")

local widgetInfo = DockWidgetPluginGuiInfo.new(
	Enum.InitialDockState.Right,
	false,
	false,
	315,
	480,
	300,
	300
)

local widget = plugin:CreateDockWidgetPluginGuiAsync("Studify_Widget", widgetInfo)
widget.Title = "Studify"
MainFrame.Parent = widget

OpenButton.Click:Connect(function()
	widget.Enabled = not widget.Enabled
end)

local Labels = MainFrame.Frame.Labels
local Values = MainFrame.Frame.Dropdowns

local function ApplySurfaces(part: Part)
	part.BackSurface = Enum.SurfaceType[Values.Back.Text]
	part.BottomSurface = Enum.SurfaceType[Values.Bottom.Text]
	part.FrontSurface = Enum.SurfaceType[Values.Front.Text]
	part.LeftSurface = Enum.SurfaceType[Values.Left.Text]
	part.RightSurface = Enum.SurfaceType[Values.Right.Text]
	part.TopSurface = Enum.SurfaceType[Values.Top.Text]
end

local function applicationFunction()
	for _, v in Selection:Get() do
		if not v:IsA("Model") and not v:IsA("BasePart") and not v:IsA("Part") and not v:IsA("Folder") then
			return
		end

		if v:IsA("Part") or v:IsA("BasePart") then
			ApplySurfaces(v)
		end

		for _, w in v:GetDescendants() do
			if w:IsA("Part") then
				ApplySurfaces(w)
			end
		end
	end
end

MainFrame.Frame.TextButton.MouseButton1Click:Connect(applicationFunction)

local KeybindBox = MainFrame.Frame.TextBox

local function verifyTextIntegrity(text: string): boolean
	return #text == 1 and text:match("^[A-Za-z0-9]$") ~= nil
end

UIS.InputBegan:Connect(function(input)
	if not widget.Enabled then
		return
	end

	if input.KeyCode == Enum.KeyCode.K then
		if KeybindBox.Text == "Keybind (default K)" then
			applicationFunction()
		end
	end

	if not verifyTextIntegrity(KeybindBox.Text) then
		return
	end

	if input.KeyCode == Enum.KeyCode[KeybindBox.Text] then
		applicationFunction()
	end
end)
