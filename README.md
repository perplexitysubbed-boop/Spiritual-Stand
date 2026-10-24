local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local StarterGui = game:GetService("StarterGui")

local player = Players.LocalPlayer

local Event = nil
local function getHackRemote()
	if Event then return Event end
	local folder = ReplicatedStorage:FindFirstChild("ALLREMBINDS")
	local remote = folder and folder:FindFirstChild("Pls_Dont_Hack_Plssssssssss")
	if remote then Event = remote end
	return remote
end

local function GetNil(ClassName)
	for _, Object in pairs(getnilinstances and getnilinstances() or {}) do
		if not ClassName or Object.ClassName == ClassName then
			return Object
		end
	end
	return nil
end

local function dealDamage(targetPlayer, amount)
	if not targetPlayer then return end
	if standTarget and targetPlayer.UserId == standTarget.UserId then return end
	local remote = getHackRemote()
	if not remote then return end
	local modelObj = GetNil("Model")
	pcall(function()
		remote:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "Main_Damger_OC_R", {
			targetPlayer,
			{ D = amount, F = modelObj }
		})
	end)
	local charactersFolder = Workspace:FindFirstChild("Characters")
	local charModel = charactersFolder and charactersFolder:FindFirstChild(targetPlayer.Name)
	if not charModel and targetPlayer.Character then
		charModel = targetPlayer.Character
	end
	if charModel then
		pcall(function()
			remote:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "Main_Damger_OC_R", {
				charModel,
				{ D = amount, F = modelObj }
			})
		end)
	end
end

local deleting = false
local function deleteCharacterAnimated(plr)
	if deleting then return end
	if standTarget and plr.UserId == standTarget.UserId then return end
	local remote = getHackRemote()
	if not remote then return end

	local localChar = player.Character
	local localHrp = localChar and localChar:FindFirstChild("HumanoidRootPart")
	if not localHrp then return end

	local charactersFolder = Workspace:FindFirstChild("Characters")
	local targetModel = charactersFolder and charactersFolder:FindFirstChild(plr.Name)
	if not targetModel and plr.Character then targetModel = plr.Character end
	if not targetModel then return end
	local targetHrp = targetModel:FindFirstChild("HumanoidRootPart")
	if not targetHrp then return end
	local rootAttachment = targetHrp:FindFirstChild("RootAttachment")
		or targetHrp:FindFirstChildOfClass("Attachment")

	deleting = true
	local savedStand = standTarget

	if standConn then
		standConn:Disconnect()
		standConn = nil
	end

	task.spawn(function()
		local arrived = false
		local startTime = tick()
		while tick() - startTime < 4 do
			if not localHrp.Parent or not targetHrp.Parent then break end
			local dist = (localHrp.Position - targetHrp.Position).Magnitude
			if dist <= 3 then
				arrived = true
				break
			end
			local goal = CFrame.new(localHrp.Position:Lerp(targetHrp.Position, 0.25), targetHrp.Position)
			pcall(function()
				localHrp.CFrame = localHrp.CFrame:Lerp(goal, 0.35)
				localHrp.AssemblyLinearVelocity = Vector3.zero
			end)
			task.wait()
		end

		if targetHrp and targetHrp.Parent then
			pcall(function()
				remote:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "Aligen_Welding", {
					true, {
						A = localHrp,
						P = targetHrp,
						C = localChar,
						N = player,
						T = targetModel,
						NA = "SSPAlty_Deathwhisper_ATCC",
						A1 = CFrame.new(120, 10000, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0),
						A0 = rootAttachment
					}
				})
			end)
		end

		task.wait(0.15)

		if savedStand and savedStand.Parent then
			standTarget = savedStand
			standConn = RunService.Heartbeat:Connect(function()
				if not standTarget or not standTarget.Parent then
					stopStand()
					return
				end
				local lc = player.Character
				if not lc then return end
				local lh = lc:FindFirstChild("HumanoidRootPart")
				if not lh then return end
				local cf = Workspace:FindFirstChild("Characters")
				local tm = cf and cf:FindFirstChild(standTarget.Name)
				if not tm and standTarget.Character then tm = standTarget.Character end
				if not tm then return end
				local th = tm:FindFirstChild("HumanoidRootPart")
				if not th then return end
				local offset = CFrame.new(1.6, 2, 3.5)
				local desired = th.CFrame * offset
				pcall(function()
					lh.CFrame = lh.CFrame:Lerp(desired, STAND_LERP)
					lh.AssemblyLinearVelocity = Vector3.zero
					lh.AssemblyAngularVelocity = Vector3.zero
				end)
				setIsSitingFalse()
				zeroCantHitStun()
			end)
			for _ = 1, 20 do
				if not standTarget then break end
				local lc = player.Character
				local lh = lc and lc:FindFirstChild("HumanoidRootPart")
				local cf = Workspace:FindFirstChild("Characters")
				local tm = cf and cf:FindFirstChild(standTarget.Name)
				if not tm and standTarget.Character then tm = standTarget.Character end
				local th = tm and tm:FindFirstChild("HumanoidRootPart")
				if lh and th then
					local desired = th.CFrame * CFrame.new(1.6, 2, 3.5)
					pcall(function() lh.CFrame = lh.CFrame:Lerp(desired, 0.4) end)
				end
				task.wait()
			end
		end

		deleting = false
	end)
end

local function flingPlayer(plr)
	if not plr then return end
	if standTarget and plr.UserId == standTarget.UserId then return end
	local remote = getHackRemote()
	if not remote then return end
	local hrp = nil
	local charactersFolder = Workspace:FindFirstChild("Characters")
	local model = charactersFolder and charactersFolder:FindFirstChild(plr.Name)
	if model then hrp = model:FindFirstChild("HumanoidRootPart") end
	if not hrp and plr.Character then
		hrp = plr.Character:FindFirstChild("HumanoidRootPart")
	end
	if not hrp then return end
	pcall(function()
		remote:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "ALVC", {
			hrp,
			Vector3.new(0, 300, 0),
			{}
		})
	end)
end

local function doNormalJump()
	local character = player.Character
	if not character then return end
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	if not humanoid then return end
	pcall(function()
		humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
	end)
end

local function zeroCantHitStun()
	pcall(function()
		local charactersFolder = Workspace:FindFirstChild("Characters")
		if not charactersFolder then return end
		local localCharacter = charactersFolder:FindFirstChild(player.Name)
		if not localCharacter then return end
		local stats = localCharacter:FindFirstChild("CharcterStats")
			or localCharacter:FindFirstChild("CharacterStats")
			or localCharacter:FindFirstChild("characterstats")
		if not stats then return end
		
		local cantHit = stats:FindFirstChild("CantHit")
		if cantHit then
			cantHit.Value = 0
		end
		
		local stun = stats:FindFirstChild("Stun")
		if stun then
			stun.Value = 0
		end
	end)
