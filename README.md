-- Variables del menú
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local DamageMultiplier = 1
local DefenseMultiplier = 1

-- Crear el menú
local ScreenGui = Instance.new("ScreenGui", Player.PlayerGui)
ScreenGui.Name = "StatModifierMenu"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0.3, 0, 0.4, 0)
Frame.Position = UDim2.new(0.35, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.new(0, 0, 0)
Frame.BorderSizePixel = 2

local Title = Instance.new("TextLabel", Frame)
Title.Text = "Modificador de Stats"
Title.Size = UDim2.new(1, 0, 0.2, 0)
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.SourceSansBold
Title.TextScaled = true

local DamageButton = Instance.new("TextButton", Frame)
DamageButton.Text = "Multiplicar Daño: x1"
DamageButton.Size = UDim2.new(0.8, 0, 0.2, 0)
DamageButton.Position = UDim2.new(0.1, 0, 0.3, 0)
DamageButton.TextColor3 = Color3.new(1, 1, 1)
DamageButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
DamageButton.Font = Enum.Font.SourceSansBold
DamageButton.TextScaled = true

local DefenseButton = Instance.new("TextButton", Frame)
DefenseButton.Text = "Multiplicar Defensa: x1"
DefenseButton.Size = UDim2.new(0.8, 0, 0.2, 0)
DefenseButton.Position = UDim2.new(0.1, 0, 0.6, 0)
DefenseButton.TextColor3 = Color3.new(1, 1, 1)
DefenseButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
DefenseButton.Font = Enum.Font.SourceSansBold
DefenseButton.TextScaled = true

-- Funciones de los botones
DamageButton.MouseButton1Click:Connect(function()
    DamageMultiplier = DamageMultiplier + 1
    DamageButton.Text = "Multiplicar Daño: x" .. DamageMultiplier
end)

DefenseButton.MouseButton1Click:Connect(function()
    DefenseMultiplier = DefenseMultiplier + 1
    DefenseButton.Text = "Multiplicar Defensa: x" .. DefenseMultiplier
end)

-- Aplicar multiplicadores en combate
local function ApplyDamageMultiplier(target)
    if target and target:FindFirstChild("Humanoid") then
        local humanoid = target.Humanoid
        humanoid:TakeDamage(10 * DamageMultiplier) -- Ajusta 10 al daño base que usas
    end
end

local function ApplyDefenseMultiplier(damage)
    return damage / DefenseMultiplier
end

-- Evento de ataque
local Tool = Character:FindFirstChildOfClass("Tool") -- Asegúrate de tener un arma equipada
if Tool then
    Tool.Activated:Connect(function()
        local Target = --[Define cómo identificas el objetivo, como raycasting o proximity check]--
        ApplyDamageMultiplier(Target)
    end)
end

-- Evento de recibir daño
Character.Humanoid:GetPropertyChangedSignal("Health"):Connect(function()
    local humanoid = Character.Humanoid
    if humanoid.Health < humanoid.MaxHealth then
        local damageTaken = humanoid.MaxHealth - humanoid.Health
        humanoid.Health = humanoid.Health + ApplyDefenseMultiplier(damageTaken)
    end
end)
