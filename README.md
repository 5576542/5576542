--[[
    万能诊断ESP - 强制显示所有Part
]]

local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local camera = workspace.CurrentCamera

-- 尝试使用CoreGui，如果被屏蔽则用PlayerGui
local guiParent = CoreGui
local success, err = pcall(function()
    local test = Instance.new("Frame")
    test.Parent = CoreGui
    test:Destroy()
end)
if not success then
    guiParent = Players.LocalPlayer:WaitForChild("PlayerGui")
end

print("使用GUI父级:", guiParent.Name)

-- 创建主GUI
local gui = Instance.new("ScreenGui")
gui.Name = "DiagnosticESP"
gui.ResetOnSpawn = false
gui.Parent = guiParent

-- 缓存
local labelCache = {}
local consoleLogged = {}

-- 生成随机颜色
local function RandomColor()
    return Color3.new(math.random(), math.random(), math.random())
end

-- 创建标签
local function CreateLabel(part, text)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 0, 0, 0)
    frame.BackgroundTransparency = 1
    frame.Parent = gui
    
    local bg = Instance.new("Frame")
    bg.Size = UDim2.new(0, 300, 0, 30)
    bg.Position = UDim2.new(0.5, -150, 0, -15)
    bg.BackgroundColor3 = Color3.new(0, 0, 0)
    bg.BackgroundTransparency = 0.5
    bg.BorderSizePixel = 1
    bg.BorderColor3 = Color3.new(1, 1, 1)
    bg.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.new(1, 1, 0)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 12
    label.TextStrokeTransparency = 0.2
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.Parent = bg
    
    return frame
end

-- 更新循环
local function UpdateESP()
    local currentParts = {}
    local playerPos = Players.LocalPlayer.Character and Players.LocalPlayer.Character.PrimaryPart and Players.LocalPlayer.Character.PrimaryPart.Position or Vector3.new(0, 0, 0)
    
    -- 遍历所有Part（包括模型内部的）
    for _, part in ipairs(workspace:GetDescendants()) do
        if part:IsA("Part") then
            -- 跳过角色身上的
            if part.Parent and part.Parent:IsA("Character") then
                continue
            end
            
            -- 跳过特殊名字（可选）
            -- if part.Name == "Baseplate" or part.Name == "Terrain" then continue end
            
            currentParts[part] = true
            
            -- 构建显示信息
            local info = string.format("[%s] %s", part.ClassName, part.Name)
            -- 检查属性
            local attrs = part:GetAttributes()
            local hasAttr = false
            for k, v in pairs(attrs) do
                hasAttr = true
                info = info .. string.format(" | %s=%s", k, tostring(v))
            end
            -- 如果没有属性，加个标志
            if not hasAttr then
                info = info .. " | (无属性)"
            end
            
            -- 控制台输出一次
            if not consoleLogged[part] then
                print(info)
                consoleLogged[part] = true
            end
            
            -- 创建标签（如果不存在）
            if not labelCache[part] then
                labelCache[part] = CreateLabel(part, info)
            end
        end
    end
    
    -- 清理移除的Part
    for part, frame in pairs(labelCache) do
        if not currentParts[part] then
            frame:Destroy()
            labelCache[part] = nil
            consoleLogged[part] = nil
        end
    end
    
    -- 更新位置（显示所有Part，即使没属性）
    for part, frame in pairs(labelCache) do
        local pos, onScreen = camera:WorldToViewportPoint(part.Position)
        if onScreen and pos.Z > 0 then
            frame.Position = UDim2.new(0, pos.X - 150, 0, pos.Y - 15)
            frame.Visible = true
        else
            frame.Visible = false
        end
    end
end

-- 启动
RunService.RenderStepped:Connect(UpdateESP)

print("✅ 诊断ESP已启动 - 所有Part都会显示标签（无论有无属性）")
print("📌 如果有任何Part在屏幕上，你都会看到金色标签")
print("📌 控制台会打印每个Part的信息")
