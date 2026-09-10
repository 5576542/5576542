local Players=game:GetService("Players")
local LP=Players.LocalPlayer
local CoreGui=game:GetService("CoreGui")
local UIS=game:GetService("UserInputService")

local G=Instance.new("ScreenGui")
G.ResetOnSpawn=false
G.Parent=CoreGui

-- 底部提示框
local TipFrame=Instance.new("Frame")
TipFrame.Size=UDim2.new(0,420,0,35)
TipFrame.Position=UDim2.new(0.5,-210,1,-55)
TipFrame.BackgroundColor3=Color3.new(0.15,0.15,0.25)
TipFrame.BackgroundTransparency=0.1
TipFrame.Parent=G
Instance.new("UICorner",TipFrame).CornerRadius=UDim.new(0,8)
local TipGrad=Instance.new("UIGradient")
TipGrad.Color=ColorSequence.new{ColorSequenceKeypoint.new(0,Color3.new(0.3,0.1,0.6)),ColorSequenceKeypoint.new(1,Color3.new(0.1,0.2,0.6))}
TipGrad.Parent=TipFrame
local TipLabel=Instance.new("TextLabel")
TipLabel.Size=UDim2.new(1,-20,1,0)
TipLabel.Position=UDim2.new(0,10,0,0)
TipLabel.BackgroundTransparency=1
TipLabel.Text="如果能摸摸我的头的话，我会很开心的！"
TipLabel.TextColor3=Color3.new(0.9,0.9,1)
TipLabel.Font=Enum.Font.Gotham
TipLabel.TextSize=13
TipLabel.Parent=TipFrame

-- 主验证窗口
local MainFrame=Instance.new("Frame")
MainFrame.Size=UDim2.new(0,320,0,420)
MainFrame.Position=UDim2.new(0.5,-160,0.5,-210)
MainFrame.BackgroundColor3=Color3.new(0.08,0.08,0.15)
MainFrame.BackgroundTransparency=0.05
MainFrame.Active=true
MainFrame.Draggable=true
MainFrame.Parent=G
Instance.new("UICorner",MainFrame).CornerRadius=UDim.new(0,16)

local MainGrad=Instance.new("UIGradient")
MainGrad.Color=ColorSequence.new{
ColorSequenceKeypoint.new(0,Color3.new(0.3,0.1,0.7)),
ColorSequenceKeypoint.new(0.5,Color3.new(0.1,0.3,0.8)),
ColorSequenceKeypoint.new(1,Color3.new(0.3,0.1,0.7))
}
MainGrad.Rotation=45
MainGrad.Parent=MainFrame

local Border=Instance.new("UIStroke")
Border.Color=Color3.new(0.5,0.2,1)
Border.Thickness=2
Border.Parent=MainFrame

-- 标题
local Title=Instance.new("TextLabel")
Title.Size=UDim2.new(1,0,0,50)
Title.Position=UDim2.new(0,0,0,15)
Title.BackgroundTransparency=1
Title.Text="AlienX Script"
Title.TextColor3=Color3.new(0.8,0.4,1)
Title.Font=Enum.Font.GothamBold
Title.TextSize=28
Title.Parent=MainFrame

-- 图标（外星人头用文本替代，可自行换成图片ID）
local Icon=Instance.new("ImageLabel")
Icon.Size=UDim2.new(0,60,0,60)
Icon.Position=UDim2.new(0.5,-30,0,70)
Icon.BackgroundTransparency=1
Icon.Image="rbxassetid://12788874504" -- 通用外星人图标
Icon.Parent=MainFrame

local SubTitle=Instance.new("TextLabel")
SubTitle.Size=UDim2.new(1,0,0,20)
SubTitle.Position=UDim2.new(0,0,0,135)
SubTitle.BackgroundTransparency=1
SubTitle.Text="卡密验证系统"
SubTitle.TextColor3=Color3.new(0.9,0.9,1)
SubTitle.Font=Enum.Font.GothamBold
SubTitle.TextSize=14
SubTitle.Parent=MainFrame

local Hint=Instance.new("TextLabel")
Hint.Size=UDim2.new(1,0,0,15)
Hint.Position=UDim2.new(0,0,0,155)
Hint.BackgroundTransparency=1
Hint.Text="请输入您的卡密"
Hint.TextColor3=Color3.new(0.6,0.6,0.8)
Hint.Font=Enum.Font.Gotham
Hint.TextSize=11
Hint.Parent=MainFrame

