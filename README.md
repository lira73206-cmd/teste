--// EPIC TUBERS93 GUI V4 //--
--================================================================================--
--// SERVICES
--================================================================================--
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
local ContentProvider = game:GetService("ContentProvider")
--================================================================================--
--// CONFIGURATION & DATA
--================================================================================--
local Config = {
	Font = Enum.Font.GothamSemibold,
	TitleFont = Enum.Font.GothamBlack,
	TextColor = Color3.fromRGB(255, 255, 255),
	BackgroundColor = Color3.fromRGB(35, 37, 41),
	SecondaryColor = Color3.fromRGB(28, 29, 33),
	BorderColor = Color3.fromRGB(60, 60, 60),
	InactiveTabColor = Color3.fromRGB(45, 47, 51),
	AnimationSpeed = 0.3,
	EasingStyle = Enum.EasingStyle.Quad,
	EasingDirection = Enum.EasingDirection.Out
}
-- This will store favorited scripts. In a real scenario, you'd use save/writefile.
local FavoritesDB = {} 
-- This table will hold scripts added dynamically to the main page.
local MainTabScripts = {}
--================================================================================--
--// SPLASH SCREEN
--================================================================================--
local splashGui = Instance.new("ScreenGui")
splashGui.Name = "SplashScreen"
splashGui.Parent = PlayerGui
local splashFrame = Instance.new("Frame")
splashFrame.Size = UDim2.new(1, 0, 1, 0)
splashFrame.BackgroundColor3 = Color3.fromRGB(28, 29, 33)
splashFrame.Parent = splashGui
splashFrame.ZIndex = 1
local splashImage = Instance.new("ImageLabel")
splashImage.Size = UDim2.new(0.3, 0, 0.6, 0)
splashImage.Position = UDim2.new(0.5, 0, 0.45, 0)
splashImage.AnchorPoint = Vector2.new(0.5, 0.5)
splashImage.BackgroundTransparency = 1
splashImage.Image = "rbxthumb://type=Asset&id=82039339420725&w=420&h=420"
splashImage.Rotation = 90
splashImage.Parent = splashFrame
splashImage.ZIndex = 2
local splashText = Instance.new("TextLabel")
splashText.Text = "made by The Noob King"
splashText.Size = UDim2.new(1, 0, 0.1, 0)
splashText.Position = UDim2.new(0.5, 0, 0.8, 0)
splashText.AnchorPoint = Vector2.new(0.5, 0)
splashText.BackgroundTransparency = 1
splashText.Font = Enum.Font.GothamBlack
splashText.TextSize = 24
splashText.TextColor3 = Color3.fromRGB(255, 255, 255)
splashText.Parent = splashFrame
-- Play splash screen sound
local splashSfx = Instance.new("Sound", splashFrame)
splashSfx.SoundId = "rbxassetid://154147007"
splashSfx:Play()
pcall(function() ContentProvider:PreloadAsync({splashImage}) end)
task.spawn(function()
	while splashText.Parent do
		for i = 0, 1, 0.05 do
			if not splashText.Parent then break end
			splashText.TextColor3 = Color3.fromHSV(i, 1, 1)
			task.wait()
		end
	end
end)
task.wait(5)
local frameTween = TweenService:Create(splashFrame, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {BackgroundTransparency = 1})
local imageTween = TweenService:Create(splashImage, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {ImageTransparency = 1})
local textTween = TweenService:Create(splashText, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {TextTransparency = 1})
frameTween:Play()
imageTween:Play()
textTween:Play()
frameTween.Completed:Wait()
splashGui:Destroy()
--================================================================================--
--// MAIN GUI SETUP
--================================================================================--
if PlayerGui:FindFirstChild("Tubers93FEUniversal_Modern") then
	PlayerGui.Tubers93FEUniversal_Modern:Destroy()
