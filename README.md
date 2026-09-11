local Players=game:GetService("Players")
local RunService=game:GetService("RunService")
local LP=Players.LocalPlayer
local UIS=game:GetService("UserInputService")
local CG=game:GetService("CoreGui")

local Tip=Instance.new("Frame")
Tip.Size=UDim2.new(0,420,0,35)
Tip.Position=UDim2.new(0.5,-210,1,-55)
Tip.BackgroundColor3=Color3.fromRGB(255,182,193)
Tip.BackgroundTransparency=0.1
Tip.Parent=CG
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
task.spawn(function() wait(3) Tip:Destroy() end)

local BG_ID="rbxassetid://131248212024332"
local G=Instance.new("ScreenGui")
G.ResetOnSpawn=false
G.Parent=CG
local GameEnv={speedMethod="WalkSpeed",hasAC=false}
task.spawn(function()
    task.wait(1)
    pcall(function()
        local h=LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
        if h then
            local old=h.WalkSpeed
            pcall(function() h.WalkSpeed=100 end)
            task.wait(0.1)
            if math.abs(h.WalkSpeed-100)>5 then GameEnv.speedMethod="Hook" end
            pcall(function() h.WalkSpeed=old end)
        end
        for _,v in pairs(game:GetDescendants()) do
            if v:IsA("Script")or v:IsA("LocalScript")then
                local n=v.Name:lower()
                if n:find("anticheat")or n:find("detect")then GameEnv.hasAC=true break end
            end
        end
        print("[环境] 加速:"..GameEnv.speedMethod.." 反作弊:"..tostring(GameEnv.hasAC))
    end)
end)

local kickLog={}
pcall(function()
    LP.Kick=function(self,msg)
        table.insert(kickLog,{time=os.time(),msg=tostring(msg)})
        warn("[防踢] 拦截: "..tostring(msg))
        return nil
    end
end)
pcall(function()
    for _,v in pairs(game:GetService("ReplicatedStorage"):GetDescendants()) do
        if v:IsA("RemoteEvent")then
            local n=v.Name:lower()
            if n:find("kick")or n:find("ban")then
                pcall(function() v.OnClientEvent=function() end end)
            end
        end
    end
end)
task.spawn(function()
    while true do
        task.wait(3)
        pcall(function()
            for _,v in pairs(workspace:GetDescendants())do
                if v:IsA("BoolValue")or v:IsA("StringValue")then
                    local n=v.Name:lower()
                    if n:find("kick")or n:find("ban")or n:find("flag")then v:Destroy() end
                end
            end
        end)
    end
end)
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
Panel.Size=UDim2.new(0,280,0,320)
Panel.Position=UDim2.new(0.5,-140,0.5,-160)
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

local CF={aim=false,esp=false,spd=false,wall=false,bt=false,jump=false,noFall=false}
local SpeedCfg={enabled=false,value=50,min=16,max=200,step=10}
local AimCfg={range=250,min=50,max=800,step=50,useRange=true}
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
local B5=Btn("物品追踪",UDim2.new(0,10,0,96))
local B6=Btn("高跳",UDim2.new(0,105,0,96))
local B7=Btn("坠落无伤",UDim2.new(0,10,0,126))
local B8=Btn("范围:开",UDim2.new(0,105,0,126))

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
local RangePanel=Instance.new("Frame")
RangePanel.Size=UDim2.new(0,240,0,26)
RangePanel.Position=UDim2.new(0,15,0,186)
RangePanel.BackgroundTransparency=1
RangePanel.Parent=Panel

local RgSub=Instance.new("TextButton")
RgSub.Size=UDim2.new(0,28,0,22)
RgSub.Position=UDim2.new(0,0,0,2)
RgSub.BackgroundColor3=Color3.fromRGB(255,182,193)
RgSub.Text="-"
RgSub.TextColor3=Color3.fromRGB(255,255,255)
RgSub.Font=Enum.Font.GothamBold
RgSub.TextSize=14
Instance.new("UICorner",RgSub).CornerRadius=UDim.new(0,6)
RgSub.Parent=RangePanel

