local Players=game:GetService("Players")local RunService=game:GetService("RunService")local LP=Players.LocalPlayer
local ScreenGui=Instance.new("ScreenGui")ScreenGui.ResetOnSpawn=false ScreenGui.Parent=game.CoreGui
local Frame=Instance.new("Frame")Frame.Size=UDim2.new(0,150,0,90)Frame.Position=UDim2.new(0.02,0,0.2,0)Frame.BackgroundColor3=Color3.new(0.1,0.1,0.15)Frame.BackgroundTransparency=0.35 Frame.Active=true Frame.Draggable=true Frame.Parent=ScreenGui
Instance.new("UICorner",Frame).CornerRadius=UDim.new(0,8)
local Title=Instance.new("Frame")Title.Size=UDim2.new(1,0,0,20)Title.BackgroundColor3=Color3.new(0.2,0.2,0.28)Title.BackgroundTransparency=0.25 Title.Parent=Frame
Instance.new("UICorner",Title).CornerRadius=UDim.new(0,8)
local Label=Instance.new("TextLabel")Label.Size=UDim2.new(1,-30,1,0)Label.Position=UDim2.new(0,6,0,0)Label.BackgroundTransparency=1 Label.Text="单吸附"Label.TextColor3=Color3.new(1,1,1)Label.Font=Enum.Font.GothamSemibold Label.TextSize=10 Label.Parent=Title
local Hide=Instance.new("TextButton")Hide.Size=UDim2.new(0,24,1,0)Hide.Position=UDim2.new(1,-26,0,0)Hide.BackgroundTransparency=0.4 Hide.BackgroundColor3=Color3.new(0.35,0.35,0.35)Hide.Text="-"Hide.TextColor3=Color3.new(1,1,1)Hide.Font=Enum.Font.GothamBold Hide.TextSize=12 Instance.new("UICorner",Hide).CornerRadius=UDim.new(0,4)Hide.Parent=Title
local Config={aim=false}
local Btn=Instance.new("TextButton")Btn.Size=UDim2.new(0,60,0,16)Btn.Position=UDim2.new(0,5,0,24)Btn.BackgroundTransparency=0.3 Btn.BackgroundColor3=Color3.new(0.5,0.1,0.1)Btn.Text="开启"Btn.TextColor3=Color3.new(1,1,1)Btn.Font=Enum.Font.Gotham Btn.TextSize=6 Instance.new("UICorner",Btn).CornerRadius=UDim.new(0,4)Btn.Parent=Frame
local Ball=Instance.new("ImageButton")Ball.Size=UDim2.new(0,40,0,40)Ball.Position=UDim2.new(0.9,-20,0.8,-20)Ball.BackgroundColor3=Color3.new(0.15,0.5,0.85)Ball.Visible=false Ball.Draggable=true Ball.AutoButtonColor=false Ball.Parent=ScreenGui
Instance.new("UICorner",Ball).CornerRadius=UDim.new(1,0)
local BallText=Instance.new("TextLabel")BallText.Size=UDim2.new(1,0,1,0)BallText.BackgroundTransparency=1 BallText.Text="吸附"BallText.TextColor3=Color3.new(1,1,1)BallText.Font=Enum.Font.GothamBold BallText.TextSize=14 BallText.Parent=Ball
local dragStart,dragStartPos
Ball.InputBegan:Connect(function(input)if input.UserInputType==Enum.UserInputType.MouseButton1 then dragStart=input.Position dragStartPos=Ball.Position end end)
Ball.InputChanged:Connect(function(input)if input.UserInputType==Enum.UserInputType.MouseButton1 and dragStart then local delta=input.Position-dragStart Ball.Position=UDim2.new(dragStartPos.X.Scale,dragStartPos.X.Offset+delta.X,dragStartPos.Y.Scale,dragStartPos.Y.Offset+delta.Y)end end)
Ball.InputEnded:Connect(function()dragStart=nil end)
Hide.MouseButton1Click:Connect(function()Frame.Visible=false Ball.Visible=true Ball.Position=Frame.Position end)
Ball.MouseButton1Click:Connect(function()Frame.Position=Ball.Position Frame.Visible=true Ball.Visible=false end)
Btn.MouseButton1Click:Connect(function()Config.aim=not Config.aim Btn.Text=Config.aim and"关闭"or"开启"Btn.BackgroundColor3=Config.aim and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
local function GetTarget()local char=LP.Character if not char then return end local root=char:FindFirstChild("HumanoidRootPart")if not root then return end local target,minDist=nil,250 for _,plr in pairs(Players:GetPlayers())do if plr~=LP and plr.Character then local r=plr.Character:FindFirstChild("HumanoidRootPart")local h=r and plr.Character:FindFirstChildOfClass("Humanoid")if r and h and h.Health>0 then local dist=(root.Position-r.Position).Magnitude if dist<minDist then minDist=dist target=r end end end end return target end
RunService.RenderStepped:Connect(function()if Config.aim then local target=GetTarget()if target and workspace.CurrentCamera then workspace.CurrentCamera.CFrame=CFrame.new(workspace.CurrentCamera.CFrame.Position,target.Position)end end end)
print("单吸附加载完成")
