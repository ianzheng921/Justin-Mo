# Justin-Mo
HEHE
-- Language: Lua (Roblox scripting)
-- Place this script inside ServerScriptService

-- Table to store player scores
local playerScores = {}

-- Function to create a Coin at a specific position
local function createCoin(position)
    local coin = Instance.new("Part")
    coin.Shape = Enum.PartType.Ball
    coin.Size = Vector3.new(2, 2, 2)
    coin.Position = position
    coin.Name = "Coin"
    coin.Anchored = true
    coin.CanCollide = false
    coin.BrickColor = BrickColor.new("Bright yellow")
    coin.Parent = workspace

    -- Touch event to detect player collecting
    coin.Touched:Connect(function(hit)
        local player = game.Players:GetPlayerFromCharacter(hit.Parent)
        if player then
            -- Increase player score
            playerScores[player.UserId] = (playerScores[player.UserId] or 0) + 1
            print(player.Name .. " collected a coin! Total: " .. playerScores[player.UserId])
            coin:Destroy()
        end
    end)
end

-- Spawn multiple coins randomly
for i = 1, 10 do
    local x = math.random(-20, 20)
    local y = 5
    local z = math.random(-20, 20)
    createCoin(Vector3.new(x, y, z))
end

-- Optional: Show leaderboard for all players
game.Players.PlayerAdded:Connect(function(player)
    playerScores[player.UserId] = 0

    -- Simple leaderboard display
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local score = Instance.new("IntValue")
    score.Name = "Coins"
    score.Value = 0
    score.Parent = leaderstats

    -- Update coins collected in leaderboard
    player:GetPropertyChangedSignal("Coins"):Connect(function()
        score.Value = playerScores[player.UserId]
    end)
end)
