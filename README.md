--[[
    AIMBOT VIDA NA PRISÃO - DELTA EXECUTOR
    COM BYPASS - VERSÃO SIMPLES
]]

-- ===== BYPASS =====
print("🔓 ATIVANDO BYPASS...")

pcall(function()
    game:GetService("CoreGui"):FindFirstChild("RobloxGui"):Destroy()
end)

pcall(function()
    for i, v in pairs(game:GetService("CoreGui"):GetChildren()) do
        if v.Name == "RobloxGui" then
            v:Destroy()
        end
    end
end)

pcall(function()
    getfenv().script = nil
end)

print("✅ BYPASS ATIVADO!")
print("🚀 INICIANDO AIMBOT VIDA NA PRISÃO...")

-- ===== CARREGAR =====
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- ===== VARIAVEIS =====
local ativo = false
local fov = 150
local gui = nil
local circulo = nil
local espLista = {}

-- ===== CRIAR GUI =====
local function CriarGUI()
    if gui then gui:Destroy() end
    
    gui = Instance.new("ScreenGui")
    gui.Name = "AimbotGUI"
    gui.Parent = LocalPlayer.PlayerGui
    gui.ResetOnSpawn = false
    
    -- ===== BOTÃO FLUTUANTE =====
    local btnFlutuante = Instance.new("TextButton")
    btnFlutuante.Size = UDim2.new(0, 40, 0, 40)
    btnFlutuante.Position = UDim2.new(0.02, 0, 0.1, 0)
    btnFlutuante.BackgroundColor3 = Color3.fromRGB(0, 200, 50)
    btnFlutuante.Text = "⚙️"
    btnFlutuante.TextColor3 = Color3.fromRGB(255, 255, 255)
    btnFlutuante.TextScaled = true
    btnFlutuante.Font = Enum.Font.GothamBold
    btnFlutuante.BorderSizePixel = 0
    btnFlutuante.Parent = gui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = btnFlutuante
    
    -- ===== PAINEL =====
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 250, 0, 150)
    frame.Position = UDim2.new(0.5, -125, 0.1, 0)
    frame.BackgroundColor3 = Color3.fromRGB(10, 10, 25)
    frame.BackgroundTransparency = 0.2
    frame.BorderSizePixel = 0
    frame.Parent = gui
    
    local frameCorner = Instance.new("UICorner")
    frameCorner.CornerRadius = UDim.new(0, 10)
    frameCorner.Parent = frame
    
    -- Titulo
    local titulo = Instance.new("TextLabel")
    titulo.Text = "⛓️ VIDA NA PRISÃO"
    titulo.Size = UDim2.new(1, 0, 0, 30)
    titulo.BackgroundTransparency = 1
    titulo.TextColor3 = Color3.fromRGB(255, 255, 255)
    titulo.TextScaled = true
    titulo.Font = Enum.Font.GothamBold
    titulo.Parent = frame
    
    -- Botão
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.85, 0, 0, 35)
    btn.Position = UDim2.new(0.075, 0, 0.25, 0)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
    btn.Text = "🔴 DESLIGADO"
    btn.TextColor3 = Color3.fromRGB(255, 100, 100)
    btn.TextScaled = true
    btn.Font = Enum.Font.GothamBold
    btn.BorderSizePixel = 0
    btn.Parent = frame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = btn
    
    -- LED
    local led = Instance.new("Frame")
    led.Size = UDim2.new(0, 12, 0, 12)
    led.Position = UDim2.new(0.85, 0, 0.5, -6)
    led.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    led.BorderSizePixel = 0
    led.Parent = btn
    
    local ledCorner = Instance.new("UICorner")
    ledCorner.CornerRadius = UDim.new(1, 0)
    ledCorner.Parent = led
    
    -- FOV
    local fovLabel = Instance.new("TextLabel")
    fovLabel.Text = "🎯 FOV:"
    fovLabel.Size = UDim2.new(0.3, 0, 0, 25)
    fovLabel.Position = UDim2.new(0.075, 0, 0.55, 0)
    fovLabel.BackgroundTransparency = 1
    fovLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    fovLabel.TextScaled = true
    fovLabel.Font = Enum.Font.Gotham
    fovLabel.Parent = frame
    
    local fovBox = Instance.new("TextBox")
    fovBox.Size = UDim2.new(0.3, 0, 0, 25)
    fovBox.Position = UDim2.new(0.5, 0, 0.53, 0)
    fovBox.BackgroundColor3 = Color3.fromRGB(30, 30, 50)
    fovBox.Text = "150"
    fovBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    fovBox.TextScaled = true
    fovBox.Font = Enum.Font.Gotham
    fovBox.TextXAlignment = Enum.TextXAlignment.Center
    fovBox.BorderSizePixel = 0
    fovBox.Parent = frame
    
    local fovCorner = Instance.new("UICorner")
    fovCorner.CornerRadius = UDim.new(0, 6)
    fovCorner.Parent = fovBox
    
    -- Fechar
    local close = Instance.new("TextButton")
    close.Text = "✕"
    close.Size = UDim2.new(0, 25, 0, 25)
    close.Position = UDim2.new(0.9, 0, 0.01, 0)
    close.BackgroundTransparency = 1
    close.TextColor3 = Color3.fromRGB(255, 80, 80)
    close.TextScaled = true
    close.Font = Enum.Font.Gotham
    close.BorderSizePixel = 0
    close.Parent = frame
    
    -- ===== EVENTOS =====
    btnFlutuante.MouseButton1Click:Connect(function()
        frame.Visible = not frame.Visible
        if frame.Visible then
            btnFlutuante.Text = "⚙️"
            btnFlutuante.BackgroundColor3 = Color3.fromRGB(0, 200, 50)
        else
            btnFlutuante.Text = "📌"
            btnFlutuante.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        end
    end)
    
    close.MouseButton1Click:Connect(function()
        gui:Destroy()
        if circulo then circulo:Destroy() end
        for _, esp in pairs(espLista) do
            esp:Destroy()
        end
        espLista = {}
        print("❌ FECHADO!")
    end)
    
    btn.MouseButton1Click:Connect(function()
        ativo = not ativo
        
        if ativo then
            btn.Text = "🟢 LIGADO"
            btn.TextColor3 = Color3.fromRGB(100, 255, 100)
            led.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
            btn.BackgroundColor3 = Color3.fromRGB(30, 60, 40)
            if circulo then circulo.Visible = true end
            print("✅ AIMBOT LIGADO!")
        else
            btn.Text = "🔴 DESLIGADO"
            btn.TextColor3 = Color3.fromRGB(255, 100, 100)
            led.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
            btn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
            if circulo then circulo.Visible = false end
            print("❌ AIMBOT DESLIGADO!")
        end
    end)
    
    fovBox.FocusLost:Connect(function()
        local val = tonumber(fovBox.Text)
        if val and val > 0 then
            fov = val
            if circulo then
                local tamanho = fov * 2
                circulo.Size = UDim2.new(0, tamanho, 0, tamanho)
                circulo.Position = UDim2.new(0.5, -fov, 0.5, -fov)
            end
        else
            fovBox.Text = tostring(fov)
        end
    end)
    
    return btn, fovBox