end
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "Tubers93FEUniversal_Modern"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Global
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui
local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.new(0, 500, 0, 380)
Main.Position = UDim2.new(-1, 0, 0.5, 0)
Main.AnchorPoint = Vector2.new(0.5, 0.5)
Main.BackgroundColor3 = Config.BackgroundColor
Main.Active = true
Main.Draggable = true
Main.Parent = ScreenGui
Main.Visible = true
-- Sound Manager Setup
local SoundManager = {
	BackgroundMusic = Instance.new("Sound", Main),
	Ultra_SFX1 = Instance.new("Sound", Main),
	Ultra_SFX2 = Instance.new("Sound", Main),
	Ultra_Music = Instance.new("Sound", Main),
	ScriptButtonSFX = Instance.new("Sound", Main) -- SFX for script buttons
}
SoundManager.BackgroundMusic.SoundId = "rbxassetid://1848354536"
SoundManager.BackgroundMusic.Looped = true
SoundManager.BackgroundMusic.Volume = 0.5
SoundManager.BackgroundMusic:Play()
SoundManager.Ultra_SFX1.SoundId = "rbxassetid://103215672097028"
SoundManager.Ultra_SFX2.SoundId = "rbxassetid://107706517765020"
SoundManager.Ultra_Music.SoundId = "rbxassetid://1848354536" 
SoundManager.Ultra_Music.PlaybackSpeed = 0.9
SoundManager.Ultra_Music.Looped = true
SoundManager.ScriptButtonSFX.SoundId = "rbxassetid://1464193038" -- Added SFX ID
task.wait(0.2)
TweenService:Create(Main, TweenInfo.new(0.5, Config.EasingStyle, Config.EasingDirection), {Position = UDim2.new(0.5, 0, 0.5, 0)}):Play()
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 8)
local stroke = Instance.new("UIStroke", Main)
stroke.Color = Config.BorderColor
stroke.Thickness = 1.5
local gradient = Instance.new("UIGradient", Main)
gradient.Color = ColorSequence.new({ColorSequenceKeypoint.new(0, Color3.fromRGB(55, 57, 61)), ColorSequenceKeypoint.new(1, Color3.fromRGB(35, 37, 41))})
gradient.Rotation = 90
local lightningFX = Instance.new("Frame", Main)
lightningFX.Size = UDim2.new(1, 0, 1, 0)
lightningFX.BackgroundTransparency = 1
lightningFX.ZIndex = 3 
lightningFX.ClipsDescendants = true
local Header = Instance.new("Frame", Main)
Header.Name = "Header"
Header.Size = UDim2.new(1, 0, 0, 40)
Header.BackgroundColor3 = Config.SecondaryColor
Header.BackgroundTransparency = 0.3
Instance.new("UIStroke", Header).Color = Config.BorderColor
local Title = Instance.new("TextLabel", Header)
Title.Name = "Title"
Title.Size = UDim2.new(1, -10, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.Text = "EPIC TUBERS93 GUI V4"
Title.Font = Config.TitleFont
Title.TextSize = 18
Title.TextColor3 = Config.TextColor
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.BackgroundTransparency = 1
local Credits = Instance.new("TextLabel", Header)
Credits.Name = "Credits"
Credits.Size = UDim2.new(1, -65, 1, 0)
Credits.Position = UDim2.new(0, 0, 0, 0)
Credits.Text = "by Tubers93/Noob King"
Credits.Font = Config.Font
Credits.TextSize = 12
Credits.TextColor3 = Color3.fromRGB(180, 180, 180)
Credits.TextXAlignment = Enum.TextXAlignment.Right
Credits.BackgroundTransparency = 1
local ButtonsFrame = Instance.new("Frame", Header)
ButtonsFrame.Name = "ButtonsFrame"
ButtonsFrame.Size = UDim2.new(0, 60, 1, 0)
ButtonsFrame.Position = UDim2.new(1, -60, 0, 0)
ButtonsFrame.BackgroundTransparency = 1
local CloseButton = Instance.new("TextButton", ButtonsFrame)
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 20, 0, 20)
CloseButton.Position = UDim2.new(1, -2, 0.5, 0)
CloseButton.AnchorPoint = Vector2.new(1, 0.5)
CloseButton.Text = "✖"
CloseButton.Font = Enum.Font.SourceSans
CloseButton.TextSize = 18
CloseButton.TextColor3 = Config.TextColor
CloseButton.BackgroundTransparency = 1
local MinMaxButton = Instance.new("TextButton", ButtonsFrame)
MinMaxButton.Name = "MinMaxButton"
MinMaxButton.Size = UDim2.new(0, 20, 0, 20)
MinMaxButton.Position = UDim2.new(1, -22, 0.5, 0)
MinMaxButton.AnchorPoint = Vector2.new(1, 0.5)
MinMaxButton.Text = "▼"
MinMaxButton.Font = Enum.Font.SourceSans
MinMaxButton.TextSize = 18
MinMaxButton.TextColor3 = Config.TextColor
MinMaxButton.BackgroundTransparency = 1
CloseButton.MouseEnter:Connect(function() TweenService:Create(CloseButton, TweenInfo.new(Config.AnimationSpeed), { TextColor3 = Color3.fromRGB(255, 50, 50) }):Play() end)
CloseButton.MouseLeave:Connect(function() TweenService:Create(CloseButton, TweenInfo.new(Config.AnimationSpeed), { TextColor3 = Color3.fromRGB(255, 255, 255) }):Play() end)
CloseButton.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)
MinMaxButton.MouseEnter:Connect(function() TweenService:Create(MinMaxButton, TweenInfo.new(Config.AnimationSpeed), { TextColor3 = Color3.fromRGB(150, 150, 150) }):Play() end)
MinMaxButton.MouseLeave:Connect(function() TweenService:Create(MinMaxButton, TweenInfo.new(Config.AnimationSpeed), { TextColor3 = Color3.fromRGB(255, 255, 255) }):Play() end)
local minButton = Instance.new("TextButton", ScreenGui)
minButton.Name = "MinimizedButton"
minButton.Size = UDim2.new(0, 40, 0, 40)
minButton.Position = UDim2.new(0.5, 0, 0, 25)
minButton.AnchorPoint = Vector2.new(0.5, 0.5)
minButton.BackgroundColor3 = Config.SecondaryColor
minButton.Text = "T"
minButton.Font = Config.TitleFont
minButton.TextSize = 24
minButton.TextColor3 = Config.TextColor
minButton.Active = true
minButton.Draggable = true
minButton.Visible = false
Instance.new("UICorner", minButton).CornerRadius = UDim.new(1, 0)
local minStroke = Instance.new("UIStroke", minButton)
minStroke.Color = Color3.fromRGB(255, 255, 255)
minStroke.Thickness = 0.8
local isMaximized = true
local originalSize = Main.Size
local originalPosition = Main.Position
local function toggleGUI()
	isMaximized = not isMaximized
	local animationInfo = TweenInfo.new(Config.AnimationSpeed, Config.EasingStyle, Config.EasingDirection)
	if isMaximized then
		minButton.Visible = false
		Main.Visible = true
		local sizeTween = TweenService:Create(Main, animationInfo, {Size = originalSize})
		local posTween = TweenService:Create(Main, animationInfo, {Position = originalPosition})
		sizeTween:Play()
		posTween:Play()
	else
		local targetPosition = minButton.Position
		local targetSize = UDim2.new(0, 0, 0, 0)
		local sizeTween = TweenService:Create(Main, animationInfo, {Size = targetSize})
		local posTween = TweenService:Create(Main, animationInfo, {Position = targetPosition})
		sizeTween:Play()
		posTween:Play()
		sizeTween.Completed:Connect(function()
			Main.Visible = false
			minButton.Visible = true
		end)
	end
end
MinMaxButton.MouseButton1Click:Connect(toggleGUI)
minButton.MouseButton1Click:Connect(toggleGUI)
local TabContainer = Instance.new("Frame", Main)
TabContainer.Name = "TabContainer"
TabContainer.Size = UDim2.new(0, 120, 1, -40)
TabContainer.Position = UDim2.new(0, 0, 0, 40)
TabContainer.BackgroundColor3 = Config.SecondaryColor
TabContainer.BorderSizePixel = 0
Instance.new("UIListLayout", TabContainer).Padding = UDim.new(0, 5)
Instance.new("UIStroke", TabContainer).Color = Config.BorderColor
local Content = Instance.new("Frame", Main)
Content.Name = "Content"
Content.Size = UDim2.new(1, -120, 1, -40)
Content.Position = UDim2.new(0, 120, 0, 40)
Content.BackgroundTransparency = 1
local currentTheme = {
	color = Color3.fromRGB(0, 170, 0),
	lightningColor = Color3.fromRGB(0, 255, 0)
}
--================================================================================--
--// HELPER FUNCTIONS & ANIMATIONS
--================================================================================--
local function createButton(parent, text, size, position)
	local btn = Instance.new("TextButton")
	btn.Size = size
	btn.Position = position
	btn.Text = text
	btn.Font = Config.Font
	btn.TextSize = 16
	btn.TextColor3 = Config.TextColor
	btn.BackgroundColor3 = Config.SecondaryColor
	btn.Parent = parent
	Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
	local stroke = Instance.new("UIStroke", btn)
	stroke.Color = currentTheme.color
	stroke.Thickness = 1.2
	btn.MouseEnter:Connect(function()
		TweenService:Create(btn, TweenInfo.new(Config.AnimationSpeed), { BackgroundColor3 = Config.BackgroundColor }):Play()
		TweenService:Create(stroke, TweenInfo.new(Config.AnimationSpeed), { Thickness = 2 }):Play()
	end)
	btn.MouseLeave:Connect(function()
		TweenService:Create(btn, TweenInfo.new(Config.AnimationSpeed), { BackgroundColor3 = Config.SecondaryColor }):Play()
		TweenService:Create(stroke, TweenInfo.new(Config.AnimationSpeed), { Thickness = 1.2 }):Play()
	end)
	return btn
end

