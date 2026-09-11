local Players=game:GetService("Players")
local RunService=game:GetService("RunService")
local LP=Players.LocalPlayer
local UIS=game:GetService("UserInputService")
local CG=game:GetService("CoreGui")

local G=Instance.new("ScreenGui")
G.ResetOnSpawn=false
G.Parent=CG

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

task.spawn(function()
    wait(3)
    Tip:Destroy()
end)

local BG_ID="rbxassetid://131248212024332"
local Main=Instance.new("Frame")
Main.Size=UDim2.new(0,320,0,420)
Main.Position=UDim2.new(0.5,-160,0.5,-210)
Main.BackgroundColor3=Color3.fromRGB(255,240,245)
Main.BackgroundTransparency=0.05
Main.Active=true
Main.Draggable=true
Main.Parent=G
Instance.new("UICorner",Main).CornerRadius=UDim.new(0,20)

local BgImg=Instance.new("ImageLabel")
BgImg.Size=UDim2.new(1,0,1,0)
BgImg.BackgroundTransparency=1
BgImg.Image=BG_ID
BgImg.ScaleType=Enum.ScaleType.Crop
BgImg.ImageTransparency=0.3
BgImg.Parent=Main
Instance.new("UICorner",BgImg).CornerRadius=UDim.new(0,20)

local Title=Instance.new("TextLabel")
Title.Size=UDim2.new(1,0,0,50)
Title.Position=UDim2.new(0,0,0,15)
Title.BackgroundTransparency=1
Title.Text="🌸 樱の辅助 🌸"
Title.TextColor3=Color3.fromRGB(255,105,180)
Title.Font=Enum.Font.GothamBold
Title.TextSize=26
Title.Parent=Main
local KeyBox=Instance.new("TextBox")
KeyBox.Size=UDim2.new(0,280,0,40)
KeyBox.Position=UDim2.new(0.5,-140,0,110)
KeyBox.BackgroundColor3=Color3.fromRGB(255,255,255)
KeyBox.BackgroundTransparency=0.2
KeyBox.TextColor3=Color3.fromRGB(50,50,50)
KeyBox.Font=Enum.Font.Gotham
KeyBox.TextSize=13
KeyBox.PlaceholderText="XXXX-XXXX-XXXX-XXXX"
KeyBox.PlaceholderColor3=Color3.fromRGB(180,180,180)
KeyBox.Parent=Main
Instance.new("UICorner",KeyBox).CornerRadius=UDim.new(0,10)

local VerBtn=Instance.new("TextButton")
VerBtn.Size=UDim2.new(0,280,0,40)
VerBtn.Position=UDim2.new(0.5,-140,0,160)
VerBtn.BackgroundColor3=Color3.fromRGB(255,255,255)
VerBtn.BackgroundTransparency=0.2
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
KeyBtn.Position=UDim2.new(0.5,-140,0,210)
KeyBtn.BackgroundColor3=Color3.fromRGB(255,105,180)
KeyBtn.Text="卡密"
KeyBtn.TextColor3=Color3.fromRGB(255,255,255)
KeyBtn.Font=Enum.Font.GothamBold
KeyBtn.TextSize=16
KeyBtn.Parent=Main
Instance.new("UICorner",KeyBtn).CornerRadius=UDim.new(0,12)
local Panel=Instance.new("Frame")
Panel.Size=UDim2.new(0,280,0,310)
Panel.Position=UDim2.new(0.5,-140,0.5,-155)
Panel.BackgroundColor3=Color3.fromRGB(255,240,245)
Panel.BackgroundTransparency=0.05
Panel.Active=true
Panel.Draggable=true
Panel.Visible=false
Panel.Parent=G
Instance.new("UICorner",Panel).CornerRadius=UDim.new(0,16)

