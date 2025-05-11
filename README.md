local plr = game.Players.LocalPlayer
local char = plr.Character or plr.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")
local humanoid = char:WaitForChild("Humanoid")
local fly = false
local bv = nil

local gui = Instance.new("ScreenGui")
gui.Name = "FlyGUI"
gui.ResetOnSpawn = false
gui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local button = Instance.new("TextButton")
button.Size = UDim2.fromOffset(100, 40)
button.Position = UDim2.new(1, -110, 0.5, -20)
button.AnchorPoint = Vector2.new(1, 0.5)
button.Text = "Ativar"
button.BackgroundColor3 = Color3.fromRGB(50, 150, 255)
button.TextColor3 = Color3.new(1, 1, 1)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 20
button.Parent = gui

button.MouseButton1Click:Connect(function()
		fly = not fly
		if fly then
				button.Text = "Desativar"
				bv = Instance.new("BodyVelocity", hrp)
				bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)
				bv.Velocity = Vector3.zero
		else
				button.Text = "Ativar"
				if bv then
						bv:Destroy()
						bv = nil
				end
		end
end)

game:GetService("RunService").RenderStepped:Connect(function()
		if fly and bv then
				local camera = workspace.CurrentCamera
				local cam = camera.CFrame.LookVector
				local moveDir = cam
				if humanoid.MoveDirection.Magnitude >0 then
						bv.Velocity = moveDir.Unit * 50
				else
						bv.Velocity = Vector3.zero
				end
		end
end)