end

-- ===== CRIAR CIRCULO PRETO =====
local function CriarCirculo()
    if circulo then circulo:Destroy() end
    
    circulo = Instance.new("Frame")
    circulo.Size = UDim2.new(0, fov * 2, 0, fov * 2)
    circulo.Position = UDim2.new(0.5, -fov, 0.5, -fov)
    circulo.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    circulo.BackgroundTransparency = 1
    circulo.BorderSizePixel = 0
    circulo.Visible = false
    circulo.Parent = gui
    
    -- Deixa redondo
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = circulo
    
    -- Borda preta
    local borda = Instance.new("Frame")
    borda.Size = UDim2.new(1, 0, 1, 0)
    borda.BackgroundTransparency = 1
    borda.BorderSizePixel = 3
    borda.BorderColor3 = Color3.fromRGB(0, 0, 0)
    borda.Parent = circulo
    
    local bordaCorner = Instance.new("UICorner")
    bordaCorner.CornerRadius = UDim.new(1, 0)
    bordaCorner.Parent = borda
    
    -- Ponto vermelho (mira)
    local ponto = Instance.new("Frame")
    ponto.Size = UDim2.new(0, 4, 0, 4)
    ponto.Position = UDim2.new(0.5, -2, 0.5, -2)
    ponto.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    ponto.BorderSizePixel = 0
    ponto.Parent = circulo
    
    local pontoCorner = Instance.new("UICorner")
    pontoCorner.CornerRadius = UDim.new(1, 0)
    pontoCorner.Parent = ponto
    
    return circulo
end

-- ===== BARRA DE VIDA =====
local healthFill = nil
local healthText = nil

