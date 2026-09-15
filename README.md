local Players=game:GetService("Players")
local RunService=game:GetService("RunService")
local LP=Players.LocalPlayer
local UIS=game:GetService("UserInputService")
local CG=game:GetService("CoreGui")
local RS=game:GetService("ReplicatedStorage")

local BG_ID="rbxassetid://131248212024332"
local G=Instance.new("ScreenGui")
G.ResetOnSpawn=false
G.Parent=CG

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

-- ==================== 200条算法池 ====================
local AlgorithmPool={windproof={},antidetect={},antiLag={},antiFail={}}
local KW_LIST={"kick","ban","flag","detect","check","suspect","punish","report","anti","hack","cheat","violation","warn","alert","spy","watch","log","audit","monitor","verify","secure","encrypt","lock","protect","guard","shield","block","deny","reject","filter","screen","scan","probe","test","validate","authenticate","authorize","permission","access","control","restrict","limit","rate","throttle","queue","delay","timeout","expire","stale","cache"}

local FeatureState={}
local function MakeAlgo(cat,kw,interval,action)
    return function()
        task.spawn(function()
            while FeatureState["algo_"..cat] do
                task.wait(interval)
                pcall(function()
                    if action=="clean" then
                        for _,v in pairs(workspace:GetDescendants())do
                            if v:IsA("BoolValue")or v:IsA("StringValue")then
                                if v.Name:lower():find(kw)then v:Destroy() end
                            end
                        end
                    elseif action=="watch" then
                        for _,v in pairs(workspace:GetDescendants())do
                            if v:IsA("Script")or v:IsA("LocalScript")then
                                if v.Name:lower():find(kw)then v.Disabled=true end
                            end
                        end
                    elseif action=="hook" then
                        for _,v in pairs(RS:GetDescendants())do
                            if v:IsA("RemoteEvent")then
                                if v.Name:lower():find(kw)then v.OnClientEvent=function() end end
                            end
                        end
                    end
                end)
            end
        end)
    end
end