end

local function setIsSitingFalse()
	pcall(function() player:SetAttribute("IsSiting", false) end)
	pcall(function()
		local val = player:FindFirstChild("IsSiting")
		if val and val:IsA("BoolValue") then val.Value = false end
	end)
end

local ragdollLoopTask = nil
local function manageCanNotRagdoll()
	if ragdollLoopTask then
		task.cancel(ragdollLoopTask)
		ragdollLoopTask = nil
	end
	
	if not standTarget then return end

	ragdollLoopTask = task.spawn(function()
		while standTarget do
			pcall(function()
				local charactersFolder = Workspace:FindFirstChild("Characters")
				if charactersFolder then
					local localCharModel = charactersFolder:FindFirstChild(player.Name)
					if localCharModel then
						local stats = localCharModel:FindFirstChild("CharcterStats")
							or localCharModel:FindFirstChild("CharacterStats")
							or localCharModel:FindFirstChild("characterstats")
						if stats then
							stats:SetAttribute("CanNotRagdoll", true)
						end
					end
				end
			end)
			task.wait(1)
		end
	end)
end

-- Save and Restore Humanoid data for StandUser
local savedStandHumanoids = {} -- [UserId] = { HumanoidInstance = hum, Properties = {...} }

local function deleteStandUserHumanoid(targetPlr)
	if not targetPlr then return end
	pcall(function()
		local charactersFolder = Workspace:FindFirstChild("Characters")
		local standCharModel = charactersFolder and charactersFolder:FindFirstChild(targetPlr.Name)
		if not standCharModel and targetPlr.Character then
			standCharModel = targetPlr.Character
		end
		if standCharModel then
			local hum = standCharModel:FindFirstChildOfClass("Humanoid")
			if hum then
				savedStandHumanoids[targetPlr.UserId] = {
					Parent = standCharModel,
					Health = hum.Health,
					MaxHealth = hum.MaxHealth,
					WalkSpeed = hum.WalkSpeed,
					JumpPower = hum.JumpPower,
					AutoRotate = hum.AutoRotate,
					Name = hum.Name,
					ClassName = hum.ClassName
				}
				hum:Destroy()
			end
		end
	end)
end

local function restoreStandUserHumanoid(targetPlr)
	if not targetPlr then return end
	pcall(function()
		local data = savedStandHumanoids[targetPlr.UserId]
		if not data then return end
		local charactersFolder = Workspace:FindFirstChild("Characters")
		local standCharModel = charactersFolder and charactersFolder:FindFirstChild(targetPlr.Name)
		if not standCharModel and targetPlr.Character then
			standCharModel = targetPlr.Character
		end
		if standCharModel and data.Parent and data.Parent.Parent then
			local newHum = Instance.new("Humanoid")
			newHum.Name = data.Name
			newHum.MaxHealth = data.MaxHealth
			newHum.Health = data.Health
			newHum.WalkSpeed = data.WalkSpeed
			newHum.JumpPower = data.JumpPower
			newHum.AutoRotate = data.AutoRotate
			newHum.Parent = standCharModel
		end
		savedStandHumanoids[targetPlr.UserId] = nil
	end)
end

-- ==========================================
-- NO COOLDOWN SYSTEM
-- ==========================================
local isNoCDEnabled = false
local savedCoolDowns = {}

local function getBackpackItem(itemName, targetPlayer)
	targetPlayer = targetPlayer or player
	local stats = targetPlayer:FindFirstChild("PlayerStats")
		or targetPlayer:FindFirstChild("playerstats")
	if not stats then return nil end
	local backpack = stats:FindFirstChild("Backpack")
	if not backpack then return nil end

	local exact = backpack:FindFirstChild(itemName)
	if exact then return exact end

	local function normalize(s)
		return string.lower((tostring(s):gsub("’", "'"):gsub("`", "'")))
	end
	local want = normalize(itemName)
	for _, child in ipairs(backpack:GetChildren()) do
		if normalize(child.Name) == want then
			return child
		end
	end
	return nil
end

local function isCoolDownName(name)
	local n = string.lower(tostring(name):gsub(" ", ""))
	return n == "cooldown" or n == "cd"
end

local function deleteCoolDown(itemName, targetPlayer)
	pcall(function()
		local item = getBackpackItem(itemName, targetPlayer)
		if not item then return end

		local attrs = {}
		pcall(function()
			attrs = item:GetAttributes()
		end)

		local direct = item:GetAttribute("CoolDown")
		if direct ~= nil then
			if savedCoolDowns[itemName] == nil then
				savedCoolDowns[itemName] = direct
			end
			item:SetAttribute("CoolDown", 0)
			item:RemoveAttribute("CoolDown")
		end

		for attrName, attrValue in pairs(attrs) do
			if isCoolDownName(attrName) then
				if savedCoolDowns[itemName] == nil then
					savedCoolDowns[itemName] = attrValue
				end
				item:SetAttribute(attrName, 0)
				item:RemoveAttribute(attrName)
			end
		end
	end)
end

local activeNoCDItems = {
	"Ashreaver", "Celestial Fang", "Crimson Oath", "Deathwhisper", "Eclipse Reaper",
	"Frostbite Blade", "Magma Cleaver", "Moonrend", "Soulpiercer", "Tempest Glaive",
	"Titan Crusher", "Zenith Katana", "Celestial Rift", "Earthquake Stomp", "Echo of Souls",
	"Eternal Blaze", "Frozen Nova", "Hydro Sphere", "Lunar Veil", "Magma Faultline Cataclysm",
	"Prism Overdrive Barrage", "Storm Surge", "Supreme Meteor Extinction", "Void Singularity Collapse",
	"Zenith Solar Annihilation", "Zephyr Dance"
}

task.spawn(function()
	while true do
		RunService.Heartbeat:Wait()
		if isNoCDEnabled then
			for _, itemName in ipairs(activeNoCDItems) do
				deleteCoolDown(itemName)
			end
		end
	end
end)
-- ==========================================

local selectedTargets = {}
local loopKillTargets = {}
local ragdollTargets = {}
local shredDodgesTargets = {}
local autoBlockTargets = {}
local bypassBlockTargets = {}
local originalHitboxSizes = {}
local playerEntries = {}
local flingTargets = {}
local trollPlayerEntries = {}
local isAutoFlingOthers = false
local damageAmount = 316
local showEnemiesOnly = false
local DAMAGE_OPTIONS = {3, 6, 25, 63, 316}
local guiVisible = true

