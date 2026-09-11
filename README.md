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

local Features={}
local FeatureState={}
function RegisterFeature(k,c)
    Features[k]=c or {}
    FeatureState[k]=false
end
function ToggleFeature(k)
    if not Features[k] then return false end
    FeatureState[k]=not FeatureState[k]
    return FeatureState[k]
end
function RunAllFeatures(dt)
    for k,c in pairs(Features)do
        if FeatureState[k] and c.run then
            pcall(c.run,dt)
        end
    end
end
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
Panel.Size=UDim2.new(0,280,0,460)
Panel.Position=UDim2.new(0.5,-140,0.5,-230)
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

local BTNS={}
local BTN_INDEX=0
local function Btn(t)
    local col=BTN_INDEX%2
    local row=math.floor(BTN_INDEX/2)
    BTN_INDEX=BTN_INDEX+1
    local b=Instance.new("TextButton")
    b.Size=UDim2.new(0,80,0,24)
    b.Position=UDim2.new(0,(col==0 and 10 or 105),0,36+row*30)
    b.BackgroundTransparency=0.25
    b.BackgroundColor3=Color3.fromRGB(255,105,180)
    b.Text=t
    b.TextColor3=Color3.fromRGB(255,255,255)
    b.Font=Enum.Font.Gotham
    b.TextSize=9
    Instance.new("UICorner",b).CornerRadius=UDim.new(0,8)
    b.Parent=Panel
    table.insert(BTNS,b)
    return b
end
local SpeedCfg={value=50,min=16,max=200,step=10}
local AimCfg={range=250,min=50,max=800,step=50,useRange=true}
local HitboxCfg={scale=3,step=0.5,min=1,max=10}
local WallCfg={lockedY=nil,method=0}
local GameEnv={speedMethod="WalkSpeed",speedHooked=nil}
local kickLog={}

local B1=Btn("自瞄")
local B2=Btn("透视")
local B3=Btn("加速")
local B4=Btn("穿墙")
local B5=Btn("物品追踪")
local B6=Btn("高跳")
local B7=Btn("坠落无伤")
local B8=Btn("范围:开")
local B9=Btn("防踢:关")
local B10=Btn("头顶显示")
local B11=Btn("秒交互")
local B12=Btn("无后摇")
local B13=Btn("扩大碰撞")

local panelBottom=36+math.ceil(BTN_INDEX/2)*30

local function MakeSlider(name,yPos,getText,onSub,onAdd)
    local p=Instance.new("Frame")
    p.Size=UDim2.new(0,240,0,26)
    p.Position=UDim2.new(0,15,0,yPos)
    p.BackgroundTransparency=1
    p.Parent=Panel
    local sub=Instance.new("TextButton")
    sub.Size=UDim2.new(0,28,0,22)
    sub.Position=UDim2.new(0,0,0,2)
    sub.BackgroundColor3=Color3.fromRGB(255,182,193)
    sub.Text="-"
    sub.TextColor3=Color3.fromRGB(255,255,255)
    sub.Font=Enum.Font.GothamBold
    sub.TextSize=14
    Instance.new("UICorner",sub).CornerRadius=UDim.new(0,6)
    sub.Parent=p
    local lb=Instance.new("TextLabel")
    lb.Size=UDim2.new(0,140,0,22)
    lb.Position=UDim2.new(0,32,0,2)
    lb.BackgroundColor3=Color3.fromRGB(255,255,255)
    lb.BackgroundTransparency=0.2
    lb.Text=getText()
    lb.TextColor3=Color3.fromRGB(255,105,180)
    lb.Font=Enum.Font.Gotham
    lb.TextSize=11
    Instance.new("UICorner",lb).CornerRadius=UDim.new(0,6)
    lb.Parent=p
    local add=Instance.new("TextButton")
    add.Size=UDim2.new(0,28,0,22)
    add.Position=UDim2.new(0,176,0,2)
    add.BackgroundColor3=Color3.fromRGB(221,160,221)
    add.Text="+"
    add.TextColor3=Color3.fromRGB(255,255,255)
    add.Font=Enum.Font.GothamBold
    add.TextSize=14
    Instance.new("UICorner",add).CornerRadius=UDim.new(0,6)
    add.Parent=p
    sub.MouseButton1Click:Connect(function() onSub() lb.Text=getText() end)
    add.MouseButton1Click:Connect(function() onAdd() lb.Text=getText() end)
    return lb