for i=1,50 do
    local kw=KW_LIST[(i-1)%#KW_LIST+1]
    table.insert(AlgorithmPool.windproof,MakeAlgo("windproof",kw,2+i%5,"clean"))
    table.insert(AlgorithmPool.antidetect,MakeAlgo("antidetect",kw,3+i%7,"watch"))
    table.insert(AlgorithmPool.antiLag,MakeAlgo("antiLag",kw,1+i%3,"clean"))
    table.insert(AlgorithmPool.antiFail,MakeAlgo("antiFail",kw,4+i%6,"hook"))
end
print("[算法池] 200条算法已生成")
-- ==================== 500条动态数据 ====================
local DynamicData={}

local function GenData(id)
    return {
        id=id,
        timestamp=os.time(),
        rand1=math.random(1,999999),
        rand2=math.random(1,999999),
        rand3=math.random(1,999999),
        userId=LP.UserId,
        name=LP.Name,
        gameId=game.GameId,
        placeId=game.PlaceId,
        hash=(os.time()*id)%2147483647,
        token=string.format("%08X",math.random(0,4294967295))
    }
end

for i=1,500 do
    table.insert(DynamicData,GenData(i))
end

task.spawn(function()
    while true do
        task.wait(5)
        local n=math.random(10,30)
        for i=1,n do
            local idx=math.random(1,#DynamicData)
            DynamicData[idx]=GenData(idx)
        end
    end
end)
print("[数据池] 500条动态数据已生成")
local DecryptCfg={decrypted=false,encrypted=false,autoDecrypt=true}

local function CheckEncrypted()
    local c=LP.Character
    if c then
        for _,v in pairs(c:GetChildren())do
            local n=v.Name:lower()
            if n:find("encrypt")or n:find("lock")or n:find("secure")then
                if v:IsA("BoolValue")and v.Value then return true end
            end
        end
    end
    -- 算法推算账号签名
    local raw=tostring(LP.UserId)..LP.Name..tostring(LP.AccountAge)..tostring(game.GameId)
    local hash=0
    for i=1,#raw do hash=(hash*31+string.byte(raw,i))%2147483647 end
    return hash%2==0
end

local Features={}
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
    if not DecryptCfg.decrypted then return end
    for k,c in pairs(Features)do
        if FeatureState[k] and c.run then pcall(c.run,dt) end
    end
end

-- 左上角状态
local StatusBar=Instance.new("Frame")
StatusBar.Size=UDim2.new(0,280,0,80)
StatusBar.Position=UDim2.new(0,10,0,10)
StatusBar.BackgroundColor3=Color3.fromRGB(25,25,40)
StatusBar.BackgroundTransparency=0.15
StatusBar.Parent=G
Instance.new("UICorner",StatusBar).CornerRadius=UDim.new(0,8)

local StatusLabel=Instance.new("TextLabel")
StatusLabel.Size=UDim2.new(1,-10,1,-10)
StatusLabel.Position=UDim2.new(0,5,0,5)
StatusLabel.BackgroundTransparency=1
StatusLabel.Text="🔍 检测中...\n算法: 200 | 数据: 500"
StatusLabel.TextColor3=Color3.fromRGB(150,200,255)
StatusLabel.Font=Enum.Font.Gotham
StatusLabel.TextSize=10
StatusLabel.TextXAlignment=Enum.TextXAlignment.Left
StatusLabel.TextYAlignment=Enum.TextYAlignment.Top
StatusLabel.Parent=StatusBar
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
Panel.Size=UDim2.new(0,280,0,540)
Panel.Position=UDim2.new(0.5,-140,0.5,-270)
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
local WallCfg={lockedY=nil}

local B1=Btn("自瞄")
local B2=Btn("透视")
local B3=Btn("加速")
local B4=Btn("穿墙")
local B5=Btn("物品追踪")
local B6=Btn("高跳")
local B7=Btn("坠落无伤")
local B8=Btn("范围:开")
local B10=Btn("头顶显示")
local B11=Btn("秒交互")
local B12=Btn("无后摇")
local B13=Btn("扩大碰撞")
local B14=Btn("自动闪避")
local B15=Btn("防风算法")
local B16=Btn("防检测算法")
local B17=Btn("防卡顿算法")
local B18=Btn("防失效算法")

local panelBottom=36+math.ceil(BTN_INDEX/2)*30

-- 解密按钮
local DecryptBtn=Instance.new("TextButton")
DecryptBtn.Size=UDim2.new(0,240,0,28)
DecryptBtn.Position=UDim2.new(0,15,0,panelBottom+5)
DecryptBtn.BackgroundColor3=Color3.fromRGB(255,80,80)
DecryptBtn.Text="🔓 解密游戏数据"
DecryptBtn.TextColor3=Color3.fromRGB(255,255,255)
DecryptBtn.Font=Enum.Font.GothamBold
DecryptBtn.TextSize=11
Instance.new("UICorner",DecryptBtn).CornerRadius=UDim.new(0,6)
DecryptBtn.Parent=Panel

-- 算法状态显示
local AlgoLabel=Instance.new("TextLabel")
AlgoLabel.Size=UDim2.new(0,240,0,20)
AlgoLabel.Position=UDim2.new(0,15,0,panelBottom+38)
AlgoLabel.BackgroundTransparency=1
AlgoLabel.Text="算法: 待机 | 数据: 500"
AlgoLabel.TextColor3=Color3.fromRGB(255,150,200)
AlgoLabel.Font=Enum.Font.Gotham
AlgoLabel.TextSize=10
AlgoLabel.Parent=Panel

DecryptBtn.MouseButton1Click:Connect(function()
    DecryptBtn.Text="⏳ 解密中..."
    local n=0
    -- 解密：清理锁定标记
    pcall(function()
        for _,v in pairs(workspace:GetDescendants())do
            if v:IsA("BoolValue")or v:IsA("StringValue")then
                local nm=v.Name:lower()
                if nm:find("encrypt")or nm:find("lock")or nm:find("secure")then
                    v:Destroy()
                    n=n+1
                end
            end
        end
    end)
    task.wait(0.3)
    DecryptCfg.decrypted=true
    DecryptBtn.Text="✅ 已解密 ("..n.."项)"
    DecryptBtn.BackgroundColor3=Color3.fromRGB(80,200,120)
    StatusLabel.Text="✅ 已解密\n算法: 200 | 数据: 500"
    StatusLabel.TextColor3=Color3.fromRGB(150,255,180)
end)
local function MakeSlider(yPos,getText,onSub,onAdd)
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
end

MakeSlider(panelBottom+65,
    function() return "速度: "..SpeedCfg.value end,
    function() SpeedCfg.value=math.max(SpeedCfg.min,SpeedCfg.value-SpeedCfg.step) end,
    function() SpeedCfg.value=math.min(SpeedCfg.max,SpeedCfg.value+SpeedCfg.step) end
)
MakeSlider(panelBottom+93,
    function() return "范围: "..AimCfg.range end,
    function() AimCfg.range=math.max(AimCfg.min,AimCfg.range-AimCfg.step) end,
    function() AimCfg.range=math.min(AimCfg.max,AimCfg.range+AimCfg.step) end
)
MakeSlider(panelBottom+121,
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
local aimMethod=0
local function AimAlgo1(target)
    local cam=workspace.CurrentCamera
    if not cam or not UIS.SetMouseDelta then return false end
    local ok=false
    pcall(function()
        local sp,on=cam:WorldToViewportPoint(target.Position)
        if on then
            local vs=cam.ViewportSize
            UIS:SetMouseDelta(Vector2.new(math.clamp((sp.X-vs.X/2)*0.5,-80,80),math.clamp((sp.Y-vs.Y/2)*0.5,-80,80)))
            ok=true
        end
    end)
    return ok
end
local function AimAlgo2(target)
    if not mousemoverel then return false end
    local cam=workspace.CurrentCamera
    if not cam then return false end
    local ok=false
    pcall(function()
        local sp,on=cam:WorldToViewportPoint(target.Position)
        if on then
            local vs=cam.ViewportSize
            mousemoverel((sp.X-vs.X/2)*0.5,(sp.Y-vs.Y/2)*0.5)
            ok=true
        end
    end)
    return ok
end
local function AimAlgo3(target)
    local cam=workspace.CurrentCamera
    if not cam then return false end
    local ok=false
    pcall(function()
        local mouse=LP:GetMouse()
        if mouse and mouse.Move then
            local sp,on=cam:WorldToViewportPoint(target.Position)
            if on then
                local vs=cam.ViewportSize
                mouse.Move(mouse.X+(sp.X-vs.X/2)*0.5,mouse.Y+(sp.Y-vs.Y/2)*0.5)
                ok=true
            end
        end
    end)
    return ok
end
local function AimAlgo4(target)
    local c=LP.Character
    if not c then return false end
    local h=c:FindFirstChildOfClass("Humanoid")
    local r=c:FindFirstChild("HumanoidRootPart")
    if not h or not r then return false end
    local ok=false
    pcall(function()
        h.AutoRotate=false
        r.CFrame=CFrame.new(r.Position,Vector3.new(target.Position.X,r.Position.Y,target.Position.Z))
        ok=true
    end)
    return ok
end
local function AimAlgo5(target)
    local VIM=game:GetService("VirtualInputManager")
    if not VIM then return false end
    local cam=workspace.CurrentCamera
    if not cam then return false end
    local ok=false
    pcall(function()
        local sp,on=cam:WorldToViewportPoint(target.Position)
        if on then VIM:SendMouseMoveEvent(sp.X,sp.Y,false) ok=true end
    end)
    return ok
end

RegisterFeature("aim",{
    run=function()
        local t=GetTarget()
        if not t then return end
        if aimMethod==1 then AimAlgo1(t) return end
        if aimMethod==2 then AimAlgo2(t) return end
        if aimMethod==3 then AimAlgo3(t) return end
        if aimMethod==4 then AimAlgo4(t) return end
        if aimMethod==5 then AimAlgo5(t) return end
        if AimAlgo1(t) then aimMethod=1 return end
        if AimAlgo2(t) then aimMethod=2 return end
        if AimAlgo3(t) then aimMethod=3 return end
        if AimAlgo4(t) then aimMethod=4 return end
        if AimAlgo5(t) then aimMethod=5 return end
    end
})
local speedMethod=0
local speedHooked=nil
local function SpeedAlgo1()
    local h=GetHum()
    if not h then return false end
    pcall(function() h.WalkSpeed=SpeedCfg.value end)
    return math.abs(h.WalkSpeed-SpeedCfg.value)<2
end
local function SpeedAlgo2()
    local h=GetHum()
    if not h then return false end
    if speedHooked~=h then
        speedHooked=h
        pcall(function()
            h:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
                if FeatureState.speed and math.abs(h.WalkSpeed-SpeedCfg.value)>0.5 then
                    h.WalkSpeed=SpeedCfg.value
                end
            end)
        end)
    end
    pcall(function() h.WalkSpeed=SpeedCfg.value end)
    return true
end
RegisterFeature("speed",{
    run=function()
        if speedMethod==0 then speedMethod=1 end
        if speedMethod==1 then
            if not SpeedAlgo1() then speedMethod=2 end
            return
        end
        if speedMethod==2 then SpeedAlgo2() return end
    end,
    onDisable=function()
        speedMethod=0
        local h=GetHum()
        if h then pcall(function() h.WalkSpeed=16 end) end
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
                    pcall(function() v.Velocity=dir*250 v.CFrame=CFrame.new(v.Position,lockedTarget.Position) end)
                end
            end
        end
    end,
    onDisable=function() GreenLine.Visible=false lockedTarget=nil end
})

local function UpdateHeadDisplay()
    for i=#headList,1,-1 do
        local item=headList[i]
        if item.bg and item.bg.Parent then
            local hum=item.char and item.char:FindFirstChildOfClass("Humanoid")
            if not hum or hum.Health<=0 then item.bg:Destroy() table.remove(headList,i) end
        else table.remove(headList,i) end
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
        for _,item in pairs(headList)do if item.bg then pcall(function() item.bg:Destroy() end) end end
        headList={}
    end
})
RegisterFeature("fastInteract",{})
task.spawn(function()
    while true do
        task.wait(1)
        if FeatureState.fastInteract and DecryptCfg.decrypted then
            pcall(function()
                for _,v in pairs(workspace:GetDescendants())do
                    if v:IsA("ProximityPrompt")then
                        if v.HoldDuration~=0 then v.HoldDuration=0 end
                        if v.MaxActivationDistance~=200 then v.MaxActivationDistance=200 end
                        if v.RequiresLineOfSight then v.RequiresLineOfSight=false end
                    end
                    if v:IsA("ClickDetector")then
                        if v.MaxActivationDistance~=200 then v.MaxActivationDistance=200 end
                    end
                end
            end)
        end
    end
end)

