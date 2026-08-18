local Players=game:GetService("Players")local RunService=game:GetService("RunService")local LP=Players.LocalPlayer
local ScreenGui=Instance.new("ScreenGui")ScreenGui.ResetOnSpawn=false ScreenGui.Parent=game.CoreGui

-- 主窗口
local Frame=Instance.new("Frame")Frame.Size=UDim2.new(0,200,0,180)Frame.Position=UDim2.new(0.02,0,0.2,0)Frame.BackgroundColor3=Color3.new(0.08,0.08,0.12)Frame.BackgroundTransparency=0.2 Frame.Active=true Frame.Draggable=true Frame.Parent=ScreenGui
Instance.new("UICorner",Frame).CornerRadius=UDim.new(0,10)

-- 标题栏
local Title=Instance.new("Frame")Title.Size=UDim2.new(1,0,0,25)Title.BackgroundColor3=Color3.new(0.15,0.15,0.22)Title.BackgroundTransparency=0.3 Title.Parent=Frame
Instance.new("UICorner",Title).CornerRadius=UDim.new(0,10)
local TitleLabel=Instance.new("TextLabel")TitleLabel.Size=UDim2.new(1,-60,1,0)TitleLabel.Position=UDim2.new(0,8,0,0)TitleLabel.BackgroundTransparency=1 TitleLabel.Text="AI 辅助 V2"TitleLabel.TextColor3=Color3.new(0.3,0.8,1)TitleLabel.Font=Enum.Font.GothamBold TitleLabel.TextSize=12 TitleLabel.TextXAlignment=Enum.TextXAlignment.Left TitleLabel.Parent=Title
local PageLabel=Instance.new("TextLabel")PageLabel.Size=UDim2.new(0,40,1,0)PageLabel.Position=UDim2.new(1,-50,0,0)PageLabel.BackgroundTransparency=1 PageLabel.Text="1/2"PageLabel.TextColor3=Color3.new(0.6,0.6,0.6)PageLabel.Font=Enum.Font.Gotham PageLabel.TextSize=10 PageLabel.Parent=Title
local Hide=Instance.new("TextButton")Hide.Size=UDim2.new(0,24,1,0)Hide.Position=UDim2.new(1,-26,0,0)Hide.BackgroundTransparency=0.4 Hide.BackgroundColor3=Color3.new(0.35,0.35,0.35)Hide.Text="-"Hide.TextColor3=Color3.new(1,1,1)Hide.Font=Enum.Font.GothamBold Hide.TextSize=12 Instance.new("UICorner",Hide).CornerRadius=UDim.new(0,4)Hide.Parent=Title

-- 页码指示器
local PageDot=Instance.new("Frame")PageDot.Size=UDim2.new(0,30,0,4)PageDot.Position=UDim2.new(0.5,-15,0,180)PageDot.BackgroundColor3=Color3.new(0.3,0.8,1)PageDot.BackgroundTransparency=0.5 PageDot.Parent=Frame
Instance.new("UICorner",PageDot).CornerRadius=UDim.new(0,2)

-- ===== 第1页：吸附 =====
local Page1=Instance.new("Frame")Page1.Size=UDim2.new(1,0,1,-30)Page1.Position=UDim2.new(0,0,0,30)Page1.BackgroundTransparency=1 Page1.Parent=Frame

local function CreateLabel(p,t,s)local l=Instance.new("TextLabel")l.Size=UDim2.new(0,60,0,16)l.Position=p l.BackgroundTransparency=1 l.Text=t l.TextColor3=Color3.new(0.8,0.8,0.8)l.Font=Enum.Font.Gotham l.TextSize=s or 10 l.TextXAlignment=Enum.TextXAlignment.Left l.Parent=Page1 return l end
local function CreateBtn(p,t,c)local b=Instance.new("TextButton")b.Size=UDim2.new(0,55,0,18)b.Position=p b.BackgroundTransparency=0.3 b.BackgroundColor3=c b.Text=t b.TextColor3=Color3.new(1,1,1)b.Font=Enum.Font.Gotham b.TextSize=9 Instance.new("UICorner",b).CornerRadius=UDim.new(0,4)b.Parent=Page1 return b end

local Config={aim=false,aimSmooth=0.3,aimRange=250,esp=false,anti=false}
local espList={}

-- 吸附开关
CreateLabel(UDim2.new(0,5,0,5),"吸附",12)
local AimBtn=CreateBtn(UDim2.new(0,65,0,3),"开启",Color3.new(0.5,0.1,0.1))

-- 平滑度
CreateLabel(UDim2.new(0,5,0,28),"平滑度: "..string.format("%.2f",Config.aimSmooth))
local SmoothAdd=CreateBtn(UDim2.new(0,120,0,26),"+",Color3.new(0.15,0.25,0.5))
local SmoothSub=CreateBtn(UDim2.new(0,65,0,26),"-",Color3.new(0.15,0.25,0.5))

-- 距离
CreateLabel(UDim2.new(0,5,0,50),"距离: "..tostring(Config.aimRange))
local RangeAdd=CreateBtn(UDim2.new(0,120,0,48),"+",Color3.new(0.15,0.25,0.5))
local RangeSub=CreateBtn(UDim2.new(0,65,0,48),"-",Color3.new(0.15,0.25,0.5))

