local P=game:GetService("Players")local R=game:GetService("RunService")local M=P.LocalPlayer
local U=game:GetService("UserInputService")local C=game:GetService("CoreGui")
local function ShowStartup()
local notify=Instance.new("Frame")
notify.Size=UDim2.new(0,220,0,60)
notify.Position=UDim2.new(1,-240,1,-80)
notify.BackgroundColor3=Color3.new(0.08,0.08,0.15)
notify.BackgroundTransparency=0.1
notify.Parent=C
Instance.new("UICorner",notify).CornerRadius=UDim.new(0,12)
local icon=Instance.new("Frame")icon.Size=UDim2.new(0,36,0,36)icon.Position=UDim2.new(0,8,0,12)icon.BackgroundColor3=Color3.new(0.2,0.6,1)icon.Parent=notify
Instance.new("UICorner",icon).CornerRadius=UDim.new(1,0)
local it=Instance.new("TextLabel")it.Size=UDim2.new(1,0,1,0)it.BackgroundTransparency=1 it.Text="AI"it.TextColor3=Color3.new(1,1,1)it.Font=Enum.Font.GothamBold it.TextSize=16 it.Parent=icon
local title=Instance.new("TextLabel")title.Size=UDim2.new(1,-60,0,18)title.Position=UDim2.new(0,52,0,12)title.BackgroundTransparency=1 title.Text="AI 过检测正在加载中..."title.TextColor3=Color3.new(0.3,0.8,1)title.Font=Enum.Font.GothamBold title.TextSize=13 title.TextXAlignment=Enum.TextXAlignment.Left title.Parent=notify
local sub=Instance.new("TextLabel")sub.Size=UDim2.new(1,-60,0,16)sub.Position=UDim2.new(0,52,0,32)sub.BackgroundTransparency=1 sub.Text="脚本正在加载中，请稍后..."sub.TextColor3=Color3.new(0.6,0.6,0.7)sub.Font=Enum.Font.Gotham sub.TextSize=10 sub.TextXAlignment=Enum.TextXAlignment.Left sub.Parent=notify
local bar=Instance.new("Frame")bar.Size=UDim2.new(0,0,0,2)bar.Position=UDim2.new(0,0,0,58)bar.BackgroundColor3=Color3.new(0.2,0.6,1)bar.Parent=notify
Instance.new("UICorner",bar).CornerRadius=UDim.new(0,1)
for i=1,100 do bar.Size=UDim2.new(i/100,0,0,2)task.wait(0.02)end
task.wait(0.3)notify:Destroy()
end
coroutine.wrap(ShowStartup)()
local G=Instance.new("ScreenGui")G.ResetOnSpawn=false G.Parent=C
local F=Instance.new("Frame")F.Size=UDim2.new(0,260,0,240)F.Position=UDim2.new(0.5,-130,0.5,-120)F.BackgroundColor3=Color3.new(0.06,0.06,0.1)F.BackgroundTransparency=0.1 F.Active=true F.Draggable=true F.Parent=G
Instance.new("UICorner",F).CornerRadius=UDim.new(0,12)
local T=Instance.new("Frame")T.Size=UDim2.new(1,0,0,28)T.BackgroundColor3=Color3.new(0.1,0.1,0.18)T.BackgroundTransparency=0.3 T.Parent=F
Instance.new("UICorner",T).CornerRadius=UDim.new(0,12)
local TL=Instance.new("TextLabel")TL.Size=UDim2.new(1,-60,1,0)TL.Position=UDim2.new(0,10,0,0)TL.BackgroundTransparency=1 TL.Text="🤖 AI V5"TL.TextColor3=Color3.new(0.3,0.9,1)TL.Font=Enum.Font.GothamBold TL.TextSize=14 TL.TextXAlignment=Enum.TextXAlignment.Left TL.Parent=T
local H=Instance.new("TextButton")H.Size=UDim2.new(0,26,1,0)H.Position=UDim2.new(1,-30,0,0)H.BackgroundTransparency=0.4 H.BackgroundColor3=Color3.new(0.3,0.3,0.3)H.Text="−"H.TextColor3=Color3.new(1,1,1)H.Font=Enum.Font.GothamBold H.TextSize=16 Instance.new("UICorner",H).CornerRadius=UDim.new(0,4)H.Parent=T
local CF={aim=false,sm=0.4,rg=280,esp=false,anti=false,box=false,tracer=false,info=false,spd=false,wall=false}
local espList={}local tracerList={}local infoList={}local frameCount=0
local currentPage=1
local PrevBtn=Instance.new("TextButton")PrevBtn.Size=UDim2.new(0,40,0,22)PrevBtn.Position=UDim2.new(0,8,0,214)PrevBtn.BackgroundTransparency=0.25 PrevBtn.BackgroundColor3=Color3.new(0.1,0.3,0.5)PrevBtn.Text="◀"PrevBtn.TextColor3=Color3.new(1,1,1)PrevBtn.Font=Enum.Font.GothamBold PrevBtn.TextSize=14 Instance.new("UICorner",PrevBtn).CornerRadius=UDim.new(0,4)PrevBtn.Parent=F
local NextBtn=Instance.new("TextButton")NextBtn.Size=UDim2.new(0,40,0,22)NextBtn.Position=UDim2.new(1,-48,0,214)NextBtn.BackgroundTransparency=0.25 NextBtn.BackgroundColor3=Color3.new(0.1,0.3,0.5)NextBtn.Text="▶"NextBtn.TextColor3=Color3.new(1,1,1)NextBtn.Font=Enum.Font.GothamBold NextBtn.TextSize=14 Instance.new("UICorner",NextBtn).CornerRadius=UDim.new(0,4)NextBtn.Parent=F
local PageLabel=Instance.new("TextLabel")PageLabel.Size=UDim2.new(0,50,0,22)PageLabel.Position=UDim2.new(0.5,-25,0,214)PageLabel.BackgroundTransparency=1 PageLabel.Text="1/3"PageLabel.TextColor3=Color3.new(0.6,0.6,0.7)PageLabel.Font=Enum.Font.Gotham PageLabel.TextSize=11 PageLabel.Parent=F
local P1=Instance.new("Frame")P1.Size=UDim2.new(1,0,1,-70)P1.Position=UDim2.new(0,0,0,32)P1.BackgroundTransparency=1 P1.Parent=F
local P2=Instance.new("Frame")P2.Size=UDim2.new(1,0,1,-70)P2.Position=UDim2.new(0,0,0,32)P2.BackgroundTransparency=1 P2.Visible=false P2.Parent=F
local P3=Instance.new("Frame")P3.Size=UDim2.new(1,0,1,-70)P3.Position=UDim2.new(0,0,0,32)P3.BackgroundTransparency=1 P3.Visible=false P3.Parent=F
local function Bt(parent,p,t,c)
local b=Instance.new("TextButton")
b.Size=UDim2.new(0,70,0,24)
b.Position=p
b.BackgroundTransparency=0.25
b.BackgroundColor3=c
b.Text=t
b.TextColor3=Color3.new(1,1,1)
b.Font=Enum.Font.Gotham
b.TextSize=9
Instance.new("UICorner",b).CornerRadius=UDim.new(0,6)
b.Parent=parent
return b
end
local function Lb(parent,p,t)
local l=Instance.new("TextLabel")
l.Size=UDim2.new(0,90,0,20)
l.Position=p
l.BackgroundTransparency=1
l.Text=t
l.TextColor3=Color3.new(0.8,0.8,0.9)
l.Font=Enum.Font.Gotham
l.TextSize=10
l.TextXAlignment=Enum.TextXAlignment.Left
l.Parent=parent
return l
end
local smLabel=Lb(P1,UDim2.new(0,5,0,34),"平滑度: "..CF.sm)
local rgLabel=Lb(P1,UDim2.new(0,5,0,60),"距离: "..CF.rg)
local AimBtn=Bt(P1,UDim2.new(0,5,0,5),"自瞄",Color3.new(0.5,0.1,0.1))
local SmAdd=Bt(P1,UDim2.new(0,140,0,32),"+",Color3.new(0.1,0.3,0.5))
local SmSub=Bt(P1,UDim2.new(0,75,0,32),"-",Color3.new(0.1,0.3,0.5))
local RgAdd=Bt(P1,UDim2.new(0,140,0,58),"+",Color3.new(0.1,0.3,0.5))
local RgSub=Bt(P1,UDim2.new(0,75,0,58),"-",Color3.new(0.1,0.3,0.5))
local AntiBtn=Bt(P1,UDim2.new(0,75,0,84),"过检测",Color3.new(0.5,0.1,0.1))
local EspBtn=Bt(P2,UDim2.new(0,5,0,5),"透视",Color3.new(0.5,0.1,0.1))
local BoxBtn=Bt(P2,UDim2.new(0,75,0,32),"方框",Color3.new(0.5,0.1,0.1))
local TracerBtn=Bt(P2,UDim2.new(0,75,0,58),"射线",Color3.new(0.5,0.1,0.1))
local InfoBtn=Bt(P2,UDim2.new(0,75,0,84),"信息",Color3.new(0.5,0.1,0.1))
local SpdBtn=Bt(P3,UDim2.new(0,5,0,5),"加速",Color3.new(0.5,0.1,0.1))
local WallBtn=Bt(P3,UDim2.new(0,75,0,5),"穿墙",Color3.new(0.5,0.1,0.1))
Lb(P3,UDim2.new(0,5,0,34),"移速: 50")
Lb(P3,UDim2.new(0,5,0,60),"穿墙: 开启后穿墙")
local function UpdatePage()
if currentPage==1 then P1.Visible=true P2.Visible=false P3.Visible=false PageLabel.Text="1/3"
elseif currentPage==2 then P1.Visible=false P2.Visible=true P3.Visible=false PageLabel.Text="2/3"
else P1.Visible=false P2.Visible=false P3.Visible=true PageLabel.Text="3/3" end
end
PrevBtn.MouseButton1Click:Connect(function()if currentPage>1 then currentPage=currentPage-1 UpdatePage()end end)
NextBtn.MouseButton1Click:Connect(function()if currentPage<3 then currentPage=currentPage+1 UpdatePage()end end)
local Ball=Instance.new("ImageButton")Ball.Size=UDim2.new(0,44,0,44)Ball.Position=UDim2.new(0.9,-22,0.8,-22)Ball.BackgroundColor3=Color3.new(0.2,0.6,1)Ball.Visible=false Ball.Draggable=true Ball.AutoButtonColor=false Ball.Parent=G
Instance.new("UICorner",Ball).CornerRadius=UDim.new(1,0)
local BallT=Instance.new("TextLabel")BallT.Size=UDim2.new(1,0,1,0)BallT.BackgroundTransparency=1 BallT.Text="AI"BallT.TextColor3=Color3.new(1,1,1)BallT.Font=Enum.Font.GothamBold BallT.TextSize=16 BallT.Parent=Ball
local DS,DP
Ball.InputBegan:Connect(function(i)if i.UserInputType==Enum.UserInputType.MouseButton1 then DS=i.Position DP=Ball.Position end end)
Ball.InputChanged:Connect(function(i)if i.UserInputType==Enum.UserInputType.MouseButton1 and DS then local d=i.Position-DS Ball.Position=UDim2.new(DP.X.Scale,DP.X.Offset+d.X,DP.Y.Scale,DP.Y.Offset+d.Y)end end)
Ball.InputEnded:Connect(function()DS=nil end)
H.MouseButton1Click:Connect(function()F.Visible=false Ball.Visible=true Ball.Position=F.Position end)
Ball.MouseButton1Click:Connect(function()F.Position=Ball.Position F.Visible=true Ball.Visible=false end)
local function GetRoot()return M.Character and M.Character:FindFirstChild("HumanoidRootPart")end
local function GetTarget()
local q=GetRoot()if not q then return end
local a,b=nil,CF.rg
for _,p in pairs(P:GetPlayers())do
if p~=M and p.Character then
local d=p.Character:FindFirstChild("HumanoidRootPart")
local h=d and p.Character:FindFirstChildOfClass("Humanoid")
if d and h and h.Health>0 then
local f=(q.Position-d.Position).Magnitude
if f<b then b=f a=d end
end
end
end
return a
end
-- 插帧优化
local function OptimizeFPS()
pcall(function()
settings().Rendering.QualityLevel=Enum.QualityLevel.Level01
settings().Rendering.FrameRateManager:SetMaxFrameRate(120)
workspace.Gravity=workspace.Gravity
end)
end
OptimizeFPS()
local antiTimer=0
-- 删除检测脚本 + 踢出管理员
local function DeleteDetection()
pcall(function()
for _,v in pairs(workspace:GetDescendants())do
if v:IsA("Script")or v:IsA("LocalScript")then
local n=v.Name:lower()
if n:find("detect")or n:find("admin")or n:find("check")or n:find("scan")or n:find("ban")or n:find("anti")then
v.Disabled=true task.wait(0.05)v:Destroy()
end
end
if v:IsA("BoolValue")or v:IsA("StringValue")or v:IsA("NumberValue")then
local n=v.Name:lower()
if n:find("detect")or n:find("check")or n:find("ban")or n:find("flag")or n:find("admin")then
v:Destroy()
end
end
end
for _,plr in pairs(P:GetPlayers())do
local n=plr.Name:lower()
if n:find("admin")or n:find("mod")or n:find("owner")or n:find("staff")then
plr:Kick("管理员检测中...")
end
end
end)
end
local function AntiDetect()
if not CF.anti then return end
antiTimer=antiTimer+1
if antiTimer%7==0 then
pcall(function()
local f=Instance.new("BoolValue")
f.Name="_sys_"..tostring(math.random(1000,9999))
f.Parent=M
task.wait(0.15)
f:Destroy()
end)
DeleteDetection()
OptimizeFPS()
end
if antiTimer>35 then antiTimer=0 end
end
AimBtn.MouseButton1Click:Connect(function()CF.aim=not CF.aim AimBtn.BackgroundColor3=CF.aim and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
SmAdd.MouseButton1Click:Connect(function()CF.sm=math.min(1,CF.sm+0.05)smLabel:Destroy()smLabel=Lb(P1,UDim2.new(0,5,0,34),"平滑度: "..string.format("%.2f",CF.sm))end)
SmSub.MouseButton1Click:Connect(function()CF.sm=math.max(0.05,CF.sm-0.05)smLabel:Destroy()smLabel=Lb(P1,UDim2.new(0,5,0,34),"平滑度: "..string.format("%.2f",CF.sm))end)
RgAdd.MouseButton1Click:Connect(function()CF.rg=math.min(600,CF.rg+10)rgLabel:Destroy()rgLabel=Lb(P1,UDim2.new(0,5,0,60),"距离: "..CF.rg)end)
RgSub.MouseButton1Click:Connect(function()CF.rg=math.max(30,CF.rg-10)rgLabel:Destroy()rgLabel=Lb(P1,UDim2.new(0,5,0,60),"距离: "..CF.rg)end)
AntiBtn.MouseButton1Click:Connect(function()CF.anti=not CF.anti AntiBtn.BackgroundColor3=CF.anti and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
EspBtn.MouseButton1Click:Connect(function()CF.esp=not CF.esp EspBtn.BackgroundColor3=CF.esp and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
BoxBtn.MouseButton1Click:Connect(function()CF.box=not CF.box BoxBtn.BackgroundColor3=CF.box and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
TracerBtn.MouseButton1Click:Connect(function()CF.tracer=not CF.tracer TracerBtn.BackgroundColor3=CF.tracer and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
InfoBtn.MouseButton1Click:Connect(function()CF.info=not CF.info InfoBtn.BackgroundColor3=CF.info and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
SpdBtn.MouseButton1Click:Connect(function()
CF.spd=not CF.spd
SpdBtn.BackgroundColor3=CF.spd and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)
local h=M.Character and M.Character:FindFirstChildOfClass("Humanoid")
if h then h.WalkSpeed=CF.spd and 50 or 16 end
end)
WallBtn.MouseButton1Click:Connect(function()
CF.wall=not CF.wall
WallBtn.BackgroundColor3=CF.wall and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)
if M.Character then
for _,v in pairs(M.Character:GetDescendants())do
if v:IsA("BasePart")then v.CanCollide=not CF.wall end
end
end
end)
R.RenderStepped:Connect(function()
AntiDetect()
local cam=workspace.CurrentCamera if not cam then return end
if CF.aim then
frameCount=frameCount+1
if frameCount%math.random(1,2)==0 then
local target=GetTarget()
if target then
local sp,on=cam:WorldToViewportPoint(target.Position)
if on then
local vs=cam.ViewportSize
local dx=(sp.X-vs.X/2)*CF.sm+math.random(-3,3)
local dy=(sp.Y-vs.Y/2)*CF.sm+math.random(-3,3)
dx=math.clamp(dx,-60,60)dy=math.clamp(dy,-60,60)
U:SetMouseDelta(Vector2.new(dx,dy))
end
end
end
end
if CF.esp then
for _,p in pairs(P:GetPlayers())do
if p~=M and p.Character then
local h=p.Character:FindFirstChildOfClass("Humanoid")
if h and h.Health>0 then
local root=p.Character:FindFirstChild("HumanoidRootPart")
if CF.box then
local has=false
for _,v in pairs(espList)do if v.Adornee==p.Character then has=true break end end
if not has then
local hl=Instance.new("Highlight")
hl.FillColor=Color3.new(1,0.2,0.2)
hl.FillTransparency=0.4
hl.OutlineColor=Color3.new(1,0,0)
hl.OutlineTransparency=0
hl.Adornee=p.Character
hl.Parent=p.Character
table.insert(espList,hl)
end
end
if CF.tracer and root then
local myRoot=GetRoot()
if myRoot then
local line=Instance.new("LineHandleAdornment")
line.Size=1
line.Thickness=0.2
line.Color3=Color3.new(0,1,0)
line.Transparency=0.5
line.Adornee=M.Character
line.Parent=M.Character
table.insert(tracerList,line)
end
end
if CF.info and root then
local myRoot=GetRoot()
if myRoot then
local dist=(myRoot.Position-root.Position).Magnitude
local bg=Instance.new("BillboardGui")
bg.Size=UDim2.new(0,80,0,16)
bg.Adornee=p.Character
bg.AlwaysOnTop=true
bg.Parent=p.Character
local tl=Instance.new("TextLabel")
tl.Size=UDim2.new(1,0,1,0)
tl.BackgroundTransparency=1
tl.Text=p.Name.." ["..tostring(math.floor(dist)).."m]"
tl.TextColor3=Color3.new(0,1,1)
tl.Font=Enum.Font.GothamBold
tl.TextSize=9
tl.TextStrokeTransparency=0.3
tl.TextStrokeColor3=Color3.new(0,0,0)
tl.Parent=bg
table.insert(infoList,bg)
end
end
end
end
end
else
for _,v in pairs(espList)do v:Destroy()end espList={}
for _,v in pairs(tracerList)do v:Destroy()end tracerList={}
for _,v in pairs(infoList)do v:Destroy()end infoList={}
end
if CF.spd then local h=M.Character and M.Character:FindFirstChildOfClass("Humanoid")if h then h.WalkSpeed=50 end end
if CF.wall and M.Character then for _,v in pairs(M.Character:GetDescendants())do if v:IsA("BasePart")then v.CanCollide=false end end end
end)
local function StartCleanLoop()
while true do
task.wait(2)
pcall(function()
DeleteDetection()
OptimizeFPS()
end)
end
end
coroutine.wrap(StartCleanLoop)()
print("AI V5 加载完成 - 每2秒清理检测脚本 + 帧率优化")