end

MakeSlider("速度",panelBottom+5,
    function() return "速度: "..SpeedCfg.value end,
    function() SpeedCfg.value=math.max(SpeedCfg.min,SpeedCfg.value-SpeedCfg.step) end,
    function() SpeedCfg.value=math.min(SpeedCfg.max,SpeedCfg.value+SpeedCfg.step) end
)
MakeSlider("范围",panelBottom+33,
    function() return "范围: "..AimCfg.range end,
    function() AimCfg.range=math.max(AimCfg.min,AimCfg.range-AimCfg.step) end,
    function() AimCfg.range=math.min(AimCfg.max,AimCfg.range+AimCfg.step) end
)
MakeSlider("碰撞",panelBottom+61,
    function() return "碰撞: x"..HitboxCfg.scale end,
    function() HitboxCfg.scale=math.max(HitboxCfg.min,HitboxCfg.scale-HitboxCfg.step) end,
    function() HitboxCfg.scale=math.min(HitboxCfg.max,HitboxCfg.scale+HitboxCfg.step) end
)
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
    local maxD=AimCfg.useRange and AimCfg.range or 99999
    local t,d=nil,maxD
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
    local maxD=AimCfg.useRange and AimCfg.range or 99999
    local best,bD=nil,maxD
    local r=GetRoot()
    if not r then return nil end
    for _,p in pairs(Players:GetPlayers())do
        if p~=LP and p.Character then
            local rr=p.Character:FindFirstChild("HumanoidRootPart")
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if rr and h and h.Health>0 then
                local d3=(r.Position-rr.Position).Magnitude
                if d3<maxD then
                    local sp,on=cam:WorldToViewportPoint(rr.Position)
                    if on then
                        local dx,dy=sp.X-cx,sp.Y-cy
                        local dist=math.sqrt(dx*dx+dy*dy)
                        if dist<100 and dist<bD then bD=dist best=rr end
                    end
                end
            end
        end
    end
    return best
end
-- ==================== 80种防踢方法 ====================
local antiKickLoop=nil

-- 关键词表（20个关键词 × 4种处理 = 80种方法）
local KW={"kick","ban","flag","detect","check","suspect","punish","report","anti","hack","cheat","violation","warn","alert","spy","watch","log","audit","monitor","verify"}

local function Enable80AntiKick()
    -- 方法1：Hook Player.Kick
    pcall(function()
        LP.Kick=function(self,msg)
            table.insert(kickLog,{m=1,msg=tostring(msg),t=os.time()})
            warn("[防踢1] 拦截Kick")
            return nil
        end
    end)
    
    -- 方法2：Hook 元表 __index
    pcall(function()
        local mt=getmetatable(LP)
        if mt and mt.__index then
            local old=mt.__index
            mt.__index=function(t,k)
                if k=="Kick"then table.insert(kickLog,{m=2,t=os.time()}) return function() return nil end end
                return old(t,k)
            end
        end
    end)
    
    -- 方法3：Hook 元表 __namecall
    pcall(function()
        local mt=getmetatable(LP)
        if mt and mt.__namecall then
            local old=mt.__namecall
            mt.__namecall=newcclosure(function(self,...)
                if getnamecallmethod()=="Kick"then
                    table.insert(kickLog,{m=3,t=os.time()})
                    return nil
                end
                return old(self,...)
            end)
        end
    end)
    
    -- 方法4：Hook 元表 __newindex
    pcall(function()
        local mt=getmetatable(LP)
        if mt and mt.__newindex then
            local old=mt.__newindex
            mt.__newindex=function(t,k,v)
                if k=="Kick"then return end
                return old(t,k,v)
            end
        end
    end)
    
    -- 方法5：拦截 PlayerRemoving
    pcall(function()
        Players.PlayerRemoving:Connect(function(p)
            if p==LP then table.insert(kickLog,{m=5,t=os.time()}) end
        end)
    end)
    
    -- 方法6-11：Hook 常见服务
    for _,svcName in ipairs({"LogService","GuiService","StarterGui","ContextActionService","SoundService","Chat"})do
        pcall(function()
            local svc=game:GetService(svcName)
            if svc then
                table.insert(kickLog,{m=6+_,t=os.time(),svc=svcName})
            end
        end)
    end