-- 过检测
CreateLabel(UDim2.new(0,5,0,72),"过检测")
local AntiBtn=CreateBtn(UDim2.new(0,65,0,70),"关闭",Color3.new(0.5,0.1,0.1))

-- ===== 第2页：透视 =====
local Page2=Instance.new("Frame")Page2.Size=UDim2.new(1,0,1,-30)Page2.Position=UDim2.new(0,0,0,30)Page2.BackgroundTransparency=1 Page2.Visible=false Page2.Parent=Frame

local EspBtn=CreateBtn(UDim2.new(0,5,0,10),"开启透视",Color3.new(0.5,0.1,0.1))
CreateLabel(UDim2.new(0,5,0,35),"透视模式: 红色高亮")
local RefreshBtn=CreateBtn(UDim2.new(0,5,0,55),"刷新透视",Color3.new(0.15,0.25,0.5))

-- 翻页按钮
local PrevBtn=Instance.new("TextButton")PrevBtn.Size=UDim2.new(0,40,0,20)PrevBtn.Position=UDim2.new(0,5,0,155)PrevBtn.BackgroundTransparency=0.3 PrevBtn.BackgroundColor3=Color3.new(0.15,0.25,0.5)PrevBtn.Text="◀"PrevBtn.TextColor3=Color3.new(1,1,1)PrevBtn.Font=Enum.Font.GothamBold PrevBtn.TextSize=12 Instance.new("UICorner",PrevBtn).CornerRadius=UDim.new(0,4)PrevBtn.Parent=Frame
local NextBtn=Instance.new("TextButton")NextBtn.Size=UDim2.new(0,40,0,20)NextBtn.Position=UDim2.new(1,-45,0,155)NextBtn.BackgroundTransparency=0.3 NextBtn.BackgroundColor3=Color3.new(0.15,0.25,0.5)NextBtn.Text="▶"NextBtn.TextColor3=Color3.new(1,1,1)NextBtn.Font=Enum.Font.GothamBold NextBtn.TextSize=12 Instance.new("UICorner",NextBtn).CornerRadius=UDim.new(0,4)NextBtn.Parent=Frame

-- 悬浮球
local Ball=Instance.new("ImageButton")Ball.Size=UDim2.new(0,40,0,40)Ball.Position=UDim2.new(0.9,-20,0.8,-20)Ball.BackgroundColor3=Color3.new(0.15,0.5,0.85)Ball.Visible=false Ball.Draggable=true Ball.AutoButtonColor=false Ball.Parent=ScreenGui
Instance.new("UICorner",Ball).CornerRadius=UDim.new(1,0)
local BallText=Instance.new("TextLabel")BallText.Size=UDim2.new(1,0,1,0)BallText.BackgroundTransparency=1 BallText.Text="AI"BallText.TextColor3=Color3.new(1,1,1)BallText.Font=Enum.Font.GothamBold BallText.TextSize=14 BallText.Parent=Ball

-- 拖拽
local dragStart,dragStartPos
Ball.InputBegan:Connect(function(i)if i.UserInputType==Enum.UserInputType.MouseButton1 then dragStart=i.Position dragStartPos=Ball.Position end end)
Ball.InputChanged:Connect(function(i)if i.UserInputType==Enum.UserInputType.MouseButton1 and dragStart then local d=i.Position-dragStart Ball.Position=UDim2.new(dragStartPos.X.Scale,dragStartPos.X.Offset+d.X,dragStartPos.Y.Scale,dragStartPos.Y.Offset+d.Y)end end)
Ball.InputEnded:Connect(function()dragStart=nil end)

-- 翻页逻辑
local currentPage=1
local function UpdatePage()
Page1.Visible=(currentPage==1)Page2.Visible=(currentPage==2)
PageLabel.Text=currentPage.."/2"
PageDot.Position=UDim2.new(0.5-(currentPage==1 and 8 or -8),0,0,180)
end
PrevBtn.MouseButton1Click:Connect(function()if currentPage>1 then currentPage=currentPage-1 UpdatePage()end end)
NextBtn.MouseButton1Click:Connect(function()if currentPage<2 then currentPage=currentPage+1 UpdatePage()end end)

-- 隐藏/显示
Hide.MouseButton1Click:Connect(function()Frame.Visible=false Ball.Visible=true Ball.Position=Frame.Position end)
Ball.MouseButton1Click:Connect(function()Frame.Position=Ball.Position Frame.Visible=true Ball.Visible=false end)

-- 吸附按钮
AimBtn.MouseButton1Click:Connect(function()Config.aim=not Config.aim AimBtn.Text=Config.aim and"关闭"or"开启"AimBtn.BackgroundColor3=Config.aim and Color3.new(0.1,0.5,0.1)or Color3.new(0.5,0.1,0.1)end)

-- 平滑度调节
SmoothAdd.MouseButton1Click:Connect(function()Config.aimSmooth=math.min(1,Config.aimSmooth+0.05)CreateLabel(UDim2.new(0,5,0,28),"平滑度: "..string.format("%.2f",Config.aimSmooth))end)
SmoothSub.MouseButton1Click:Connect(function()Config.aimSmooth=math.max(0.05,Conf