-- 卡密输入框
local KeyBox=Instance.new("TextBox")
KeyBox.Size=UDim2.new(0,280,0,40)
KeyBox.Position=UDim2.new(0.5,-140,0,180)
KeyBox.BackgroundColor3=Color3.new(0.15,0.15,0.25)
KeyBox.TextColor3=Color3.new(1,1,1)
KeyBox.Font=Enum.Font.Gotham
KeyBox.TextSize=13
KeyBox.PlaceholderText="XXXX-XXXX-XXXX-XXXX"
KeyBox.PlaceholderColor3=Color3.new(0.4,0.4,0.6)
KeyBox.Parent=MainFrame
Instance.new("UICorner",KeyBox).CornerRadius=UDim.new(0,8)

-- 版本切换（下拉菜单）
local VersionBtn=Instance.new("TextButton")
VersionBtn.Size=UDim2.new(0,280,0,40)
VersionBtn.Position=UDim2.new(0.5,-140,0,235)
VersionBtn.BackgroundColor3=Color3.new(0.15,0.15,0.25)
VersionBtn.Text="版本: 通用版 ▼"
VersionBtn.TextColor3=Color3.new(1,1,1)
VersionBtn.Font=Enum.Font.Gotham
VersionBtn.TextSize=13
VersionBtn.Parent=MainFrame
Instance.new("UICorner",VersionBtn).CornerRadius=UDim.new(0,8)

local isWar=false
VersionBtn.MouseButton1Click:Connect(function()
isWar=not isWar
VersionBtn.Text=isWar and "版本: 战争大亨 ▼" or "版本: 通用版 ▼"
end)

-- 验证按钮
local KeyBtn=Instance.new("TextButton")
KeyBtn.Size=UDim2.new(0,280,0,45)
KeyBtn.Position=UDim2.new(0.5,-140,0,290)
KeyBtn.BackgroundColor3=Color3.new(0.4,0.2,0.8)
KeyBtn.Text="卡密"
KeyBtn.TextColor3=Color3.new(1,1,1)
KeyBtn.Font=Enum.Font.GothamBold
KeyBtn.TextSize=16
KeyBtn.Parent=MainFrame
Instance.new("UICorner",KeyBtn).CornerRadius=UDim.new(0,8)

local BtnGrad=Instance.new("UIGradient")
BtnGrad.Color=ColorSequence.new{
ColorSequenceKeypoint.new(0,Color3.new(0.4,0.1,0.9)),
ColorSequenceKeypoint.new(1,Color3.new(0.1,0.3,0.9))
}
BtnGrad.Rotation=90
BtnGrad.Parent=KeyBtn

-- 底部水印
local Watermark=Instance.new("TextLabel")
Watermark.Size=UDim2.new(1,0,0,20)
Watermark.Position=UDim2.new(0,0,1,-25)
Watermark.BackgroundTransparency=1
Watermark.Text="AlienX Script Key System"
Watermark.TextColor3=Color3.new(0.3,0.3,0.5)
Watermark.Font=Enum.Font.Gotham
Watermark.TextSize=10
Watermark.Parent=MainFrame
-- 功能悬浮窗（验证成功后显示）
local FunctionFrame=Instance.new("Frame")
FunctionFrame.Size=UDim2.new(0,260,0,220)
FunctionFrame.Position=UDim2.new(0.5,-130,0.5,-110)
FunctionFrame.BackgroundColor3=Color3.new(0.08,0.08,0.15)
FunctionFrame.BackgroundTransparency=0.05
FunctionFrame.Active=true
FunctionFrame.Draggable=true
FunctionFrame.Visible=false
FunctionFrame.Parent=G
Instance.new("UICorner",FunctionFrame).CornerRadius=UDim.new(0,12)

local FuncTitle=Instance.new("Frame")
FuncTitle.Size=UDim2.new(1,0,0,28)
FuncTitle.BackgroundColor3=Color3.new(0.15,0.15,0.25)
FuncTitle.Parent=FunctionFrame
Instance.new("UICorner",FuncTitle).CornerRadius=UDim.new(0,12)

local FuncLabel=Instance.new("TextLabel")
FuncLabel.Size=UDim2.new(1,-60,1,0)
FuncLabel.Position=UDim2.new(0,10,0,0)
FuncLabel.BackgroundTransparency=1
FuncLabel.Text="AlienX 功能面板"
FuncLabel.TextColor3=Color3.new(0.8,0.4,1)
FuncLabel.Font=Enum.Font.GothamBold
FuncLabel.TextSize=13
FuncLabel.Parent=FuncTitle