local standTarget = nil
local standConn = nil
local STAND_LERP = 0.18

local function getIsSiting(plr)
	local attr = plr:GetAttribute("IsSiting")
	if attr ~= nil then return attr == true end
	local val = plr:FindFirstChild("IsSiting")
	if val and val:IsA("BoolValue") then return val.Value == true end
	return false
end

local function refreshEnemyFilter()
	for uid, data in pairs(playerEntries) do
		local plr = data.Player
		if not plr or not plr.Parent then
			data.Frame.Visible = false
		elseif showEnemiesOnly then
			local hide = (standTarget and plr.UserId == standTarget.UserId)
				or (plr == player)
				or getIsSiting(plr)
			data.Frame.Visible = not hide
		else
			data.Frame.Visible = true
		end
	end
end

local function stopStand()
	if standTarget then
		restoreStandUserHumanoid(standTarget)
	end
	standTarget = nil
	if standConn then
		standConn:Disconnect()
		standConn = nil
	end
	if ragdollLoopTask then
		task.cancel(ragdollLoopTask)
		ragdollLoopTask = nil
	end
	refreshEnemyFilter()
end

local function startStand(targetPlr)
	stopStand()
	standTarget = targetPlr
	local uid = targetPlr.UserId
	selectedTargets[uid] = nil
	loopKillTargets[uid] = nil
	ragdollTargets[uid] = nil
	shredDodgesTargets[uid] = nil
	flingTargets[uid] = nil

	deleteStandUserHumanoid(targetPlr)

	if targetPlr.Character then
		local hrp = targetPlr.Character:FindFirstChild("HumanoidRootPart")
		if hrp and originalHitboxSizes[hrp] then
			local d = originalHitboxSizes[hrp]
			pcall(function()
				hrp.Size = d.Size
				hrp.Transparency = d.Transparency
				hrp.CanCollide = d.CanCollide
				hrp.Massless = d.Massless
			end)
			originalHitboxSizes[hrp] = nil
		end
	end
	setIsSitingFalse()
	doNormalJump()
	manageCanNotRagdoll()
	refreshEnemyFilter()

	standConn = RunService.Heartbeat:Connect(function()
		if not standTarget or not standTarget.Parent then
			stopStand()
			return
		end

		local localChar = player.Character
		if not localChar then return end
		local localHrp = localChar:FindFirstChild("HumanoidRootPart")
		if not localHrp then return end

		local charactersFolder = Workspace:FindFirstChild("Characters")
		local targetModel = charactersFolder and charactersFolder:FindFirstChild(standTarget.Name)
		if not targetModel and standTarget.Character then
			targetModel = standTarget.Character
		end
		if not targetModel then return end
		local targetHrp = targetModel:FindFirstChild("HumanoidRootPart")
		if not targetHrp then return end

		local offset = CFrame.new(1.6, 2, 3.5)
		local desired = targetHrp.CFrame * offset
		pcall(function()
			localHrp.CFrame = localHrp.CFrame:Lerp(desired, STAND_LERP)
			localHrp.AssemblyLinearVelocity = Vector3.zero
			localHrp.AssemblyAngularVelocity = Vector3.zero
		end)
		setIsSitingFalse()
		zeroCantHitStun()
	end)
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "CombatPlayerlist"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.DisplayOrder = 999

local function getParent()
	if gethui then return gethui() end
	return player:WaitForChild("PlayerGui")
end
ScreenGui.Parent = getParent()

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Parent = ScreenGui
Main.BackgroundColor3 = Color3.fromRGB(12, 14, 20)
Main.Position = UDim2.new(0.5, -220, 0.5, -240)
Main.Size = UDim2.new(0, 440, 0, 480)
Main.Active = true
Main.Draggable = true
Main.BorderSizePixel = 0
Main.Visible = true
Main.ClipsDescendants = true

Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 12)
local stroke = Instance.new("UIStroke", Main)
stroke.Color = Color3.fromRGB(60, 230, 240)
stroke.Thickness = 1.5

local Title = Instance.new("TextLabel")
Title.Parent = Main
Title.BackgroundColor3 = Color3.fromRGB(12, 16, 24)
Title.Size = UDim2.new(1, 0, 0, 44)
Title.Font = Enum.Font.GothamBold
Title.Text = "  Combat + OP"
Title.TextColor3 = Color3.fromRGB(240, 252, 255)
Title.TextSize = 18
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.BorderSizePixel = 0
Instance.new("UICorner", Title).CornerRadius = UDim.new(0, 12)

local HideBtn = Instance.new("TextButton")
HideBtn.Parent = Title
HideBtn.BackgroundColor3 = Color3.fromRGB(25, 185, 195)
HideBtn.Position = UDim2.new(1, -88, 0.5, -15)
HideBtn.Size = UDim2.new(0, 74, 0, 30)
HideBtn.Font = Enum.Font.GothamBold
HideBtn.Text = "Hide"
HideBtn.TextColor3 = Color3.new(1,1,1)
HideBtn.TextSize = 13
HideBtn.BorderSizePixel = 0
HideBtn.ZIndex = 50
Instance.new("UICorner", HideBtn).CornerRadius = UDim.new(0, 8)

local ShowBtn = Instance.new("TextButton")
ShowBtn.Name = "ShowButton"
ShowBtn.Parent = ScreenGui
ShowBtn.BackgroundColor3 = Color3.fromRGB(8, 10, 14)
ShowBtn.BackgroundTransparency = 0.35
ShowBtn.AnchorPoint = Vector2.new(0.5, 0)
ShowBtn.Position = UDim2.new(0.5, 0, 0, 28)
ShowBtn.Size = UDim2.new(0, 124, 0, 38)
ShowBtn.Font = Enum.Font.GothamBold
ShowBtn.Text = "Show Hub"
ShowBtn.TextColor3 = Color3.new(1,1,1)
ShowBtn.TextSize = 13
ShowBtn.ZIndex = 100
ShowBtn.Visible = false
ShowBtn.BorderSizePixel = 0
ShowBtn.Active = true
Instance.new("UICorner", ShowBtn).CornerRadius = UDim.new(0, 8)
local ShowStroke = Instance.new("UIStroke", ShowBtn)
ShowStroke.Color = Color3.fromRGB(40, 240, 245)
ShowStroke.Thickness = 2

local showDragging, showDidDrag = false, false
local showDragInput, showDragStart, showStartPos = nil, nil, nil
local SHOW_DRAG_THRESHOLD = 10

ShowBtn.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		showDragging = true
		showDidDrag = false
		showDragInput = input
		showDragStart = input.Position
		showStartPos = ShowBtn.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				showDragging = false
				showDragInput = nil
			end
		end)
	end
end)