--// MODIFIED: Seizure warning function with timer fix
local isWarningActive = false 
local function showSeizureWarning(onConfirm)
	if isWarningActive then return end
	isWarningActive = true

	local warningGui = Instance.new("ScreenGui")
	warningGui.Name = "SeizureWarning"
	warningGui.Parent = PlayerGui
	warningGui.ZIndexBehavior = Enum.ZIndexBehavior.Global
	warningGui.DisplayOrder = 1000

	local overlay = Instance.new("Frame", warningGui)
	overlay.Size = UDim2.new(1, 0, 1, 0)
	overlay.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	overlay.BackgroundTransparency = 0.7

	local warningContainer = Instance.new("Frame", overlay)
	warningContainer.Size = UDim2.new(0, 400, 0, 320)
	warningContainer.Position = UDim2.new(0.5, 0, 0.5, 0)
	warningContainer.AnchorPoint = Vector2.new(0.5, 0.5)
	warningContainer.BackgroundColor3 = Config.SecondaryColor
	warningContainer.BorderSizePixel = 0
	Instance.new("UICorner", warningContainer).CornerRadius = UDim.new(0, 8)
	Instance.new("UIStroke", warningContainer).Color = Color3.fromRGB(255, 0, 0)

	local layout = Instance.new("UIListLayout", warningContainer)
	layout.Padding = UDim.new(0, 10)
	layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
	layout.SortOrder = Enum.SortOrder.LayoutOrder

	local function createWarningLabel(text, size, isTitle)
		local label = Instance.new("TextLabel", warningContainer)
		label.Text = text
		label.Size = UDim2.new(1, -20, 0, size)
		label.BackgroundTransparency = 1
		label.Font = isTitle and Config.TitleFont or Config.Font
		label.TextSize = isTitle and 24 or 18
		label.TextColor3 = Color3.fromRGB(255, 20, 20)
		label.TextWrapped = true
		return label
	end

	createWarningLabel("⚠️WARNING⚠️", 30, true)
	createWarningLabel("EFFECTS CAN TRIGGER EPILEPTIC SEIZURES", 25, false)
	createWarningLabel("DO NOT USE THIS IF YOU ARE SENSITIVE TO FLASHING LIGHTS", 50, false)

	local continuePrompt = Instance.new("TextLabel", warningContainer)
	continuePrompt.Text = "DO YOU WANT TO CONTINUE"
	continuePrompt.Size = UDim2.new(1, -20, 0, 25)
	continuePrompt.BackgroundTransparency = 1
	continuePrompt.Font = Config.Font
	continuePrompt.TextSize = 18
	continuePrompt.TextColor3 = Config.TextColor

	local timerLabel = Instance.new("TextLabel", warningContainer)
	timerLabel.Text = "10"
	timerLabel.Size = UDim2.new(1, -20, 0, 30)
	timerLabel.BackgroundTransparency = 1
	timerLabel.Font = Config.TitleFont
	timerLabel.TextSize = 28
	timerLabel.TextColor3 = Config.TextColor

	local buttonFrame = Instance.new("Frame", warningContainer)
	buttonFrame.Size = UDim2.new(1, -20, 0, 50)
	buttonFrame.BackgroundTransparency = 1
	local buttonLayout = Instance.new("UIListLayout", buttonFrame)
	buttonLayout.FillDirection = Enum.FillDirection.Horizontal
	buttonLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
	buttonLayout.Padding = UDim.new(0, 20)

	local noButton = createButton(buttonFrame, "NO", UDim2.new(0.4, 0, 1, 0), UDim2.new())
	noButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
	noButton:FindFirstChildOfClass("UIStroke").Color = Color3.fromRGB(255,255,255)

	local yesButton = createButton(buttonFrame, "YES", UDim2.new(0.4, 0, 1, 0), UDim2.new())
	yesButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
	yesButton:FindFirstChildOfClass("UIStroke").Color = Color3.fromRGB(255,255,255)

	local cancelled = false
	local countdownCoroutine

	local function cleanup()
		cancelled = true
		if countdownCoroutine then
			coroutine.close(countdownCoroutine)
		end
		warningGui:Destroy()
		isWarningActive = false
	end

	noButton.MouseButton1Click:Connect(cleanup)

	yesButton.MouseButton1Click:Connect(function()
		cleanup()
		if onConfirm then onConfirm() end
	end)
	
	countdownCoroutine = coroutine.create(function()
		-- BUG FIX: Loop from 10 to 1, then set to 0 and cleanup immediately
		for i = 10, 1, -1 do
			if cancelled then break end
			if timerLabel and timerLabel.Parent then timerLabel.Text = tostring(i) end
			task.wait(1)
		end
		if not cancelled then
			if timerLabel and timerLabel.Parent then timerLabel.Text = "0" end
			task.wait(0.05) -- Tiny delay to show the "0" before disappearing
			cleanup()
		end
	end)
	coroutine.resume(countdownCoroutine)
end

local Pages, Tabs = {}, {}
local function switchTab(tabName)
	for name, page in pairs(Pages) do
		page.Visible = (name == tabName)
	end
	for name, tabButton in pairs(Tabs) do
		local targetColor = (name == tabName) and currentTheme.color or Config.InactiveTabColor
		TweenService:Create(tabButton, TweenInfo.new(Config.AnimationSpeed), { BackgroundColor3 = targetColor }):Play()
	end
end
local function createTab(name, layoutOrder)
	local page = Instance.new("Frame", Content)
	page.Name = name
	page.Size = UDim2.new(1, -10, 1, -10)
	page.Position = UDim2.new(0, 5, 0, 5)
	page.BackgroundTransparency = 1
	page.Visible = false
	Pages[name] = page
	local tabButton = Instance.new("TextButton", TabContainer)
	tabButton.Name = name .. "Tab"
	tabButton.Size = UDim2.new(1, -10, 0, 35)
	tabButton.Position = UDim2.new(0, 5, 0, 0)
	tabButton.Text = name
	tabButton.Font = Config.Font
	tabButton.TextSize = 16
	tabButton.TextColor3 = Config.TextColor
	tabButton.BackgroundColor3 = Config.InactiveTabColor
	tabButton.LayoutOrder = layoutOrder
	Instance.new("UICorner", tabButton).CornerRadius = UDim.new(0, 6)
	Tabs[name] = tabButton
	tabButton.MouseButton1Click:Connect(function() switchTab(name) end)
	return page
end
local function executeScript(button, scriptCode, originalText)
	button.Text = "Executing..."
	local pcallSuccess, pcallError = pcall(function()
		local func = loadstring(scriptCode)
		if func then func() end
	end)
	if not pcallSuccess then
		button.Text = "Syntax Error!"
		warn("Script execution error:", pcallError)
		task.wait(2)
		button.Text = originalText
	else
		button.Text = "Success! 🎉"
		task.wait(2)
		button.Text = originalText
	end
end
local function fetchAndExecute(button, url)
	SoundManager.ScriptButtonSFX:Play()
	local originalText = button.Text
	button.Text = "Fetching..."
	task.spawn(function()
		local success, scriptCode = pcall(function() return game:HttpGet(url, true) end)
		if not success or not scriptCode then
			button.Text = "HTTP Error!"
			task.wait(2)
			button.Text = originalText
			return
		end
		executeScript(button, scriptCode, originalText) 
	end)
end
--================================================================================--
--// CREATE PAGES & CONTENT
--================================================================================--
local mainPage = createTab("Main", 1)
local mainScroll = Instance.new("ScrollingFrame", mainPage)
mainScroll.Size = UDim2.new(1, 0, 1, 0)
mainScroll.ScrollBarThickness = 6
mainScroll.BackgroundTransparency = 1
mainScroll.BorderSizePixel = 0
local mainLayout = Instance.new("UIListLayout", mainScroll)
mainLayout.Padding = UDim.new(0, 5)
mainLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	mainScroll.CanvasSize = UDim2.new(0, 0, 0, mainLayout.AbsoluteContentSize.Y)
end)
local function addScriptToMainTab(name, url)
	for _, existingScript in ipairs(MainTabScripts) do
		if existingScript.url == url then return end
	end
	local scriptData = {name = name, url = url}
	table.insert(MainTabScripts, scriptData)
	local button = createButton(mainScroll, name, UDim2.new(1, 0, 0, 40), UDim2.new())
	button.MouseButton1Click:Connect(function() fetchAndExecute(button, url) end)
	return button
