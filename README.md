local Players=game:GetService("Players")
local RunService=game:GetService("RunService")
local LocalPlayer=Players.LocalPlayer
local UserInputService=game:GetService("UserInputService")
local CoreGui=game:GetService("CoreGui")

local ScreenGui=Instance.new("ScreenGui")
ScreenGui.ResetOnSpawn=false
ScreenGui.Parent=CoreGui

local MainFrame=Instance.new("Frame")
MainFrame.Size=UDim2.new(0,220,0,200)
MainFrame.Position=UDim2.new(0.5,-110,0.5,-100)
MainFrame.BackgroundColor3=Color3.new(0.06,0.06,0.1)
MainFrame.BackgroundTransparency=0.1
MainFrame.Active=true
MainFrame.Draggable=true
MainFrame.Parent=ScreenGui
Instance.new("UICorner",MainFrame).CornerRadius=UDim.new(0,12)

local Title=Instance.new("Frame")
Title.Size=UDim2.new(1,0,0,28)
Title.BackgroundColor3=Color3.new(0.1,0.1,0.18)
Title.BackgroundTransparency=0.3
Title.Parent=MainFrame
Instance.new("UICorner",Title).CornerRadius=UDim.new(0,12)

local TitleLabel=Instance.new("TextLabel")
TitleLabel.Size=UDim2.new(1,-60,1,0)
TitleLabel.Position=UDim2.new(0,10,0,0)
TitleLabel.BackgroundTransparency=1
TitleLabel.Text="AI辅助"
TitleLabel.TextColor3=Color3.new(0.3,0.9,1)
TitleLabel.Font=Enum.Font.GothamBold
TitleLabel.TextSize=14
TitleLabel.TextXAlignment=Enum.TextXAlignment.Left
TitleLabel.Parent=Title

local HideBtn=Instance.new("TextButton")
HideBtn.Size=UDim2.new(0,26,1,0)
HideBtn.Position=UDim2.new(1,-30,0,0)
HideBtn.BackgroundTransparency=0.4
HideBtn.BackgroundColor3=Color3.new(0.3,0.3,0.3)
HideBtn.Text="-"
HideBtn.TextColor3=Color3.new(1,1,1)
HideBtn.Font=Enum.Font.GothamBold
HideBtn.TextSize=16
Instance.new("UICorner",HideBtn).CornerRadius=UDim.new(0,4)
HideBtn.Parent=Title
local Config={
aim=false,
esp=false,
speed=false,
wall=false,
bt=false
}

local function CreateBtn(text,pos,color)
local btn=Instance.new("TextButton")
btn.Size=UDim2.new(0,80,0,24)
btn.Position=pos
btn.BackgroundTransparency=0.25
btn.BackgroundColor3=color
btn.Text=text
btn.TextColor3=Color3.new(1,1,1)
btn.Font=Enum.Font.Gotham
btn.TextSize=9
Instance.new("UICorner",btn).CornerRadius=UDim.new(0,6)
btn.Parent=MainFrame
return btn
end

local AimBtn=CreateBtn("自瞄",UDim2.new(0,10,0,36),Color3.new(0.5,0.1,0.1))
local EspBtn=CreateBtn("透视",UDim2.new(0,110,0,36),Color3.new(0.5,0.1,0.1))
local SpeedBtn=CreateBtn("加速",UDim2.new(0,10,0,68),Color3.new(0.5,0.1,0.1))
local WallBtn=CreateBtn("穿墙",UDim2.new(0,110,0,68),Color3.new(0.5,0.1,0.1))
local BtBtn=CreateBtn("子弹追踪",UDim2.new(0,10,0,100),Color3.new(0.5,0.1,0.1))

local StatusLabel=Instance.new("TextLabel")
StatusLabel.Size=UDim2.new(1,-20,0,40)
StatusLabel.Position=UDim2.new(0,10,0,134)
StatusLabel.BackgroundTransparency=1
StatusLabel.Text="状态: 自瞄关 | 透视关 | 加速关 | 穿墙关 | 子弹追踪关"
StatusLabel.TextColor3=Color3.new(0.6,0.6,0.7)
StatusLabel.Font=Enum.Font.Gotham
StatusLabel.TextSize=10
StatusLabel.TextXAlignment=Enum.TextXAlignment.Left
StatusLabel.Parent=MainFrame
local espList={}
local frameCount=0
local bulletList={}

