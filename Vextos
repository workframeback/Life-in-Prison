-- 1. CLEANUP PREVIOUS SESSIONS
if game.CoreGui:FindFirstChild("Rayfield") then
    game.CoreGui.Rayfield:Destroy()
end
if game.CoreGui:FindFirstChild("WazFOV") then
    game.CoreGui.WazFOV:Destroy()
end

-- 2. INITIALIZE CONFIGURATION
getgenv().hdjsjxjsjsjsksjsjhi = getgenv().hdjsjxjsjsjsksjsjhi or {
	FovSize = 126,
	HitChance = 73,
	IgnorePolice = false,
	IgnoreCriminals = false,
	IgnorePrisoners = false,
    Enabled = true
}

local Configs = {
    AutoGrab = true,
    InfJump = false,
    Targets = {
        ["FAL"] = true, ["SPAS"] = true, ["Keycard"] = true, ["AWN"] = true,
        ["M60"] = true, ["Minigun"] = true, ["MP6"] = true, ["Medkit"] = true,
        ["Pretzel"] = true, ["AR 15"] = true, ["UZI"] = true, ["Taser"] = true
    }
}

-- 3. SERVICES
local GetService = setmetatable({}, {
	__index = function(Table, Index)
		Table[Index] = game:GetService(Index)
		return Table[Index]
	end
})

local Players = GetService.Players
local Workspace = GetService.Workspace
local RunService = GetService.RunService
local UserInputService = GetService.UserInputService
local ReplicatedStorage = GetService.ReplicatedStorage
local TeleportService = GetService.TeleportService
local HttpService = GetService.HttpService
local LocalPlayer = Players.LocalPlayer
local Camera = Workspace.CurrentCamera

-- 4. LOGIC & UI VARIABLES
local CurrentTarget = nil
local MousePosition = nil
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- AUTO-COPY DISCORD LINK
setclipboard("https://discord.gg/qwWGTqv4yu")

-- 5. RAYFIELD WINDOW (PURPLE EDITION)
local Window = Rayfield:CreateWindow({
   Name = "Vextos V3",
   LoadingTitle = "Loading Amethyst System...",
   LoadingSubtitle = "by Vextos",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "VextoslConfig",
      FileName = "MainConfig",
      Theme = "Purple" -- TEMA INTEGRALE VIOLA
   },
   Discord = {
      Enabled = true,
      Invite = "qwWGTqv4yu",
      RememberJoins = true
   },
   KeySystem = true,
   KeySettings = {
      Title = "Vextos V3 | Access Control",
      Subtitle = "Join Discord for the Key",
      Note = "Join discord",
      FileName = "VextosKey", 
      SaveKey = true, 
      GrabKeyFromSite = false, 
      Key = {"wastedtime"}
   }
})

-- 6. FOV DRAWING (PURPLE)
local ScreenGui = Instance.new("ScreenGui")
local FovCircle = Instance.new("Frame")
local CircleCorner = Instance.new("UICorner")
local CircleStroke = Instance.new("UIStroke")

ScreenGui.Name = "VextosFOV"
ScreenGui.Parent = gethui and gethui() or GetService.CoreGui
ScreenGui.IgnoreGuiInset = true
FovCircle.Parent = ScreenGui
FovCircle.AnchorPoint = Vector2.new(0.5, 0.5)
FovCircle.BackgroundTransparency = 1
FovCircle.Visible = getgenv().hdjsjxjsjsjsksjsjhi.Enabled
CircleCorner.CornerRadius = UDim.new(1, 0)
CircleCorner.Parent = FovCircle

-- COLORE FOV SETTATO SU VIOLA
CircleStroke.Color = Color3.fromRGB(160, 32, 240) 
CircleStroke.Thickness = 1.5
CircleStroke.Transparency = 0.3
CircleStroke.Parent = FovCircle

UserInputService.InputChanged:Connect(function(Input)
	if Input.UserInputType == Enum.UserInputType.MouseMovement then
		MousePosition = Vector2.new(Input.Position.X, Input.Position.Y)
	end
end)

