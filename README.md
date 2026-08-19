--[[
    超级物品透视ESP - 穿墙 + 价值 + UI控制
]]

local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local camera = workspace.CurrentCamera

-- ===== 创建主GUI =====
local mainGui = Instance.new("ScreenGui")
mainGui.Name = "ItemESP"
mainGui.ResetOnSpawn = false
mainGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
mainGui.Parent = CoreGui

-- ===== 设置 =====
local settings = {
    Enabled = true,
    ShowBox = true,
    ShowValue = true,
    ShowDistance = true,
    BoxColor = Color3.new(1, 0, 0),
    TextColor = Color3.new(1, 0.92, 0),
    MaxDistance = 1000
}
-- ===== 创建UI控制面板 =====
local function CreateUI()
    local uiFrame = Instance.new("Frame")
    uiFrame.Name = "ControlPanel"
    uiFrame.Size = UDim2.new(0, 230, 0, 250)
    uiFrame.Position = UDim2.new(0, 10, 0, 10)
    uiFrame.BackgroundColor3 = Color3.new(0.08, 0.08, 0.08)
    uiFrame.BackgroundTransparency = 0.1
    uiFrame.BorderSizePixel = 2
    uiFrame.BorderColor3 = Color3.new(0.3, 0.3, 0.3)
    uiFrame.Active = true
    uiFrame.Draggable = true
    uiFrame.Parent = mainGui
    
    -- 标题
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 35)
    title.Position = UDim2.new(0, 0, 0, 0)
    title.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
    title.BackgroundTransparency = 0.5
    title.Text = "🎯 物品透视ESP"
    title.TextColor3 = Color3.new(1, 1, 1)
    title.Font = Enum.Font.GothamBold
    title.TextSize = 16
    title.Parent = uiFrame
    
    -- 开关函数
    local function CreateToggle(parent, text, yPos, settingKey)
        local toggle = Instance.new("TextButton")
        toggle.Size = UDim2.new(0.9, 0, 0, 30)
        toggle.Position = UDim2.new(0.05, 0, 0, yPos)
        toggle.BackgroundColor3 = Color3.new(0.15, 0.15, 0.15)
        toggle.BackgroundTransparency = 0.3
        toggle.Text = "✅ " .. text
        toggle.TextColor3 = Color3.new(1, 1, 1)
        toggle.Font = Enum.Font.Gotham
        toggle.TextSize = 13
        toggle.Parent = parent
        
        toggle.MouseButton1Click:Connect(function()
            settings[settingKey] = not settings[settingKey]
            toggle.Text = (settings[settingKey] and "✅ " or "❌ ") .. text
        end)
        
        return toggle
    end
    
    CreateToggle(uiFrame, "启用ESP", 40, "Enabled")
    CreateToggle(uiFrame, "显示红框", 75, "ShowBox")
    CreateToggle(uiFrame, "显示价值 💰", 110, "ShowValue")
    CreateToggle(uiFrame, "显示距离 📏", 145, "ShowDistance")
    
    -- 关闭按钮
    local closeBtn = Instance.new("TextButton")
    closeBtn.Size = UDim2.new(0, 35, 0, 35)
    closeBtn.Position = UDim2.new(1, -40, 0, 0)
    closeBtn.BackgroundColor3 = Color3.new(1, 0, 0)
    closeBtn.BackgroundTransparency = 0.3
    closeBtn.Text = "✕"
    closeBtn.TextColor3 = Color3.new(1, 1, 1)
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.TextSize = 18
    closeBtn.Parent = uiFrame
    
    closeBtn.MouseButton1Click:Connect(function()
        uiFrame.Visible = not uiFrame.Visible
    end)
    
    -- 快捷键 F1
    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end
        if input.KeyCode == Enum.KeyCode.F1 then
            uiFrame.Visible = not uiFrame.Visible
        end
    end)
end

CreateUI()
-- ===== 创建ESP元素 =====
local espCache = {}

local function CreateESP(part, value, distance)
    local container = Instance.new("Frame")
    container.Size = UDim2.new(0, 0, 0, 0)
    container.BackgroundTransparency = 1
    container.Parent = mainGui
    container.ZIndex = 10
    
    -- 红框（4条边）
    local function MakeBorder(parent, size, position, color)
        local border = Instance.new("Frame")
        border.Size = size
        border.Position = position
        border.BackgroundColor3 = color or settings.BoxColor
        border.BackgroundTransparency = 0.15
        border.BorderSizePixel = 0
        border.Parent = parent
        return border
    end
    
    -- 边框
    MakeBorder(container, UDim2.new(1, 0, 0, 2.5), UDim2.new(0, 0, 0, 0))
    MakeBorder(container, UDim2.new(1, 0, 0, 2.5), UDim2.new(0, 0, 1, -2.5))
    MakeBorder(container, UDim2.new(0, 2.5, 1, 0), UDim2.new(0, 0, 0, 0))
    MakeBorder(container, UDim2.new(0, 2.5, 1, 0), UDim2.new(1, -2.5, 0, 0))
    
    -- 发光光晕
    local glow = Instance.new("Frame")
    glow.Size = UDim2.new(1.4, 0, 1.4, 0)
    glow.Position = UDim2.new(-0.2, 0, -0.2, 0)
    glow.BackgroundColor3 = settings.BoxColor
    glow.BackgroundTransparency = 0.92
    glow.BorderSizePixel = 0
    glow.Parent = container
    
    -- 价值标签（上方）
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 120, 0, 22)
    label.Position = UDim2.new(0.5, -60, 0, -28)
    label.BackgroundTransparency = 1
    label.Text = string.format("💰 %d", value)
    label.TextColor3 = settings.TextColor
    label.Font = Enum.Font.GothamBold
    label.TextSize = 14
    label.TextStrokeTransparency = 0.2
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.Parent = container
    
    -- 距离标签（下方）
    local distLabel = Instance.new("TextLabel")
    distLabel.Size = UDim2.new(0, 80, 0, 18)
    distLabel.Position = UDim2.new(0.5, -40, 1, 5)
    distLabel.BackgroundTransparency = 1
    distLabel.Text = string.format("%dm", math.floor(distance))
    distLabel.TextColor3 = Color3.new(0.5, 0.8, 1)
    distLabel.Font = Enum.Font.Gotham
    distLabel.TextSize = 12
    distLabel.TextStrokeTransparency = 0.3
    distLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
    distLabel.Parent = container
    
    return container