UserInputService.InputEnded:Connect(function(input)
	if input == showDragInput or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		showDragging = false
		showDragInput = nil
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if not showDragging or not showDragStart or not showStartPos then return end
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		local delta = input.Position - showDragStart
		if delta.Magnitude >= SHOW_DRAG_THRESHOLD then showDidDrag = true end
		if showDidDrag then
			ShowBtn.Position = UDim2.new(
				showStartPos.X.Scale, showStartPos.X.Offset + delta.X,
				showStartPos.Y.Scale, showStartPos.Y.Offset + delta.Y
			)
		end
	end
end)

local function setGuiVisible(vis)
	guiVisible = vis
	Main.Visible = vis
	ShowBtn.Visible = not vis
end

HideBtn.MouseButton1Click:Connect(function()
	setGuiVisible(false)
end)
ShowBtn.MouseButton1Click:Connect(function()
	if showDidDrag then
		showDidDrag = false
		return
	end
	setGuiVisible(true)
end)

UserInputService.InputBegan:Connect(function(input, gp)
	if gp then return end
	if input.KeyCode == Enum.KeyCode.RightShift then
		setGuiVisible(not guiVisible)
	end
end)

local TabBar = Instance.new("Frame")
TabBar.Parent = Main
TabBar.BackgroundTransparency = 1
TabBar.Position = UDim2.new(0, 10, 0, 50)
TabBar.Size = UDim2.new(1, -20, 0, 34)

local CombatTab = Instance.new("TextButton")
CombatTab.Parent = TabBar
CombatTab.BackgroundColor3 = Color3.fromRGB(18, 160, 168)
CombatTab.Size = UDim2.new(0.33, -4, 1, 0)
CombatTab.Position = UDim2.new(0, 0, 0, 0)
CombatTab.Font = Enum.Font.GothamBold
CombatTab.Text = "Combat"
CombatTab.TextColor3 = Color3.new(1,1,1)
CombatTab.TextSize = 13
CombatTab.BorderSizePixel = 0
Instance.new("UICorner", CombatTab).CornerRadius = UDim.new(0, 8)

local NoCDTab = Instance.new("TextButton")
NoCDTab.Parent = TabBar
NoCDTab.BackgroundColor3 = Color3.fromRGB(22, 28, 38)
NoCDTab.Size = UDim2.new(0.33, -4, 1, 0)
NoCDTab.Position = UDim2.new(0.33, 2, 0, 0)
NoCDTab.Font = Enum.Font.GothamBold
NoCDTab.Text = "No CD"
NoCDTab.TextColor3 = Color3.fromRGB(170, 185, 195)
NoCDTab.TextSize = 13
NoCDTab.BorderSizePixel = 0
Instance.new("UICorner", NoCDTab).CornerRadius = UDim.new(0, 8)

local OPTab = Instance.new("TextButton")
OPTab.Parent = TabBar
OPTab.BackgroundColor3 = Color3.fromRGB(22, 28, 38)
OPTab.Size = UDim2.new(0.33, -4, 1, 0)
OPTab.Position = UDim2.new(0.66, 4, 0, 0)
OPTab.Font = Enum.Font.GothamBold
OPTab.Text = "OP"
OPTab.TextColor3 = Color3.fromRGB(170, 185, 195)
OPTab.TextSize = 13
OPTab.BorderSizePixel = 0
Instance.new("UICorner", OPTab).CornerRadius = UDim.new(0, 8)

local CombatFrame = Instance.new("Frame")
CombatFrame.Parent = Main
CombatFrame.BackgroundTransparency = 1
CombatFrame.Position = UDim2.new(0, 0, 0, 90)
CombatFrame.Size = UDim2.new(1, 0, 1, -100)
CombatFrame.Visible = true

local NoCDFrame = Instance.new("Frame")
NoCDFrame.Parent = Main
NoCDFrame.BackgroundTransparency = 1
NoCDFrame.Position = UDim2.new(0, 0, 0, 90)
NoCDFrame.Size = UDim2.new(1, 0, 1, -100)
NoCDFrame.Visible = false

local OPFrame = Instance.new("Frame")
OPFrame.Parent = Main
OPFrame.BackgroundTransparency = 1
OPFrame.Position = UDim2.new(0, 0, 0, 90)
OPFrame.Size = UDim2.new(1, 0, 1, -100)
OPFrame.Visible = false

local function selectTab(name)
	local on = Color3.fromRGB(18, 160, 168)
	local off = Color3.fromRGB(22, 28, 38)
	local onT = Color3.new(1,1,1)
	local offT = Color3.fromRGB(170, 185, 195)
	
	CombatFrame.Visible = (name == "Combat")
	NoCDFrame.Visible = (name == "No CD")
	OPFrame.Visible = (name == "OP")
	
	CombatTab.BackgroundColor3 = (name == "Combat") and on or off
	CombatTab.TextColor3 = (name == "Combat") and onT or offT
	
	NoCDTab.BackgroundColor3 = (name == "No CD") and on or off
	NoCDTab.TextColor3 = (name == "No CD") and onT or offT
	
	OPTab.BackgroundColor3 = (name == "OP") and on or off
	OPTab.TextColor3 = (name == "OP") and onT or offT
end

CombatTab.MouseButton1Click:Connect(function() selectTab("Combat") end)
NoCDTab.MouseButton1Click:Connect(function() selectTab("No CD") end)
OPTab.MouseButton1Click:Connect(function() selectTab("OP") end)

local NoCDBtn = Instance.new("TextButton")
NoCDBtn.Parent = NoCDFrame
NoCDBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
NoCDBtn.Position = UDim2.new(0.04, 0, 0, 10)
NoCDBtn.Size = UDim2.new(0.92, 0, 0, 40)
NoCDBtn.Font = Enum.Font.GothamBold
NoCDBtn.Text = "No Cooldown: OFF"
NoCDBtn.TextColor3 = Color3.new(1,1,1)
NoCDBtn.TextSize = 14
NoCDBtn.BorderSizePixel = 0
NoCDBtn.AutoButtonColor = false
Instance.new("UICorner", NoCDBtn).CornerRadius = UDim.new(0, 8)

NoCDBtn.MouseButton1Click:Connect(function()
	isNoCDEnabled = not isNoCDEnabled
	if isNoCDEnabled then
		NoCDBtn.Text = "No Cooldown: ON"
		NoCDBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
	else
		NoCDBtn.Text = "No Cooldown: OFF"
		NoCDBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
	end
end)