-- 7. SILENT AIM LOGIC
local o; o = hookmetamethod(game, "__namecall", newcclosure(function(...)
	local Arguments = {...}
	local CalledMethod = getnamecallmethod()
	local CalledSelf = Arguments[1]
	if CalledSelf == Workspace and not checkcaller() and getgenv().hdjsjxjsjsjsksjsjhi.Enabled then
		if CurrentTarget then
			local HitRoll = math.random(1, 100)
			if HitRoll <= getgenv().hdjsjxjsjsjsksjsjhi.HitChance then
				if CalledMethod == "FindPartOnRayWithIgnoreList" or CalledMethod == "FindPartOnRay" then
					local RayOrigin = Arguments[2].Origin
					Arguments[2] = Ray.new(RayOrigin, CurrentTarget.Position - RayOrigin)
					return o(unpack(Arguments))
				elseif CalledMethod == "Raycast" then
					Arguments[3] = CurrentTarget.Position - Arguments[2]
					return o(unpack(Arguments))
				end
			end
		end
	end
	return o(...)
end))

-- 8. TABS
local MainTab = Window:CreateTab("Main", 4483362458)
local OthersTab = Window:CreateTab("Others", 4483362458)
local ConfigTab = Window:CreateTab("Config UI", 4483362458)

-- MAIN SECTION
MainTab:CreateSection("Automation")
MainTab:CreateToggle({
   Name = "Auto-Grab Tools",
   CurrentValue = true,
   Callback = function(v) 
       Configs.AutoGrab = v 
       Rayfield:Notify({Title = "Vextos V3", Content = "Auto-Grab: " .. (v and "Enabled" or "Disabled"), Duration = 2})
   end,
})

MainTab:CreateSection("Visuals")
MainTab:CreateSlider({
   Name = "FOV Visual Size",
   Range = {50, 600},
   Increment = 1,
   CurrentValue = 126,
   Callback = function(v) getgenv().hdjsjxjsjsjsksjsjhi.FovSize = v end,
})

-- OTHERS SECTION
OthersTab:CreateSection("Silent Aim Control")
OthersTab:CreateToggle({
   Name = "Enable Silent Aim",
   CurrentValue = true,
   Callback = function(v) 
       getgenv().hdjsjxjsjsjsksjsjhi.Enabled = v
       FovCircle.Visible = v
       Rayfield:Notify({Title = "Vextos V3", Content = "Silent Aim: " .. (v and "Enabled" or "Disabled"), Duration = 2})
   end,
})

OthersTab:CreateSlider({
   Name = "Hit Chance",
   Range = {1, 100},
   Increment = 1,
   CurrentValue = 73,
   Callback = function(v) getgenv().hdjsjxjsjsjsksjsjhi.HitChance = v end,
})

OthersTab:CreateSection("Movement")
OthersTab:CreateToggle({
   Name = "Auto Jump",
   CurrentValue = false,
   Callback = function(v) 
       Configs.InfJump = v 
       Rayfield:Notify({Title = "Vextos V3", Content = "Auto Jump: " .. (v and "Enabled" or "Disabled"), Duration = 2})
   end,
})

OthersTab:CreateSection("Filters")
OthersTab:CreateToggle({
    Name = "Ignore Police", 
    CurrentValue = false, 
    Callback = function(v) 
        getgenv().hdjsjxjsjsjsksjsjhi.IgnorePolice = v 
        Rayfield:Notify({Title = "Filter", Content = "Ignore Police: " .. (v and "Enabled" or "Disabled"), Duration = 2})
    end
})
OthersTab:CreateToggle({
    Name = "Ignore Criminals", 
    CurrentValue = false, 
    Callback = function(v) 
        getgenv().hdjsjxjsjsjsksjsjhi.IgnoreCriminals = v 
        Rayfield:Notify({Title = "Filter", Content = "Ignore Criminals: " .. (v and "Enabled" or "Disabled"), Duration = 2})
    end
})
OthersTab:CreateToggle({
    Name = "Ignore Prisoners", 
    CurrentValue = false, 
    Callback = function(v) 
        getgenv().hdjsjxjsjsjsksjsjhi.IgnorePrisoners = v 
        Rayfield:Notify({Title = "Filter", Content = "Ignore Prisoners: " .. (v and "Enabled" or "Disabled"), Duration = 2})
    end
})

-- CONFIG SECTION
ConfigTab:CreateSection("System Management")
ConfigTab:CreateButton({
    Name = "Rejoin Server",
    Callback = function()
        TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, LocalPlayer)
    end
})
ConfigTab:CreateButton({
    Name = "Server Hop",
    Callback = function()
        local servers = HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100")).data
        for _, s in pairs(servers) do
            if s.playing < s.maxPlayers and s.id ~= game.JobId then
                TeleportService:TeleportToPlaceInstance(game.PlaceId, s.id)
                break
            end
        end
    end
})
ConfigTab:CreateButton({
    Name = "Destroy UI",
    Callback = function()
        Rayfield:Destroy()
        ScreenGui:Destroy()
    end
})

