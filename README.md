local Players=game:GetService("Players")
local RunService=game:GetService("RunService")
local LP=Players.LocalPlayer
local UIS=game:GetService("UserInputService")
local CG=game:GetService("CoreGui")

local G=Instance.new("ScreenGui")
G.ResetOnSpawn=false
G.Parent=CG

-- 底部可爱提示
local Tip=Instance.new("Frame")
Tip.Size=UDim2.new(0,420,0,35)
Tip.Position=UDim2.new(0.5,-210,1,-55)
Tip.BackgroundColor3=Color3.fromRGB(255,182,193)
Tip.BackgroundTransparency=0.1
Tip.Parent=G
Instance.new("UICorner",Tip).CornerRadius=UDim.new(0,17)

local TipLabel=Instance.new("TextLabel")
TipLabel.Size=UDim2.new(1,-20,1,0)
TipLabel.Position=UDim2.new(0,10,0,0)
TipLabel.BackgroundTransparency=1
TipLabel.Text="今天也要元气满满哦~"
TipLabel.TextColor3=Color3.fromRGB(255,255,255)
TipLabel.Font=Enum.Font.GothamBold
TipLabel.TextSize=13
TipLabel.Parent=Tip
local Main=Instance.new("Frame")
Main.Size=UDim2.new(0,320,0,420)
Main.Position=UDim2.new(0.5,-160,0.5,-210)
Main.BackgroundColor3=Color3.fromRGB(255,240,245)
Main.BackgroundTransparency=0.05
Main.Active=true
Main.Draggable=true
Main.Parent=G
Instance.new("UICorner",Main).CornerRadius=UDim.new(0,20)

local Grad=Instance.new("UIGradient")
Grad.Color=ColorSequence.new{
ColorSequenceKeypoint.new(0,Color3.fromRGB(255,182,193)),
ColorSequenceKeypoint.new(1,Color3.fromRGB(221,160,221))
}
Grad.Rotation=45
Grad.Parent=Main

local Title=Instance.new("TextLabel")
Title.Size=UDim2.new(1,0,0,50)
Title.Position=UDim2.new(0,0,0,15)
Title.BackgroundTransparency=1
Title.Text="🌸 樱の辅助 🌸"
Title.TextColor3=Color3.fromRGB(255,105,180)
Title.Font=Enum.Font.GothamBold
Title.TextSize=26
Title.Parent=Main

local Sub=Instance.new("TextLabel")
Sub.Size=UDim2.new(1,0,0,20)
Sub.Position=UDim2.new(0,0,0,70)
Sub.BackgroundTransparency=1
Sub.Text="卡密验证系统"
Sub.TextColor3=Color3.fromRGB(255,255,255)
Sub.Font=Enum.Font.GothamBold
Sub.TextSize=14
Sub.Parent=Main

local Hint=Instance.new("TextLabel")
Hint.Size=UDim2.new(1,0,0,15)
Hint.Position=UDim2.new(0,0,0,92)
Hint.BackgroundTransparency=1
Hint.Text="请输入您的卡密"
Hint.TextColor3=Color3.fromRGB(200,200,200)
Hint.Font=Enum.Font.Gotham
Hint.TextSize=11
Hint.Parent=Main
local KeyBox=Instance.new("TextBox")
KeyBox.Size=UDim2.new(0,280,0,40)
KeyBox.Position=UDim2.new(0.5,-140,0,115)
KeyBox.BackgroundColor3=Color3.fromRGB(255,255,255)
KeyBox.TextColor3=Color3.fromRGB(50,50,50)
KeyBox.Font=Enum.Font.Gotham
KeyBox.TextSize=13
KeyBox.PlaceholderText="XXXX-XXXX-XXXX-XXXX"
KeyBox.PlaceholderColor3=Color3.fromRGB(180,180,180)
KeyBox.Parent=Main
Instance.new("UICorner",KeyBox).CornerRadius=UDim.new(0,10)