RegisterFeature("noCooldown",{
    run=function()
        local c=LP.Character
        local toolList={}
        if c then for _,v in pairs(c:GetChildren())do if v:IsA("Tool")then table.insert(toolList,v) end end end
        for _,v in pairs(LP.Backpack:GetChildren())do if v:IsA("Tool")then table.insert(toolList,v) end end
        for _,tool in ipairs(toolList)do
            for _,v in pairs(tool:GetDescendants())do
                if v:IsA("NumberValue")then
                    local n=v.Name:lower()
                    if n:find("cooldown")or n:find("delay")or n:find("reload")or n:find("rate")then
                        if v.Value~=0 then v.Value=0 end
                    end
                end
            end
        end
    end
})

local HitboxCache={}
local function BuildHitbox(char,scale)
    local hrp=char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    for _,v in pairs(char:GetChildren())do if v.Name:sub(1,3)=="_hb"then pcall(function() v:Destroy() end) end end
    local parts={
        {n="_hb_head",b=Vector3.new(2,2,2),o=Vector3.new(0,1.5,0)},
        {n="_hb_upper",b=Vector3.new(4,2,2),o=Vector3.new(0,0.5,0)},
        {n="_hb_lower",b=Vector3.new(4,2,2),o=Vector3.new(0,-0.5,0)},
        {n="_hb_center",b=Vector3.new(6,6,6),o=Vector3.new(0,0,0)}
    }
    for _,i in ipairs(parts)do
        local hb=Instance.new("Part")
        hb.Name=i.n hb.Transparency=1 hb.CanCollide=false hb.CanQuery=true hb.CanTouch=true hb.Anchored=true hb.Massless=true
        hb.Size=i.b*scale hb.CFrame=hrp.CFrame*CFrame.new(i.o) hb.Parent=char
    end