local function UpdateStatus()
local s="状态: "
s=s.."自瞄"..(Config.aim and "开" or "关").." | "
s=s.."透视"..(Config.esp and "开" or "关").." | "
s=s.."加速"..(Config.speed and "开" or "关").." | "
s=s.."穿墙"..(Config.wall and "开" or "关").." | "
s=s.."子弹追踪"..(Config.bt and "开" or "关")
StatusLabel.Text=s
end

HideBtn.MouseButton1Click:Connect(function()
MainFrame.Visible=not MainFrame.Visible
HideBtn.Text=MainFrame.Visible and "-" or "+"
end)

AimBtn.MouseButton1Click:Connect(function()
Config.aim=not Config.aim
AimBtn.BackgroundColor3=Config.aim and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1)
UpdateStatus()
end)

EspBtn.MouseButton1Click:Connect(function()
Config.esp=not Config.esp
EspBtn.BackgroundColor3=Config.esp and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1)
UpdateStatus()
end)

SpeedBtn.MouseButton1Click:Connect(function()
Config.speed=not Config.speed
SpeedBtn.BackgroundColor3=Config.speed and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1)
if Config.speed then
local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if h then h.WalkSpeed=50 end
else
local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if h then h.WalkSpeed=16 end
end
UpdateStatus()
end)

WallBtn.MouseButton1Click:Connect(function()
Config.wall=not Config.wall
WallBtn.BackgroundColor3=Config.wall and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1)
if LocalPlayer.Character then
for _,v in pairs(LocalPlayer.Character:GetDescendants()) do
if v:IsA("BasePart") then
v.CanCollide=not Config.wall
end
end
end
UpdateStatus()
end)

BtBtn.MouseButton1Click:Connect(function()
Config.bt=not Config.bt
BtBtn.BackgroundColor3=Config.bt and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1)
UpdateStatus()
end)
local function GetRoot()
return LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
end

local function GetTarget()
local root=GetRoot()
if not root then return nil end
local target,minDist=nil,300
for _,plr in pairs(Players:GetPlayers()) do
if plr~=LocalPlayer and plr.Character then
local r=plr.Character:FindFirstChild("HumanoidRootPart")
local h=r and plr.Character:FindFirstChildOfClass("Humanoid")
if r and h and h.Health>0 then
local dist=(root.Position-r.Position).Magnitude
if dist<minDist then
minDist=dist
target=r
end
end
end
end
return target
end

local function TrackBullets()
if not Config.bt then return end
local target=GetTarget()
if not target then return end
for _,v in pairs(workspace:GetDescendants()) do
if v:IsA("BasePart") and (v.Name:lower():find("bullet") or v.Name:lower():find("projectile")) then
local dist=(v.Position-target.Position).Magnitude
if dist<300 then
local dir=(target.Position-v.Position).Unit
v.Velocity=dir*250
v.CFrame=CFrame.new(v.Position,target.Position)
end
end
end
end
RunService.RenderStepped:Connect(function()
local cam=workspace.CurrentCamera
if not cam then return end

if Config.esp then
for _,plr in pairs(Players:GetPlayers()) do
if plr~=LocalPlayer and plr.Character then
local h=plr.Character:FindFirstChildOfClass("Humanoid")
if h and h.Health>0 then
local has=false
for _,v in pairs(espList) do
if v.Adornee==plr.Character then has=true break end
end
if not has then
local hl=Instance.new("Highlight")
hl.FillColor=Color3.new(1,0,0)
hl.FillTransparency=0.5
hl.Adornee=plr.Character
hl.Parent=plr.Character
table.insert(espList,hl)
end
end
end
end
else
for _,v in pairs(espList) do
v:Destroy()
end
espList={}
end

if Config.aim then
local target=GetTarget()
if target then
local sp,on=cam:WorldToViewportPoint(target.Position)
if on then
local vs=cam.ViewportSize
local dx=(sp.X-vs.X/2)*0.3
local dy=(sp.Y-vs.Y/2)*0.3
dx=math.clamp(dx,-30,30)
dy=math.clamp(dy,-30,30)
UserInputService:SetMouseDelta(Vector2.new(dx,dy))
end
end
end

if Config.bt then
TrackBullets()
end

if Config.speed then
local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if h then h.WalkSpeed=50 end
end

if Config.wall and LocalPlayer.Character then
for _,v in pairs(LocalPlayer.Character:GetDescendants()) do
if v:IsA("BasePart") then
v.CanCollide=false
end
end
end
end)

print("AI辅助加载完成 - 无过检测版本")