local VerBtn=Instance.new("TextButton")
VerBtn.Size=UDim2.new(0,280,0,40)
VerBtn.Position=UDim2.new(0.5,-140,0,165)
VerBtn.BackgroundColor3=Color3.fromRGB(255,255,255)
VerBtn.Text="版本: 通用版 ▼"
VerBtn.TextColor3=Color3.fromRGB(50,50,50)
VerBtn.Font=Enum.Font.Gotham
VerBtn.TextSize=13
VerBtn.Parent=Main
Instance.new("UICorner",VerBtn).CornerRadius=UDim.new(0,10)

local isWar=false
VerBtn.MouseButton1Click:Connect(function()
    isWar=not isWar
    VerBtn.Text=isWar and "版本: 战争大亨 ▼" or "版本: 通用版 ▼"
end)

local KeyBtn=Instance.new("TextButton")
KeyBtn.Size=UDim2.new(0,280,0,45)
KeyBtn.Position=UDim2.new(0.5,-140,0,215)
KeyBtn.BackgroundColor3=Color3.fromRGB(255,105,180)
KeyBtn.Text="卡密"
KeyBtn.TextColor3=Color3.fromRGB(255,255,255)
KeyBtn.Font=Enum.Font.GothamBold
KeyBtn.TextSize=16
KeyBtn.Parent=Main
Instance.new("UICorner",KeyBtn).CornerRadius=UDim.new(0,12)

local Water=Instance.new("TextLabel")
Water.Size=UDim2.new(1,0,0,20)
Water.Position=UDim2.new(0,0,1,-25)
Water.BackgroundTransparency=1
Water.Text="樱の辅助 Key System"
Water.TextColor3=Color3.fromRGB(180,180,180)
Water.Font=Enum.Font.Gotham
Water.TextSize=10
Water.Parent=Main
local Panel=Instance.new("Frame")
Panel.Size=UDim2.new(0,260,0,260)
Panel.Position=UDim2.new(0.5,-130,0.5,-130)
Panel.BackgroundColor3=Color3.fromRGB(255,240,245)
Panel.BackgroundTransparency=0.05
Panel.Active=true
Panel.Draggable=true
Panel.Visible=false
Panel.Parent=G
Instance.new("UICorner",Panel).CornerRadius=UDim.new(0,16)

local PTitle=Instance.new("Frame")
PTitle.Size=UDim2.new(1,0,0,28)
PTitle.BackgroundColor3=Color3.fromRGB(255,182,193)
PTitle.Parent=Panel
Instance.new("UICorner",PTitle).CornerRadius=UDim.new(0,16)

local PLabel=Instance.new("TextLabel")
PLabel.Size=UDim2.new(1,-60,1,0)
PLabel.Position=UDim2.new(0,10,0,0)
PLabel.BackgroundTransparency=1
PLabel.Text="🌸 樱の功能面板"
PLabel.TextColor3=Color3.fromRGB(255,255,255)
PLabel.Font=Enum.Font.GothamBold
PLabel.TextSize=13
PLabel.Parent=PTitle

local HideP=Instance.new("TextButton")
HideP.Size=UDim2.new(0,26,1,0)
HideP.Position=UDim2.new(1,-30,0,0)
HideP.BackgroundTransparency=0.4
HideP.BackgroundColor3=Color3.fromRGB(255,255,255)
HideP.Text="-"
HideP.TextColor3=Color3.fromRGB(255,105,180)
HideP.Font=Enum.Font.GothamBold
HideP.TextSize=16
Instance.new("UICorner",HideP).CornerRadius=UDim.new(0,6)
HideP.Parent=PTitle

local CF={aim=false,esp=false,spd=false,wall=false,bt=false}

local function Btn(t,p)
    local b=Instance.new("TextButton")
    b.Size=UDim2.new(0,75,0,24)
    b.Position=p
    b.BackgroundTransparency=0.25
    b.BackgroundColor3=Color3.fromRGB(255,105,180)
    b.Text=t
    b.TextColor3=Color3.fromRGB(255,255,255)
    b.Font=Enum.Font.Gotham
    b.TextSize=9
    Instance.new("UICorner",b).CornerRadius=UDim.new(0,8)
    b.Parent=Panel
    return b
end

