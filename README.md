-- Получаем необходимые сервисы
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- Получаем Remote
local GetInventory = ReplicatedStorage.Remotes.Data.GetInventory
local GiveItem = ReplicatedStorage.Remotes.Data.GiveItem

-- Создаем GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = PlayerGui

-- Создаем фон для меню
local menuFrame = Instance.new("Frame")
menuFrame.Size = UDim2.new(0, 300, 0, 200)
menuFrame.Position = UDim2.new(0.5, -150, 0.5, -100)
menuFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
menuFrame.BackgroundTransparency = 0.5
menuFrame.Parent = ScreenGui

-- Создаем кнопку включения
local enableButton = Instance.new("TextButton")
enableButton.Size = UDim2.new(0, 200, 0, 50)
enableButton.Position = UDim2.new(0.5, -100, 0.25, 0)
enableButton.Text = "Включить выдачу предметов"
enableButton.TextColor3 = Color3.fromRGB(255, 255, 255)
enableButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
enableButton.Parent = menuFrame

-- Создаем кнопку отключения
local disableButton = Instance.new("TextButton")
disableButton.Size = UDim2.new(0, 200, 0, 50)
disableButton.Position = UDim2.new(0.5, -100, 0.5, 0)
disableButton.Text = "Отключить выдачу предметов"
disableButton.TextColor3 = Color3.fromRGB(255, 255, 255)
disableButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
disableButton.Parent = menuFrame

-- Переменная состояния
local isEnabled = false

-- Твой ID предмета
local wantedID = "1744541324"  -- <- ТВОЙ ID ПРЕДМЕТА

-- Функция автоматической выдачи
task.spawn(function()
    while true do
        if isEnabled then
            local inventory = GetInventory:InvokeServer()
            if inventory[wantedID] then
                GiveItem:FireServer(wantedID)
                print("[Авто-выдача]: Предмет выдан:", wantedID)
            else
                print("[Авто-выдача]: Предмет с таким ID не найден в инвентаре.")
            end
        end
        task.wait(1) -- Ждем 1 секунду
    end
end)

-- Обработчики кнопок
enableButton.MouseButton1Click:Connect(function()
    isEnabled = true
    print("Автоматическая выдача включена.")
end)

disableButton.MouseButton1Click:Connect(function()
    isEnabled = false
    print("Автоматическая выдача отключена.")
end)
