-- LocalScript inside StarterPlayerScripts
-- Script name: QuyPro

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local fruitsFolder = workspace:WaitForChild("Fruits")
local autoFarmEnabled = false
local farmDistance = 12

-- Create GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "QuyProHub"
screenGui.Parent = player:WaitForChild("PlayerGui")

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 200, 0, 50)
button.Position = UDim2.new(0.4, 0, 0.8, 0)
button.Text = "QuyPro Farm: OFF"
button.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
button.TextColor3 = Color3.new(1,1,1)
button.Parent = screenGui

-- Toggle when button is clicked
button.MouseButton1Click:Connect(function()
    autoFarmEnabled = not autoFarmEnabled
    if autoFarmEnabled then
        button.Text = "QuyPro Farm: ON"
        button.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
    else
        button.Text = "QuyPro Farm: OFF"
        button.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
    end
end)

-- Auto farm loop
task.spawn(function()
    while true do
        if autoFarmEnabled then
            for _, fruit in pairs(fruitsFolder:GetChildren()) do
                if fruit:IsA("Part") and (fruit.Position - humanoidRootPart.Position).Magnitude < farmDistance then
                    -- If the fruit has a Health value, decrease it
                    if fruit:FindFirstChild("Health") then
                        fruit.Health.Value = fruit.Health.Value - 10
                        print("Farmed fruit:", fruit.Name)
                        if fruit.Health.Value <= 0 then
                            fruit:Destroy()
                        end
                    else
                        fruit:Destroy() -- If no Health, destroy immediately
                        print("Collected fruit:", fruit.Name)
                    end
                end
            end
        end
        task.wait(1)
    end
end)