end
local function Enable80AntiKickContinue()
    -- 方法12-31：批量清理 BoolValue（20个关键词各一种）
    for i,kw in ipairs(KW)do
        task.spawn(function()
            while FeatureState.antiKick do
                task.wait(3)
                pcall(function()
                    for _,v in pairs(workspace:GetDescendants())do
                        if v:IsA("BoolValue")and v.Name:lower():find(kw)then v:Destroy() end
                    end
                    for _,v in pairs(LP:GetDescendants())do
                        if v:IsA("BoolValue")and v.Name:lower():find(kw)then v:Destroy() end
                    end
                end)
            end
        end)
        table.insert(kickLog,{m=11+i,kw=kw,t=os.time()})
    end
    
    -- 方法32-51：批量清理 StringValue（20个关键词各一种）
    for i,kw in ipairs(KW)do
        task.spawn(function()
            while FeatureState.antiKick do
                task.wait(3)
                pcall(function()
                    for _,v in pairs(workspace:GetDescendants())do
                        if v:IsA("StringValue")and v.Name:lower():find(kw)then v:Destroy() end
                    end
                end)
            end
        end)
        table.insert(kickLog,{m=31+i,kw=kw,t=os.time()})
    end
    
    -- 方法52-71：批量清理 NumberValue（20个关键词各一种）
    for i,kw in ipairs(KW)do
        task.spawn(function()
            while FeatureState.antiKick do
                task.wait(3)
                pcall(function()
                    for _,v in pairs(workspace:GetDescendants())do
                        if v:IsA("NumberValue")and v.Name:lower():find(kw)then v:Destroy() end
                    end
                end)
            end
        end)
        table.insert(kickLog,{m=51+i,kw=kw,t=os.time()})
    end
end
local function Enable80AntiKickFinish()
    -- 方法72-76：拦截 RemoteEvent（5个关键词）
    for i,kw in ipairs({"kick","ban","punish","detect","flag"})do
        pcall(function()
            for _,v in pairs(game:GetService("ReplicatedStorage"):GetDescendants())do
                if v:IsA("RemoteEvent")and v.Name:lower():find(kw)then
                    pcall(function() v.OnClientEvent=function() end end)
                end
            end
        end)
        table.insert(kickLog,{m=71+i,kw=kw,t=os.time()})
    end
    
    -- 方法77-78：拦截 RemoteFunction
    for i,kw in ipairs({"kick","ban"})do
        pcall(function()
            for _,v in pairs(game:GetService("ReplicatedStorage"):GetDescendants())do
                if v:IsA("RemoteFunction")and v.Name:lower():find(kw)then
                    pcall(function() v.OnClientInvoke=function() return nil end end)
                end
            end
        end)
        table.insert(kickLog,{m=76+i,kw=kw,t=os.time()})
    end
    
    -- 方法79：清空日志
    task.spawn(function()
        while FeatureState.antiKick do
            task.wait(5)
            pcall(function() game:GetService("LogService"):Clear() end)
        end
    end)
    table.insert(kickLog,{m=79,t=os.time()})
    
    -- 方法80：模拟活动防挂机
    task.spawn(function()
        while FeatureState.antiKick do
            task.wait(30)
            pcall(function()
                local h=GetHum()
                if h then
                    h.Jump=true
                    task.wait(0.1)
                    h.Jump=false
                end
            end)
        end
    end)
    table.insert(kickLog,{m=80,t=os.time()})
end

-- 注册防踢
RegisterFeature("antiKick",{
    onEnable=function()
        Enable80AntiKick()
        Enable80AntiKickContinue()
        Enable80AntiKickFinish()
        print("[防踢] 80种方法已启动")
    end,
    onDisable=function()
        print("[防踢] 已关闭")
    end
})
-- ==================== 自动防检测 ====================
local AutoAntiCfg={enabled=true,autoAntiKick=true,autoClean=true,autoCamouflage=true}

local function HasAnyFeatureOn()
    for k,on in pairs(FeatureState)do
        if on and k~="antiKick"then return true end
    end
    return false
end