local function CriarVida()
    local frame = Instance.new("Frame")
    frame.Name = "HealthBar"
    frame.Size = UDim2.new(0, 200, 0, 25)
    frame.Position = UDim2.new(0.02, 0, 0.93, 0)
    frame.BackgroundColor3 = Color3.fromRGB(10, 10, 25)
    frame.BackgroundTransparency = 0.3
    frame.BorderSizePixel = 0
    frame.Parent = gui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = frame
    
    healthFill = Instance.new("Frame")
    healthFill.Name = "Fill"
    healthFill.Size = UDim2.new(1, 0, 1, 0)
    healthFill.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
    healthFill.BorderSizePixel = 0
    healthFill.Parent = frame
    
    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(0, 10)
    fillCorner.Parent = healthFill
    
    healthText = Instance.new("TextLabel")
    healthText.Name = "Text"
    healthText.Size = UDim2.new(1, 0, 1, 0)
    healthText.BackgroundTransparency = 1
    healthText.Text = "❤️ 100%"
    healthText.TextColor3 = Color3.fromRGB(255, 255, 255)
    healthText.TextScaled = true
    healthText.Font = Enum.Font.GothamBold
    healthText.Parent = healthFill
    
    return healthFill, healthText
end

-- ===== ATUALIZAR VIDA =====
local function AtualizarVida()
    if not healthFill or not healthText then return end
    
    local character = LocalPlayer.Character
    if not character then return end
    
    local humanoid = character:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    local percent = humanoid.Health / humanoid.MaxHealth
    healthFill.Size = UDim2.new(percent, 0, 1, 0)
    
    if percent > 0.5 then
        healthFill.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
    elseif percent > 0.25 then
        healthFill.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    else
        healthFill.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    end
    
    healthText.Text = "❤️ " .. math.floor(percent * 100) .. "%"
end

-- ===== ESP =====
local function CriarESP(jogador)
    if jogador == LocalPlayer then return end
    if not jogador.Character then return end
    
    if espLista[jogador] then
        espLista[jogador]:Destroy()
        espLista[jogador] = nil
    end
    
    local esp = Instance.new("Frame")
    esp.Size = UDim2.new(0, 60, 0, 45)
    esp.BackgroundTransparency = 1
    esp.Parent = gui
    
    local bg = Instance.new("Frame")
    bg.Size = UDim2.new(1, 0, 1, 0)
    bg.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    bg.BackgroundTransparency = 0.5
    bg.BorderSizePixel = 0
    bg.Parent = esp
    
    local bgCorner = Instance.new("UICorner")
    bgCorner.CornerRadius = UDim.new(0, 5)
    bgCorner.Parent = bg
    
    local nome = Instance.new("TextLabel")
    nome.Size = UDim2.new(1, 0, 0, 18)
    nome.Position = UDim2.new(0, 0, 0, 0)
    nome.BackgroundTransparency = 1
    nome.Text = jogador.Name
    nome.TextColor3 = Color3.fromRGB(255, 255, 255)
    nome.TextScaled = true
    nome.Font = Enum.Font.GothamBold
    nome.Parent = esp
    
    local vida = Instance.new("Frame")
    vida.Name = "Health"
    vida.Size = UDim2.new(0.8, 0, 0, 4)
    vida.Position = UDim2.new(0.1, 0, 0.85, 0)
    vida.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
    vida.BorderSizePixel = 0
    vida.Parent = esp
    
    local vidaCorner = Instance.new("UICorner")
    vidaCorner.CornerRadius = UDim.new(0, 2)
    vidaCorner.Parent = vida
    
    -- Distância
    local dist = Instance.new("TextLabel")
    dist.Name = "Dist"
    dist.Size = UDim2.new(1, 0, 0, 15)
    dist.Position = UDim2.new(0, 0, 0.65, 0)
    dist.BackgroundTransparency = 1
    dist.Text = "0m"
    dist.TextColor3 = Color3.fromRGB(200, 200, 200)
    dist.TextScaled = true
    dist.Font = Enum.Font.Gotham
    dist.Parent = esp
    
    espLista[jogador] = esp
    return esp
end

