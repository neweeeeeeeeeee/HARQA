local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "DBU",
   LoadingTitle = "DBU Script",
   LoadingSubtitle = "by Spec",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil, -- Create a custom folder for your hub/game
      FileName = "Big Hub"
   },
   Discord = {
      Enabled = false,
      Invite = "noinvitelink", -- The Discord invite code, do not include discord.gg/. E.g. discord.gg/ABCD would be ABCD
      RememberJoins = true -- Set this to false to make them join the discord every time they load it up
   },
   KeySystem = false, -- Set this to true to use our key system
   KeySettings = {
      Title = "Untitled",
      Subtitle = "Key System",
      Note = "No method of obtaining the key is provided",
      FileName = "Key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
      SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
      GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
      Key = {"Hello"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
   }
})

-- Tabs
local MainTab = Window:CreateTab("Main") -- Title, Image
-- 
local plr = game.Players.LocalPlayer
local RunService = game:GetService('RunService')
local questRemote = game:GetService("ReplicatedStorage").Package.Events.Qaction 
local punchRemote = game:GetService("ReplicatedStorage").Package.Events.p
local equipRemote = game:GetService("ReplicatedStorage").Package.Events.equipskill
local rebirthRemote = game:GetService("ReplicatedStorage").Package.Events.reb

local Settings = {Tables = {Forms = {'SSJB4','True God of Creation','True God of Destruction','Super Broly','LSSJG','LSSJ4','SSJG4','LSSJ3','SSJ5','Mystic Kaioken','LSSJ Kaioken','SSJ2 Kaioken','LBSSJ4','SSJR3','SSJB3','God of Destruction','God of Creation','Jiren Ultra Instinct', 'Mastered Ultra Instinct','Godly SSJ2','Evil SSJ','Blue Evolution','Dark Rose','Kefla SSJ2','SSJ Berserker','True Rose', 'SSJB Kaioken','SSJ Rose', 'SSJ Blue','Corrupt SSJ','SSJ Rage','SSJG','SSJ4','Mystic','LSSJ','SSJ3','Spirit SSJ','SSJ2 Majin','SSJ2','SSJ Kaioken','SSJ','FSSJ','Kaioken'}};Variables = {Farm = false}}
setmetatable(Settings, {__index = function() warn('f') end}) -- literally no use i was just bored

local StackForms = {'Godly SSJ2','Mystic','SSJ3'}

local function returnQuest(boolean)
    local quest = getrenv()._G.x.GetRecommendedQuest(game.Players.LocalPlayer)
    if (boolean) and quest:find('Vills Planet') then 
        return 'SSJG Kakata'
    else 
        return quest 
    end 
end 
local function transform()
    pcall(function()
        for i,v in pairs(Settings.Tables.Forms) do
            if equipRemote:InvokeServer(v) then
                break
            end
        end
        repeat wait()
            if plr:WaitForChild("Status"):WaitForChild("SelectedTransformation").Value ~= plr.Status.Transformation.Value then
                game:GetService("ReplicatedStorage"):WaitForChild("Package"):WaitForChild("Events"):WaitForChild("ta"):InvokeServer()
            end
        until game.Players.LocalPlayer.Status.SelectedTransformation.Value == game.Players.LocalPlayer.Status.Transformation.Value
    end)
end

          
       

local function stacktransform()
    pcall(function()
        for i,v in pairs(StackForms) do
        print(v)
            if equipRemote:InvokeServer(v) then
                break
            end
        end
       
            plr.Character:WaitForChild("Humanoid"):ChangeState(15)
            wait(5.7)
            repeat 
            game:WaitForChild("ReplicatedStorage"):WaitForChild("Package"):WaitForChild("Events"):WaitForChild("ta"):InvokeServer()
            wait()
            until game.Players.LocalPlayer.Character.Humanoid.Health > 0
            wait(4)
            transform()
    end)
    
end

RunService.RenderStepped:Connect(function()
    if Settings.Variables.Farm then 
        plr.Character:WaitForChild('Humanoid'):ChangeState(11)
    end 
end)

Rayfield:Notify({
   Title = "DBU Script Loaded",
   Content = "Time to exploit",
   Duration = 5,
   Image = 4483362458,
   Actions = { -- Notification Buttons
      Ignore = {
         Name = "Okay!",
         Callback = function()
         print("The user tapped Okay!")
      end
   },
},
})