local B1=Btn("自瞄",UDim2.new(0,8,0,36))
local B2=Btn("透视",UDim2.new(0,100,0,36))
local B3=Btn("加速",UDim2.new(0,8,0,66))
local B4=Btn("穿墙",UDim2.new(0,100,0,66))
local B5=Btn("子弹追踪",UDim2.new(0,8,0,96))

-- 加速面板
local SpeedPanel=Instance.new("Frame")
SpeedPanel.Size=UDim2.new(0,240,0,26)
SpeedPanel.Position=UDim2.new(0,8,0,128)
SpeedPanel.BackgroundTransparency=1
SpeedPanel.Parent=Panel

local SpeedCfg={enabled=false, value=50, min=16, max=200, step=10}

local SubBtn=Instance.new("TextButton")
SubBtn.Size=UDim2.new(0,28,0,22)
SubBtn.Position=UDim2.new(0,0,0,2)
SubBtn.BackgroundColor3=Color3.fromRGB(255,182,193)
SubBtn.Text="-"
SubBtn.TextColor3=Color3.fromRGB(255,255,255)
SubBtn.Font=Enum.Font.GothamBold
SubBtn.TextSize=14
Instance.new("UICorner",SubBtn).CornerRadius=UDim.new(0,6)
SubBtn.Parent=SpeedPanel

local SpeedLabel=Instance.new("TextLabel")
SpeedLabel.Size=UDim2.new(0,140,0,22)
SpeedLabel.Position=UDim2.new(0,32,0,2)
SpeedLabel.BackgroundColor3=Color3.fromRGB(255,255,255)
SpeedLabel.Text="速度: 50"
SpeedLabel.TextColor3=Color3.fromRGB(255,105,180)
SpeedLabel.Font=Enum.Font.Gotham
SpeedLabel.TextSize=11
Instance.new("UICorner",SpeedLabel).CornerRadius=UDim.new(0,6)
SpeedLabel.Parent=SpeedPanel

local AddBtn=Instance.new("TextButton")
AddBtn.Size=UDim2.new(0,28,0,22)
AddBtn.Position=UDim2.new(0,176,0,2)
AddBtn.BackgroundColor3=Color3.fromRGB(221,160,221)
AddBtn.Text="+"
AddBtn.TextColor3=Color3.fromRGB(255,255,255)
AddBtn.Font=Enum.Font.GothamBold
AddBtn.TextSize=14
Instance.new("UICorner",AddBtn).CornerRadius=UDim.new(0,6)
AddBtn.Parent=SpeedPanel
local function GetHum()
    local c=LP.Character
    return c and c:FindFirstChildOfClass("Humanoid")
end

local function ApplySpeed()
    local h=GetHum()
    if h then
        h.WalkSpeed=SpeedCfg.enabled and math.clamp(SpeedCfg.value,SpeedCfg.min,SpeedCfg.max) or 16
    end
end

local function GetTarget()
    local r=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
    if not r then return nil end
    local t,d=nil,999
    for _,p in pairs(Players:GetPlayers()) do
        if p~=LP and p.Character then
            local rr=p.Character:FindFirstChild("HumanoidRootPart")
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if rr and h and h.Health>0 then
                local dist=(r.Position-rr.Position).Magnitude
                if dist<d then d=dist t=rr end
            end
        end
    end
    return t
end

local espList={}
local function ApplyWall()
    local c=LP.Character
    if not c then return end
    for _,v in pairs(c:GetDescendants()) do
        if v:IsA("BasePart") then v.CanCollide=not CF.wall end
    end
    if CF.wall then
        pcall(function()
            for _,v in pairs(workspace:GetDescendants()) do
                if v:IsA("BasePart") and v.CanCollide then v.CanCollide=false end
            end
        end)
    end
end

local function Track()
    if not CF.bt then return end
    local t=GetTarget()
    if not t then return end
    for _,v in pairs(workspace:GetDescendants()) do
        if v:IsA("BasePart") and (v.Name:lower():find("bullet") or v.Name:lower():find("projectile")) then
            local creator=v:FindFirstChild("Creator")
            if not (creator and creator.Value==LP) then
                if (v.Position-t.Position).Magnitude<300 then
                    local dir=(t.Position-v.Position).Unit
                    v.Velocity=dir*250
                    v.CFrame=CFrame.new(v.Position,t.Position)
                end
            end
        end
    end
