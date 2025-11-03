print("Полный Halloween Авто запущен!")

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

-- Удаление старого скрипта
if _G.HalloweenAuto then
    _G.HalloweenAuto:Disconnect()
    print("Старый скрипт удалён.")
end
_G.HalloweenAuto = true

-- Переменные
local isEnabled = true
local bindKey = Enum.KeyCode.M

-- === TRICK OR TREAT ===
local trickLocations = {
    { coords = Vector3.new(-4961.17, 16.33 - 5, -690.97), house = "House Small" },
    { coords = Vector3.new(-4986.03, 16.32 - 5, -691.51), house = "House Big" },
    { coords = Vector3.new(-5054.26, 22.31 - 5, -695.03), house = "House Small" },
    { coords = Vector3.new(-5079.87, 22.31 - 4, -687.86), house = "House Big" },
    { coords = Vector3.new(-5035.50, 22.37 - 5, -622.73), house = "House Big" },
    { coords = Vector3.new(-5054.29, 22.37 - 5, -597.16), house = "House Small" },
    { coords = Vector3.new(-5057.27, 25.16 - 5, -555.29), house = "House Small" },
    { coords = Vector3.new(-5035.31, 25.16 - 5, -547.13), house = "House Big" },
    { coords = Vector3.new(-4963.71, 19.08 - 5, -460.12), house = "House Big" },
    { coords = Vector3.new(-4937.12, 19.08 - 5, -450.49), house = "House Small" },
    { coords = Vector3.new(-4888.41, 21.87 - 5, -465.69), house = "House Big" },
    { coords = Vector3.new(-4890.34, 21.87 - 5, -486.73), house = "House Small" }
}

local teleportTween = TweenInfo.new(0.3, Enum.EasingStyle.Linear)
local inoutTween = TweenInfo.new(0.25, Enum.EasingStyle.Sine)

-- === EGG FARM ===
local function hatchEgg()
    local halloween = workspace:FindFirstChild("HalloweenEvent")
    if not halloween then return end
    local summoned = halloween:FindFirstChild("SummonedEgg")
    if not summoned then return end
    local platform = summoned:FindFirstChild("EggPlatformSpawn")
    if not platform then return end

    local parts = {}
    for _, part in ipairs(platform:GetChildren()) do
        if part:IsA("BasePart") then
            table.insert(parts, part)
        end
    end
    if #parts == 0 then return end

    local oldCFrame = HumanoidRootPart.CFrame
    for _, part in ipairs(parts) do
        HumanoidRootPart.CFrame = part.CFrame
        pcall(function()
            local args = { [1] = "HatchEgg", [2] = "Mutant Egg", [3] = 9 }
            ReplicatedStorage.Shared.Framework.Network.Remote.RemoteEvent:FireServer(unpack(args))
        end)
        task.wait(0.01)
    end
    HumanoidRootPart.CFrame = oldCFrame
end

local function hatchFixed()
    local oldCFrame = HumanoidRootPart.CFrame
    HumanoidRootPart.CFrame = CFrame.new(-4938.87, 25.93 - 5, -548)
    pcall(function()
        local args = { [1] = "HatchEgg", [2] = "Mutant Egg", [3] = 9 }
        ReplicatedStorage.Shared.Framework.Network.Remote.RemoteEvent:FireServer(unpack(args))
    end)
    task.wait(0.01)
    HumanoidRootPart.CFrame = oldCFrame
end

-- === SHOP ===
local shopItems = {
    { shop = "halloween-shop", id = 1 }, { shop = "halloween-shop", id = 2 },
    { shop = "halloween-shop", id = 3 }, { shop = "halloween-shop", id = 4 },
    { shop = "halloween-shop", id = 5 }, { shop = "halloween-shop", id = 6 },
    { shop = "spooky-shop", id = 1 }, { shop = "spooky-shop", id = 2 },
    { shop = "spooky-shop", id = 3 }
}

local potions = {
    { name = "Halloween Infinity Elixir", count = 1 },
    { name = "Lucky", count = 6 },
    { name = "Speed", count = 6 },
    { name = "Halloween Elixir", count = 4 }
}

-- === ОСНОВНОЙ ЦИКЛ ===
spawn(function()
    local shopIndex = 1
    while true do
        if not isEnabled then
            task.wait(0.1)
            continue
        end

        -- TrickOrTreat
        if Character and HumanoidRootPart then
            for _, loc in ipairs(trickLocations) do
                if not isEnabled then break end
                local goal = {CFrame = CFrame.new(loc.coords)}
                local tween = TweenService:Create(HumanoidRootPart, teleportTween, goal)
                tween:Play()
                tween.Completed:Wait()

                pcall(function()
                    local args = { [1] = "TrickOrTreat", [2] = workspace.HalloweenEvent.Houses:FindFirstChild(loc.house) }
                    ReplicatedStorage.Shared.Framework.Network.Remote.RemoteEvent:FireServer(unpack(args))
                end)

                -- Влет-вылет
                local start = CFrame.new(loc.coords)
                local endPos = CFrame.new(loc.coords + Vector3.new(0, 0, 5))
                local outTween = TweenService:Create(HumanoidRootPart, inoutTween, {CFrame = endPos})
                local inTween = TweenService:Create(HumanoidRootPart, inoutTween, {CFrame = start})
                outTween:Play()
                outTween.Completed:Wait()
                inTween:Play()
                inTween.Completed:Wait()

                task.wait(2.2) -- Итого 3 сек
            end
        end

        -- Egg каждые 4 сек
        spawn(function()
            if workspace:FindFirstChild("HalloweenEvent") and workspace.HalloweenEvent:FindFirstChild("SummonedEgg") then
                hatchEgg()
            else
                hatchFixed()
            end
        end)

        -- Shop каждую секунду
        spawn(function()
            local item = shopItems[shopIndex]
            pcall(function()
                local args = { [1] = "BuyShopItem", [2] = item.shop, [3] = item.id, [4] = false }
                ReplicatedStorage.Shared.Framework.Network.Remote.RemoteEvent:FireServer(unpack(args))
            end)
            shopIndex = (shopIndex % #shopItems) + 1
        end)

        -- Potions
        spawn(function()
            for _, pot in ipairs(potions) do
                pcall(function()
                    local args = { [1] = "UsePotion", [2] = pot.name, [3] = pot.count }
                    ReplicatedStorage.Shared.Framework.Network.Remote.RemoteEvent:FireServer(unpack(args))
                end)
            end
        end)

        task.wait(1)
    end
end)

-- M — вкл/выкл
UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == bindKey then
        isEnabled = not isEnabled
        print("Авто: " .. (isEnabled and "ВКЛ" or "ВЫКЛ"))
    end
end)

-- Респавн
LocalPlayer.CharacterAdded:Connect(function(newChar)
    Character = newChar
    HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
    print("Персонаж обновлён.")
end)

print("Скрипт работает! M — вкл/выкл")