-- ===== ATUALIZAR ESP =====
local function AtualizarESP()
    for _, jogador in ipairs(Players:GetPlayers()) do
        if jogador ~= LocalPlayer then
            if jogador.Character and jogador.Character:FindFirstChild("Head") then
                if not espLista[jogador] then
                    CriarESP(jogador)
                end
                
                local esp = espLista[jogador]
                if esp then
                    local cabeca = jogador.Character.Head
                    local pos, naTela = Camera:WorldToScreenPoint(cabeca.Position + Vector3.new(0, 2, 0))
                    
                    if naTela then
                        esp.Visible = true
                        esp.Position = UDim2.new(0, pos.X - 30, 0, pos.Y - 45)
                        
                        -- Distância
                        local dist = esp:FindFirstChild("Dist")
                        if dist and jogador.Character:FindFirstChild("HumanoidRootPart") then
                            local distancia = (Camera.CFrame.Position - jogador.Character.HumanoidRootPart.Position).Magnitude
                            dist.Text = string.format("%.0fm", distancia)
                        end
                        
                        local vida = esp:FindFirstChild("Health")
                        if vida and jogador.Character:FindFirstChild("Humanoid") then
                            local humanoid = jogador.Character.Humanoid
                            local percent = humanoid.Health / humanoid.MaxHealth
                            vida.Size = UDim2.new(0.8 * percent, 0, 0, 4)
                            
                            if percent > 0.5 then
                                vida.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
                            elseif percent > 0.25 then
                                vida.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
                            else
                                vida.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
                            end
                        end
                    else
                        esp.Visible = false
                    end
                end
            else
                if espLista[jogador] then
                    espLista[jogador]:Destroy()
                    espLista[jogador] = nil
                end
            end
        end
    end
end

-- ===== PEGAR INIMIGO =====
local function PegarInimigo()
    if not Camera then return nil end
    
    local centro = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    local maisPerto = nil
    local menorDist = fov + 1
    
    for _, jogador in ipairs(Players:GetPlayers()) do
        if jogador ~= LocalPlayer and jogador.Character then
            -- Pula aliados
            if LocalPlayer.Team and jogador.Team then
                if jogador.Team == LocalPlayer.Team then
                    continue
                end
            end
            
            local cabeca = jogador.Character:FindFirstChild("Head")
            if cabeca then
                local pos, naTela = Camera:WorldToScreenPoint(cabeca.Position)
                if naTela then
                    local dist = (Vector2.new(pos.X, pos.Y) - centro).Magnitude
                    if dist < fov and dist < menorDist then
                        menorDist = dist
                        maisPerto = jogador
                    end
                end
            end
        end
    end
    
    return maisPerto
end

-- ===== AIMBOT =====
local function Aimbot()
    if not ativo or not Camera then return end
    
    local alvo = PegarInimigo()
    if alvo and alvo.Character then
        local cabeca = alvo.Character:FindFirstChild("Head")
        if cabeca then
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, cabeca.Position)
        end
    end
end

-- ===== LOOP =====
RunService.RenderStepped:Connect(function()
    Aimbot()
    AtualizarVida()
    AtualizarESP()
end)

-- ===== INICIAR =====
print("⛓️ INICIANDO VIDA NA PRISÃO...")

CriarGUI()
CriarVida()
CriarCirculo()

print("✅ PRONTO!")
print("🎯 Clique em LIGADO para ativar")
print("⭕ CÍRCULO PRETO CENTRALIZADO")
print("📌 Botão ⚙️ abre/fecha")
print("🔓 BYPASS ATIVADO!")

-- ===== COMANDOS =====
_G.AimbotPrisao = {
    Ligar = function()
        local btn = gui:FindFirstChild("MainFrame"):FindFirstChild("TextButton")
        if btn then btn.MouseButton1Click:Fire() end
    end,
    SetFOV = function(val)
        local box = gui:FindFirstChild("MainFrame"):FindFirstChild("TextBox")
        if box then
            box.Text = tostring(val)
            box.FocusLost:Fire()
        end
    end,
    AbrirInterface = function()
        local btn = gui:FindFirstChild("TextButton")
        if btn then btn.MouseButton1Click:Fire() end
    end,
    Fechar = function()
        local close = gui:FindFirstChild("MainFrame"):FindFirstChild("TextButton")
        if close then close.MouseButton1Click:Fire() end
    end
}

print("📌 COMANDOS:")
print("  _G.AimbotPrisao.Ligar() - Liga/Desliga")
print("  _G.AimbotPrisao.SetFOV(150) - Muda FOV")
print("  _G.AimbotPrisao.AbrirInterface() - Abre/Fecha")
print("  _G.AimbotPrisao.Fechar() - Fecha o script")