end
local initialScripts = {
	{name = "Infinite Yield", url = "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"}, {name = "Nameless Admin", url = "https://rawscripts.net/raw/Universal-Script-Nameless-admin-REWORKED-43502"}, {name = "FE INVIS UNIVERSAL", url = "https://rawscripts.net/raw/Universal-Script-UNIVERSAL-FE-INVISIBLE-39557"}, {name = "XP Backdoor Scanner", url = "https://rawscripts.net/raw/Universal-Script-XP-Backdoor-Scanner-51591"}, {name = "God Mode", url = "https://rawscripts.net/raw/Universal-Script-Invincible-Godmode-46968"}, {name = "FE Ragdoll", url = "https://pastebin.com/raw/z5NwZ0BF"}, {name = "FE POV Changer/Camera Spy (Mobile)", url = "https://pastebin.com/raw/8g6bHC7H"}, {name = "Walk on air", url = "https://rawscripts.net/raw/Universal-Script-Airwalk-43305"}, {name = "Goku UI Moveset (TSB)", url = "https://rawscripts.net/raw/Universal-Script-The-Strongest-Battleground-Goku-UI-Moveset-30839"}, {name = "XVC Hub", url = "https://rawscripts.net/raw/Universal-Script-XVC-Hub-159-Games-keyless-52467"}, {name = "Car Anim GUI", url = "https://rawscripts.net/raw/Universal-Script-FE-Car-Gui-50947"}, {name = "Car script v2", url = "https://rawscripts.net/raw/Universal-Script-FE-car-script-v2-18714"}, {name = "Roblox Egor Script", url = "https://rawscripts.net/raw/Universal-Script-Roblox-Egor-Script-49040"}, {name = "0 Gravity Trip FE", url = "https://rawscripts.net/raw/Universal-Script-0-Gravity-Trip-FE-35632"}, {name = "Sander XY (Brookhaven)", url = "https://rawscripts.net/raw/Brookhaven-RP-Sander-XY-35845"}, {name = "Tiger X (Brookhaven)", url = "https://rawscripts.net/raw/Brookhaven-RP-Tiger-X-39488"}, {name = "Ink Game Script (abdo)", url = "https://raw.githubusercontent.com/wefwef127382/inkgames.github.io/refs/heads/main/ringta.lua"}, {name = "R15 Dance Script UNIVERSAL", url = "https://rawscripts.net/raw/Universal-Script-Universal-FE-R15-Animation-Player-47905"},
}
for _, data in ipairs(initialScripts) do addScriptToMainTab(data.name, data.url) end
local clickTPBtn = createButton(mainScroll, "Click TP Tool", UDim2.new(1, 0, 0, 40), UDim2.new())
local clickTPTool
clickTPBtn.MouseButton1Click:Connect(function()
	SoundManager.ScriptButtonSFX:Play()
	if not clickTPTool or not clickTPTool.Parent then
		clickTPTool = Instance.new("Tool")
		clickTPTool.Name = "ClickTP"
		clickTPTool.RequiresHandle = false
		clickTPTool.CanBeDropped = false
		clickTPTool.Parent = LocalPlayer.Backpack
		local mouse = LocalPlayer:GetMouse()
		clickTPTool.Activated:Connect(function()
			local char = LocalPlayer.Character
			if char and mouse.Target then char:MoveTo(mouse.Hit.Position + Vector3.new(0, 5, 0)) end
		end)
	end
end)
local searchPage = createTab("Search", 2)
local searchPageLayout = Instance.new("UIListLayout", searchPage)
searchPageLayout.Padding = UDim.new(0, 5)
searchPageLayout.SortOrder = Enum.SortOrder.LayoutOrder
local searchBar = Instance.new("TextBox", searchPage)
searchBar.Size = UDim2.new(1, 0, 0, 40)
searchBar.PlaceholderText = "Search ScriptBlox..."
searchBar.Font = Config.Font
searchBar.TextSize = 16
searchBar.TextColor3 = Config.TextColor
searchBar.BackgroundColor3 = Config.SecondaryColor
searchBar.LayoutOrder = 1
Instance.new("UICorner", searchBar).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", searchBar).Color = Config.BorderColor
local favoritesContainer = Instance.new("ScrollingFrame", searchPage)
favoritesContainer.Name = "FavoritesContainer"
favoritesContainer.Size = UDim2.new(1, 0, 0.3, 0)
favoritesContainer.BackgroundTransparency = 1
favoritesContainer.ScrollBarThickness = 5
favoritesContainer.LayoutOrder = 2
favoritesContainer.Visible = true
local favLayout = Instance.new("UIListLayout", favoritesContainer)
favLayout.Padding = UDim.new(0, 5)
favLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	favoritesContainer.CanvasSize = UDim2.new(0, 0, 0, favLayout.AbsoluteContentSize.Y)
end)
local favsLabel = Instance.new("TextLabel", searchPage)
favsLabel.Text = "⭐ Favorites"
favsLabel.Size = UDim2.new(1, 0, 0, 20)
favsLabel.Font = Config.Font
favsLabel.TextColor3 = Config.TextColor
favsLabel.TextXAlignment = Enum.TextXAlignment.Left
favsLabel.BackgroundTransparency = 1
favsLabel.LayoutOrder = 1
favsLabel.Name = "FavoritesLabel"
local resultsFrame = Instance.new("ScrollingFrame", searchPage)
resultsFrame.Size = UDim2.new(1, 0, 0.7, -70)
resultsFrame.Position = UDim2.new(0, 0, 0.3, 70)
resultsFrame.BackgroundTransparency = 1
resultsFrame.ScrollBarThickness = 6
resultsFrame.LayoutOrder = 3
resultsFrame.Visible = false
local resultsLayout = Instance.new("UIListLayout", resultsFrame)
resultsLayout.Padding = UDim.new(0, 5)
resultsLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	resultsFrame.CanvasSize = UDim2.new(0, 0, 0, resultsLayout.AbsoluteContentSize.Y)
end)
local editorText
local createResultItem
local function refreshFavoritesDisplay()
	for _, child in ipairs(favoritesContainer:GetChildren()) do
		if child:IsA("Frame") then child:Destroy() end
	end
	if #FavoritesDB == 0 then
		local noFavsLabel = favoritesContainer:FindFirstChild("NoFavorites")
		if not noFavsLabel then
			noFavsLabel = Instance.new("TextLabel", favoritesContainer)
			noFavsLabel.Name = "NoFavorites"
			noFavsLabel.Text = "You have no favorited scripts."
			noFavsLabel.Size = UDim2.new(1, 0, 0, 30)
			noFavsLabel.BackgroundTransparency = 1
			noFavsLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
		end
	else
		local noFavsLabel = favoritesContainer:FindFirstChild("NoFavorites")
		if noFavsLabel then noFavsLabel:Destroy() end
		for _, scriptData in ipairs(FavoritesDB) do
			createResultItem(favoritesContainer, scriptData.name, scriptData.url)
		end
	end
