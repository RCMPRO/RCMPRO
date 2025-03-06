local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:FindFirstChildOfClass("Humanoid")
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local flying = false
local noclip = false
local walkspeedEnabled = false
local flySpeed = 50
local walkSpeed = 16

-- GUI Creation
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Frame = Instance.new("Frame", ScreenGui)
local UICorner = Instance.new("UICorner", Frame)
local Title = Instance.new("TextLabel", Frame)

-- Button Setup
local FlyButton = Instance.new("TextButton", Frame)
local NoclipButton = Instance.new("TextButton", Frame)
local WalkSpeedButton = Instance.new("TextButton", Frame)

-- Sliders
local FlySpeedSlider = Instance.new("TextBox", Frame)
local WalkSpeedSlider = Instance.new("TextBox", Frame)

-- Footer (RGB)
local Footer = Instance.new("TextLabel", Frame)

-- GUI Styling
Frame.Size = UDim2.new(0, 250, 0, 300)
Frame.Position = UDim2.new(0.05, 0, 0.2, 0)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
UICorner.CornerRadius = UDim.new(0, 10)

Title.Text = "Mod Menu"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18

FlyButton.Text = "Toggle Fly (OFF)"
FlyButton.Size = UDim2.new(1, 0, 0, 30)
FlyButton.Position = UDim2.new(0, 0, 0.1, 0)
FlyButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
FlyButton.TextColor3 = Color3.fromRGB(255, 255, 255)

NoclipButton.Text = "Toggle Noclip (OFF)"
NoclipButton.Size = UDim2.new(1, 0, 0, 30)
NoclipButton.Position = UDim2.new(0, 0, 0.25, 0)
NoclipButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
NoclipButton.TextColor3 = Color3.fromRGB(255, 255, 255)

WalkSpeedButton.Text = "Toggle WalkSpeed (OFF)"
WalkSpeedButton.Size = UDim2.new(1, 0, 0, 30)
WalkSpeedButton.Position = UDim2.new(0, 0, 0.4, 0)
WalkSpeedButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
WalkSpeedButton.TextColor3 = Color3.fromRGB(255, 255, 255)

FlySpeedSlider.Text = "Fly Speed: 50"
FlySpeedSlider.Size = UDim2.new(1, 0, 0, 30)
FlySpeedSlider.Position = UDim2.new(0, 0, 0.55, 0)
FlySpeedSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
FlySpeedSlider.TextColor3 = Color3.fromRGB(255, 255, 255)

WalkSpeedSlider.Text = "Walk Speed: 16"
WalkSpeedSlider.Size = UDim2.new(1, 0, 0, 30)
WalkSpeedSlider.Position = UDim2.new(0, 0, 0.7, 0)
WalkSpeedSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
WalkSpeedSlider.TextColor3 = Color3.fromRGB(255, 255, 255)

Footer.Text = "Script by WonderfulGamingRPG"
Footer.Size = UDim2.new(1, 0, 0, 30)
Footer.Position = UDim2.new(0, 0, 0.85, 0)
Footer.TextColor3 = Color3.fromRGB(255, 0, 0)
Footer.BackgroundTransparency = 1
Footer.Font = Enum.Font.GothamBold
Footer.TextSize = 14

-- RGB Footer Animation
spawn(function()
    while true do
        for i = 0, 255, 5 do
            Footer.TextColor3 = Color3.fromRGB(i, 255 - i, math.random(100, 255))
            wait(0.05)
        end
    end
end)

-- Fly Toggle
FlyButton.MouseButton1Click:Connect(function()
    flying = not flying
    FlyButton.Text = flying and "Toggle Fly (ON)" or "Toggle Fly (OFF)"
end)

-- Noclip Toggle
NoclipButton.MouseButton1Click:Connect(function()
    noclip = not noclip
    NoclipButton.Text = noclip and "Toggle Noclip (ON)" or "Toggle Noclip (OFF)"
end)

-- WalkSpeed Toggle
WalkSpeedButton.MouseButton1Click:Connect(function()
    walkspeedEnabled = not walkspeedEnabled
    humanoid.WalkSpeed = walkspeedEnabled and walkSpeed or 16
    WalkSpeedButton.Text = walkspeedEnabled and "Toggle WalkSpeed (ON)" or "Toggle WalkSpeed (OFF)"
end)

-- Fly Speed Adjust
FlySpeedSlider.FocusLost:Connect(function()
    local newSpeed = tonumber(FlySpeedSlider.Text:match("%d+"))
    if newSpeed then
        flySpeed = newSpeed
        FlySpeedSlider.Text = "Fly Speed: " .. newSpeed
    end
end)

-- WalkSpeed Adjust
WalkSpeedSlider.FocusLost:Connect(function()
    local newSpeed = tonumber(WalkSpeedSlider.Text:match("%d+"))
    if newSpeed then
        walkSpeed = newSpeed
        humanoid.WalkSpeed = walkSpeed
        WalkSpeedSlider.Text = "Walk Speed: " .. newSpeed
    end
end)

-- Noclip Logic
game:GetService("RunService").Stepped:Connect(function()
    if noclip then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- Fly Movement
game:GetService("RunService").Heartbeat:Connect(function()
    if flying then
        humanoidRootPart.Velocity = humanoidRootPart.CFrame.LookVector * flySpeed
    end
end)

-- Chat Commands
game:GetService("Players").LocalPlayer.Chatted:Connect(function(msg)
    msg = msg:lower()
    if msg == "!fly" then
        flying = not flying
        FlyButton.Text = flying and "Toggle Fly (ON)" or "Toggle Fly (OFF)"
    elseif msg == "!noclip" then
        noclip = not noclip
        NoclipButton.Text = noclip and "Toggle Noclip (ON)" or "Toggle Noclip (OFF)"
    elseif msg:match("!speed %d+") then
        local newSpeed = tonumber(msg:match("%d+"))
        if newSpeed then
            walkSpeed = newSpeed
            humanoid.WalkSpeed = newSpeed
            WalkSpeedSlider.Text = "Walk Speed: " .. newSpeed
        end
    end
end)