-- 9. LOOP RENDERING
RunService.RenderStepped:Connect(function()
	local PlayerCharacter = LocalPlayer.Character
	if not PlayerCharacter then CurrentTarget = nil return end
    
    local ScreenSize = Camera.ViewportSize
    local ScreenCenter = MousePosition or Vector2.new(ScreenSize.X / 2, ScreenSize.Y / 2)

    if Configs.InfJump then
        local hum = PlayerCharacter:FindFirstChildOfClass("Humanoid")
        if hum and hum.FloorMaterial ~= Enum.Material.Air then hum.Jump = true end
    end

    FovCircle.Position = UDim2.new(0, ScreenCenter.X, 0, ScreenCenter.Y)
    FovCircle.Size = UDim2.new(0, getgenv().hdjsjxjsjsjsksjsjhi.FovSize * 2, 0, getgenv().hdjsjxjsjsjsksjsjhi.FovSize * 2)
	
    if not getgenv().hdjsjxjsjsjsksjsjhi.Enabled then CurrentTarget = nil return end

	CurrentTarget = nil
	local ClosestDistance = math.huge

	for _, OtherPlayer in Players:GetPlayers() do
		if OtherPlayer == LocalPlayer or OtherPlayer.Team == LocalPlayer.Team then continue end
		
		if OtherPlayer.Team then
			if getgenv().hdjsjxjsjsjsksjsjhi.IgnorePolice and OtherPlayer.Team.Name == "Police" then continue end
			if getgenv().hdjsjxjsjsjsksjsjhi.IgnoreCriminals and OtherPlayer.Team.Name == "Criminals" then continue end
			if getgenv().hdjsjxjsjsjsksjsjhi.IgnorePrisoners and OtherPlayer.Team.Name == "Prisoners" then continue end
		end

		local TargetCharacter = OtherPlayer.Character
		if not TargetCharacter or not TargetCharacter:FindFirstChild("Head") then continue end
        if TargetCharacter:FindFirstChildOfClass("Humanoid").Health <= 0 then continue end

		local HeadPosition = TargetCharacter.Head.Position
		local ScreenPosition, IsOnScreen = Camera:WorldToViewportPoint(HeadPosition)

		if IsOnScreen then
			local DistanceFromCenter = (Vector2.new(ScreenPosition.X, ScreenPosition.Y) - ScreenCenter).Magnitude
			if DistanceFromCenter <= getgenv().hdjsjxjsjsjsksjsjhi.FovSize and DistanceFromCenter < ClosestDistance then
                local WallCheckRay = Ray.new(Camera.CFrame.Position, HeadPosition - Camera.CFrame.Position)
                local RayHit = Workspace:FindPartOnRayWithIgnoreList(WallCheckRay, {PlayerCharacter})
                if RayHit and (RayHit.Parent == TargetCharacter or RayHit.Parent.Parent == TargetCharacter) then
                    ClosestDistance = DistanceFromCenter
                    CurrentTarget = TargetCharacter.Head
                end
			end
		end
	end
end)

-- 10. AUTO-GRAB
local Remote = ReplicatedStorage:WaitForChild("Events"):WaitForChild("RemoteEvent")
local Pickups = Workspace:WaitForChild("pickups", 5)

local function AttemptGrab(obj)
    if not Configs.AutoGrab or not obj:IsA("Model") then return end
    local name = obj.Name:gsub("%s+$", "")
    if Configs.Targets[name] then
        local bp = LocalPlayer:FindFirstChild("Backpack")
        local char = LocalPlayer.Character
        local has = (bp and bp:FindFirstChild(name)) or (char and char:FindFirstChild(name))
        if not has then
            Remote:FireServer(12, obj)
        end
    end
end

if Pickups then Pickups.ChildAdded:Connect(AttemptGrab) end
task.spawn(function()
    while task.wait(1.5) do
        if Configs.AutoGrab and Pickups then
            for _, i in pairs(Pickups:GetChildren()) do AttemptGrab(i) end
        end
    end
end)

Rayfield:Notify({Title = "Vextos V3", Content = "Purple Edition Active!", Duration = 5})
