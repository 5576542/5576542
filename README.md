local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local camera = workspace.CurrentCamera

-- 创建主GUI容器
local lootGui = Instance.new("ScreenGui")
lootGui.Name = "LootESP"
lootGui.ResetOnSpawn = false
lootGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
lootGui.Parent = CoreGui

-- 缓存
local espCache = {}
local labelPool = {}

-- 创建单个ESP（红框+价值标签）
local function CreateESP(part, value)
    local container = Instance.new("Frame")
    container.Size = UDim2.new(0, 0, 0, 0)
    container.BackgroundTransparency = 1
    container.Parent = lootGui
    
    -- 红框（四个边组成）
    local function MakeBorder(parent, size, position)
        local border = Instance.new("Frame")
        border.Size = size
        border.Position = position
        border.BackgroundColor3 = Color3.new(1, 0, 0)
        border.BackgroundTransparency = 0.3
        border.BorderSizePixel = 0
        border.Parent = parent
        return border
    end
    
    -- 上边
    local top = MakeBorder(container, UDim2.new(1, 0, 0, 2), UDim2.new(0, 0, 0, 0))
    -- 下边
    local bottom = MakeBorder(container, UDim2.new(1, 0, 0, 2), UDim2.new(0, 0, 1, -2))
    -- 左边
    local left = MakeBorder(container, UDim2.new(0, 2, 1, 0), UDim2.new(0, 0, 0, 0))
    -- 右边
    local right = MakeBorder(container, UDim2.new(0, 2, 1, 0), UDim2.new(1, -2, 0, 0))
    
    -- 价值标签（在框上方）
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 100, 0, 20)
    label.Position = UDim2.new(0.5, -50, 0, -25)
    label.BackgroundTransparency = 1
    label.Text = string.format("💰 %d", value)
    label.TextColor3 = Color3.new(1, 0.92, 0)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 14
    label.TextStrokeTransparency = 0.3
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.Parent = container
    
    -- 背景光晕
    local glow = Instance.new("Frame")
    glow.Size = UDim2.new(1.2, 0, 1.2, 0)
    glow.Position = UDim2.new(-0.1, 0, -0.1, 0)
    glow.BackgroundColor3 = Color3.new(1, 0, 0)
    glow.BackgroundTransparency = 0.85
    glow.BorderSizePixel = 0
    glow.Parent = container
    
    return container
end

-- 获取对象池标签
local function GetESP(part, value)
    local esp = table.remove(labelPool)
    if esp then
        esp.Parent = lootGui
        esp:FindFirstChild("TextLabel").Text = string.format("💰 %d", value)
        return esp
    end
    return CreateESP(part, value)
end

-- 回收ESP
local function RecycleESP(esp)
    esp.Parent = nil
    table.insert(labelPool, esp)
end

-- 更新ESP位置和大小
local function UpdateESP(esp, part)
    if not part or not part.Parent then
        return false
    end
    
    -- 获取部件在屏幕上的位置和大小
    local screenPos, onScreen = camera:WorldToViewportPoint(part.Position)
    
    if not onScreen then
        esp.Visible = false
        return true
    end
    
    -- 计算部件在屏幕上的大小（基于部件的实际大小）
    local size = part.Size
    local halfSize = size / 2
    
    -- 获取部件的8个顶点，计算屏幕边界
    local corners = {
        part.CFrame:PointToWorldSpace(-halfSize),
        part.CFrame:PointToWorldSpace(halfSize),
        part.CFrame:PointToWorldSpace(Vector3.new(-halfSize.X, halfSize.Y, halfSize.Z)),
        part.CFrame:PointToWorldSpace(Vector3.new(halfSize.X, -halfSize.Y, halfSize.Z)),
        part.CFrame:PointToWorldSpace(Vector3.new(-halfSize.X, -halfSize.Y, -halfSize.Z)),
        part.CFrame:PointToWorldSpace(Vector3.new(halfSize.X, halfSize.Y, -halfSize.Z)),
        part.CFrame:PointToWorldSpace(Vector3.new(-halfSize.X, halfSize.Y, -halfSize.Z)),
        part.CFrame:PointToWorldSpace(Vector3.new(halfSize.X, -halfSize.Y, -halfSize.Z))
    }
    
    local minX, maxX, minY, maxY = math.huge, -math.huge, math.huge, -math.huge
    
    for _, corner in ipairs(corners) do
        local pos, vis = camera:WorldToViewportPoint(corner)
        if vis then
            minX = math.min(minX, pos.X)
            maxX = math.max(maxX, pos.X)
            minY = math.min(minY, pos.Y)
            maxY = math.max(maxY, pos.Y)
        end
    end
    
    -- 如果无法计算边界，使用默认大小
    if minX == math.huge then
        local pos, _ = camera:WorldToViewportPoint(part.Position)
        minX = pos.X - 30
        maxX = pos.X + 30
        minY = pos.Y - 30
        maxY = pos.Y + 30
    end
    
    -- 应用边界（增加一些padding）
    local padding = 5
    minX = math.max(0, minX - padding)
    maxX = math.min(camera.ViewportSize.X, maxX + padding)
    minY = math.max(0, minY - padding)
    maxY = math.min(camera.ViewportSize.Y, maxY + padding)
    
    -- 更新UI位置和大小
    esp.Size = UDim2.new(0, maxX - minX, 0, maxY - minY)
    esp.Position = UDim2.new(0, minX, 0, minY)
    esp.Visible = true
    
    return true
end

-- 清理所有ESP
local function ClearESP()
    for part, data in pairs(espCache) do
        RecycleESP(data.esp)
        espCache[part] = nil
    end
end

-- 主更新循环
local function UpdateESPList()
    local currentParts = {}
    
    -- 遍历所有工具
    for _, obj in ipairs(workspace:GetChildren()) do
        if obj:IsA("Tool") then
            local val = obj:GetAttribute("ItemValue")
            if type(val) == "number" and val > 0 then
                local handle = obj:FindFirstChildOfClass("Part")
                if handle and handle.Parent then
                    currentParts[handle] = true
                    
                    if not espCache[handle] then
                        -- 新增
                        local esp = GetESP(handle, val)
                        espCache[handle] = {
                            esp = esp,
                            value = val
                        }
                    end
                end
            end
        end
    end
    
    -- 移除不存在的
    for part, data in pairs(espCache) do
        if not currentParts[part] then
            RecycleESP(data.esp)
            espCache[part] = nil
        end
    end
    
    -- 更新所有ESP位置
    for part, data in pairs(espCache) do
        UpdateESP(data.esp, part)
    end
end

-- 监听新物品
workspace.ChildAdded:Connect(function(child)
    if child:IsA("Tool") then
        task.wait(0.1) -- 等待部件加载
        local val = child:GetAttribute("ItemValue")
        if type(val) == "number" and val > 0 then
            local handle = child:FindFirstChildOfClass("Part")
            if handle and not espCache[handle] then
                local esp = GetESP(handle, val)
                espCache[handle] = {
                    esp = esp,
                    value = val
                }
            end
        end
    end
end)

-- 物品移除时清理
workspace.ChildRemoved:Connect(function(child)
    if child:IsA("Tool") then
        local handle = child:FindFirstChildOfClass("Part")
        if handle and espCache[handle] then
            RecycleESP(espCache[handle].esp)
            espCache[handle] = nil
        end
    end
end)

-- 每帧更新（红框需要实时跟随）
RunService.RenderStepped:Connect(function()
    UpdateESPList()
end)

-- 窗口大小变化时重新计算
camera:GetPropertyChangedSignal("ViewportSize"):Co