local RangeLabel=Instance.new("TextLabel")
RangeLabel.Size=UDim2.new(0,140,0,22)
RangeLabel.Position=UDim2.new(0,32,0,2)
RangeLabel.BackgroundColor3=Color3.fromRGB(255,255,255)
RangeLabel.BackgroundTransparency=0.2
RangeLabel.Text="范围: 250"
RangeLabel.TextColor3=Color3.fromRGB(255,105,180)
RangeLabel.Font=Enum.Font.Gotham
RangeLabel.TextSize=11
Instance.new("UICorner",RangeLabel).CornerRadius=UDim.new(0,6)
RangeLabel.Parent=RangePanel

local RgAdd=Instance.new("TextButton")
RgAdd.Size=UDim2.new(0,28,0,22)
RgAdd.Position=UDim2.new(0,176,0,2)
RgAdd.BackgroundColor3=Color3.fromRGB(221,160,221)
RgAdd.Text="+"
RgAdd.TextColor3=Color3.fromRGB(255,255,255)
RgAdd.Font=Enum.Font.GothamBold
RgAdd.TextSize=14
Instance.new("UICorner",RgAdd).CornerRadius=UDim.new(0,6)
RgAdd.Parent=RangePanel

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

local function GetRoot()
    local c=LP.Character
    return c and c:FindFirstChild("HumanoidRootPart")
end

local function GetTarget()
    local r=GetRoot()
    if not r then return nil end
    local maxDist=AimCfg.useRange and AimCfg.range or 99999
    local t,d=nil,maxDist
    for _,p in pairs(Players:GetPlayers())do
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
    local maxDist=AimCfg.useRange and AimCfg.range or 99999
    local best,bestD=nil,maxDist
    local r=GetRoot()
    if not r then return nil end
    for _,p in pairs(Players:GetPlayers())do
        if p~=LP and p.Character then
            local rr=p.Character:FindFirstChild("HumanoidRootPart")
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if rr and h and h.Health>0 then
                local d3=(r.Position-rr.Position).Magnitude
                if d3<maxDist then
                    local sp,on=cam:WorldToViewportPoint(rr.Position)
                    if on then
                        local dx,dy=sp.X-cx,sp.Y-cy
                        local dist=math.sqrt(dx*dx+dy*dy)
                        if dist<100 and dist<bestD then bestD=dist best=rr end
                    end
                end
            end
        end
    end
    return best
end
local speedMethod=0
local speedHooked=nil

local function SpeedM1()
    local h=GetHum()
    if not h then return false end
    pcall(function() h.WalkSpeed=SpeedCfg.value end)
    task.wait(0.05)
    local h2=GetHum()
    return h2 and math.abs(h2.WalkSpeed-SpeedCfg.value)<2
end

local function SpeedM2()
    local h=GetHum()
    if not h then return false end
    local ok=false
    pcall(function()
        if speedHooked~=h then
            speedHooked=h
            h:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
                if SpeedCfg.enabled and math.abs(h.WalkSpeed-SpeedCfg.value)>0.5 then
                    h.WalkSpeed=SpeedCfg.value
                end
            end)
        end
        h.WalkSpeed=SpeedCfg.value
        ok=true
    end)
    task.wait(0.1)
    return ok
end

local function ApplySpeed()
    if not SpeedCfg.enabled then
        speedMethod=0
        local h=GetHum()
        if h then pcall(function() h.WalkSpeed=16 end) end
        return
    end
    if GameEnv.speedMethod=="Hook" then
        if speedMethod==0 then speedMethod=2 end
    else
        if speedMethod==0 then speedMethod=1 end
    end
    if speedMethod==1 then SpeedM1() return end
    if speedMethod==2 then SpeedM2() return end
    if SpeedM1() then speedMethod=1 print("[加速] 方法1") return end
    if SpeedM2() then speedMethod=2 print("[加速] 方法2") return end