local HideF=Instance.new("TextButton")
HideF.Size=UDim2.new(0,26,1,0)
HideF.Position=UDim2.new(1,-30,0,0)
HideF.BackgroundTransparency=0.4
HideF.BackgroundColor3=Color3.new(0.3,0.3,0.3)
HideF.Text="-"
HideF.TextColor3=Color3.new(1,1,1)
HideF.Font=Enum.Font.GothamBold
HideF.TextSize=16
Instance.new("UICorner",HideF).CornerRadius=UDim.new(0,4)
HideF.Parent=FuncTitle

-- 临时配置和功能按钮（你可以按需填入之前的功能）
local CF={aim=false, esp=false, spd=false, wall=false, bt=false}
local function Btn(t,p)
local b=Instance.new("TextButton")
b.Size=UDim2.new(0,75,0,24)
b.Position=p
b.BackgroundTransparency=0.25
b.BackgroundColor3=Color3.new(0.5,0.1,0.1)
b.Text=t
b.TextColor3=Color3.new(1,1,1)
b.Font=Enum.Font.Gotham
b.TextSize=9
Instance.new("UICorner",b).CornerRadius=UDim.new(0,6)
b.Parent=FunctionFrame
return b
end

local B1=Btn("自瞄",UDim2.new(0,8,0,36))
local B2=Btn("透视",UDim2.new(0,100,0,36))
local B3=Btn("加速",UDim2.new(0,8,0,66))
local B4=Btn("穿墙",UDim2.new(0,100,0,66))
local B5=Btn("子弹追踪",UDim2.new(0,8,0,96))

local SFunc=Instance.new("TextLabel")
SFunc.Size=UDim2.new(1,-16,0,30)
SFunc.Position=UDim2.new(0,8,0,130)
SFunc.BackgroundTransparency=1
SFunc.Text="状态: 等待开启"
SFunc.TextColor3=Color3.new(0.6,0.6,0.7)
SFunc.Font=Enum.Font.Gotham
SFunc.TextSize=9
SFunc.TextXAlignment=Enum.TextXAlignment.Left
SFunc.Parent=FunctionFrame

-- 验证逻辑
KeyBtn.MouseButton1Click:Connect(function()
local inputKey=KeyBox.Text
local isValid=false

if isWar then
-- 战争大亨专属卡密
if inputKey=="XDD-AX39F-F64XL-8MKHX-62S75" then
isValid=true
else
TipLabel.Text="❌ 战争大亨卡密错误！"
wait(2)
TipLabel.Text="如果能摸摸我的头的话，我会很开心的！"
end
else
-- 通用版：随便输入或者特定格式（这里随便填，方便测试）
if #inputKey>0 then
isValid=true
else
TipLabel.Text="❌ 请输入卡密！"
wait(2)
TipLabel.Text="如果能摸摸我的头的话，我会很开心的！"
end
end

if isValid then
MainFrame.Visible=false
FunctionFrame.Visible=true
TipLabel.Text="✅ 验证成功，欢迎使用！"
wait(2)
TipLabel.Text="如果能摸摸我的头的话，我会很开心的！"
end
end)

-- 验证成功后，功能按钮事件（这里先写简单开关，记得把具体功能代码填进去）
local function Upd()
SFunc.Text="状态: 自瞄"..(CF.aim and"开"or"关").." | 透视"..(CF.esp and"开"or"关").." | 加速"..(CF.spd and"开"or"关").." | 穿墙"..(CF.wall and"开"or"关").." | 子弹追踪"..(CF.bt and"开"or"关")
end

B1.MouseButton1Click:Connect(function() CF.aim=not CF.aim B1.BackgroundColor3=CF.aim and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1) Upd() end)
B2.MouseButton1Click:Connect(function() CF.esp=not CF.esp B2.BackgroundColor3=CF.esp and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1) Upd() end)
B3.MouseButton1Click:Connect(function() 
CF.spd=not CF.spd 
B3.BackgroundColor3=CF.spd and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1)
local h=LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
if h then h.WalkSpeed=CF.spd and 50 or 16 end
Upd() 
end)
B4.MouseButton1Click:Connect(function() 
CF.wall=not CF.wall 
B4.BackgroundColor3=CF.wall and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1)
if LP.Character then
for _,v in pairs(LP.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide=not CF.wall end end
end
Upd() 
end)
B5.MouseButton1Click:Connect(function() CF.bt=not CF.bt B5.BackgroundColor3=CF.bt and Color3.new(0.1,0.5,0.1) or Color3.new(0.5,0.1,0.1) Upd() end)

-- 隐藏功能面板
HideF.MouseButton1Click:Connect(function()
FunctionFrame.Visible=not FunctionFrame.Visible
HideF.Text=FunctionFrame.Visible and "-" or "+"
end)

print("AlienX 卡密系统加载完成")