local DmgLabel = Instance.new("TextLabel")
DmgLabel.Parent = CombatFrame
DmgLabel.BackgroundTransparency = 1
DmgLabel.Position = UDim2.new(0, 12, 0, 0)
DmgLabel.Size = UDim2.new(1, -24, 0, 18)
DmgLabel.Font = Enum.Font.GothamMedium
DmgLabel.Text = "Hurt Damage: 316"
DmgLabel.TextColor3 = Color3.fromRGB(200, 200, 210)
DmgLabel.TextSize = 12
DmgLabel.TextXAlignment = Enum.TextXAlignment.Left

local DmgRow = Instance.new("Frame")
DmgRow.Parent = CombatFrame
DmgRow.BackgroundTransparency = 1
DmgRow.Position = UDim2.new(0, 10, 0, 22)
DmgRow.Size = UDim2.new(1, -20, 0, 28)

local dmgButtons = {}
for i, val in ipairs(DAMAGE_OPTIONS) do
	local b = Instance.new("TextButton")
	b.Parent = DmgRow
	b.Size = UDim2.new(1 / #DAMAGE_OPTIONS, -4, 1, 0)
	b.Position = UDim2.new((i - 1) / #DAMAGE_OPTIONS, 2, 0, 0)
	b.BackgroundColor3 = (val == damageAmount) and Color3.fromRGB(40, 180, 40) or Color3.fromRGB(40, 48, 64)
	b.Font = Enum.Font.GothamBold
	b.Text = tostring(val)
	b.TextColor3 = Color3.new(1,1,1)
	b.TextSize = 12
	b.BorderSizePixel = 0
	Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)
	b.MouseButton1Click:Connect(function()
		damageAmount = val
		DmgLabel.Text = "Hurt Damage: " .. tostring(val)
		for _, other in ipairs(dmgButtons) do
			other.BackgroundColor3 = Color3.fromRGB(40, 48, 64)
		end
		b.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
	end)
	table.insert(dmgButtons, b)
end

local EnemiesOnlyBtn = Instance.new("TextButton")
EnemiesOnlyBtn.Parent = CombatFrame
EnemiesOnlyBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
EnemiesOnlyBtn.Position = UDim2.new(0.04, 0, 0, 54)
EnemiesOnlyBtn.Size = UDim2.new(0.92, 0, 0, 28)
EnemiesOnlyBtn.Font = Enum.Font.GothamBold
EnemiesOnlyBtn.Text = "Show Enemies Only: OFF"
EnemiesOnlyBtn.TextColor3 = Color3.new(1,1,1)
EnemiesOnlyBtn.TextSize = 12
EnemiesOnlyBtn.BorderSizePixel = 0
EnemiesOnlyBtn.AutoButtonColor = false
Instance.new("UICorner", EnemiesOnlyBtn).CornerRadius = UDim.new(0, 6)

EnemiesOnlyBtn.MouseButton1Click:Connect(function()
	showEnemiesOnly = not showEnemiesOnly
	if showEnemiesOnly then
		EnemiesOnlyBtn.Text = "Show Enemies Only: ON"
		EnemiesOnlyBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
	else
		EnemiesOnlyBtn.Text = "Show Enemies Only: OFF"
		EnemiesOnlyBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
	end
	refreshEnemyFilter()
end)

local Scroll = Instance.new("ScrollingFrame")
Scroll.Parent = CombatFrame
Scroll.BackgroundColor3 = Color3.fromRGB(22, 26, 40)
Scroll.Position = UDim2.new(0, 10, 0, 88)
Scroll.Size = UDim2.new(1, -20, 1, -98)
Scroll.BorderSizePixel = 0
Scroll.ScrollBarThickness = 5
Scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
Scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
Instance.new("UICorner", Scroll).CornerRadius = UDim.new(0, 8)

local Layout = Instance.new("UIListLayout", Scroll)
Layout.Padding = UDim.new(0, 6)
Layout.SortOrder = Enum.SortOrder.LayoutOrder
local Padding = Instance.new("UIPadding", Scroll)
Padding.PaddingTop = UDim.new(0, 6)
Padding.PaddingBottom = UDim.new(0, 6)
Padding.PaddingLeft = UDim.new(0, 6)
Padding.PaddingRight = UDim.new(0, 6)

local AutoFlingBtn = Instance.new("TextButton")
AutoFlingBtn.Parent = OPFrame
AutoFlingBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
AutoFlingBtn.Position = UDim2.new(0.04, 0, 0, 4)
AutoFlingBtn.Size = UDim2.new(0.92, 0, 0, 36)
AutoFlingBtn.Font = Enum.Font.GothamMedium
AutoFlingBtn.Text = "Fling Others: OFF"
AutoFlingBtn.TextColor3 = Color3.fromRGB(235, 250, 255)
AutoFlingBtn.TextSize = 14
AutoFlingBtn.BorderSizePixel = 0
AutoFlingBtn.AutoButtonColor = false
Instance.new("UICorner", AutoFlingBtn).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", AutoFlingBtn).Color = Color3.fromRGB(50, 160, 170)

AutoFlingBtn.MouseButton1Click:Connect(function()
	isAutoFlingOthers = not isAutoFlingOthers
	if isAutoFlingOthers then
		AutoFlingBtn.Text = "Fling Others: ON"
		AutoFlingBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
	else
		AutoFlingBtn.Text = "Fling Others: OFF"
		AutoFlingBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
	end
end)

local TrollScroll = Instance.new("ScrollingFrame")
TrollScroll.Parent = OPFrame
TrollScroll.BackgroundColor3 = Color3.fromRGB(22, 26, 40)
TrollScroll.Position = UDim2.new(0.04, 0, 0, 48)
TrollScroll.Size = UDim2.new(0.92, 0, 1, -58)
TrollScroll.BorderSizePixel = 0
TrollScroll.ScrollBarThickness = 5
TrollScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
TrollScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
Instance.new("UICorner", TrollScroll).CornerRadius = UDim.new(0, 8)

local TrollLayout = Instance.new("UIListLayout", TrollScroll)
TrollLayout.Padding = UDim.new(0, 5)
TrollLayout.SortOrder = Enum.SortOrder.LayoutOrder
local TrollPad = Instance.new("UIPadding", TrollScroll)
TrollPad.PaddingTop = UDim.new(0, 6)
TrollPad.PaddingBottom = UDim.new(0, 6)
TrollPad.PaddingLeft = UDim.new(0, 4)
TrollPad.PaddingRight = UDim.new(0, 4)

local universalAutoBlockTargets = {}
local universalBypassBlockTargets = {}

local function createPlayerEntry(plr)
	if playerEntries[plr.UserId] then return end
	local isLocal = (plr == player)

	local entry = Instance.new("Frame")
	entry.Name = plr.Name
	entry.BackgroundColor3 = Color3.fromRGB(30, 36, 54)
	entry.Size = UDim2.new(1, -6, 0, isLocal and 122 or 122)
	entry.BorderSizePixel = 0
	entry.LayoutOrder = isLocal and 0 or (plr.UserId % 1000000000)
	Instance.new("UICorner", entry).CornerRadius = UDim.new(0, 8)

	local nameLabel = Instance.new("TextLabel")
	nameLabel.Parent = entry
	nameLabel.BackgroundTransparency = 1
	nameLabel.Position = UDim2.new(0, 8, 0, 3)
	nameLabel.Size = UDim2.new(0.52, -8, 0, 18)
	nameLabel.Font = Enum.Font.GothamBold
	nameLabel.Text = plr.DisplayName .. (isLocal and " (You)" or "")
	nameLabel.TextColor3 = Color3.fromRGB(235, 235, 240)
	nameLabel.TextSize = 13
	nameLabel.TextXAlignment = Enum.TextXAlignment.Left
	nameLabel.TextTruncate = Enum.TextTruncate.AtEnd

	local hpLabel = Instance.new("TextLabel")
	hpLabel.Parent = entry
	hpLabel.BackgroundTransparency = 1
	hpLabel.Position = UDim2.new(0.52, 0, 0, 3)
	hpLabel.Size = UDim2.new(0.48, -8, 0, 18)
	hpLabel.Font = Enum.Font.GothamMedium
	hpLabel.Text = "HP: ? / ?"
	hpLabel.TextColor3 = Color3.fromRGB(120, 220, 160)
	hpLabel.TextSize = 11
	hpLabel.TextXAlignment = Enum.TextXAlignment.Right

	local function refreshHp()
		local hp, mx = "?", "?"
		local char = plr.Character
		local hum = char and char:FindFirstChildOfClass("Humanoid")
		if hum then hp = math.floor(hum.Health + 0.5) end
		local stats = plr:FindFirstChild("PlayerStats") or plr:FindFirstChild("playerstats")
		local maxH = stats and (stats:FindFirstChild("MaxHealth") and stats.MaxHealth.Value or stats:GetAttribute("MaxHealth"))
		if maxH == nil and hum then maxH = hum.MaxHealth end
		if maxH then mx = math.floor(maxH + 0.5) end
		hpLabel.Text = "HP: " .. tostring(hp) .. " / " .. tostring(mx)
	end
	refreshHp()
	task.spawn(function()
		while entry.Parent do refreshHp() task.wait(0.25) end
	end)

	local function makeBtn(text, x, y, w, color)
		local btn = Instance.new("TextButton")
		btn.Parent = entry
		btn.BackgroundColor3 = color or Color3.fromRGB(180, 40, 40)
		btn.Position = UDim2.new(x, 0, 0, y)
		btn.Size = UDim2.new(w, -4, 0, 26)
		btn.Font = Enum.Font.GothamBold
		btn.Text = text
		btn.TextColor3 = Color3.new(1,1,1)
		btn.TextSize = 11
		btn.BorderSizePixel = 0
		btn.AutoButtonColor = false
		Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
		return btn
	end

	local hitboxBtn  = makeBtn("Hitbox", 0.02, 24, 0.23)
	local hurtBtn    = makeBtn("Hurt",   0.27, 24, 0.23, Color3.fromRGB(200, 80, 40))
	local loopBtn    = makeBtn("Loop",   0.52, 24, 0.23)
	local ragdollBtn = makeBtn("Ragdoll", 0.77, 24, 0.21, Color3.fromRGB(180, 100, 40))
	local shredBtn   = makeBtn("Shred Dodges", 0.02, 56, 0.46, Color3.fromRGB(160, 60, 140))
	shredBtn.TextSize = 10
	local bringBtn   = makeBtn("Bring player", 0.50, 56, 0.48, Color3.fromRGB(40, 160, 100))
	bringBtn.TextSize = 10

	local standBtn
	if not isLocal then
		standBtn = makeBtn("Become Stand", 0.02, 88, 0.46, Color3.fromRGB(120, 60, 180))
		standBtn.TextSize = 10
	end

	local autoBlockBtn, bypassBlockBtn
	if isLocal then
		autoBlockBtn = makeBtn("Force AutoBlock", 0.02, 88, 0.46, Color3.fromRGB(50, 120, 180))
		autoBlockBtn.TextSize = 10
		bypassBlockBtn = makeBtn("Bypass Block", 0.50, 88, 0.48, Color3.fromRGB(80, 80, 160))
		bypassBlockBtn.TextSize = 10
	else
		autoBlockBtn = makeBtn("AutoBlock", 0.50, 88, 0.23, Color3.fromRGB(50, 120, 180))
		autoBlockBtn.TextSize = 9
		bypassBlockBtn = makeBtn("Bypass", 0.75, 88, 0.23, Color3.fromRGB(80, 80, 160))
		bypassBlockBtn.TextSize = 9
	end

	hitboxBtn.MouseButton1Click:Connect(function()
		if selectedTargets[plr.UserId] then
			selectedTargets[plr.UserId] = nil
			hitboxBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
			if plr.Character then
				local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
				if hrp and originalHitboxSizes[hrp] then
					local d = originalHitboxSizes[hrp]
					pcall(function()
						hrp.Size = d.Size
						hrp.Transparency = d.Transparency
						hrp.CanCollide = d.CanCollide
						hrp.Massless = d.Massless
					end)
					originalHitboxSizes[hrp] = nil
				end
			end
		else
			selectedTargets[plr.UserId] = true
			hitboxBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
		end
	end)

	hurtBtn.MouseButton1Click:Connect(function()
		dealDamage(plr, damageAmount)
	end)

	loopBtn.MouseButton1Click:Connect(function()
		if loopKillTargets[plr.UserId] then
			loopKillTargets[plr.UserId] = nil
			loopBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
			loopBtn.Text = "Loop"
		else
			loopKillTargets[plr.UserId] = true
			loopBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
			loopBtn.Text = "ON"
		end
	end)

	ragdollBtn.MouseButton1Click:Connect(function()
		if ragdollTargets[plr.UserId] then
			ragdollTargets[plr.UserId] = nil
			ragdollBtn.BackgroundColor3 = Color3.fromRGB(180, 100, 40)
			ragdollBtn.Text = "Ragdoll"
		else
			ragdollTargets[plr.UserId] = true
			ragdollBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
			ragdollBtn.Text = "ON"
		end
	end)

	shredBtn.MouseButton1Click:Connect(function()
		if shredDodgesTargets[plr.UserId] then
			shredDodgesTargets[plr.UserId] = nil
			shredBtn.BackgroundColor3 = Color3.fromRGB(160, 60, 140)
			shredBtn.Text = "Shred Dodges"
		else
			shredDodgesTargets[plr.UserId] = true
			shredBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
			shredBtn.Text = "Shred: ON"
		end
	end)

	bringBtn.MouseButton1Click:Connect(function()
		if standTarget and plr.UserId == standTarget.UserId then return end
		local remote = getHackRemote()
		if not remote then return end
		local localChar = player.Character
		if not localChar then return end
		local localHrp = localChar:FindFirstChild("HumanoidRootPart")
		if not localHrp then return end
		local charactersFolder = Workspace:FindFirstChild("Characters")
		local targetModel = charactersFolder and charactersFolder:FindFirstChild(plr.Name)
		if not targetModel then return end
		local targetHrp = targetModel:FindFirstChild("HumanoidRootPart")
		if not targetHrp then return end
		local rootAttachment = targetHrp:FindFirstChild("RootAttachment")
		if not rootAttachment then return end
		pcall(function()
			remote:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "Aligen_Welding", {
				true, {
					A = localHrp, P = targetHrp, C = localChar, N = player, T = targetModel,
					NA = "SSPAlty_Deathwhisper_ATCC",
					A1 = CFrame.new(0, 0, -4, -1, 0, -8.7422776573475858e-08, 0, 1, 0, 8.7422776573475858e-08, 0, -1),
					A0 = rootAttachment
				}
			})
		end)
	end)

	if standBtn then
		standBtn.MouseButton1Click:Connect(function()
			if standTarget == plr then
				stopStand()
				standBtn.BackgroundColor3 = Color3.fromRGB(120, 60, 180)
				standBtn.Text = "Become Stand"
			else
				startStand(plr)
				standBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
				standBtn.Text = "Stand: ON"
				for uid, data in pairs(playerEntries) do
					if uid ~= plr.UserId and data.StandBtn then
						data.StandBtn.BackgroundColor3 = Color3.fromRGB(120, 60, 180)
						data.StandBtn.Text = "Become Stand"
					end
				end
			end
		end)
	end

	if autoBlockBtn then
		autoBlockBtn.MouseButton1Click:Connect(function()
			if isLocal then
				if autoBlockTargets[plr.UserId] then
					autoBlockTargets[plr.UserId] = nil
					autoBlockBtn.BackgroundColor3 = Color3.fromRGB(50, 120, 180)
					autoBlockBtn.Text = "Force AutoBlock"
					if Event and plr.Character then
						pcall(function()
							Event:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "ChangeProptyOfSmtHSHHH__", {
								plr.Character, "Block", false, { A = false }
							})
						end)
					end
				else
					autoBlockTargets[plr.UserId] = true
					autoBlockBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
					autoBlockBtn.Text = "AutoBlock: ON"
				end
			else
				if universalAutoBlockTargets[plr.UserId] then
					universalAutoBlockTargets[plr.UserId] = nil
					autoBlockBtn.BackgroundColor3 = Color3.fromRGB(50, 120, 180)
					autoBlockBtn.Text = "AutoBlock"
					pcall(function()
						local cf = Workspace:FindFirstChild("Characters")
						local tm = cf and cf:FindFirstChild(plr.Name)
						if tm and Event then
							Event:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "ChangeProptyOfSmtHSHHH__", {
								tm, "Block", false, { A = false }
							})
						end
					end)
				else
					universalAutoBlockTargets[plr.UserId] = true
					autoBlockBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
					autoBlockBtn.Text = "AB: ON"
				end
			end
		end)
	end

	if bypassBlockBtn then
		bypassBlockBtn.MouseButton1Click:Connect(function()
			if isLocal then
				if bypassBlockTargets[plr.UserId] then
					bypassBlockTargets[plr.UserId] = nil
					bypassBlockBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 160)
					bypassBlockBtn.Text = "Bypass Block"
				else
					bypassBlockTargets[plr.UserId] = true
					bypassBlockBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
					bypassBlockBtn.Text = "Bypass: ON"
				end
			else
				if universalBypassBlockTargets[plr.UserId] then
					universalBypassBlockTargets[plr.UserId] = nil
					bypassBlockBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 160)
					bypassBlockBtn.Text = "Bypass"
				else
					universalBypassBlockTargets[plr.UserId] = true
					bypassBlockBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
					bypassBlockBtn.Text = "BP: ON"
				end
			end
		end)
	end

	entry.Parent = Scroll
	playerEntries[plr.UserId] = { Frame = entry, Player = plr, StandBtn = standBtn }