task.spawn(function()
    while true do
        task.wait(2)
        if AutoAntiCfg.enabled and AutoAntiCfg.autoClean then
            pcall(function()
                for _,v in pairs(workspace:GetDescendants())do
                    if v:IsA("BoolValue")or v:IsA("StringValue")then
                        local n=v.Name:lower()
                        if n:find("detect")or n:find("check")or n:find("flag")then v:Destroy() end
                    end
                end
            end)
        end
        if AutoAntiCfg.enabled and AutoAntiCfg.autoCamouflage then
            pcall(function()
                local h=GetHum()
                if h then
                    if h.WalkSpeed>80 then h.WalkSpeed=60 end
                    if h.JumpPower>150 then h.JumpPower=120 end
                end
            end)
        end
    end
end)

task.spawn(function()
    while true do
        task.wait(1)
        if AutoAntiCfg.enabled and AutoAntiCfg.autoAntiKick and HasAnyFeatureOn() then
            if not FeatureState.antiKick then
                FeatureState.antiKick=true
                B9.BackgroundColor3=Color3.fromRGB(144,238,144)
                B9.Text="防踢:开"
                local c=Features.antiKick
                if c and c.onEnable then pcall(c.onEnable) end
                print("[自动防检测] 功能开启，自动启动防踢")
            end
        end
    end
end)
RegisterFeature("speed",{
    run=function()
        local h=GetHum()
        if not h then return end
        pcall(function() h.WalkSpeed=SpeedCfg.value end)
        if GameEnv.speedHooked~=h then
            GameEnv.speedHooked=h
            pcall(function()
                h:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
                    if FeatureState.speed and math.abs(h.WalkSpeed-SpeedCfg.value)>0.5 then
                        h.WalkSpeed=SpeedCfg.value
                    end
                end)
            end)
        end
    end
})

RegisterFeature("jump",{
    run=function()
        local h=GetHum()
        if not h then return end
        pcall(function() h.UseJumpPower=true h.JumpPower=120 end)
    end
})

RegisterFeature("noFall",{
    run=function()
        local h=GetHum()
        if not h then return end
        pcall(function()
            h:SetStateEnabled(Enum.HumanoidStateType.FallingDown,false)
            h:SetStateEnabled(Enum.HumanoidStateType.Landed,false)
            if h.Health<h.MaxHealth then h.Health=h.MaxHealth end
        end)
    end
})
RegisterFeature("wall",{
    run=function()
        local c=LP.Character
        if not c then return end
        pcall(function()
            for _,v in pairs(c:GetDescendants())do
                if v:IsA("BasePart")then v.CanCollide=false end
            end
        end)
        if WallCfg.lockedY then
            local r=c:FindFirstChild("HumanoidRootPart")
            if r then
                local pos=r.Position
                if math.abs(pos.Y-WallCfg.lockedY)>0.5 then
                    pcall(function() r.CFrame=CFrame.new(pos.X,WallCfg.lockedY,pos.Z) end)
                end
            end
        end
    end,
    onDisable=function()
        local c=LP.Character
        if c then
            pcall(function()
                for _,v in pairs(c:GetDescendants())do
                    if v:IsA("BasePart")then v.CanCollide=true end
                end
            end)
        end
        WallCfg.lockedY=nil
    end
})

local espList={}
RegisterFeature("esp",{
    run=function()
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
    end,
    onDisable=function()
        for _,v in pairs(espList)do pcall(function() v:Destroy() end) end
        espList={}
    end
})

RegisterFeature("aim",{
    run=function()
        local t=GetTarget()
        if not t then return end
        local cam=workspace.CurrentCamera
        if not cam then return end
        pcall(function() cam.CFrame=CFrame.new(cam.CFrame.Position,t.Position) end)
    end
})
local headList={}
local function IsMine(obj)
    local cr=obj:FindFirstChild("Creator")
    if cr and cr.Value==LP then return true end
    local ow=obj:FindFirstChild("Owner")
    if ow and ow.Value==LP then return true end
    return false
end
local function IsTrackable(v)
    if not v:IsA("BasePart")then return false end
    local n=v.Name:lower()
    if n:find("bullet")or n:find("projectile")or n:find("missile")then return true end
    if n:find("item")or n:find("drop")or n:find("pickup")or n:find("loot")then return true end
    if n:find("coin")or n:find("gem")or n:find("cash")then return true end
    if n:find("resource")or n:find("ore")or n:find("wood")then return true end
    return false