local PBg=Instance.new("ImageLabel")
PBg.Size=UDim2.new(1,0,1,0)
PBg.BackgroundTransparency=1
PBg.Image=BG_ID
PBg.ScaleType=Enum.ScaleType.Crop
PBg.ImageTransparency=0.4
PBg.Parent=Panel
Instance.new("UICorner",PBg).CornerRadius=UDim.new(0,16)

local PTitle=Instance.new("Frame")
PTitle.Size=UDim2.new(1,0,0,28)
PTitle.BackgroundColor3=Color3.fromRGB(255,182,193)
PTitle.BackgroundTransparency=0.2
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
local CF={aim=false,esp=false,spd=false,wall=false,bt=false,jump=false,noFall=false,globalSpd=false}

local function Btn(t,p)
    local b=Instance.new("TextButton")
    b.Size=UDim2.new(0,80,0,24)
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

local B1=Btn("自瞄",UDim2.new(0,10,0,36))
local B2=Btn("透视",UDim2.new(0,105,0,36))
local B3=Btn("加速",UDim2.new(0,10,0,66))
local B4=Btn("穿墙",UDim2.new(0,105,0,66))
local B5=Btn("子弹追踪",UDim2.new(0,10,0,96))
local B6=Btn("高跳",UDim2.new(0,105,0,96))
local B7=Btn("坠落无伤",UDim2.new(0,10,0,126))
local B8=Btn("全局加速",UDim2.new(0,105,0,126))

local SpeedCfg={enabled=false,value=50,min=16,max=200,step=10}
local SpeedPanel=Instance.new("Frame")
SpeedPanel.Size=UDim2.new(0,240,0,26)
SpeedPanel.Position=UDim2.new(0,15,0,158)
SpeedPanel.BackgroundTransparency=1
SpeedPanel.Parent=Panel

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
SpeedLabel.BackgroundTransparency=0.2
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

local AimRing=Instance.new("Frame")
AimRing.Size=UDim2.new(0,200,0,200)
AimRing.Position=UDim2.new(0.5,-100,0.5,-100)
AimRing.BackgroundTransparency=1
AimRing.Visible=false
AimRing.Parent=G
local RingStroke=Instance.new("UIStroke")
RingStroke.Color=Color3.fromRGB(255,0,0)
RingStroke.Thickness=1.5
RingStroke.Transparency=0.3
RingStroke.Parent=AimRing
Instance.new("UICorner",AimRing).CornerRadius=UDim.new(1,0)

local CrossV=Instance.new("Frame")
CrossV.Size=UDim2.new(0,2,0,20)
CrossV.Position=UDim2.new(0.5,-1,0.5,-30)
CrossV.BackgroundColor3=Color3.fromRGB(255,0,0)
CrossV.BorderSizePixel=0
CrossV.Visible=false
CrossV.Parent=G

local CrossV2=CrossV:Clone()
CrossV2.Position=UDim2.new(0.5,-1,0.5,10)
CrossV2.Parent=G

local CrossH=Instance.new("Frame")
CrossH.Size=UDim2.new(0,20,0,2)
CrossH.Position=UDim2.new(0.5,-30,0.5,-1)
CrossH.BackgroundColor3=Color3.fromRGB(255,0,0)
CrossH.BorderSizePixel=0
CrossH.Visible=false
CrossH.Parent=G

local CrossH2=CrossH:Clone()
CrossH2.Position=UDim2.new(0.5,10,0.5,-1)
CrossH2.Parent=G
local GreenLine=Instance.new("Frame")
GreenLine.BackgroundColor3=Color3.fromRGB(0,255,0)
GreenLine.BorderSizePixel=0
GreenLine.Visible=false
GreenLine.ZIndex=5
GreenLine.Parent=G

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

local function ApplyJump()
    local h=GetHum()
    if h then
        h.UseJumpPower=true
        h.JumpPower=CF.jump and 120 or 50
    end
end

local function ApplyNoFall()
    local c=LP.Character
    if not c then return end
    local h=c:FindFirstChildOfClass("Humanoid")
    if h and CF.noFall then
        h:SetStateEnabled(Enum.HumanoidStateType.FallingDown,false)
    end