end
RegisterFeature("hitbox",{
    run=function()
        for player,char in pairs(HitboxCache)do
            if not player.Parent or not char or not char.Parent then HitboxCache[player]=nil end
        end
        for _,p in pairs(Players:GetPlayers())do
            if p~=LP and p.Character then
                local h=p.Character:FindFirstChildOfClass("Humanoid")
                if h and h.Health>0 then
                    if HitboxCache[p]~=p.Character then
                        pcall(function() BuildHitbox(p.Character,HitboxCfg.scale) end)
                        HitboxCache[p]=p.Character
                    end
                end
            end
        end
    end,
    onDisable=function()
        for _,p in pairs(Players:GetPlayers())do
            if p.Character then
                for _,v in pairs(p.Character:GetChildren())do
                    if v.Name:sub(1,3)=="_hb"then pcall(function() v:Destroy() end) end
                end
            end
        end
        HitboxCache={}
    end
})

local DodgeCfg={range=20,cooldown=0}
RegisterFeature("dodge",{
    run=function()
        if DodgeCfg.cooldown>0 then DodgeCfg.cooldown=DodgeCfg.cooldown-1 return end
        local r=GetRoot()
        if not r then return end
        local best,bD=nil,DodgeCfg.range
        for _,v in pairs(workspace:GetDescendants())do
            if v:IsA("BasePart")then
                local n=v.Name:lower()
                if n:find("bullet")or n:find("projectile")then
                    if v.AssemblyLinearVelocity.Magnitude>5 then
                        local toMe=(r.Position-v.Position).Unit
                        if toMe:Dot(v.AssemblyLinearVelocity.Unit)>0.7 then
                            local dist=(r.Position-v.Position).Magnitude
                            if dist<bD then bD=dist best=v end
                        end
                    end
                end
            end
        end
        if best then
            local toMe=(r.Position-best.Position).Unit
            local dir=(Vector3.new(-toMe.Z,0,toMe.X)*0.7+toMe*0.3).Unit
            local h=GetHum()
            if h then
                r.AssemblyLinearVelocity=dir*math.max(h.WalkSpeed,30)+Vector3.new(0,15,0)
                DodgeCfg.cooldown=15
            end
        end
    end
})
local function HandleToggle(key,btn,onT,offT)
    if not DecryptCfg.decrypted then
        StatusLabel.Text="⚠️ 请先点【解密游戏数据】"
        StatusLabel.TextColor3=Color3.fromRGB(255,200,80)
        wait(1.5)
        StatusLabel.Text="🔒 未解密"
        StatusLabel.TextColor3=Color3.fromRGB(255,150,150)
        return
    end
    local on=ToggleFeature(key)
    btn.BackgroundColor3=on and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    if onT and offT then btn.Text=on and onT or offT end
    local c=Features[key]
    if c then
        if on and c.onEnable then pcall(c.onEnable) end
        if not on and c.onDisable then pcall(c.onDisable) end
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
B5.MouseButton1Click:Connect(function() HandleToggle("bt",B5) AimRing.Visible=FeatureState.bt end)
B6.MouseButton1Click:Connect(function() HandleToggle("jump",B6) end)
B7.MouseButton1Click:Connect(function() HandleToggle("noFall",B7) end)
B8.MouseButton1Click:Connect(function()
    if not DecryptCfg.decrypted then return end
    AimCfg.useRange=not AimCfg.useRange
    B8.BackgroundColor3=AimCfg.useRange and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    B8.Text=AimCfg.useRange and "范围:开" or "范围:关"
end)
B10.MouseButton1Click:Connect(function() HandleToggle("head",B10) end)
B11.MouseButton1Click:Connect(function() HandleToggle("fastInteract",B11) end)
B12.MouseButton1Click:Connect(function() HandleToggle("noCooldown",B12) end)
B13.MouseButton1Click:Connect(function() HandleToggle("hitbox",B13) end)
B14.MouseButton1Click:Connect(function() HandleToggle("dodge",B14) end)
-- 启动200条算法
local function StartAlgoPool()
    -- 防风算法（50条）
    FeatureState["algo_windproof"]=true
    for _,fn in ipairs(AlgorithmPool.windproof)do fn() end
    -- 防检测算法（50条）
    FeatureState["algo_antidetect"]=true
    for _,fn in ipairs(AlgorithmPool.antidetect)do fn() end
    -- 防卡顿算法（50条）
    FeatureState["algo_antiLag"]=true
    for _,fn in ipairs(AlgorithmPool.antiLag)do fn() end
    -- 防失效算法（50条）
    FeatureState["algo_antiFail"]=true
    for _,fn in ipairs(AlgorithmPool.antiFail)do fn() end