end

local function createTrollPlayerEntry(plr)
	if trollPlayerEntries[plr.UserId] then return end

	local entry = Instance.new("Frame")
	entry.Name = plr.Name
	entry.BackgroundColor3 = Color3.fromRGB(30, 36, 54)
	entry.Size = UDim2.new(1, -6, 0, 40)
	entry.BorderSizePixel = 0
	entry.LayoutOrder = (plr == player) and 0 or (plr.UserId % 1000000000)
	Instance.new("UICorner", entry).CornerRadius = UDim.new(0, 8)

	local nameLabel = Instance.new("TextLabel")
	nameLabel.Parent = entry
	nameLabel.BackgroundTransparency = 1
	nameLabel.Position = UDim2.new(0, 8, 0, 0)
	nameLabel.Size = UDim2.new(0.30, 0, 1, 0)
	nameLabel.Font = Enum.Font.GothamBold
	nameLabel.Text = plr.DisplayName .. (plr == player and " (You)" or "")
	nameLabel.TextColor3 = Color3.fromRGB(235, 235, 240)
	nameLabel.TextSize = 12
	nameLabel.TextXAlignment = Enum.TextXAlignment.Left
	nameLabel.TextTruncate = Enum.TextTruncate.AtEnd

	local flingBtn = Instance.new("TextButton")
	flingBtn.Parent = entry
	flingBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
	flingBtn.Position = UDim2.new(0.32, 0, 0.5, -13)
	flingBtn.Size = UDim2.new(0.32, -4, 0, 26)
	flingBtn.Font = Enum.Font.GothamBold
	flingBtn.Text = "Fling Player"
	flingBtn.TextColor3 = Color3.new(1,1,1)
	flingBtn.TextSize = 10
	flingBtn.BorderSizePixel = 0
	flingBtn.AutoButtonColor = false
	Instance.new("UICorner", flingBtn).CornerRadius = UDim.new(0, 6)

	flingBtn.MouseButton1Click:Connect(function()
		if flingTargets[plr.UserId] then
			flingTargets[plr.UserId] = nil
			flingBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
			flingBtn.Text = "Fling Player"
		else
			flingTargets[plr.UserId] = true
			flingBtn.BackgroundColor3 = Color3.fromRGB(40, 180, 40)
			flingBtn.Text = "ON"
		end
	end)

	local deleteBtn = Instance.new("TextButton")
	deleteBtn.Parent = entry
	deleteBtn.BackgroundColor3 = Color3.fromRGB(140, 30, 30)
	deleteBtn.Position = UDim2.new(0.66, 0, 0.5, -13)
	deleteBtn.Size = UDim2.new(0.32, -6, 0, 26)
	deleteBtn.Font = Enum.Font.GothamBold
	deleteBtn.Text = "Delete Character"
	deleteBtn.TextColor3 = Color3.new(1,1,1)
	deleteBtn.TextSize = 9
	deleteBtn.BorderSizePixel = 0
	deleteBtn.AutoButtonColor = false
	Instance.new("UICorner", deleteBtn).CornerRadius = UDim.new(0, 6)

	deleteBtn.MouseButton1Click:Connect(function()
		deleteCharacterAnimated(plr)
	end)

	entry.Parent = TrollScroll
	trollPlayerEntries[plr.UserId] = { Frame = entry, Player = plr }
