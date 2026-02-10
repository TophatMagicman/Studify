# Studify code

local Selection = game:GetService("Selection")
local UIS = game:GetService("UserInputService")

local ChangeHistoryService = game:GetService("ChangeHistoryService")

local MainFrame = script.Parent.UI.CanvasGroup

local toolbar = plugin:CreateToolbar("Studify")
local OpenButton = toolbar:CreateButton("Studify", "Open UI", "rbxassetid://103032922773803")

local widgetInfo = DockWidgetPluginGuiInfo.new(
	Enum.InitialDockState.Right,
	false, -- start enabled
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