local RebirthFarm = MainTab:CreateToggle({
    Name = 'Rebirth-Farm',
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(bool)
    _G.Autofarm = not _G.Autofarm
        Settings.Variables.Farm = bool 
        while bool and wait() and _G.Autofarm == true do 
            if (not plr.PlayerGui:WaitForChild("Main"):WaitForChild("MainFrame"):WaitForChild("Frames"):WaitForChild("Quest"):WaitForChild("Nop").Visible and bool) then
                for i,v in next, workspace.Living:GetChildren() do 
                    if (v.Name == returnQuest(true) or v.Name == game:GetService("ReplicatedStorage").Package.Quests[returnQuest(true)].Objective.Value) and not plr.PlayerGui.Main.MainFrame.Frames.Quest.Nop.Visible and v:FindFirstChild('Humanoid') and v:FindFirstChild('HumanoidRootPart') and v.Humanoid.Health > 0 then 
                        repeat wait()
                            pcall(function() plr.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,2) end)
                            punchRemote:FireServer('Blacknwhite27',1)
                        until not v or not v:FindFirstChild('Humanoid') or not v:FindFirstChild('HumanoidRootPart') or v.Humanoid.Health <= 0 or not Settings.Variables.Farm or plr.PlayerGui:WaitForChild('Main').MainFrame.Frames.Quest.Nop.Visible
                    end 
                end 
            else 
                pcall(function() questRemote:InvokeServer(workspace.Others.NPCs[returnQuest(true)]) end)
            end 
        end 
    end
})



local GodMode = MainTab:CreateToggle({
    Name = 'GodMode',
    CurrentValue = false,
    Flag = "Toggle2",
    Callback = function(bool)
    _G.Godmode = not _G.Godmode
        pcall(function()
            if _G.Godmode then
                plr.Character.Status.Blocking.Parent = nil 
            else
                plr.Character.Humanoid.Health = -math.huge 
            end
        end)
    end 
})
local AutoTransform = MainTab:CreateToggle({
    Name = 'Auto-Transform',
    CurrentValue = false,
    Flag = "Toggle3",
    Callback = function(bool)
    _G.Autoform = not _G.Autoform
        pcall(function()
            while bool and wait() and _G.Autoform == true do 
                transform()
            end
        end)
    end 
})





local AutoRebirth = MainTab:CreateToggle({
    Name = 'Rebirth',
    CurrentValue = false,
    Flag = "Toggle4",
    Callback = function(bool)
    _G.rebirth = not _G.rebirth
        while bool and wait() and _G.rebirth == true do 
            rebirthRemote:InvokeServer()
        end 
    end 
})

local AutoCharge = MainTab:CreateToggle({
    Name = 'Auto-Charge',
    CurrentValue = false,
    Flag = "Toggle5",
    Callback = function(bool) 
    _G.Autocharge = not _G.Autocharge
        while bool and wait() and _G.Autocharge == true do 
            game:GetService("ReplicatedStorage").Package.Events.cha:InvokeServer('Blacknwhite27')
        end
    end 
})

local AutoVolley = MainTab:CreateToggle({
    Name = 'Auto Energy Volley/Mach Kick',
    CurrentValue = false,
    Flag = "Toggle6",
    Callback = function(bool) 
    _G.AutoEnergy = not _G.AutoEnergy
        while bool and wait() and _G.AutoEnergy == true and plr.Character do
                    local args2 = {
            [1] = "Energy Volley"
        } 

        game:GetService("ReplicatedStorage"):WaitForChild("Package"):WaitForChild("Events"):WaitForChild("equipskill"):InvokeServer(unpack(args2))
        equipRemote:InvokeServer("Mach Kick")
       if plr.Character.Stats.Ki.Value > (plr.Character.Stats.Ki.MaxValue * .35) then
       local args = {
    [1] = "Energy Volley",
    [2] = {
        ["FaceMouse"] = false
    },
    [3] = "Blacknwhite27"
}

game:GetService("ReplicatedStorage"):WaitForChild("Package"):WaitForChild("Events"):WaitForChild("voleys"):InvokeServer(unpack(args))

local args3 = {
    [1] = "Mach Kick",
    [2] = "Blacknwhite27",
}

game:GetService("ReplicatedStorage"):WaitForChild("Package"):WaitForChild("Events"):WaitForChild("mel"):InvokeServer(unpack(args3))
            end
        end
    end 
})

local AutoStats = MainTab:CreateToggle({
    Name = 'Auto Show Stats',
    CurrentValue = false,
    Callback = function(bool) 
    _G.Autostats = not _G.Autostats
        while bool and wait() and _G.Autostats == true do 
            plr.PlayerGui:WaitForChild("Main"):WaitForChild("MainFrame"):WaitForChild("Frames"):WaitForChild("Stats").Visible = true
        plr.PlayerGui:WaitForChild("Main"):WaitForChild("MainFrame"):WaitForChild("Frames"):WaitForChild("Stats").Position = UDim2.new(0.01, 0, 0.02, 0)
        end
    end 
})


local StackToggle = MainTab:CreateButton({
   Name = "Stack Forms",
   Callback = function()
    stacktransform() 
   end,
})



local Button = MainTab:CreateButton({
   Name = "Remote Spy",
   Callback = function()
   _G.RemoteSpy = not _G.RemoteSpy
loadstring(game:HttpGet("https://raw.githubusercontent.com/78n/SimpleSpy/main/SimpleSpySource.lua"))()
   end,
})

local Button = MainTab:CreateButton({
   Name = "Infinite Yield",
   Callback = function()
   _G.IY = not _G.IY
loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
   end,
})