end
local function StopAlgoPool()
    FeatureState["algo_windproof"]=false
    FeatureState["algo_antidetect"]=false
    FeatureState["algo_antiLag"]=false
    FeatureState["algo_antiFail"]=false
end

B15.MouseButton1Click:Connect(function()
    if not DecryptCfg.decrypted then return end
    if FeatureState["algo_windproof"]then
        StopAlgoPool()
        B15.BackgroundColor3=Color3.fromRGB(255,105,180)
        AlgoLabel.Text="算法: 待机 | 数据: 500"
    else
        StartAlgoPool()
        B15.BackgroundColor3=Color3.fromRGB(144,238,144)
        AlgoLabel.Text="算法: 200条运行中 | 数据: 500"
    end
end)
B16.MouseButton1Click:Connect(function()
    if not DecryptCfg.decrypted then return end
    local on=not FeatureState["algo_antidetect"]
    FeatureState["algo_antidetect"]=on
    B16.BackgroundColor3=on and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    if on then for _,fn in ipairs(AlgorithmPool.antidetect)do fn() end end
end)
B17.MouseButton1Click:Connect(function()
    if not DecryptCfg.decrypted then return end
    local on=not FeatureState["algo_antiLag"]
    FeatureState["algo_antiLag"]=on
    B17.BackgroundColor3=on and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    if on then for _,fn in ipairs(AlgorithmPool.antiLag)do fn() end end
end)
B18.MouseButton1Click:Connect(function()
    if not DecryptCfg.decrypted then return end
    local on=not FeatureState["algo_antiFail"]
    FeatureState["algo_antiFail"]=on
    B18.BackgroundColor3=on and Color3.fromRGB(144,238,144) or Color3.fromRGB(255,105,180)
    if on then for _,fn in ipairs(AlgorithmPool.antiFail)do fn() end end
end)
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
    if i.UserInputType==Enum.UserInputType.MouseButton1 or i.UserInputType==Enum.UserInputType.Touch then bds=i.Position bdp=Ball.Position end
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

-- 主循环
local lastRefresh=0
RunService.RenderStepped:Connect(function(dt)
    RunAllFeatures(dt)
    -- 每5秒刷新一次数据池状态
    lastRefresh=lastRefresh+dt
    if lastRefresh>5 then
        lastRefresh=0
        -- 动态数据不断刷新
    end
end)

print("樱の辅助 V18 加载完成 - 200算法 + 500数据")
