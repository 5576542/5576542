local P=game:GetService("Players")local R=game:GetService("RunService")local M=P.LocalPlayer
local G=Instance.new("ScreenGui")G.ResetOnSpawn=false G.Parent=game.CoreGui
local F=Instance.new("Frame")F.Size=UDim2.new(0,200,0,180)F.Position=UDim2.new(0.02,0,0.2,0)F.BackgroundColor3=Color3.new(0.08,0.08,0.12)F.BackgroundTransparency=0.2 F.Active=true F.Draggable=true F.Parent=G
Instance.new("UICorner",F).CornerRadius=UDim.new(0,10)
local T=Instance.new("Frame")T.Size=UDim2.new(1,0,0,25)T.BackgroundColor3=Color3.new(0.15,0.15,0.22)T.BackgroundTransparency=0.3 T.Parent=F
Instance.new("UICorner",T).CornerRadius=UDim.new(0,10)
local TL=Instance.new("TextLabel")TL.Size=UDim2.new(1,-60,1,0)TL.Position=UDim2.new(0,8,0,0)TL.BackgroundTransparency=1 TL.Text="AI 辅助"TL.TextColor3=Color3.new(0.3,0.8,1)TL.Font=Enum.Font.GothamBold TL.TextSize=12 TL.TextXAlignment=Enum.TextXAlignment.Left TL.Parent=T
local PL=Instance.new("TextLabel")PL.Size=UDim2.new(0,40,1,0)PL.Position=UDim2.new(1,-50,0,0)PL.BackgroundTransparency=1 PL.Text="1/2"PL.TextColor3=Color3.new(0.6,0.6,0.6)PL.Font=Enum.Font.Gotham PL.TextSize=10 PL.Parent=T
local H=Instance.new("TextButton")H.Size=UDim2.new(0,24,1,0)H.Position=UDim2.new(1,-26,0,0)H.BackgroundTransparency=0.4 H.BackgroundColor3=Color3.new(0.35,0.35,0.35)H.Text="-"H.TextColor3=Color3.new(1,1,1)H.Font=Enum.Font.GothamBold H.TextSize=12 Instance.new("UICorner",H).CornerRadius=UDim.new(0,4)H.Parent=T
local D=Instance.new("Frame")D.Size=UDim2.new(0,30,0,4)D.Position=UDim2.new(0.5,-15,0,180)D.BackgroundColor3=Color3.new(0.3,0.8,1)D.BackgroundTransparency=0.5 D.Parent=F
Instance.new("UICorner",D).CornerRadius=UDim.new(0,2)
local P1=Instance.new("Frame")P1.Size=UDim2.new(1,0,1,-30)P1.Position=UDim2.new(0,0,0,30)P1.BackgroundTransparency=1 P1.Parent=F
local P2=Instance.new("Frame")P2.Size=UDim2.new(1,0,1,-30)P2.Position=UDim2.new(0,0,0,30)P2.BackgroundTransparency=1 P2.Visible=false P2.Parent=F
local C={aim=false,sm=0.3,rg=250,esp=false,anti=false}
local E={}
local function Lb(p,t,s)local l=Instance.new("TextLabel")l.Size=UDim2.new(0,60,0,16)l.Position=p l.BackgroundTransparency=1 l.Text=t l.TextColor3=Color3.new(0.8,0.8,0.8)l.Font=Enum.Font.Gotham l.TextSize=s or 10 l.TextXAlignment=Enum.TextXAlignment.Left l.Parent=P1 return l end
local function Bt(p,t,c)local b=Instance.new("TextButton")b.Size=UDim2.new(0,55,0,18)b.Position=p b.BackgroundTransparency=0.3 b.BackgroundColor3=c b.Text=t b.TextColor3=Color3.new(1,1,1)b.Font=Enum.Font.Gotham b.TextSize=9 Instance.new("UICorner",b).CornerRadius=UDim.new(0,4)b.Parent=P1 return b end
local A1=Bt(UDim2.new(0,65,0,3),"开启",Color3.new(0.5,0.1,0.1))
Lb(UDim2.new(0,5,0,28),"平滑度: 0.30")
local SA=Bt(UDim2.new(0,120,0,26),"+",Color3.new(0.15,0.25,0.5))local SS=Bt(UDim2.new(0,65,0,26),"-",Color3.new(0.15,0.25,0.5))
Lb(UDim2.new(0,5,0,50),"距离: 250")
local RA=Bt(UDim2.new(0,120,0,48),"+",Color3.new(0.15,0.25,0.5))local RS=Bt(UDim2.new(0,65,0,48),"-",Color3.new(0.15,0.25,0.5))
Lb(UDim2.new(0,5,0,72),"过检测")
local AB=Bt(UDim2.new(0,65,0,70),"关闭",Color3.new(0.5,0.1,0.1))
local EB=Bt(UDim2.new(0,5,0,10),"开启透视",Color3.new(0.5,0.1,0.1))
local RB=Bt(UDim2.new(0,5,0,55),"刷新透视",Color3.new(0.15,0.25,0.5))
local PB=Instance.new("TextButton")PB.Size=UDim2.new(0,40,0,20)PB.Position=UDim2.new(0,5,0,155)PB.BackgroundTransparency=0.3 PB.BackgroundColor3=Color3.new(0.15,0.25,0.5)PB.Text="◀"PB.TextColor3=Color3.new(1,1,1)PB.Font=Enum.Font.GothamBold PB.TextSize=12 Instance.new("UICorner",PB).CornerRadius=UDim.new(0,4)PB.Parent=F
local NB=Instance.new("TextButton")NB.Size=UDim2.new(0,40,0,20)NB.Position=UDim2.new(1,-45,0,155)NB.BackgroundTransparency=0.3 NB.BackgroundColor3=Color3.new(0.15,0.25,0.5)NB.Text="▶"NB.TextColor3=Color3.new(1,1,1)NB.Font=Enum.Font.GothamBold NB.TextSize=12 Instance.new("UICorner",NB).CornerRadius=UDim.new(0,4)NB.Parent=F
local BALL=Instance.new("ImageButton")BALL.Size=UDim2.new(0,40,0,40)BALL.Position=UDim2.new(0.9,-20,0.8,-20)BALL.BackgroundColor3=Color3.new(0.15,0.5,0.85)BALL.Visible=false BALL.Draggable=true BALL.AutoButtonColor=false BALL.Parent=G
Instance.new("UICorner",BALL).CornerRadius=UDim.new(1,0)
local BT=Instance.new("TextLabel")BT.Size=UDim2.new(1,0,1,0)BT.BackgroundTransparency=1 BT.Text="AI"BT.TextColor3=Color3.new(1,1,1)BT.Font=Enum.Font.GothamBold BT.TextSize=14 BT.Parent=BALL
local DS,DP
BALL.InputBegan:Connect(function(i)if i.UserInputType==Enum.UserInputType.MouseButton1 then DS=i.Position DP=BALL.Position end end)
BALL.InputChanged:Connect(function(i)if i.UserInputType==Enum.UserInputType.MouseButton1 and DS then local d=i.Position-DS BALL.Position=UDim2.new(DP.X.Scale,DP.X.Offset+d.X,DP.Y.Scale,DP.Y.Offset+d.Y)end end)
BALL.InputEnded:Connect(function()DS=nil end)
local CP=1
local function UP()
P1.Visible=(CP==1)P2.Visible=(CP==2)PL.Text=CP.."/2"D.Position=UDim2.new(0.5-(CP==1 and 8 or -8),0,0,180)
end
PB.MouseButton1Click:Connect(function()if CP>1 then CP=CP-1 UP()end end)
NB.MouseButton1Click:Connect(function()if CP<2 then CP=CP+1 UP()end end)
H.MouseButton1Click:Connect(function()F.Visible=false BALL.Visible=true BALL.Position=F.Position end)
BALL.MouseButton1Click:Connect(function()F.Position=BALL.Position F.Visible=true BALL.Visible=false end)
A1.MouseButton1Click:Connect(function()C.aim=not C.aim A1.Text=C.aim and"关闭"or"开启"A1.BackgroundColor3=C.aim and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
SA.MouseButton1Click:Connect(function()C.sm=math.min(1,C.sm+0.05)end)
SS.MouseButton1Click:Connect(function()C.sm=math.max(0.05,C.sm-0.05)end)
RA.MouseButton1Click:Connect(function()C.rg=math.min(600,C.rg+10)end)
RS.MouseButton1Click:Connect(function()C.rg=math.max(30,C.rg-10)end)
AB.MouseButton1Click:Connect(function()C.anti=not C.anti AB.Text=C.anti and"开启"or"关闭"AB.BackgroundColor3=C.anti and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
EB.MouseButton1Click:Connect(function()C.esp=not C.esp EB.Text=C.esp and"关闭透视"or"开启透视"EB.BackgroundColor3=C.esp and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)
RB.MouseButton1Click:Connect(function()for _,v in pairs(E)do v:Destroy()end E={}end)
local function GR()return M.Character and M.Character:FindFirstChild("HumanoidRootPart")end
local function GT()local q=GR()if not q then return end local a,b=nil,C.rg for _,p in pairs(P:GetPlayers())do if p~=M and p.Character then local d=p.Character:FindFirstChild("HumanoidRootPart")local h=d and p.Character:FindFirstChildOfClass("Humanoid")if d and h and h.Health>0 then local f=(q.Position-d.Position).Magnitude if f<b then b=f a=d end end end end return a end
local AT=0
R.RenderStepped:Connect(function()
if C.anti then AT=AT+1 if AT%5==0 then pcall(function()local f=Instance.new("BoolValue")f.Name="_"..tostring(math.random(100,999))f.Parent=M task.wait(0.1)f:Destroy()end)end if AT>30 then AT=0 end end
if C.aim then local t=GT()if t and workspace.CurrentCamera then workspace.CurrentCamera.CFrame=workspace.CurrentCamera.CFrame:Lerp(CFrame.new(workspace.CurrentCamera.CFrame.Position,t.Position),C.sm)end end
if C.esp then for _,p in pairs(P:GetPlayers())do if p~=M and p.Character then local h=p.Character:FindFirstChildOfClass("Humanoid")if h and h.Health>0 then local has=false for _,v in pairs(E)do if v.Adornee==p.Character then has=true break end end if not has then local hl=Instance.new("Highlight")hl.FillColor=Color3.new(1,0,0)hl.FillTransparency=0.5 hl.Adornee=p.Character hl.Parent=p.Character table.insert(E,hl)end end end end else for _,v in pairs(E)do v:Destroy()end E={}end end)
print("AI V2 加载完成")