end
function createResultItem(parent, text, url)
	local frame = Instance.new("Frame", parent)
	frame.Size = UDim2.new(1, 0, 0, 80)
	frame.BackgroundTransparency = 1
	local titleLabel = Instance.new("TextLabel", frame)
	titleLabel.Size = UDim2.new(1, -50, 0, 20)
	titleLabel.TextXAlignment = Enum.TextXAlignment.Left
	titleLabel.Position = UDim2.new(0, 5, 0, 0)
	titleLabel.Text = text
	titleLabel.Font = Config.Font
	titleLabel.TextSize = 16
	titleLabel.TextColor3 = Config.TextColor
	titleLabel.BackgroundTransparency = 1
	local favoriteBtn = Instance.new("TextButton", frame)
	favoriteBtn.Size = UDim2.new(0, 30, 0, 30)
	favoriteBtn.Position = UDim2.new(1, -35, 0, -5)
	favoriteBtn.Text = "★"
	favoriteBtn.Font = Enum.Font.SourceSansBold
	favoriteBtn.TextSize = 30
	favoriteBtn.BackgroundTransparency = 1
	local function isFavorited()
		for _, fav in ipairs(FavoritesDB) do if fav.url == url then return true end end
		return false
	end
	favoriteBtn.TextColor3 = isFavorited() and Color3.fromRGB(255, 255, 0) or Color3.fromRGB(100, 100, 100)
	favoriteBtn.MouseButton1Click:Connect(function()
		if isFavorited() then
			for i, fav in ipairs(FavoritesDB) do if fav.url == url then table.remove(FavoritesDB, i) break end end
			favoriteBtn.TextColor3 = Color3.fromRGB(100, 100, 100)
		else
			table.insert(FavoritesDB, {name = text, url = url})
			favoriteBtn.TextColor3 = Color3.fromRGB(255, 255, 0)
		end
		refreshFavoritesDisplay()
	end)
	local buttonsFrame = Instance.new("Frame", frame)
	buttonsFrame.Size = UDim2.new(1, -10, 0, 40)
	buttonsFrame.Position = UDim2.new(0, 5, 0, 30)
	buttonsFrame.BackgroundTransparency = 1
	local buttonsLayout = Instance.new("UIListLayout", buttonsFrame)
	buttonsLayout.FillDirection = Enum.FillDirection.Horizontal
	buttonsLayout.Padding = UDim.new(0, 5)
	local copyBtn = createButton(buttonsFrame, "Copy", UDim2.new(0.25, 0, 1, 0), UDim2.new())
	copyBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 150)
	copyBtn.MouseButton1Click:Connect(function()
		if setclipboard then
			setclipboard(url)
			local originalText = copyBtn.Text; copyBtn.Text = "Copied!"; task.wait(1.5); copyBtn.Text = originalText
		else
			warn("setclipboard is not available in this environment.")
			local originalText = copyBtn.Text; copyBtn.Text = "Failed!"; task.wait(1.5); copyBtn.Text = originalText
		end
	end)
	local addToMainBtn = createButton(buttonsFrame, "Add to Main", UDim2.new(0.35, 0, 1, 0), UDim2.new())
	addToMainBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
	addToMainBtn.MouseButton1Click:Connect(function()
		addScriptToMainTab(text, url)
		local originalText = addToMainBtn.Text; addToMainBtn.Text = "Added!"; task.wait(1.5); addToMainBtn.Text = originalText
	end)
	local openEditorBtn = createButton(buttonsFrame, "Open in Editor", UDim2.new(0.4, 0, 1, 0), UDim2.new())
	openEditorBtn.BackgroundColor3 = Color3.fromRGB(200, 100, 0)
	openEditorBtn.MouseButton1Click:Connect(function()
		if editorText then editorText.Text = url end
		switchTab("Editor")
	end)
	return frame
end
local function searchScripts(query)
	for _, v in ipairs(resultsFrame:GetChildren()) do if v:IsA("Frame") or v:IsA("TextLabel") then v:Destroy() end end
	favoritesContainer.Visible = false
	favsLabel.Visible = false
	resultsFrame.Visible = true
	local loadingLabel = Instance.new("TextLabel", resultsFrame)
	loadingLabel.Text = "Searching..."
	loadingLabel.Size = UDim2.new(1, 0, 0, 40)
	loadingLabel.BackgroundTransparency = 1
	loadingLabel.TextColor3 = Config.TextColor
	task.spawn(function()
		local success, response = pcall(function() return HttpService:JSONDecode(game:HttpGet("https://scriptblox.com/api/script/search?q=" .. HttpService:UrlEncode(query) .. "&mode=free", true)) end)
		if not loadingLabel.Parent then return end
		loadingLabel:Destroy()
		if not success or not response or not response.result or #response.result.scripts == 0 then
			local noResults = Instance.new("TextLabel", resultsFrame)
			noResults.Text = "No results found or API error."
			noResults.Size = UDim2.new(1, 0, 0, 40)
			noResults.BackgroundTransparency = 1
			noResults.TextColor3 = Config.TextColor
			return
		end
		for _, scriptData in ipairs(response.result.scripts) do
			createResultItem(resultsFrame, scriptData.title:gsub("&#39;", "'"), scriptData.script)
		end
	end)
end
searchBar.FocusLost:Connect(function(enterPressed)
	if enterPressed then
		if searchBar.Text ~= "" then
			searchScripts(searchBar.Text)
		else
			favoritesContainer.Visible = true
			favsLabel.Visible = true
			resultsFrame.Visible = false
			for _, v in ipairs(resultsFrame:GetChildren()) do if v:IsA("Frame") or v:IsA("TextLabel") then v:Destroy() end end
		end
	end
end)
refreshFavoritesDisplay()
local editorPage = createTab("Editor", 3)
local editorLayout = Instance.new("UIListLayout", editorPage)
editorLayout.Padding = UDim.new(0, 5)
editorLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
editorText = Instance.new("TextBox", editorPage)
editorText.Size = UDim2.new(1, 0, 1, -70)
editorText.MultiLine = true
editorText.TextWrapped = true
editorText.TextXAlignment = Enum.TextXAlignment.Left
editorText.TextYAlignment = Enum.TextYAlignment.Top
editorText.PlaceholderText = "Paste script URL or code here..."
editorText.Font = Enum.Font.Code
editorText.TextSize = 14
editorText.TextColor3 = Config.TextColor
editorText.BackgroundColor3 = Config.SecondaryColor
Instance.new("UICorner", editorText).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", editorText).Color = Config.BorderColor
local editorButtonsFrame = Instance.new("Frame", editorPage)
editorButtonsFrame.Size = UDim2.new(1, 0, 0, 60)
editorButtonsFrame.BackgroundTransparency = 1
local editorButtonsLayout = Instance.new("UIListLayout", editorButtonsFrame)
editorButtonsLayout.Padding = UDim.new(0, 5)
editorButtonsLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
local executeBtn = createButton(editorButtonsFrame, "Execute", UDim2.new(1, 0, 0, 25), UDim2.new())
executeBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
executeBtn.MouseButton1Click:Connect(function()
    SoundManager.ScriptButtonSFX:Play()
    local codeOrUrl = editorText.Text
    local originalText = executeBtn.Text
    if codeOrUrl == "" then
        executeBtn.Text = "No Code/URL!"; task.wait(2); executeBtn.Text = originalText; return
    end
    task.spawn(function()
        local scriptCode = codeOrUrl
        if codeOrUrl:match("^https?://") then
            executeBtn.Text = "Fetching..."
            local success, fetchedCode = pcall(function() return game:HttpGet(codeOrUrl, true) end)
            if not success or not fetchedCode then
                executeBtn.Text = "HTTP Error!"; task.wait(2); executeBtn.Text = originalText; return
            end
            scriptCode = fetchedCode
        end
        executeScript(executeBtn, scriptCode, originalText)
    end)
end)
local deleteBtn = createButton(editorButtonsFrame, "Delete", UDim2.new(1, 0, 0, 25), UDim2.new())
deleteBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
deleteBtn.MouseButton1Click:Connect(function()
    editorText.Text = ""
    local originalText = deleteBtn.Text; deleteBtn.Text = "Cleared!"; task.wait(1); deleteBtn.Text = originalText
end)
local funPage = createTab("Fun", 4)
local funLayout = Instance.new("UIListLayout", funPage)
funLayout.Padding = UDim.new(0, 10)
funLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
local tralalaSound = Instance.new("Sound", Main)
tralalaSound.SoundId = "rbxassetid://105044304109159"
tralalaSound.Volume = 1
local tralala = createButton(funPage, "Tralala Mode", UDim2.new(0, 200, 0, 40), UDim2.new())
local spinning = false
tralala.MouseButton1Click:Connect(function()
	if spinning then return end
	spinning = true
	tralalaSound:Play()
	local spinAnim = TweenService:Create(Main, TweenInfo.new(2, Enum.EasingStyle.Linear), { Rotation = 360 * 4 })
	spinAnim:Play()
	spinAnim.Completed:Connect(function()
		Main.Rotation = 0
		spinning = false
	end)
end)
local soundEncore = Instance.new("Sound", Main)
soundEncore.SoundId = "rbxassetid://122461990558098" 
soundEncore.Volume = 1
local encore = createButton(funPage, "Tralala Encore", UDim2.new(0, 200, 0, 40), UDim2.new())
local disco = Instance.new("Frame", ScreenGui)
disco.Size = UDim2.new(2, 0, 2, 0)
disco.Position = UDim2.new(-0.5, 0, -0.5, 0)
disco.BackgroundTransparency = 0.5
disco.ZIndex = -1
disco.Visible = false
local ultimateSound = Instance.new("Sound", Main)
ultimateSound.SoundId = "rbxassetid://85475858993795"
ultimateSound.Volume = 1
local ultimateDisco = Instance.new("Frame", ScreenGui)
ultimateDisco.Size = UDim2.new(2, 0, 2, 0)
ultimateDisco.Position = UDim2.new(-0.5, 0, -0.5, 0)
ultimateDisco.BackgroundTransparency = 0.2
ultimateDisco.ZIndex = -1
ultimateDisco.Visible = false
local ultimateBtn = createButton(funPage, "TRALALA ULTIMATE 🌈", UDim2.new(0, 200, 0, 40), UDim2.new())
task.spawn(function()
	while ultimateBtn.Parent do
		ultimateBtn.BackgroundColor3 = Color3.fromHSV(math.random(), 1, 1)
		task.wait(0.02)
	end
end)
local isUltimateActive = false