end
-- ===== 更新ESP =====
local function UpdateESP()
    if not settings.Enabled then
        for part, data in pairs(espCache) do
            data.esp.Visible = false
        end
        return
    end
    
    local currentParts = {}
    local playerPos = player.Character and player.Character.PrimaryPart and player.Character.PrimaryPart.Position or Vector3.new(0, 0, 0)
    
    for _, obj in ipairs(workspace:GetChildren()) do
        if obj:IsA("Tool") then
            local value = obj:GetAttribute("ItemValue")
            if type(value) == "number" and value > 0 then
                local handle = obj:FindFirstChildOfClass("Part")
                if handle and handle.Parent then
                    local distance = (handle.Position - playerPos).Magnitude
                    if distance <= settings.MaxDistance then
                        currentParts[handle] = true
                        
                        if not espCache[handle] then
                            local esp = CreateESP(handle, value, distance)
                            espCache[handle] = { esp = esp, value = value }
                        end
                        
                        -- 更新距离
                        local labels = {}
                        for _, child in ipairs(espCache[handle].esp:GetChildren()) do
                            if child:IsA("TextLabel") then table.insert(labels, child) end
                        end
                        if labels[2] then
                            labels[2].Text = string.format("%dm", math.floor(distance))
                        end
                    end
                end
            end
        end
    end
    
    -- 移除不存在的
    for part, data in pairs(espCache) do
        if not currentParts[part] then
            data.esp:Destroy()
            espCache[part] = nil
        end
    end
    
    -- 更新位置（穿墙）
    for part, data in pairs(espCache) do
        local screenPos, onScreen = camera:WorldToViewportPoint(part.Position)
        
        if screenPos.Z > 0 then
            local size = part.Size
            local halfSize = size / 2
            
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
            
            if minX == math.huge then
                local center = camera:WorldToViewportPoint(part.Position)
                minX = center.X - 30
                maxX = center.X + 30
                minY = center.Y - 30
                maxY = center.Y + 30
            end
            
            local padding = 8
            minX = math.max(0, minX - padding)
            maxX = math.min(camera.ViewportSize.X, maxX + padding)
            minY = math.max(0, minY - padding)
            maxY = math.min(camera.ViewportSize.Y, maxY + padding)
            
            data.esp.Size = UDim2.new(0, maxX - minX, 0, maxY - minY)
            data.esp.Position = UDim2.new(0, minX, 0, minY)
            data.esp.Visible = true
            
            -- 控制显示
            for _, child in ipairs(data.esp:GetChildren()) do
                if child:IsA("Frame") then
                    child.Visible = settings.ShowBox
                elseif child:IsA("TextLabel") then
                    if child.Text:match("💰") then
                        child.Visible = settings.ShowValue
                    else
                        child.Visible = settings.ShowDistance
                    end
                end
            end
        else
            data.esp.Visible = false
        end
    end
end

-- ===== 事件监听 =====
workspace.ChildAdded:Connect(function(child)
    task.wait(0.1)
    if child:IsA("Tool") then
        local value = child:GetAttribute("ItemValue")
        if type(value) == "number" and value > 0 then
            local handle = child:FindFirstChildOfClass("Part")
            if handle and not espCache[handle] then
                local esp = CreateESP(handle, value, 0)
                espCache[handle] = { esp = esp, value = value }
            end
        end
    end
end)

workspace.ChildRemoved:Connect(function(child)
    if child:IsA("Tool") then
        local handle = child:FindFirstChildOfClass("Part")
        if handle and espCache[handle] then
            espCache[handle].esp:Destroy()
            espCache[handle] = nil
        end
    end
end)

-- ===== 主循环 =====
RunService.RenderStepped:Connect(UpdateESP)

camera:GetPropertyChangedSignal("ViewportSize"):Connect(function()
    for part, data in pairs(espCache) do
        data.esp:Destroy()
    end
    espCache = {}
end)

print("✅ 物品透视ESP已加载")
print("📌 按 F1 切换控制面板")
print("📌 穿墙显示所有带ItemValue的物品")
