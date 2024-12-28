local ball = game.Workspace:WaitForChild("Ball")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local controlDistance = 40
local hitboxPart = nil

-- Função para criar a hitbox visual no mundo
local function createHitboxVisual()
    if not hitboxPart then
        hitboxPart = Instance.new("Part")
        hitboxPart.Shape = Enum.PartType.Ball
        hitboxPart.Size = Vector3.new(20, 20, 20)
        hitboxPart.Position = ball.Position
        hitboxPart.Color = Color3.fromRGB(255, 0, 0)
        hitboxPart.Anchored = true
        hitboxPart.CanCollide = false
        hitboxPart.Transparency = 0.5
        hitboxPart.Parent = game.Workspace
    end
end

-- Função para aumentar a hitbox da bola
local function enlargeHitbox()
    local sphere = Instance.new("Part")
    sphere.Shape = Enum.PartType.Ball
    sphere.Size = Vector3.new(20, 20, 20)
    sphere.Transparency = 0.5
    sphere.CanCollide = false
    sphere.Anchored = true
    sphere.Position = ball.Position
    sphere.Parent = game.Workspace

    -- Verifica a proximidade com o jogador
    if (sphere.Position - humanoidRootPart.Position).Magnitude <= controlDistance then
        ball.CFrame = humanoidRootPart.CFrame * CFrame.new(0, 0, 3)
    end

    wait(0.2)
    sphere:Destroy() -- Remove a esfera depois de um curto tempo
end

-- Função para verificar a proximidade da bola e criar a hitbox
local function checkProximity()
    local distance = (ball.Position - humanoidRootPart.Position).Magnitude
    if distance <= controlDistance then
        enlargeHitbox()   -- Aumenta a hitbox da bola
        createHitboxVisual()  -- Cria a visualização da hitbox
    elseif hitboxPart then
        hitboxPart:Destroy()  -- Remove a visualização da hitbox quando o jogador se afasta
    end
end

-- Chama a verificação de proximidade a cada frame
game:GetService("RunService").Heartbeat:Connect(checkProximity)