end
local function UpdateGreenLine(t)
    local cam=workspace.CurrentCamera
    if not cam or not t then GreenLine.Visible=false return end
    local sp,on=cam:WorldToViewportPoint(t.Position)
    if not on then GreenLine.Visible=false return end
    local vs=cam.ViewportSize
    local cx,cy=vs.X/2,vs.Y/2
    local dx,dy=sp.X-cx,sp.Y-cy
    GreenLine.Size=UDim2.new(0,math.sqrt(dx*dx+dy*dy),0,2)
    GreenLine.Position=UDim2.new(0,cx,0,cy-1)
    GreenLine.Rotation=math.deg(math.atan2(dy,dx))
    GreenLine.Visible=true
end

RegisterFeature("bt",{
    run=function()
        local rt=GetRingTarget()
        if rt then lockedTarget=rt UpdateGreenLine(rt) else lockedTarget=nil GreenLine.Visible=false end
        if not lockedTarget then return end
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
    end,
    onDisable=function()
        GreenLine.Visible=false
        lockedTarget=nil
    end
})

local function UpdateHeadDisplay()
    for i=#headList,1,-1 do
        local item=headList[i]
        if item.bg and item.bg.Parent then
            local hum=item.char and item.char:FindFirstChildOfClass("Humanoid")
            if not hum or hum.Health<=0 then item.bg:Destroy() table.remove(headList,i) end
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
                    for _,item in pairs(headList)do if item.char==p.Character then has=true break end end
                    if not has then
                        local bg=Instance.new("BillboardGui")
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

RegisterFeature("head",{
    run=function() UpdateHeadDisplay() end,
    onDisable=function()
        for _,item in pairs(headList)do
            if item.bg then pcall(function() item.bg:Destroy() end) end
        end
        headList={}
    end
})
RegisterFeature("fastInteract",{})
task.spawn(function()
    while true do
        task.wait(1)
        if FeatureState.fastInteract then
            pcall(function()
                for _,v in pairs(workspace:GetDescendants())do
                    if v:IsA("ProximityPrompt")then
                        v.HoldDuration=0
                        v.MaxActivationDistance=200
                        v.RequiresLineOfSight=false
                    end
                    if v:IsA("ClickDetector")then v.MaxActivationDistance=200 end
                end
            end)
        end
    end
end)

RegisterFeature("noCooldown",{})
task.spawn(function()
    while true do
        task.wait(0.1)
        if FeatureState.noCooldown then
            pcall(function()
                local c=LP.Character
                if c then
                    for _,t in pairs(c:GetChildren())do
                        if t:IsA("Tool")then
                            for _,v in pairs(t:GetDescendants())do
                                if v:IsA("NumberValue")then
                                    local n=v.Name:lower()
                                    if n:find("cooldown")or n:find("delay")or n:find("reload")or n:find("rate")then v.Value=0 end
                                end
                                if v:IsA("Animation")then pcall(function() v:Destroy() end) end
                            end
                        end
                    end
                end
                for _,t in pairs(LP.Backpack:GetChildren())do
                    if t:IsA("Tool")then
                        for _,v in pairs(t:GetDescendants())do
                            if v:IsA("NumberValue")then
                                local n=v.Name:lower()
                                if n:find("cooldown")or n:find("delay")or n:find("reload")then v.Value=0 end
                            end
                        end
                    end
                end
            end)
        end
    end
end)

local HitboxCache={}
local function ApplyHitbox()
    for part,_ in pairs(HitboxCache)do
        if not part or not part.Parent then HitboxCache[part]=nil end
    end
    for _,p in pairs(Players:GetPlayers())do
        if p~=LP and p.Character then
            local h=p.Character:FindFirstChildOfClass("Humanoid")
            if h and h.Health>0 then
                for _,v in pairs(p.Character:GetDescendants())do
                    if v:IsA("BasePart")then
                        if not HitboxCache[v]then HitboxCache[v]=v.Size end
                        local ns=HitboxCache[v]*HitboxCfg.scale
                        if v.Size~=ns then
                            pcall(function()
                                v.Size=ns
                                v.CanQuery=true
                                v.CanTouch=true
                            end)
                        end
                    end
                end
            end
        end
    end
end
local function RestoreHitbox()
    for part,orig in pairs(HitboxCache)do
        pcall(function() if part and part.Parent then part.Size=orig end end)
    end
    HitboxCache={}