end
local WallCfg={enabled=false,lockedY=nil,method=0}
local wallBP=nil

local function WallM1()
    local c=LP.Character
    if not c then return false end
    pcall(function()
        for _,v in pairs(c:GetDescendants())do
            if v:IsA("BasePart")then v.CanCollide=false end
        end
    end)
    task.wait(0.05)
    local r=c:FindFirstChild("HumanoidRootPart")
    return r and r.CanCollide==false
end

local function WallM2()
    local c=LP.Character
    if not c then return false end
    local ok=false
    pcall(function()
        local PS=game:GetService("PhysicsService")
        for _,v in pairs(c:GetDescendants())do
            if v:IsA("BasePart")then
                pcall(function() PS:SetPartCollisionGroup(v,"NoCollide") end)
            end
        end
        ok=true
    end)
    return ok
end

local function WallM3()
    local r=GetRoot()
    if not r then return false end
    pcall(function()
        if not wallBP then
            wallBP=Instance.new("BodyPosition")
            wallBP.Name="_wallBP"
            wallBP.MaxForce=Vector3.new(0,1e5,0)
            wallBP.Position=r.Position+Vector3.new(0,2,0)
            wallBP.Parent=r
        end
    end)
    return wallBP~=nil
end

local function ApplyWall()
    if not WallCfg.enabled then
        WallCfg.method=0
        if wallBP then wallBP:Destroy() wallBP=nil end
        local c=LP.Character
        if c then
            pcall(function()
                for _,v in pairs(c:GetDescendants())do
                    if v:IsA("BasePart")then v.CanCollide=true end
                end
            end)
        end
        return
    end
    if WallCfg.method==1 then WallM1() return end
    if WallCfg.method==2 then WallM2() return end
    if WallCfg.method==3 then WallM3() return end
    if WallM1() then WallCfg.method=1 print("[穿墙] 方法1") return end
    if WallM2() then WallCfg.method=2 print("[穿墙] 方法2") return end
    if WallM3() then WallCfg.method=3 print("[穿墙] 方法3") return end
end
local jumpMethod=0

local function JumpM1()
    local h=GetHum()
    if not h then return false end
    pcall(function() h.UseJumpPower=true h.JumpPower=120 end)
    task.wait(0.05)
    return h and h.JumpPower and h.JumpPower>=100
end

local function JumpM2()
    local h=GetHum()
    if not h then return false end
    pcall(function() h.UseJumpPower=false h.JumpHeight=30 end)
    task.wait(0.05)
    return h and h.JumpHeight and h.JumpHeight>=20
end

local function ApplyJump()
    if not CF.jump then
        jumpMethod=0
        local h=GetHum()
        if h then pcall(function() h.UseJumpPower=true h.JumpPower=50 end) end
        return
    end
    if jumpMethod==1 then JumpM1() return end
    if jumpMethod==2 then JumpM2() return end
    if JumpM1() then jumpMethod=1 print("[高跳] 方法1") return end
    if JumpM2() then jumpMethod=2 print("[高跳] 方法2") return end
end

local noFallMethod=0

local function NoFallM1()
    local h=GetHum()
    if not h then return false end
    pcall(function()
        h:SetStateEnabled(Enum.HumanoidStateType.FallingDown,false)
        h:SetStateEnabled(Enum.HumanoidStateType.Landed,false)
        if h.Health<h.MaxHealth then h.Health=h.MaxHealth end
    end)
    return true
end

local function NoFallM2()
    local h=GetHum()
    if not h then return false end
    pcall(function() if h.Health<h.MaxHealth then h.Health=h.MaxHealth end end)
    return h.Health==h.MaxHealth
end

local function ApplyNoFall()
    if not CF.noFall then noFallMethod=0 return end
    if noFallMethod==1 then NoFallM1() return end
    if noFallMethod==2 then NoFallM2() return end
    if NoFallM1() then noFallMethod=1 print("[坠落] 方法1") return end
    if NoFallM2() then noFallMethod=2 print("[坠落] 方法2") return end
