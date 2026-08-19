--[[
    诊断ESP - 显示所有物品信息
]]

local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local camera = workspace.CurrentCamera

print("========== 诊断ESP开始 ==========")

-- 创建GUI
local gui = Instance.new("ScreenGui")
gui.Name = "DiagnosticESP"
gui.ResetOnSpawn = false
gui.Parent = CoreGui

-- 缓存
local cache = {}
local alreadyLogged = {}  -- 防止重复打印
-- ===== 创建标签 =====
local function CreateLabel(part, info, color)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 0, 0, 0)
    frame.BackgroundTransparency = 1
    frame.Parent = gui
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 350, 0, 30)
    label.Position = UDim2.new(0.5, -175, 0, -15)
    label.BackgroundTransparency = 1
    label.Text = info
    label.TextColor3 = color or Color3.new(1, 1, 0)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 11
    label.TextStrokeTransparency = 0.2
    label.TextStrokeColor3 = Color3.new(0, 0, 0)
    label.Parent = frame
    
    return frame
end

-- ===== 获取物品信息 =====
local function GetItemInfo(obj)
    local info = {
        Name = obj.Name,
        Class = obj.ClassName,
        Attributes = {}
    }
    
    -- 读取所有属性
    local attrs = obj:GetAttributes()
    for k, v in pairs(attrs) do
        info.Attributes[k] = v
    end
    
    -- 如果有ItemValue突出显示
    if info.Attributes.ItemValue then
        info.Value = info.Attributes.ItemValue
    end
    
    return info
end
-- ===== 扫描和更新 =====
local function UpdateDiagnostic()
    local current = {}
    
    -- 遍历所有对象
    for _, obj in ipairs(workspace:GetDescendants()) do
        -- 只关注 Part 和 带Part的Model/Tool
        local isRelevant = obj:IsA("Part") or obj:IsA("Tool") or obj:IsA("Model")
        if not isRelevant then continue end
        
        -- 跳过角色、地形等
        if obj:IsA("Part") and obj.Parent and obj.Parent:IsA("Character") then
            continue
        end
        
        -- 获取主Part（如果是Model/Tool则找Part）
        local part = obj
        if obj:IsA("Model") or obj:IsA("Tool") then
            part = obj:FindFirstChildOfClass("Part")
        end
        if not part or not part:IsA("Part") then 
            continue 
        end
        
        -- 获取信息
        local info = GetItemInfo(obj)
        
        -- 如果有价值或者有属性，才显示
        if not next(info.Attributes) and not info.Value then
            -- 但如果是Part，也显示，为了调试
            if obj:IsA("Part") then
                -- 但跳过普通地板
                if obj.Name == "Part" or obj.Name == "Baseplate" then
                    continue
                end
            else
                continue
            end
        end
        
        current[part] = true
        
        -- 创建标签（如果不存在）
        if not cache[part] then
            -- 构建显示文本
            local text = string.format("[%s] %s", info.Class, info.Name)
            if info.Value then
                text = text .. " | 💰" .. info.Value
            end
            for k, v in pairs(info.Attributes) do
                if k ~= "ItemValue" then
                    text = text .. string.format(" | %s=%s", k, tostring(v))
                end
            end
            
            -- 控制台打印一次（不重复）
            if not alreadyLogged[part] then
                print(text)
                alreadyLogged[part] = true
            end
            
            -- 颜色：有价值用金色，否则白色
            local color = info.Value and Color3.new(1, 0.92, 0) or Color3.new(1, 1, 1)
            cache[part] = CreateLabel(part, text, color)
        end
    end
    
    -- 清理已移除的
    for part, frame in pairs(cache) do
        if not current[part] then
            frame:Destroy()
            cache[part] = nil
        end
    end
    
    -- 更新位置
    for part, frame in pairs(cache) do
        local pos, onScreen = camera:WorldToViewportPoint(part.Position)
        if onScreen and pos.Z > 0 then
            frame.Position = UDim2.new(0, pos.X - 175, 0, pos.Y - 15)
            frame.Visible = true
        else
            frame.Visible = false
        end
    end
end

-- ===== 运行 =====
RunService.RenderStepped:Connect(UpdateDiagnostic)

print("✅ 诊断ESP已启动 - 显示所有带属性的物品")
print("📌 查看控制台输出获取详细信息")