end

local function ApplyGlobalSpeed()
    if not CF.globalSpd then return end
    for _,p in pairs(Players:GetPlayers()) do
        if p.Character then
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if h then h.WalkSpeed=100 end
        end
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
local lockedTarget=nil

local function GetRingTarget()
    local cam=workspace.CurrentCamera
    if not cam then return nil end
    local vs=cam.ViewportSize
    local cx,cy=vs.X/2,vs.Y/2
    local best,bestD=nil,999
    for _,p in pairs(Players:GetPlayers()) do
        if p~=LP and p.Character then
            local rr=p.Character:FindFirstChild("HumanoidRootPart")
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if rr and h and h.Health>0 then
                local sp,on=cam:WorldToViewportPoint(rr.Position)
                if on then
                    local dx,dy=sp.X-cx,sp.Y-cy
                    local dist=math.sqrt(dx*dx+dy*dy)
                    if dist<100 and dist<bestD then
                        bestD=dist
                        best=rr
                    end
                end
            end
        end
    end
    return best
end

local WallCfg={enabled=false,lockedY=nil}
local function ApplyWall()
    local c=LP.Character
    if not c then return end
    local root=c:FindFirstChild("HumanoidRootPart")
    if not root then return end
    if WallCfg.enabled then
        for _,v in pairs(c:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide=false end
        end
        if WallCfg.lockedY then
            local pos=root.Position
            if math.abs(pos.Y-WallCfg.lockedY)>0.5 then
                root.CFrame=CFrame.new(pos.X,WallCfg.lockedY,pos.Z)
            end
        end
    else
        for _,v in pairs(c:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide=true end
        end
    end
end

local function Track()
    if not CF.bt or not lockedTarget then return end
    for _,v in pairs(workspace:GetDescendants()) do
        if v:IsA("BasePart") and (v.Name:lower():find("bullet") or v.Name:lower():find("projectile")) then
            local creator=v:FindFirstChild("Creator")
            local isMine=false
            if creator and creator.Value==LP then isMine=true end
            if not isMine then
                if (v.Position-lockedTarget.Position).Magnitude<350 then
                    local dir=(lockedTarget.Position-v.Position).Unit
                    v.Velocity=dir*250
                    v.CFrame=CFrame.new(v.Position,lockedTarget.Position)
                end
            end
        end
    end
end
local function UpdateGreenLine(target)
    local cam=workspace.CurrentCamera
    if not cam or not target then GreenLine.Visible=false return end
    local sp,on=cam:WorldToViewportPoint(target.Position)
    if not on then GreenLine.Visible=false return end
    local vs=cam.ViewportSize
    local cx,cy=vs.X/2,vs.Y/2
    local dx,dy=sp.X-cx,sp.Y-cy
    local len=math.sqrt(dx*dx+dy*dy)
    local ang=math.atan2(dy,dx)
    GreenLine.Size=UDim2.new(0,len,0,2)
    GreenLine.Position=UDim2.new(0,cx,0,cy-1)
    GreenLine.Rotation=math.deg(ang)
    GreenLine.Visible=true
end

KeyBtn.MouseButton1Click:Connect(function()
    local key=KeyBox.Text
    local ok=false
    if isWar then
        if key=="XDD-AX39F-F64XL-8MKHX-62S75" then ok=true end
    else
        if #key>0 then ok=true end
    end
    if ok then Main.Visible=false Panel.Visible=true end
end)

B1.MouseButton1Click:Connect(function() CF.aim=not CF.aim B1.BackgroundColor3=CF.aim and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180) end)
B2.MouseButton1Click:Connect(function() CF.esp=not CF.esp B2.BackgroundColor3=CF.esp and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180) end)
B3.MouseButton1Click:Connect(function() SpeedCfg.enabled=not SpeedCfg.enabled B3.BackgroundColor3=SpeedCfg.enabled and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180) ApplySpeed() end)
B4.MouseButton1Click:Connect(function()
    WallCfg.enabled=not WallCfg.enabled
    B4.BackgroundColor3=WallCfg.enabled and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    if WallCfg.enabled then
        local r=LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
        if r then WallCfg.lockedY=r.Position.Y end
    else WallCfg.lockedY=nil end
    ApplyWall()
end)
B5.MouseButton1Click:Connect(function()
    CF.bt=not CF.bt
    B5.BackgroundColor3=CF.bt and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    AimRing.Visible=CF.bt
    CrossV.Visible=CF.bt CrossV2.Visible=CF.bt CrossH.Visible=CF.bt CrossH2.Visible=CF.bt
    if not CF.bt then GreenLine.Visible=false lockedTarget=nil end
end)
B6.MouseButton1Click:Connect(function()
    CF.jump=not CF.jump
    B6.BackgroundColor3=CF.jump and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    ApplyJump()
end)
B7.MouseButton1Click:Connect(function()
    CF.noFall=not CF.noFall
    B7.BackgroundColor3=CF.noFall and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    ApplyNoFall()
end)
B8.MouseButton1Click:Connect(function()
    CF.globalSpd=not CF.globalSpd
    B8.BackgroundColor3=CF.globalSpd and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
end)
SubBtn.MouseButton1Click:Connect(function() SpeedCfg.value=math.max(SpeedCfg.min,SpeedCfg.value-SpeedCfg.step) SpeedLabel.Text="速度: "..SpeedCfg.value ApplySpeed() end)
AddBtn.MouseButton1Click:Connect(function() SpeedCfg.value=math.min(SpeedCfg.max,SpeedCfg.value+SpeedCfg.step) SpeedLabel.Text="速度: "..SpeedCfg.value ApplySpeed() end)
local Ball=Instance.new("TextButton")
Ball.Size=UDim2.new(0,44,0,44)
Ball.Position=UDim2.new(1,-60,1,-60)
Ball.BackgroundColor3=Color3.fromRGB(255,182,193)
Ball.Text="樱"
Ball.TextColor3=Color3.fromRGB(255,255,255)
Ball.Font=Enum.Font.GothamBold
Ball.TextSize=16
Ball.Visible=false
Ball.Parent=G
Instance.new("UICorner",Ball).CornerRadius=UDim.new(1,0)
HideP.MouseButton1Click:Connect(function() Panel.Visible=false Ball.Visible=true end)
Ball.MouseButton1Click:Connect(function() Panel.Visible=true Ball.Visible=false end)

