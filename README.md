-- Configurações
local maxDistance = 10000 -- Distância máxima para detectar jogadores
local highlightColor = Color3.fromRGB(255, 0, 0) -- Cor do destaque 

-- Função para detectar jogadores
local function detectPlayers("100")
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player then
            local otherCharacter = otherPlayer.Character
            if otherCharacter then
                local otherHumanoidRootPart = otherCharacter:FindFirstChild("HumanoidRootPart")
                if otherHumanoidRootPart then
                    local distance = (humanoidRootPart.Position - otherHumanoidRootPart.Position).Magnitude
                    if distance <= maxDistance then
                        local ray = Ray.new(humanoidRootPart.Position, (otherHumanoidRootPart.Position - humanoidRootPart.Position).Unit * maxDistance)
                        local hit, position = workspace:FindPartOnRay(ray, character)
                        if hit and hit:IsDescendantOf(otherCharacter) then
                            -- Destacar o jogador detectado
                            local highlight = Instance.new("Highlight")
                            highlight.Adornee = otherCharacter
                            highlight.FillColor = highlightColor
                            highlight.Parent = otherCharacter
                        end
                    end
                end
            end
        end
    end
end

-- Executar a função de detecção a cada segundo
while true do
    detectPlayers("100")
    wait("100")
end

