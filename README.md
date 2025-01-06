local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local ScrollingFrame = Instance.new("ScrollingFrame")
local UIListLayout = Instance.new("UIListLayout")

-- GUI Properties
ScreenGui.Name = "EventsAndFunctionsGui"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

Frame.Name = "MainFrame"
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
Frame.Position = UDim2.new(0.2, 0, 0.2, 0)
Frame.Size = UDim2.new(0.6, 0, 0.6, 0)
Frame.Draggable = true
Frame.Active = true
Frame.Selectable = true

ScrollingFrame.Name = "ScrollingFrame"
ScrollingFrame.Parent = Frame
ScrollingFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
ScrollingFrame.Position = UDim2.new(0.05, 0, 0.05, 0)
ScrollingFrame.Size = UDim2.new(0.9, 0, 0.9, 0)
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 5, 0)
ScrollingFrame.ScrollBarThickness = 10

UIListLayout.Parent = ScrollingFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

local function addTextToGui(text)
    local TextLabel = Instance.new("TextLabel")
    TextLabel.Parent = ScrollingFrame
    TextLabel.Size = UDim2.new(1, -10, 0, 20)
    TextLabel.BackgroundTransparency = 1
    TextLabel.TextColor3 = Color3.new(1, 1, 1)
    TextLabel.TextXAlignment = Enum.TextXAlignment.Left
    TextLabel.Text = text
    TextLabel.Font = Enum.Font.SourceSans
    TextLabel.TextSize = 14
end

local function listEventsAndFunctions()
    local function logInstanceDetails(instance)
        addTextToGui("Instance: " .. instance:GetFullName())
        addTextToGui("  Class: " .. instance.ClassName)

        -- List methods (functions)
        local methods = {}
        local mt = getmetatable(instance)
        if mt then
            for key, _ in pairs(mt) do
                table.insert(methods, key)
            end
        end
        for _, method in ipairs(methods) do
            addTextToGui("    Function: " .. method)
        end

        -- List events
        for _, desc in pairs(instance:GetDescendants()) do
            if typeof(desc) == "RBXScriptSignal" then
                addTextToGui("    Event: " .. tostring(desc))
            end
        end
    end

    -- Iterate through all instances
    for _, child in pairs(game:GetDescendants()) do
        logInstanceDetails(child)
    end
end

-- Run the function
listEventsAndFunctions()