local espList={}
RunService.RenderStepped:Connect(function()
    local cam=workspace.CurrentCamera
    if not cam then return end
    if CF.aim then
        local t=GetTarget()
        if t then
            local sp,on=cam:WorldToViewportPoint(t.Position)
            if on then
                local vs=cam.ViewportSize
                local dx=(sp.X-vs.X/2)*0.3
                local dy=(sp.Y-vs.Y/2)*0.3
                dx=math.clamp(dx,-30,30) dy=math.clamp(dy,-30,30)
                UIS:SetMouseDelta(Vector2.new(dx,dy))
            end
        end
    end
    if CF.esp then
        for _,p in pairs(Players:GetPlayers()) do
            if p~=LP and p.Character then
                local h=p.Character:FindFirstChildOfClass("Humanoid")
                if h and h.Health>0 then
                    local has=false
                    for _,v in pairs(espList) do if v.Adornee==p.Character then has=true break end end
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
    if SpeedCfg.enabled then ApplySpeed() end
    if CF.jump then ApplyJump() end
    if CF.noFall then ApplyNoFall() end
    if CF.globalSpd then ApplyGlobalSpeed() end
    if WallCfg.enabled then ApplyWall() end
    if CF.bt then
        local rt=GetRingTarget()
        if rt then lockedTarget=rt UpdateGreenLine(rt) else lockedTarget=nil GreenLine.Visible=false end
        Track()
    end
end)

print("樱の辅助 加载完成")
