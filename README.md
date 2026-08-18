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
local icon=Instance.new("Frame")
icon.Size=UDim2.new(0,36,0,36)
icon.Position=UDim2.new(0,8,0,12)
icon.BackgroundColor3=Color3.new(0.2,0.6,1)
icon.Parent=notify
Instance.new("UICorner",icon).CornerRadius=UDim.new(1,0)
local it=Instance.new("TextLabel")
it.Size=UDim2.new(1,0,1,0)
it.BackgroundTransparency=1
it.Text="AI"
it.TextColor3=Color3.new(1,1,1)
it.Font=Enum.Font.GothamBold
it.TextSize=16
it.Parent=icon
local title=Instance.new("TextLabel")
title.Size=UDim2.new(1,-60,0,18)
title.Position=UDim2.new(0,52,0,12)
title.BackgroundTransparency=1
title.Text="AI 过检测正在加载中..."
title.TextColor3=Color3.new(0.3,0.8,1)
title.Font=Enum.Font.GothamBold
title.TextSize=13
title.TextXAlignment=Enum.TextXAlignment.Left
title.Parent=notify
local sub=Instance.new("TextLabel")
sub.Size=UDim2.new(1,-60,0,16)
sub.Position=UDim2.new(0,52,0,32)
sub.BackgroundTransparency=1
sub.Text="脚本正在加载中，请稍后..."
sub.TextColor3=Color3.new(0.6,0.6,0.7)
sub.Font=Enum.Font.Gotham
sub.TextSize=10
sub.TextXAlignment=Enum.TextXAlignment.Left
sub.Parent=notify
local bar=Instance.new("Frame")
bar.Size=UDim2.new(0,0,0,2)
bar.Position=UDim2.new(0,0,0,58)
bar.BackgroundColor3=Color3.new(0.2,0.6,1)
bar.Parent=notify
Instance.new("UICorner",bar).CornerRadius=UDim.new(0,1)
for i=1,100 do bar.Size=UDim2.new(i/100,0,0,2)task.wait(0.02)end
task.wait(0.3)notify:Destroy()
end
coroutine.wrap(ShowStartup)()
local G=Instance.new("ScreenGui")G.ResetOnSpawn=false G.Parent=C
local F=Instance.new("Frame")F.Size=UDim2.new(0,260,0,260)F.Position=UDim2.new(0.5,-130,0.5,-130)F.BackgroundColor3=Color3.new(0.06,0.06,0.1)F.BackgroundTransparency=0.1 F.Active=true F.Draggable=true F.Parent=G
Instance.new("UICorner",F).CornerRadius=UDim.new(0,14)
local T=Instance.new("Frame")T.Size=UDim2.new(1,0,0,32)T.BackgroundColor3=Color3.new(0.1,0.1,0.18)T.BackgroundTransparency=0.3 T.Parent=F
Instance.new("UICorner",T).CornerRadius=UDim.new(0,14)
local TL=Instance.new("TextLabel")TL.Size=UDim2.new(1,-70,1,0)TL.Position=UDim2.new(0,10,0,0)TL.BackgroundTransparency=1 TL.Text="🤖 AI V4"TL.TextColor3=Color3.new(0.3,0.9,1)TL.Font=Enum.Font.GothamBold TL.TextSize=15 TL.TextXAlignment=Enum.TextXAlignment.Left TL.Parent=T
local H=Instance.new("TextButton")H.Size=UDim2.new(0,26,1,0)H.Position=UDim2.new(1,-30,0,0)H.BackgroundTransparency=0.4 H.BackgroundColor3=Color3.new(0.3,0.3,0.3)H.Text="−"H.TextColor3=Color3.new(1,1,1)H.Font=Enum.Font.GothamBold H.TextSize=16 Instance.new("UICorner",H).CornerRadius=UDim.new(0,4)H.Parent=T
local TabFrame=Instance.new("Frame")TabFrame.Size=UDim2.new(1,0,0,30)TabFrame.Position=UDim2.new(0,0,0,36)TabFrame.BackgroundTransparency=1 TabFrame.Parent=F
local tabs={"自瞄","透视","状态"}local tabBtns={}local currentTab=1
for i,name in ipairs(tabs)do
local btn=Instance.new("TextButton")
btn.Size=UDim2.new(1/3,0,1,0)
btn.Position=UDim2.new((i-1)/3,0,0,0)
btn.BackgroundTransparency=0.2
btn.BackgroundColor3=(i==1)and Color3.new(0.2,0.6,1)or Color3.new(0.1,0.1,0.15)
btn.Text=name
btn.TextColor3=(i==1)and Color3.new(1,1,1)or Color3.new(0.5,0.5,0.5)
btn.Font=Enum.Font.GothamBold
btn.TextSize=11
btn.Parent=TabFrame
Instance.new("UICorner",btn).CornerRadius=UDim.new(0,4)
tabBtns[i]=btn
btn.MouseButton1Click:Connect(function()
for j,b in pairs(tabBtns)do
b.BackgroundColor3=(j==i)and Color3.new(0.2,0.6,1)or Color3.new(0.1,0.1,0.15)
b.TextColor3=(j==i)and Color3.new(1,1,1)or Color3.new(0.5,0.5,0.5)
end
currentTab=i
for j,p in pairs(Pages)do p.Visible=(j==i)end
end)
end
local Pages={}
for i=1,3 do
local p=Instance.new("Frame")
p.Size=UDim2.new(1,0,1,-70)
p.Position=UDim2.new(0,0,0,68)
p.BackgroundTransparency=1
p.Visible=(i==1)
p.Parent=F
Pages[i]=p
end
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
local function Lb(parent,p,t,s)
local l=Instance.new("TextLabel")
l.Size=UDim2.new(0,90,0,20)
l.Position=p
l.BackgroundTransparency=1
l.Text=t
l.TextColor3=Color3.new(0.8,0.8,0.9)
l.Font=Enum.Font.Gotham
l.TextSize=s or 10
l.TextXAlignment=Enum.TextXAlignment.Left
l.Parent=parent
return l
end
local CF={aim=false,sm=0.4,rg=280,esp=false,anti=false,box=false,tracer=false,info=false}
local espList={}local tracerList={}local infoList={}local frameCount=0
local P1=Pages[1]
local AimBtn=Bt(P1,UDim2.new(0,5,0,5),"🔒 自瞄",Color3.new(0.5,0.1,0.1))
Lb(P1,UDim2.new(0,5,0,34),"平滑度: "..CF.sm)
local SmAdd=Bt(P1,UDim2.new(0,140,0,32),"+",Color3.new(0.1,0.3,0.5))
local SmSub=Bt(P1,UDim2.new(0,75,0,32),"-",Color3.new(0.1,0.3,0.5))
Lb(P1,UDim2.new(0,5,0,60),"距离: "..CF.rg)
local RgAdd=Bt(P1,UDim2.new(0,140,0,58),"+",Color3.new(0.1,0.3,0.5))
local RgSub=Bt(P1,UDim2.new(0,75,0,58),"-",Color3.new(0.1,0.3,0.5))
Lb(P1,UDim2.new(0,5,0,86),"🛡️ 过检测")
local AntiBtn=Bt(P1,UDim2.new(0,75,0,84),"关闭",Color3.new(0.5,0.1,0.1))
local P2=Pages[2]
local EspBtn=Bt(P2,UDim2.new(0,5,0,5),"👁️ 透视",Color3.new(0.5,0.1,0.1))
Lb(P2,UDim2.new(0,5,0,34),"方框")
local BoxBtn=Bt(P2,UDim2.new(0,75,0,32),"关闭",Color3.new(0.5,0.1,0.1))
Lb(P2,UDim2.new(0,5,0,60),"射线")
local TracerBtn=Bt(P2,UDim2.new(0,75,0,58),"关闭",Color3.new(0.5,0.1,0.1))
Lb(P2,UDim2.new(0,5,0,86),"信息")
local InfoBtn=Bt(P2,UDim2.new(0,75,0,84),"关闭",Color3.new(0.5,0.1,0.1))
local P3=Pages[3]
local StatusLabel=Instance.new("TextLabel")
StatusLabel.Size=UDim2.new(1,-10,0,140)
StatusLabel.Position=UDim2.new(0,5,0,5)
StatusLabel.BackgroundTransparency=1
StatusLabel.Text="状态信息\n\n自瞄: 关闭\n透视: 关闭\n过检测: 关闭\n目标: 无"
StatusLabel.TextColor3=Color3.new(0.7,0.7,0.8)
StatusLabel.Font=Enum.Font.Gotham
StatusLabel.TextSize=10
StatusLabel.TextXAlignment=Enum.TextXAlignment.Left
StatusLabel.Parent=P3
local Ball=Instance.new("ImageButton")
Ball.Size=UDim2.new(0,44,0,44)
Ball.Position=UDim2.new(0.9,-22,0.8,-22)
Ball.BackgroundColor3=Color3.new(0.2,0.6,1)
Ball.Visible=false
Ball.Draggable=true
Ball.AutoButtonColor=false
Ball.Parent=G
Instance.new("UICorner",Ball).CornerRadius=UDim.new(1,0)
local BallT=Instance.new("TextLabel")
BallT.Size=UDim2.new(1,0,1,0)
BallT.BackgroundTransparency=1
BallT.Text="AI"
BallT.TextColor3=Color3.new(1,1,1)
BallT.Font=Enum.Font.GothamBold
BallT.TextSize=16
BallT.Parent=Ball
local DS,DP
Ball.InputBegan:Connect(function(i)
if i.UserInputType==Enum.UserInputType.MouseButton1 then DS=i.Position DP=Ball.Position end
end)
Ball.InputChanged:Connect(function(i)
if i.UserInputType==Enum.UserInputType.MouseButton1 and DS then
local d=i.Position-DS
Ball.Position=UDim2.new(DP.X.Scale,DP.X.Offset+d.X,DP.Y.Scale,DP.Y.Offset+d.Y)
end
end)
Ball.InputEnded:Connect(function()DS=nil end)
H.MouseButton1Click:Connect(function()F.Visible=false Ball.Visible=true Ball.Position=F.Position end)
Ball.MouseButton1Click:Connect(function()F.Position=Ball.Position F.Visible=true Ball.Visible=false end)
local function GetRoot()
return M.Character and M.Character:FindFirstChild("HumanoidRootPart")
end
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
local antiTimer=0
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
end
if antiTimer>35 then antiTimer=0 end
end
AimBtn.MouseButton1Click:Connect(function()
CF.aim=not CF.aim
AimBtn.Text=CF.aim and"🔒 关闭"or"🔒 自瞄"
AimBtn.BackgroundColor3=CF.aim and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)
end)
SmAdd.MouseButton1Click:Connect(function()
CF.sm=math.min(1,CF.sm+0.05)
Lb(P1,UDim2.new(0,5,0,34),"平滑度: "..string.format("%.2f",CF.sm))
end)
SmSub.MouseButton1Click:Connect(function()
CF.sm=math.max(0.05,CF.sm-0.05)
Lb(P1,UDim2.new(0,5,0,34),"平滑度: "..string.format("%.2f",CF.sm))
end)
RgAdd.MouseButton1Click:Connect(function()
CF.rg=math.min(600,CF.rg+10)
Lb(P1,UDim2.new(0,5,0,60),"距离: "..CF.rg)
end)
RgSub.MouseButton1Click:Connect(function()
CF.rg=math.max(30,CF.rg-10)
Lb(P1,UDim2.new(0,5,0,60),"距离: "..CF.rg)
end)
AntiBtn.MouseButton1Click:Connect(function()
CF.anti=not CF.anti
AntiBtn.Text=CF.anti and"开启"or"关闭"
AntiBtn.BackgroundColor3=CF.anti and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)
end)
EspBtn.MouseButton1Click:Connect(function()
CF.esp=not CF.esp
EspBtn.Text=CF.esp and"👁️ 关闭"or"👁️ 透视"
EspBtn.BackgroundColor3=CF.esp and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)
end)
BoxBtn.MouseButton1Click:Connect(function()
CF.box=not CF.box
BoxBtn.Text=CF.box and"开启"or"关闭"
BoxBtn.BackgroundColor3=CF.box and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)
end)
TracerBtn.MouseButton1Click:Connect(function()
CF.tracer=not CF.tracer
TracerBtn.Text=CF.tracer and"开启"or"关闭"
TracerBtn.BackgroundColor3=CF.tracer and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)
end)
InfoBtn.MouseButton1Click:Connect(function()
CF.info=not CF.info
InfoBtn.Text=CF.info and"开启"or"关闭"
InfoBtn.BackgroundColor3=CF.info and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)
end)
local function UpdateStatus()
local s="状态信息\n\n"
s=s.."自瞄: "..(CF.aim and"✅ 开启"or"❌ 关闭").."\n"
s=s.."透视: "..(CF.esp and"✅ 开启"or"❌ 关闭").."\n"
s=s.."过检测: "..(CF.anti and"✅ 开启"or"❌ 关闭").."\n"
local t=GetTarget()
s=s.."目标: "..(t and"✅ 锁定"or"❌ 无")
StatusLabel.Text=s
end
R.RenderStepped:Connect(function()
if currentTab==3 then UpdateStatus()end
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
dx=math.clamp(dx,-60,60)
dy=math.clamp(dy,-60,60)
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
end)
print("AI V4 加载完成")