end

-- 验证
KeyBtn.MouseButton1Click:Connect(function()
    local key=KeyBox.Text
    local ok=false
    if isWar then
        if key=="XDD-AX39F-F64XL-8MKHX-62S75" then ok=true
        else TipLabel.Text="❌ 战争大亨卡密错误" wait(2) TipLabel.Text="今天也要元气满满哦~" end
    else
        if #key>0 then ok=true
        else TipLabel.Text="❌ 请输入卡密" wait(2) TipLabel.Text="今天也要元气满满哦~" end
    end
    if ok then
        Main.Visible=false
        Panel.Visible=true
        TipLabel.Text="✅ 验证成功，欢迎使用！"
        wait(2)
        TipLabel.Text="今天也要元气满满哦~"
    end
end)

-- 按钮事件
B1.MouseButton1Click:Connect(function()
    CF.aim=not CF.aim
    B1.BackgroundColor3=CF.aim and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
end)
B2.MouseButton1Click:Connect(function()
    CF.esp=not CF.esp
    B2.BackgroundColor3=CF.esp and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
end)
B3.MouseButton1Click:Connect(function()
    SpeedCfg.enabled=not SpeedCfg.enabled
    B3.BackgroundColor3=SpeedCfg.enabled and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    ApplySpeed()
end)
B4.MouseButton1Click:Connect(function()
    CF.wall=not CF.wall
    B4.BackgroundColor3=CF.wall and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    ApplyWall()
end)
B5.MouseButton1Click:Connect(function()
    CF.bt=not CF.bt
    B5.BackgroundColor3=CF.bt and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
end)

SubBtn.MouseButton1Click:Connect(function()
    SpeedCfg.value=math.max(SpeedCfg.min,SpeedCfg.value-SpeedCfg.step)
    SpeedLabel.Text="速度: "..SpeedCfg.value
    ApplySpeed()
end)
AddBtn.MouseButton1Click:Connect(function()
    SpeedCfg.value=math.min(SpeedCfg.max,SpeedCfg.value+SpeedCfg.step)
    SpeedLabel.Text="速度: "..SpeedCfg.value
    ApplySpeed()
end)

HideP.MouseButton1Click:Connect(function()
    Panel.Visible=not Panel.Visible
    HideP.Text=Panel.Visible and "-" or "+"
end)

-- 主循环
RunService.RenderStepped:Connect(function()
    local cam=workspace.CurrentCamera
    if not cam then return end
    -- 自瞄
    if CF.aim then
        local t=GetTarget()
        if t then
            local sp,on=cam:WorldToViewportPoint(t.Position)
            if on then
                local vs=cam.ViewportSize
                local dx=(sp.X-vs.X/2)*0.3
                local dy=(sp.Y-vs.Y/2)*0.3
                dx=math.clamp(dx,-30,30)
                dy=math.clamp(dy,-30,30)
                UIS:SetMouseDelta(Vector2.new(dx,dy))
            end
        end
    end
    -- 透视
    if CF.esp then
        for _,p in pairs(Players:GetPlayers()) do
            if p~=LP and p.Character then
                local h=p.Character:FindFirstChildOfClass("Humanoid")
                if h and h.Health>0 then
                    local has=false
                    for _,v in pairs(espList) do
                        if v.Adornee==p.Character then has=true break end
                    end
                    if not has then
                        local hl=Instance.new("Highlight")
                        hl.FillColor=Color3.fromRGB(255,182,193)
                        hl.FillTransparency=0.5
                        hl.Adornee=p.Character
                        hl.Parent=p.Character
                        table.insert(espList,hl)
                    end
                end
            end
        end
    else
        for _,v in pairs(espList) do v:Destroy() end
        espList={}
    end
    -- 加速保持
    if SpeedCfg.enabled then ApplySpeed() end
    -- 穿墙保持
    if CF.wall then ApplyWall() end
    -- 子弹追踪
    if CF.bt then Track() end
end)

print("樱の辅助 加载完成")