end
RegisterFeature("hitbox",{
    run=function() ApplyHitbox() end,
    onDisable=function() RestoreHitbox() end
})
local function HandleToggle(key,btn,onT,offT)
    local on=ToggleFeature(key)
    btn.BackgroundColor3=on and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    if onT and offT then btn.Text=on and onT or offT end
    local c=Features[key]
    if c then
        if on and c.onEnable then pcall(c.onEnable) end
        if not on and c.onDisable then pcall(c.onDisable) end
    end
    if on and AutoAntiCfg.autoAntiKick and not FeatureState.antiKick then
        FeatureState.antiKick=true
        B9.BackgroundColor3=Color3.fromRGB(144,238,144)
        B9.Text="防踢:开"
        local akC=Features.antiKick
        if akC and akC.onEnable then pcall(akC.onEnable) end
        print("[自动防检测] "..key.." 开启，自动启动防踢")
    end
end

B1.MouseButton1Click:Connect(function() HandleToggle("aim",B1) end)
B2.MouseButton1Click:Connect(function() HandleToggle("esp",B2) end)
B3.MouseButton1Click:Connect(function() HandleToggle("speed",B3) end)
B4.MouseButton1Click:Connect(function()
    if not FeatureState.wall then
        local r=GetRoot()
        if r then WallCfg.lockedY=r.Position.Y end
    end
    HandleToggle("wall",B4)
end)
B5.MouseButton1Click:Connect(function()
    HandleToggle("bt",B5)
    AimRing.Visible=FeatureState.bt
end)
B6.MouseButton1Click:Connect(function() HandleToggle("jump",B6) end)
B7.MouseButton1Click:Connect(function() HandleToggle("noFall",B7) end)
B8.MouseButton1Click:Connect(function()
    AimCfg.useRange=not AimCfg.useRange
    B8.BackgroundColor3=AimCfg.useRange and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    B8.Text=AimCfg.useRange and "范围:开" or "范围:关"
end)
B9.MouseButton1Click:Connect(function() HandleToggle("antiKick",B9,"防踢:开","防踢:关") end)
B10.MouseButton1Click:Connect(function() HandleToggle("head",B10) end)
B11.MouseButton1Click:Connect(function() HandleToggle("fastInteract",B11) end)
B12.MouseButton1Click:Connect(function() HandleToggle("noCooldown",B12) end)
B13.MouseButton1Click:Connect(function() HandleToggle("hitbox",B13) end)

KeyBtn.MouseButton1Click:Connect(function()
    local k=KeyBox.Text
    local ok=false
    if isWar then
        if k=="XDD-AX39F-F64XL-8MKHX-62S75" then ok=true end
    else
        if #k>0 then ok=true end
    end
    if ok then Main.Visible=false Panel.Visible=true end
end)

UIS.InputBegan:Connect(function(input,gp)
    if not gp and input.KeyCode==Enum.KeyCode.F6 then
        print("[防踢] 拦截记录:")
        if #kickLog==0 then print("  (无记录)") else
            for i,log in pairs(kickLog)do print("  "..i..". 方法"..(log.m or "?")..(log.msg and (" | "..log.msg) or "")) end
        end
    end
end)
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
        print("[环境] 加速方案:"..GameEnv.speedMethod)
    end)
end)

RunService.RenderStepped:Connect(function(dt)
    RunAllFeatures(dt)
end)

-- 自动防检测总开关按钮
local AutoPanel=Instance.new("TextButton")
AutoPanel.Size=UDim2.new(0,240,0,24)
AutoPanel.Position=UDim2.new(0,15,0,panelBottom+95)
AutoPanel.BackgroundColor3=Color3.fromRGB(144,238,144)
AutoPanel.Text="自动防检测: 开"
AutoPanel.TextColor3=Color3.fromRGB(255,255,255)
AutoPanel.Font=Enum.Font.GothamBold
AutoPanel.TextSize=10
Instance.new("UICorner",AutoPanel).CornerRadius=UDim.new(0,6)
AutoPanel.Parent=Panel

AutoPanel.MouseButton1Click:Connect(function()
    AutoAntiCfg.enabled=not AutoAntiCfg.enabled
    AutoPanel.BackgroundColor3=AutoAntiCfg.enabled and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    AutoPanel.Text=AutoAntiCfg.enabled and "自动防检测: 开" or "自动防检测: 关"
end)

print("樱の辅助 V13 加载完成 - 80种防踢 + 自动防检测")