--// ADDED: Stop button and effect management variables
local activeSpinAnim = nil
local stopButtonFrame

--// ADDED: Central function to stop all effects
local function stopAllEffects()
	if activeSpinAnim then
		activeSpinAnim:Cancel()
		activeSpinAnim = nil
	end
	if soundEncore.IsPlaying then soundEncore:Stop() end
	if ultimateSound.IsPlaying then ultimateSound:Stop() end
	
	Main.Rotation = 0
	disco.Visible = false
	ultimateDisco.Visible = false
	TweenService:Create(Main, TweenInfo.new(0.3), {BackgroundColor3 = Config.BackgroundColor}):Play()

	isUltimateActive = false

	if stopButtonFrame then stopButtonFrame.Visible = false end
end

--// ADDED: Create the stop button UI
stopButtonFrame = Instance.new("Frame", ScreenGui)
stopButtonFrame.Name = "StopButtonFrame"
stopButtonFrame.Size = UDim2.new(0, 80, 0, 80)
stopButtonFrame.Position = UDim2.new(0.5, 0, 1, -20)
stopButtonFrame.AnchorPoint = Vector2.new(0.5, 1)
stopButtonFrame.BackgroundTransparency = 1
stopButtonFrame.Visible = false
local stopButton = Instance.new("TextButton", stopButtonFrame)
stopButton.Size = UDim2.new(1, 0, 0, 60)
stopButton.Text = "❌"
stopButton.Font = Enum.Font.SourceSans
stopButton.TextSize = 50
stopButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopButton.BackgroundColor3 = Color3.fromRGB(200, 40, 40)
Instance.new("UICorner", stopButton).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", stopButton).Color = Color3.fromRGB(255, 255, 255)
local stopLabel = Instance.new("TextLabel", stopButtonFrame)
stopLabel.Size = UDim2.new(1, 0, 0, 20)
stopLabel.Position = UDim2.new(0, 0, 0, 60)
stopLabel.Text = "STOP"
stopLabel.Font = Config.TitleFont
stopLabel.TextSize = 18
stopLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
stopLabel.BackgroundTransparency = 1
stopButton.MouseButton1Click:Connect(stopAllEffects)

local function startEncoreEffects()
	if soundEncore.IsPlaying or isUltimateActive then return end
	stopAllEffects() -- Stop any previous effects just in case
	soundEncore:Play()
	disco.Visible = true
	stopButtonFrame.Visible = true

	task.spawn(function()
		while soundEncore.IsPlaying and disco.Parent do
			disco.BackgroundColor3 = Color3.fromHSV(math.random(), 1, 1)
			task.wait(0.05)
		end
		disco.Visible = false
	end)
	activeSpinAnim = TweenService:Create(Main, TweenInfo.new(20, Enum.EasingStyle.Linear), { Rotation = 360 * 40 })
	activeSpinAnim:Play()
	task.delay(20, function() if soundEncore.IsPlaying then stopAllEffects() end end)
	activeSpinAnim.Completed:Connect(function() stopAllEffects() end)
end
encore.MouseButton1Click:Connect(function()
	if isWarningActive or isUltimateActive or soundEncore.IsPlaying then return end
	showSeizureWarning(startEncoreEffects)
end)

local function startUltimateEffects()
	if isUltimateActive then return end
	stopAllEffects() -- Stop any previous effects
	isUltimateActive = true
	ultimateSound:Play()
	ultimateDisco.Visible = true
	stopButtonFrame.Visible = true

	task.spawn(function()
		while ultimateSound.IsPlaying and ultimateDisco.Parent do
			ultimateDisco.BackgroundColor3 = Color3.fromHSV(math.random(), 1, 1)
			task.wait(0.005)
		end
		ultimateDisco.Visible = false
	end)
	task.spawn(function()
		while ultimateSound.IsPlaying and Main.Parent do
			Main.BackgroundColor3 = Color3.fromHSV(math.random(), 1, 1)
			task.wait()
		end
		TweenService:Create(Main, TweenInfo.new(1), {BackgroundColor3 = Config.BackgroundColor}):Play()
	end)
	activeSpinAnim = TweenService:Create(Main, TweenInfo.new(36, Enum.EasingStyle.Linear), { Rotation = 360 * 90 })
	activeSpinAnim:Play()
	task.delay(36, function() if ultimateSound.IsPlaying then stopAllEffects() end end)
	activeSpinAnim.Completed:Connect(function() stopAllEffects() end)
end
ultimateBtn.MouseButton1Click:Connect(function()
	if isWarningActive or isUltimateActive or soundEncore.IsPlaying then return end
	showSeizureWarning(startUltimateEffects)
end)
local settingsPage = createTab("Settings", 5)
local settingsScroll = Instance.new("ScrollingFrame", settingsPage)
settingsScroll.Size = UDim2.new(1, 0, 1, 0)
settingsScroll.BackgroundTransparency = 1
settingsScroll.ScrollBarThickness = 6
settingsScroll.BorderSizePixel = 0
local settingsLayout = Instance.new("UIListLayout", settingsScroll)
settingsLayout.Padding = UDim.new(0, 5)
settingsLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
settingsLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	settingsScroll.CanvasSize = UDim2.new(0, 0, 0, settingsLayout.AbsoluteContentSize.Y)
end)
local function updateAllElementColors()
	mainScroll.ScrollBarImageColor3 = currentTheme.color
	resultsFrame.ScrollBarImageColor3 = currentTheme.color
	settingsScroll.ScrollBarImageColor3 = currentTheme.color
	favoritesContainer.ScrollBarImageColor3 = currentTheme.color
	local containers = {mainScroll, resultsFrame, favoritesContainer, funPage, settingsScroll, Pages["Theme Editor"], editorPage, searchPage}
	for _, container in ipairs(containers) do
		for _, child in ipairs(container:GetDescendants()) do
			if child:IsA("TextButton") then
				local s = child:FindFirstChildOfClass("UIStroke")
				if s and s.Parent.Name ~= "CloseButton" and s.Parent.Name ~= "MinMaxButton" then 
					TweenService:Create(s, TweenInfo.new(0.3), {Color = currentTheme.color}):Play() 
				end
			end
		end
	end
	for name, tabButton in pairs(Tabs) do
		if Pages[name].Visible then
			TweenService:Create(tabButton, TweenInfo.new(0.3), { BackgroundColor3 = currentTheme.color }):Play()
		end
	end
