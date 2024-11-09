-- Create the GUI elements
local gui = Instance.new("ScreenGui")
local frame = Instance.new("Frame")
local closeButton = Instance.new("TextButton")
local ghostTypeLabel = Instance.new("TextLabel")
local toggleButton = Instance.new("TextButton")  -- Toggle button to open/close the frame

-- Set properties for the GUI
gui.Name = "GhostTypeGui"
gui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")  -- Use PlayerGui for safety

-- Frame settings (Draggable, Size, etc.)
frame.Size = UDim2.new(0, 300, 0, 150)
frame.Position = UDim2.new(0.5, -150, 0.5, -75)  -- Centered
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BorderSizePixel = 3
frame.Draggable = true
frame.Active = true
frame.Visible = false  -- Initially hidden
frame.Parent = gui

-- Close button settings
closeButton.Size = UDim2.new(0, 100, 0, 50)
closeButton.Position = UDim2.new(1, -100, 0, 0)  -- Top right of frame
closeButton.Text = "Close"
closeButton.Parent = frame

-- Ghost type label settings
ghostTypeLabel.Size = UDim2.new(0, 250, 0, 50)
ghostTypeLabel.Position = UDim2.new(0, 25, 0, 50)  -- Centered within the frame
ghostTypeLabel.TextColor3 = Color3.new(1, 1, 1)  -- White text
ghostTypeLabel.TextScaled = true  -- Auto-scale text to fit the label
ghostTypeLabel.Text = "Ghost Type: Loading..."  -- Initial text while waiting for the event
ghostTypeLabel.Parent = frame

-- Toggle button settings
toggleButton.Size = UDim2.new(0, 100, 0, 50)
toggleButton.Position = UDim2.new(0.5, -50, 0, 0)  -- At the top of the screen, centered
toggleButton.Text = "Toggle GUI"
toggleButton.Parent = gui

-- Function to update ghost type when the remote event is fired
local remotesFolder = game:GetService("ReplicatedStorage"):WaitForChild("Remotes")

local function onGhostTypeChosen(ghostType)
    -- Update the label to show the actual ghost type
    ghostTypeLabel.Text = "Ghost Type: " .. tostring(ghostType)
end

-- Connect the GhostTypeChosen remote event to update the GUI
local ghostTypeChosenRemote = remotesFolder:WaitForChild("GhostTypeChosen")
ghostTypeChosenRemote.OnClientEvent:Connect(onGhostTypeChosen)

-- Toggle the visibility of the frame when the toggle button is clicked
local isFrameVisible = false
toggleButton.MouseButton1Click:Connect(function()
    isFrameVisible = not isFrameVisible
    frame.Visible = isFrameVisible
end)

-- Function to hide the frame when the close button is clicked
closeButton.MouseButton1Click:Connect(function()
    frame.Visible = false
    isFrameVisible = false  -- Ensure the toggle state stays in sync
end)