end
local espList={}
local espMethod=0

local function ClearESP()
    for _,v in pairs(espList)do pcall(function() v:Destroy() end) end
    espList={}
end

local function EspM1()
    for _,p in pairs(Players:GetPlayers())do
        if p~=LP and p.Character then
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if h and h.Health>0 then
                local has=false
                for _,v in pairs(espList)do if v.Adornee==p.Character then has=true break end end
                if not has then
                    pcall(function()
                        local hl=Instance.new("Highlight")
                        hl.FillColor=Color3.fromRGB(255,182,193)
                        hl.FillTransparency=0.5
                        hl.Adornee=p.Character
                        hl.Parent=p.Character
                        table.insert(espList,hl)
                    end)
                end
            end
        end
    end
    return #espList>0
end

local function EspM2()
    for _,p in pairs(Players:GetPlayers())do
        if p~=LP and p.Character then
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if h and h.Health>0 then
                local has=false
                for _,v in pairs(espList)do if v.Adornee==p.Character then has=true break end end
                if not has then
                    pcall(function()
                        local sb=Instance.new("SelectionBox")
                        sb.Color3=Color3.fromRGB(255,105,180)
                        sb.Adornee=p.Character
                        sb.Parent=p.Character
                        table.insert(espList,sb)
                    end)
                end
            end
        end
    end
    return #espList>0
end

local function ApplyESP()
    if not CF.esp then espMethod=0 ClearESP() return end
    if espMethod==1 then EspM1() return end
    if espMethod==2 then EspM2() return end
    if EspM1() then espMethod=1 print("[透视] 方法1") return end
    if EspM2() then espMethod=2 print("[透视] 方法2") return end
end
local aimMethod=0

local function AimM1(target)
    local cam=workspace.CurrentCamera
    if not cam then return false end
    local ok=false
    pcall(function()
        cam.CFrame=CFrame.new(cam.CFrame.Position,target.Position)
        ok=true
    end)
    return ok
end

local function AimM2(target)
    local cam=workspace.CurrentCamera
    if not cam or not UIS.SetMouseDelta then return false end
    local ok=false
    pcall(function()
        local sp,on=cam:WorldToViewportPoint(target.Position)
        if on then
            local vs=cam.ViewportSize
            local dx=(sp.X-vs.X/2)*0.3
            local dy=(sp.Y-vs.Y/2)*0.3
            UIS:SetMouseDelta(Vector2.new(math.clamp(dx,-30,30),math.clamp(dy,-30,30)))
            ok=true
        end
    end)
    return ok
end

local function AimM3(target)
    local r=GetRoot()
    if not r then return false end
    local ok=false
    pcall(function()
        r.CFrame=CFrame.new(r.Position,Vector3.new(target.Position.X,r.Position.Y,target.Position.Z))
        ok=true
    end)
    return ok
end

local function ApplyAim()
    if not CF.aim then aimMethod=0 return end
    local target=GetTarget()
    if not target then return end
    if aimMethod==1 then AimM1(target) return end
    if aimMethod==2 then AimM2(target) return end
    if aimMethod==3 then AimM3(target) return end
    if AimM1(target) then aimMethod=1 print("[自瞄] 方法1") return end
    if AimM2(target) then aimMethod=2 print("[自瞄] 方法2") return end
    if AimM3(target) then aimMethod=3 print("[自瞄] 方法3") return end
end
local headList={}

local function IsMine(obj)
    local creator=obj:FindFirstChild("Creator")
    if creator and creator.Value==LP then return true end
    local owner=obj:FindFirstChild("Owner")
    if owner and owner.Value==LP then return true end
    return false
end