end

local function removePlayerEntry(userId)
	local data = playerEntries[userId]
	if data then
		if standTarget and standTarget.UserId == userId then stopStand() end
		data.Frame:Destroy()
		playerEntries[userId] = nil
		selectedTargets[userId] = nil
		loopKillTargets[userId] = nil
		ragdollTargets[userId] = nil
		shredDodgesTargets[userId] = nil
		autoBlockTargets[userId] = nil
		bypassBlockTargets[userId] = nil
		universalAutoBlockTargets[userId] = nil
		universalBypassBlockTargets[userId] = nil
	end
	local tdata = trollPlayerEntries[userId]
	if tdata then
		tdata.Frame:Destroy()
		trollPlayerEntries[userId] = nil
		flingTargets[userId] = nil
	end
end

for _, plr in ipairs(Players:GetPlayers()) do
	createPlayerEntry(plr)
	createTrollPlayerEntry(plr)
end
Players.PlayerAdded:Connect(function(plr)
	task.wait(0.3)
	createPlayerEntry(plr)
	createTrollPlayerEntry(plr)
end)
Players.PlayerRemoving:Connect(function(plr)
	removePlayerEntry(plr.UserId)
end)

task.spawn(function()
	while true do
		task.wait(0.08)
		for userId in pairs(loopKillTargets) do
			local plr = Players:GetPlayerByUserId(userId)
			if plr and not (standTarget and plr.UserId == standTarget.UserId) then dealDamage(plr, damageAmount) end
		end
	end
end)