end
local ultraActive = false 
local musicToggleButton = Instance.new("TextButton", settingsScroll)
musicToggleButton.Size = UDim2.new(1, -20, 0, 40)
musicToggleButton.Text = "🎵 Music: ON"
musicToggleButton.Font = Config.Font
musicToggleButton.TextSize = 16
musicToggleButton.TextColor3 = Config.TextColor
musicToggleButton.BackgroundColor3 = Color3.fromRGB(60, 120, 60)
Instance.new("UICorner", musicToggleButton).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", musicToggleButton).Color = Color3.fromRGB(25, 25, 25)
musicToggleButton.LayoutOrder = 1
local isMusicOn = true
musicToggleButton.MouseButton1Click:Connect(function()
	isMusicOn = not isMusicOn
	if isMusicOn then
		musicToggleButton.Text = "🎵 Music: ON"
		if not ultraActive then TweenService:Create(SoundManager.BackgroundMusic, TweenInfo.new(0.3), {Volume = 0.5}):Play() end
		TweenService:Create(musicToggleButton, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(60, 120, 60)}):Play()
	else
		musicToggleButton.Text = "🎵 Music: OFF"
		TweenService:Create(SoundManager.BackgroundMusic, TweenInfo.new(0.3), {Volume = 0}):Play()
		TweenService:Create(SoundManager.Ultra_Music, TweenInfo.new(0.3), {Volume = 0}):Play()
		TweenService:Create(musicToggleButton, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(120, 60, 60)}):Play()
	end
end)
local separator = Instance.new("Frame", settingsScroll)
separator.Size = UDim2.new(1, -20, 0, 2)
separator.BackgroundColor3 = Config.BorderColor
separator.BorderSizePixel = 0
separator.LayoutOrder = 2
Instance.new("TextLabel", settingsScroll).Text = "Themes"
local binaryOverlay = nil
local ultraSoundCoroutine = nil
local function disableUltraTheme()
	if ultraSoundCoroutine then coroutine.close(ultraSoundCoroutine); ultraSoundCoroutine = nil end
	if not ultraActive then return end
	ultraActive = false
	SoundManager.Ultra_Music:Stop()
	SoundManager.Ultra_SFX1:Stop()
	SoundManager.Ultra_SFX2:Stop()
	if binaryOverlay then binaryOverlay:Destroy(); binaryOverlay = nil end
	SoundManager.BackgroundMusic.PlaybackSpeed = 1.0
	if isMusicOn then
		if not SoundManager.BackgroundMusic.IsPlaying then SoundManager.BackgroundMusic:Play() end
		TweenService:Create(SoundManager.BackgroundMusic, TweenInfo.new(0.3), {Volume = 0.5}):Play()
	end
	TweenService:Create(Main, TweenInfo.new(0.3), {BackgroundColor3 = Config.BackgroundColor}):Play()
	gradient.Enabled = true
end
local function enableUltraTheme()
	if ultraActive then return end
	ultraActive = true
	ultraSoundCoroutine = coroutine.create(function()
		SoundManager.BackgroundMusic:Stop()
		SoundManager.Ultra_SFX1:Play()
		SoundManager.Ultra_SFX1.Ended:Wait()
		SoundManager.Ultra_SFX2:Play()
		SoundManager.Ultra_SFX2.Ended:Wait()
		if ultraActive then SoundManager.Ultra_Music:Play() end
	end)
	coroutine.resume(ultraSoundCoroutine)
	gradient.Enabled = false
	TweenService:Create(Main, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(0, 50, 0)}):Play()
	if binaryOverlay then binaryOverlay:Destroy() end
	binaryOverlay = Instance.new("Frame", Main)
	binaryOverlay.Size = UDim2.new(1, 0, 1, 0)
	binaryOverlay.BackgroundTransparency = 1
	binaryOverlay.ZIndex = 2
	binaryOverlay.ClipsDescendants = true
	task.spawn(function()
		while ultraActive and binaryOverlay.Parent do
			local rainLine = Instance.new("TextLabel", binaryOverlay)
			rainLine.Size = UDim2.new(0, 15, 1, 0)
			rainLine.Position = UDim2.new(math.random(), 0, -1, 0)
			rainLine.BackgroundTransparency = 1
			rainLine.TextColor3 = Color3.fromRGB(0, 255, 70)
			rainLine.Font = Enum.Font.Code
			rainLine.TextSize = 14
			rainLine.TextWrapped = true
			local binStr = ""
			for i = 1, 60 do binStr = binStr .. math.random(0, 1) .. "\n" end
			rainLine.Text = binStr
			local anim = TweenService:Create(rainLine, TweenInfo.new(math.random(4, 7), Enum.EasingStyle.Linear), {Position = UDim2.new(rainLine.Position.X.Scale, 0, 1, 0), BackgroundTransparency = 1 })
			anim:Play()
			anim.Completed:Connect(function() rainLine:Destroy() end)
			task.wait(0.08)
		end
	end)
end
local function showUltraWarning(onComplete)
	local warningGui = Instance.new("ScreenGui")
	warningGui.Name = "UltraWarningGui"
	warningGui.Parent = PlayerGui
	warningGui.ZIndexBehavior = Enum.ZIndexBehavior.Global
	warningGui.DisplayOrder = 999
	local warningFrame = Instance.new("Frame")
	warningFrame.Size = UDim2.new(1, 0, 1, 0)
	warningFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	warningFrame.BackgroundTransparency = 0.5
	warningFrame.Parent = warningGui
	local function showMessage(text)
		local messageLabel = Instance.new("TextLabel")
		messageLabel.Text = text
		messageLabel.Size = UDim2.new(1, -20, 1, 0)
		messageLabel.Position = UDim2.new(0, 10, 0, 0)
		messageLabel.BackgroundTransparency = 1
		messageLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
		messageLabel.Font = Enum.Font.GothamBlack
		messageLabel.TextSize = 20
		messageLabel.TextTransparency = 1
		messageLabel.Parent = warningFrame
		TweenService:Create(messageLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad), {TextTransparency = 0}):Play()
		task.wait(1.5)
		local tween = TweenService:Create(messageLabel, TweenInfo.new(0.5, Enum.EasingStyle.Quad), {TextTransparency = 1})
		tween:Play()
		tween.Completed:Wait()
		messageLabel:Destroy()
	end
	task.spawn(function()
		showMessage("⚠️WARNING⚠️")
		task.wait(0.5)
		showMessage("DO NOT TOGGLE MUSIC ON/OFF BEFORE OR WHILE USING THIS")
		task.wait(0.5)
		showMessage("OR THE ULTRA MUSIC WILL BREAK")
		warningGui:Destroy()
		onComplete()
	end)