local function IsTrackable(v)
    if not v:IsA("BasePart")then return false end
    local n=v.Name:lower()
    if n:find("bullet")or n:find("projectile")or n:find("missile")then return true end
    if n:find("item")or n:find("drop")or n:find("pickup")or n:find("loot")then return true end
    if n:find("coin")or n:find("gem")or n:find("cash")or n:find("money")then return true end
    if n:find("resource")or n:find("ore")or n:find("wood")or n:find("stone")then return true end
    return false
end

local function Track()
    if not CF.bt or not lockedTarget then return end
    for _,v in pairs(workspace:GetDescendants())do
        if IsTrackable(v) and not IsMine(v) then
            if (v.Position-lockedTarget.Position).Magnitude<350 then
                local dir=(lockedTarget.Position-v.Position).Unit
                pcall(function()
                    v.Velocity=dir*250
                    v.CFrame=CFrame.new(v.Position,lockedTarget.Position)
                end)
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
    GreenLine.Size=UDim2.new(0,math.sqrt(dx*dx+dy*dy),0,2)
    GreenLine.Position=UDim2.new(0,cx,0,cy-1)
    GreenLine.Rotation=math.deg(math.atan2(dy,dx))
    GreenLine.Visible=true
end
local function UpdateHeadDisplay()
    for i=#headList,1,-1 do
        local item=headList[i]
        if item.bg and item.bg.Parent then
            local hum=item.char and item.char:FindFirstChildOfClass("Humanoid")
            if not hum or hum.Health<=0 then
                item.bg:Destroy()
                table.remove(headList,i)
            end
        else
            table.remove(headList,i)
        end
    end
    for _,p in pairs(Players:GetPlayers())do
        if p~=LP and p.Character then
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if h and h.Health>0 then
                local head=p.Character:FindFirstChild("Head")
                if head then
                    local has=false
                    for _,item in pairs(headList)do
                        if item.char==p.Character then has=true break end
                    end
                    if not has then
                        local bg=Instance.new("BillboardGui")
                        bg.Name="_headDisplay"
                        bg.Size=UDim2.new(0,200,0,30)
                        bg.Adornee=head
                        bg.StudsOffsetWorldSpace=Vector3.new(0,2.5,0)
                        bg.AlwaysOnTop=true
                        bg.LightInfluence=0
                        bg.MaxDistance=500
                        bg.Parent=head
                        local tl=Instance.new("TextLabel")
                        tl.Size=UDim2.new(1,0,1,0)
                        tl.BackgroundTransparency=1
                        tl.TextColor3=Color3.fromRGB(0,255,255)
                        tl.Font=Enum.Font.GothamBold
                        tl.TextSize=12
                        tl.TextStrokeTransparency=0
                        tl.TextStrokeColor3=Color3.fromRGB(0,0,0)
                        tl.Text=p.Name.." [0m]"
                        tl.Parent=bg
                        table.insert(headList,{char=p.Character,bg=bg,label=tl})
                    end
                end
            end
        end
    end
    local r=GetRoot()
    if not r then return end
    for _,item in pairs(headList)do
        if item.label and item.char then
            local hrp=item.char:FindFirstChild("HumanoidRootPart")
            if hrp then
                local dist=(r.Position-hrp.Position).Magnitude
                item.label.Text=item.char.Name.." ["..tostring(math.floor(dist)).."m]"
            end
        end
    end
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
B3.MouseButton1Click:Connect(function() SpeedCfg.enabled=not SpeedCfg.enabled B3.BackgroundColor3=SpeedCfg.enabled and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180) end)
B4.MouseButton1Click:Connect(function()
    WallCfg.enabled=not WallCfg.enabled
    B4.BackgroundColor3=WallCfg.enabled and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    if WallCfg.enabled then local r=GetRoot() if r then WallCfg.lockedY=r.Position.Y end else WallCfg.lockedY=nil end
end)
B5.MouseButton1Click:Connect(function()
    CF.bt=not CF.bt
    B5.BackgroundColor3=CF.bt and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    AimRing.Visible=CF.bt
    if not CF.bt then GreenLine.Visible=false lockedTarget=nil end
end)
B6.MouseButton1Click:Connect(function() CF.jump=not CF.jump B6.BackgroundColor3=CF.jump and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180) end)
B7.MouseButton1Click:Connect(function() CF.noFall=not CF.noFall B7.BackgroundColor3=CF.noFall and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180) end)
B8.MouseButton1Click:Connect(function()
    AimCfg.useRange=not AimCfg.useRange
    B8.BackgroundColor3=AimCfg.useRange and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    B8.Text=AimCfg.useRange and "范围:开" or "范围:关"
end)
SubBtn.MouseButton1Click:Connect(function() SpeedCfg.value=math.max(SpeedCfg.min,SpeedCfg.value-SpeedCfg.step) SpeedLabel.Text="速度: "..SpeedCfg.value end)
AddBtn.MouseButton1Click:Connect(function() SpeedCfg.value=math.min(SpeedCfg.max,SpeedCfg.value+SpeedCfg.step) SpeedLabel.Text="速度: "..SpeedCfg.value end)
RgSub.MouseButton1Click:Connect(function() AimCfg.range=math.max(AimCfg.min,AimCfg.range-AimCfg.step) RangeLabel.Text="范围: "..AimCfg.range end)
RgAdd.MouseButton1Click:Connect(function() AimCfg.range=math.min(AimCfg.max,AimCfg.range+AimCfg.step) RangeLabel.Text="范围: "..AimCfg.range end)
local Ball=Instance.new("TextButton")
Ball.Size=UDim2.new(0,44,0,44)
Ball.Position=UDim2.new(1,-60,1,-60)
Ball.BackgroundColor3=Color3.fromRGB(255,182,193)
Ball.Text="🥵快射🥵"
Ball.TextColor3=Color3.fromRGB(255,255,255)
Ball.Font=Enum.Font.GothamBold
Ball.TextSize=16
Ball.Visible=false
Ball.Parent=G
Instance.new("UICorner",Ball).CornerRadius=UDim.new(1,0)