task.spawn(function()
	while true do
		task.wait(0.15)
		for userId in pairs(ragdollTargets) do
			local plr = Players:GetPlayerByUserId(userId)
			if plr and Event and not (standTarget and plr.UserId == standTarget.UserId) then
				local charactersFolder = Workspace:FindFirstChild("Characters")
				local characterModel = charactersFolder and charactersFolder:FindFirstChild(plr.Name)
				if characterModel then
					pcall(function()
						Event:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "Ragdoll_Character", {
							characterModel, true, { T = 2.5, J = 5 }
						})
					end)
				end
			end
		end
	end
end)

task.spawn(function()
	while true do
		task.wait(0.1)
		for userId in pairs(shredDodgesTargets) do
			local plr = Players:GetPlayerByUserId(userId)
			if plr and Event and not (standTarget and plr.UserId == standTarget.UserId) then
				pcall(function()
					Event:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "IsCANDOdgee", {
						plr, { NOPLrrr = plr, AniMNumm = 1 }
					})
				end)
			end
		end
	end
end)

task.spawn(function()
	while true do
		task.wait()
		for userId in pairs(flingTargets) do
			local plr = Players:GetPlayerByUserId(userId)
			if plr and not (standTarget and plr.UserId == standTarget.UserId) then flingPlayer(plr) end
		end
		if isAutoFlingOthers then
			for _, plr in ipairs(Players:GetPlayers()) do
				if plr ~= player and not (standTarget and plr.UserId == standTarget.UserId) then flingPlayer(plr) end
			end
		end
	end
end)

RunService.Heartbeat:Connect(function()
	for userId in pairs(selectedTargets) do
		local plr = Players:GetPlayerByUserId(userId)
		if plr and plr.Character and not (standTarget and plr.UserId == standTarget.UserId) then
			local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
			if hrp and not originalHitboxSizes[hrp] then
				originalHitboxSizes[hrp] = {
					Size = hrp.Size, Transparency = hrp.Transparency,
					CanCollide = hrp.CanCollide, Massless = hrp.Massless
				}
				pcall(function()
					hrp.Size = Vector3.new(12, 12, 12)
					hrp.Transparency = 0.7
					hrp.CanCollide = false
					hrp.Massless = true
				end)
			end
		end
	end
	for userId in pairs(autoBlockTargets) do
		local plr = Players:GetPlayerByUserId(userId)
		if plr and Event and plr.Character then
			pcall(function()
				Event:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "ChangeProptyOfSmtHSHHH__", {
					plr.Character, "Block", true, { A = true }
				})
			end)
		end
	end
	for userId in pairs(bypassBlockTargets) do
		local plr = Players:GetPlayerByUserId(userId)
		if plr and Event and plr.Character then
			pcall(function()
				Event:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "ChangeProptyOfSmtHSHHH__", {
					plr.Character, "Block", false, { A = false }
				})
			end)
		end
	end
	for userId in pairs(universalAutoBlockTargets) do
		local plr = Players:GetPlayerByUserId(userId)
		if plr and Event then
			pcall(function()
				local charactersFolder = Workspace:FindFirstChild("Characters")
				local targetModel = charactersFolder and charactersFolder:FindFirstChild(plr.Name)
				if targetModel then
					Event:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "ChangeProptyOfSmtHSHHH__", {
						targetModel, "Block", true, { A = true }
					})
				end
			end)
		end
	end
	for userId in pairs(universalBypassBlockTargets) do
		local plr = Players:GetPlayerByUserId(userId)
		if plr and Event then
			pcall(function()
				local charactersFolder = Workspace:FindFirstChild("Characters")
				local targetModel = charactersFolder and charactersFolder:FindFirstChild(plr.Name)
				if targetModel then
					Event:FireServer("EMMFOSS__!ZCNSJNXCSDWQSANBX", "ChangeProptyOfSmtHSHHH__", {
						targetModel, "Block", false, { A = false }
					})
				end
			end)
		end
	end
end)

task.spawn(function()
	while true do
		task.wait(0.5)
		if showEnemiesOnly then refreshEnemyFilter() end
	end
end)

print("Combat + OP loaded | RightShift toggle")