end
local themes = {
	{name = "🟢 Hacker Green 🟢", color = Color3.fromRGB(0, 170, 0), lightningColor = Color3.fromRGB(0, 255, 0)}, {name = "🔵 Sky Blue 🔵", color = Color3.fromRGB(0, 180, 255), lightningColor = Color3.fromRGB(0, 255, 255)}, {name = "💕 Lollipop Pink 💕", color = Color3.fromRGB(255, 105, 180), lightningColor = Color3.fromRGB(255, 150, 200)}, {name = "🟠 Fall Orange 🟠", color = Color3.fromRGB(255, 140, 0), lightningColor = Color3.fromRGB(255, 190, 50)}, {name = "🟣 Amethyst Purple 🟣", color = Color3.fromRGB(138, 43, 226), lightningColor = Color3.fromRGB(178, 83, 255)}, {name = "🟨 Noob :D 🟨", color = Color3.fromRGB(255, 255, 0), lightningColor = Color3.fromRGB(0, 100, 255)}, {name = "☣️ Tubers93 ULTRA ☣️", color = Color3.fromRGB(0, 200, 0), lightningColor = Color3.fromRGB(0, 255, 0), isUltra = true},
}
local function createThemeButton(themeData)
	local btn = Instance.new("TextButton", settingsScroll)
	btn.Size = UDim2.new(1, -20, 0, 40)
	btn.Text = themeData.name
	btn.Font = Config.Font
	btn.TextSize = 16
	btn.TextColor3 = Config.TextColor
	btn.BackgroundColor3 = themeData.color
	Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
	Instance.new("UIStroke", btn).Color = Color3.fromRGB(25, 25, 25)
	btn.LayoutOrder = 3
	btn.MouseButton1Click:Connect(function()
		if themeData.isUltra then
			if not ultraActive then
				showUltraWarning(function()
					enableUltraTheme()
					currentTheme.color = themeData.color
					currentTheme.lightningColor = themeData.lightningColor
					updateAllElementColors()
				end)
			end
		else
			disableUltraTheme()
			currentTheme.color = themeData.color
			currentTheme.lightningColor = themeData.lightningColor
			updateAllElementColors()
		end
	end)
	return btn
end
for _, themeData in ipairs(themes) do createThemeButton(themeData) end
local themeEditorPage = createTab("Theme Editor", 6)
local teLayout = Instance.new("UIListLayout", themeEditorPage)
teLayout.Padding = UDim.new(0, 5)
local function createColorInput(parent, labelText, defaultColor)
	local frame = Instance.new("Frame", parent)
	frame.Size = UDim2.new(1, 0, 0, 30)
	frame.BackgroundTransparency = 1
	local layout = Instance.new("UIListLayout", frame)
	layout.FillDirection = Enum.FillDirection.Horizontal
	layout.VerticalAlignment = Enum.VerticalAlignment.Center
	layout.Padding = UDim.new(0, 5)
	local label = Instance.new("TextLabel", frame)
	label.Size = UDim2.new(0.4, 0, 1, 0)
	label.Text = labelText
	label.Font = Config.Font
	label.TextColor3 = Config.TextColor
	label.BackgroundTransparency = 1
	label.TextXAlignment = Enum.TextXAlignment.Left
	local inputs = {}
	for i, colorComp in ipairs({"R", "G", "B"}) do
		local box = Instance.new("TextBox", frame)
		box.Size = UDim2.new(0.15, 0, 1, 0)
		box.Font = Config.Font
		box.PlaceholderText = colorComp
		box.Text = math.floor(defaultColor[colorComp] * 255)
		box.BackgroundColor3 = Config.SecondaryColor
		box.TextColor3 = Config.TextColor
		Instance.new("UICorner", box).CornerRadius = UDim.new(0, 4)
		table.insert(inputs, box)
	end
	return inputs
end
local mainColorInputs = createColorInput(themeEditorPage, "Main Color:", currentTheme.color)
local lightningColorInputs = createColorInput(themeEditorPage, "Lightning FX:", currentTheme.lightningColor)
local applyBtn = createButton(themeEditorPage, "Apply Custom Theme", UDim2.new(1, 0, 0, 40), UDim2.new())
applyBtn.MouseButton1Click:Connect(function()
	local r1, g1, b1 = tonumber(mainColorInputs[1].Text), tonumber(mainColorInputs[2].Text), tonumber(mainColorInputs[3].Text)
	local r2, g2, b2 = tonumber(lightningColorInputs[1].Text), tonumber(lightningColorInputs[2].Text), tonumber(lightningColorInputs[3].Text)
	if r1 and g1 and b1 and r2 and g2 and b2 then
		disableUltraTheme()
		currentTheme.color = Color3.fromRGB(r1, g1, b1)
		currentTheme.lightningColor = Color3.fromRGB(r2, g2, b2)
		updateAllElementColors()
	end
end)
local themeNameInput = Instance.new("TextBox", themeEditorPage)
themeNameInput.Size = UDim2.new(1, 0, 0, 40)
themeNameInput.PlaceholderText = "Enter new theme name..."
themeNameInput.Font = Config.Font
themeNameInput.BackgroundColor3 = Config.SecondaryColor
themeNameInput.TextColor3 = Config.TextColor
Instance.new("UICorner", themeNameInput).CornerRadius = UDim.new(0, 6)
local saveBtn = createButton(themeEditorPage, "Save Theme", UDim2.new(1, 0, 0, 40), UDim2.new())
saveBtn.MouseButton1Click:Connect(function()
	local themeName = themeNameInput.Text
	if themeName ~= "" then
		local newTheme = {
			name = "🎨 " .. themeName .. " 🎨",
			color = Color3.fromRGB(tonumber(mainColorInputs[1].Text) or 255, tonumber(mainColorInputs[2].Text) or 255, tonumber(mainColorInputs[3].Text) or 255),
			lightningColor = Color3.fromRGB(tonumber(lightningColorInputs[1].Text) or 255, tonumber(lightningColorInputs[2].Text) or 255, tonumber(lightningColorInputs[3].Text) or 255)
		}
		table.insert(themes, newTheme)
		createThemeButton(newTheme)
		local originalText = saveBtn.Text; saveBtn.Text = "Saved!"; task.wait(1.5); saveBtn.Text = originalText; themeNameInput.Text = ""
	end
end)
--================================================================================--
--// FINAL ANIMATIONS & LOOPS
--================================================================================--
local function sineRGB(time) return Color3.fromHSV((time / 5) % 1, 0.8, 1) end
task.spawn(function()
	local startTime = tick()
	while Main.Parent do
		local rgbColor = sineRGB(tick() - startTime)
		if Title.Parent then Title.TextColor3 = rgbColor end
		if minButton.Parent then
			minButton.TextColor3 = rgbColor
			minStroke.Color = rgbColor
		end
		task.wait()
	end
end)
task.spawn(function()
	while lightningFX.Parent do
		local light = Instance.new("Frame", lightningFX)
		light.BackgroundTransparency = 0.5
		light.BackgroundColor3 = currentTheme.lightningColor
		light.Size = UDim2.new(0, 1, 1, 0)
		light.Position = UDim2.new(math.random(), 0, -1, 0)
		local tween = TweenService:Create(light, TweenInfo.new(0.5, Enum.EasingStyle.Linear), { Position = UDim2.new(light.Position.X.Scale, 0, 1, 0), BackgroundTransparency = 1 })
		tween:Play()
		tween.Completed:Connect(function() light:Destroy() end)
		task.wait(math.random() / 3)
	end
end)
switchTab("Main")
updateAllElementColors()