local bds,bdp
Ball.InputBegan:Connect(function(i)
    if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then
        bds=i.Position bdp=Ball.Position
    end
end)
Ball.InputChanged:Connect(function(i)
    if (i.UserInputType==Enum.UserInputType.MouseMovement or i.UserInputType==Enum.UserInputType.Touch) and bds then
        local d=i.Position-bds
        Ball.Position=UDim2.new(bdp.X.Scale,bdp.X.Offset+d.X,bdp.Y.Scale,bdp.Y.Offset+d.Y)
    end
end)
Ball.InputEnded:Connect(function(i)
    if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then bds=nil end
end)
HideP.MouseButton1Click:Connect(function() Panel.Visible=false Ball.Visible=true end)
Ball.MouseButton1Click:Connect(function() Panel.Visible=true Ball.Visible=false end)
RunService.RenderStepped:Connect(function()
    local cam=workspace.CurrentCamera
    if not cam then return end
    if CF.aim then ApplyAim() end
    if CF.esp then ApplyESP() end
    if SpeedCfg.enabled then ApplySpeed() end
    if CF.jump then ApplyJump() end
    if CF.noFall then ApplyNoFall() end
    if WallCfg.enabled then
        ApplyWall()
        if WallCfg.lockedY then
            local r=GetRoot()
            if r then
                local pos=r.Position
                if math.abs(pos.Y-WallCfg.lockedY)>0.5 then
                    pcall(function() r.CFrame=CFrame.new(pos.X,WallCfg.lockedY,pos.Z) end)
                end
            end
        end
    end
    if CF.bt then
        local rt=GetRingTarget()
        if rt then lockedTarget=rt UpdateGreenLine(rt) else lockedTarget=nil GreenLine.Visible=false end
        Track()
    end
    UpdateHeadDisplay()
end)

print("樱の辅助 V10 加载完成 - 全功能多重验证")
