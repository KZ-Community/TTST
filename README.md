  
    repeat wait() until game.Players
    repeat wait() until game.Players.LocalPlayer
    repeat wait() until game.ReplicatedStorage
    repeat wait() until game.ReplicatedStorage:FindFirstChild("Remotes");
    repeat wait() until game:GetService("ReplicatedStorage").Effect.Container
    repeat wait() until game.Players.LocalPlayer:FindFirstChild("PlayerGui");
    repeat wait() until game.Players.LocalPlayer.PlayerGui:FindFirstChild("Main");
    repeat wait() until game:GetService("Players")
    repeat wait() until game:GetService("Players").LocalPlayer.Character:FindFirstChild("Energy")
    
        wait(1)
    
    if not game:IsLoaded() then repeat game.Loaded:Wait() until game:IsLoaded() end
    
    if game:GetService("Players").LocalPlayer.PlayerGui.Main:FindFirstChild("ChooseTeam") then
        repeat wait()
            if game:GetService("Players").LocalPlayer.PlayerGui:WaitForChild("Main").ChooseTeam.Visible == true then
                if _G.Team == "Pirate" then
                    for i, v in pairs(getconnections(game:GetService("Players").LocalPlayer.PlayerGui.Main.ChooseTeam.Container.Pirates.Frame.ViewportFrame.TextButton.Activated)) do                                                                                                
                        v.Function()
                    end
                elseif _G.Team == "Marine" then
                    for i, v in pairs(getconnections(game:GetService("Players").LocalPlayer.PlayerGui.Main.ChooseTeam.Container.Marines.Frame.ViewportFrame.TextButton.Activated)) do                                                                                                
                        v.Function()
                    end
                else
                    for i, v in pairs(getconnections(game:GetService("Players").LocalPlayer.PlayerGui.Main.ChooseTeam.Container.Pirates.Frame.ViewportFrame.TextButton.Activated)) do                                                                                                
                        v.Function()
                    end
                end
            end
        until game.Players.LocalPlayer.Team ~= nil and game:IsLoaded()
    end
	local DisplayButton = game:GetService("Players").LocalPlayer.PlayerGui.Main.Settings.DisplayButton
	local SettingsButton = game:GetService("Players").LocalPlayer.PlayerGui.Main.Settings
	
	DisplayButton.TextLabel.Text = "Namfon Hub"
	DisplayButton.Notify.Text = "Enable or Disable UI script"
	DisplayButton.Visible = false
	Toggle = true
	
	SettingsButton.MouseButton1Click:Connect(function()
		if Toggle then
			Toggle = false
			DisplayButton.Visible = true
		else
			Toggle = true
			DisplayButton.Visible = false
		end
	end)
	
	if DisplayButton.Visible == true then
		DisplayButton.MouseButton1Click:Connect(function()
			game:GetService("VirtualInputManager"):SendKeyEvent(true,305,false,game)
			game:GetService("VirtualInputManager"):SendKeyEvent(false,305,false,game)
		end)
	end

	local library = {
		Version = "Beta",
		WorkspaceName = "Namfon Hub",
		flags = {},
		signals = {},
		objects = {},
		elements = {},
		globals = {},
		subs = {},
		colored = {},
		configuration = {
			hideKeybind = Enum.KeyCode.RightControl,
			smoothDragging = false,
			easingStyle = Enum.EasingStyle.Quart,
			easingDirection = Enum.EasingDirection.Out
		},
		colors = {
			main = Color3.fromRGB(3, 223, 252),
			background = Color3.fromRGB(40, 40, 40),
			outerBorder = Color3.fromRGB(15, 15, 15),
			innerBorder = Color3.fromRGB(73, 63, 73),
			topGradient = Color3.fromRGB(35, 35, 35),
			bottomGradient = Color3.fromRGB(29, 29, 29),
			sectionBackground = Color3.fromRGB(35, 34, 34),
			section = Color3.fromRGB(176, 175, 176),
			otherElementText = Color3.fromRGB(255, 255, 250),
			elementText = Color3.fromRGB(255, 255, 250),
			elementBorder = Color3.fromRGB(20, 20, 20),
			selectedOption = Color3.fromRGB(55, 55, 55),
			unselectedOption = Color3.fromRGB(40, 40, 40),
			hoveredOptionTop = Color3.fromRGB(65, 65, 65),
			unhoveredOptionTop = Color3.fromRGB(50, 50, 50),
			hoveredOptionBottom = Color3.fromRGB(45, 45, 45),
			unhoveredOptionBottom = Color3.fromRGB(35, 35, 35),
			tabText = Color3.fromRGB(185, 185, 185)
		},
		gui_parent = (function()
			local x, c = pcall(function()
				return game:GetService("CoreGui")
			end)
			if x and c then
				return c
			end
			x, c = pcall(function()
				return (game:IsLoaded() or (game.Loaded:Wait() or 1)) and game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
			end)
			if x and c then
				return c
			end
			x, c = pcall(function()
				return game:GetService("StarterGui")
			end)
			if x and c then
				return c
			end
			return error("Seriously bad engine. Can't find a place to store the GUI. Robust code can't help this much incompetence.")
		end)(),
		colorpicker = false,
		colorpickerconflicts = {},
		rainbowflags = {},
		rainbows = 0,
		rainbowsg = 0
	}
	library.Subs = library.subs
	local library_flags = library.flags
	library.Flags = library_flags
	local destroyrainbows, destroyrainbowsg = nil
	function darkenColor(clr, intensity)
		if not intensity or (intensity == 1) then
			return clr
		end
		if clr and ((typeof(clr) == "Color3") or (type(clr) == "table")) then
			return Color3.new(clr.R / intensity, clr.G / intensity, clr.B / intensity)
		end
	end
	library.subs.darkenColor = darkenColor
	local __runscript = true
	local function wait_check(...)
		if __runscript then
			return wait(...)
		else
			wait()
			return false
		end
	end
	library.subs.Wait, library.subs.wait = wait_check, wait_check
	function library.IsGuiValid()
		return __runscript
	end
	local lasthidebing = 0
	local temp = game:FindService("MarketplaceService") or game:GetService("MarketplaceService")
	local Marketplace = (temp and (cloneref and cloneref(temp))) or temp
	local resolvevararg, temp = nil
	do
		local lwr = string.lower
		function library.defaultSort(a, b)
			return lwr(tostring(b)) > lwr(tostring(a))
		end
	end
	do
		local varargresolve = {
			Window = {"Name", "Theme"},
			Tab = {"Name", "Image"},
			Section = {"Name", "Side"},
			Label = {"Text", "Flag", "UnloadValue", "UnloadFunc"},
			Toggle = {"Name", "Value", "Callback", "Flag", "Location", "LocationFlag", "UnloadValue", "UnloadFunc", "Locked", "Keybind", "Condition", "AllowDuplicateCalls"},
			Textbox = {"Name", "Value", "Callback", "Flag", "Location", "LocationFlag", "UnloadValue", "UnloadFunc", "Placeholder", "Type", "Min", "Max", "Decimals", "Hex", "Binary", "Base", "RichTextBox", "MultiLine", "TextScaled", "TextFont", "PreFormat", "PostFormat", "CustomProperties", "AllowDuplicateCalls"},
			Slider = {"Name", "Value", "Callback", "Flag", "Location", "LocationFlag", "UnloadValue", "UnloadFunc", "Min", "Max", "Decimals", "Format", "IllegalInput", "Textbox", "AllowDuplicateCalls"},
			Button = {"Name", "Callback", "Locked", "Condition"},
			Keybind = {"Name", "Value", "Callback", "Flag", "Location", "LocationFlag", "UnloadValue", "UnloadFunc", "Pressed", "KeyNames", "AllowDuplicateCalls"},
			Dropdown = {"Name", "Value", "Callback", "Flag", "Location", "LocationFlag", "UnloadValue", "UnloadFunc", "List", "Filter", "Method", "Nothing", "Sort", "MultiSelect", "ItemAdded", "ItemRemoved", "ItemChanged", "ItemsCleared", "ScrollUpButton", "ScrollDownButton", "ScrollButtonRate", "DisablePrecisionScrolling", "AllowDuplicateCalls"},
			SearchBox = {"Name", "Value", "Callback", "Flag", "Location", "LocationFlag", "UnloadValue", "UnloadFunc", "List", "Filter", "Method", "Nothing", "Sort", "MultiSelect", "ItemAdded", "ItemRemoved", "ItemChanged", "ItemsCleared", "ScrollUpButton", "ScrollDownButton", "ScrollButtonRate", "DisablePrecisionScrolling", "RegEx", "AllowDuplicateCalls"},
			Colorpicker = {"Name", "Value", "Callback", "Flag", "Location", "LocationFlag", "UnloadValue", "UnloadFunc", "Rainbow", "Random", "AllowDuplicateCalls"},
			Persistence = {"Name", "Value", "Callback", "Flag", "Location", "LocationFlag", "UnloadValue", "UnloadFunc", "Workspace", "Persistive", "Suffix", "LoadCallback", "SaveCallback", "PostLoadCallback", "PostSaveCallback", "ScrollUpButton", "ScrollDownButton", "ScrollButtonRate", "DisablePrecisionScrolling", "AllowDuplicateCalls"},
			Designer = {"Backdrop", "Image", "Info", "Credit"}
		}
		function resolvevararg(objtype, ...)
			local data = varargresolve[objtype]
			local t = {}
			if data then
				for index, value in next, {...} do
					t[data[index]] = value
				end
			end
			return t
		end
	
	local resolvercache = {}
	library.resolvercache = resolvercache
	local function resolveid(image, flag)
		if image then
			if type(image) == "string" then
				if (#image > 14 and string.sub(image, 1, 13) == "rbxassetid://") or (#image > 12 and string.sub(image, 1, 11) == "rbxasset://") or (#image > 12 and string.sub(image, 1, 11) ~= "rbxthumb://") then
					if flag then
						local thing = library.elements[flag] or library.designerelements[flag]
						if thing and thing.Set then
							task.spawn(thing.Set, thing, image)
					end
					return image
				end
			end
			local orig = image
			if resolvercache[orig] then
				if flag then
					local thing = library.elements[flag] or library.designerelements[flag]
					if thing and thing.Set then
						task.spawn(thing.Set, thing, resolvercache[orig])
					end
				end
				return resolvercache[orig]
			end
			image = tonumber(image) or image
			local succezz = pcall(function()
				local typ = type(image)
				if typ == "string" then
					if getsynasset then
						if #image > 11 and (string.sub(image, 1, 11) == "synasset://") then
							return getsynasset(string.sub(image, 12))
						elseif (#image > 14) and (string.sub(image, 1, 14) == "synasseturl://") then
							local x, e = pcall(function()
								local codename, fixes = string.gsub(image, ".", function(c)
									if c:lower() == c:upper() and not tonumber(c) then
										return ""
									end
								end)
								codename = string.sub(codename, 1, 24) .. tostring(fixes)
								local fold = isfolder("./Pepsi Lib")
								if fold then
								else
									makefolder("./Pepsi Lib")
								end
								fold = isfolder("./Pepsi Lib/Themes")
								if fold then
								else
									makefolder("./Pepsi Lib/Themes")
								end
								fold = isfolder("./Pepsi Lib/Themes/SynapseAssetsCache")
								if fold then
								else
									makefolder("./Pepsi Lib Themes/SynapseAssetsCache")
								end
								if not fold or not isfile("./Pepsi Lib/Themes/SynapseAssetsCache/" .. codename .. ".dat") then
									local res = game:HttpGet(string.sub(image, 15))
									if res ~= nil then
										writefile("./Pepsi Lib/Themes/SynapseAssetsCache/" .. codename .. ".dat", res)
									end
								end
								return getsynasset(readfile("./Pepsi Lib/Themes/SynapseAssetsCache/" .. codename .. ".dat"))
							end)
							if x and e ~= nil then
								return e
							end
						end
					end
					if (#image < 11) or ((string.sub(image, 1, 13) ~= "rbxassetid://") and (string.sub(image, 1, 11) ~= "rbxasset://") and string.sub(image, 1, 11) ~= "rbxthumb://") then
						image = tonumber(image:gsub("%D", ""), 10) or image
						typ = type(image)
					end
				end
				if (typ == "number") and (image > 0) then
					pcall(function()
						local nfo = Marketplace and Marketplace:GetProductInfo(image)
						image = tostring(image)
						if nfo and nfo.AssetTypeId == 1 then
							image = "rbxassetid://" .. image
						elseif nfo.AssetTypeId == 13 then
							local decal = game:GetObjects("rbxassetid://" .. image)[1]
							image = "rbxassetid://" .. ((decal and decal.Texture) or "0"):match("%d+$")
							decal = (decal and decal:Destroy() and nil) or nil
						end
					end)
				else
					image = nil
				end
			end)
			if succezz and image then
				if orig then
					resolvercache[orig] = image
				end
				resolvercache[image] = image
				if flag then
					local thing = library.elements[flag] or library.designerelements[flag]
					if thing and thing.Set then
						task.spawn(thing.Set, thing, image)
					end
				end
			end
		end
		return image
	end
	library.subs.ResolveID = resolveid
	library.resolvercache = resolvercache
	local colored, colors = library.colored, library.colors
	local tweenService = game:GetService("TweenService")
	local updatecolors, MainScreenGui = nil
	do
		local MayGC = 0
		spawn(function()
			local IsDescendantOf = game.IsDescendantOf
			local RemoveTable = table.remove
			while wait_check() do
				wait(10)
				local Breathe = 30
				for DataIndex = #colored, 1, -1 do
					if MayGC > 0 then
						break
					end
					Breathe -= 1
					if Breathe <= 0 then
						Breathe = 30
						if wait_check() then
							if MayGC > 0 then
								break
							end
						else
							return
						end
					end
					if MayGC > 0 then
						break
					end
					local data = colored[DataIndex]
					data = data and data[1]
					if data and (typeof(data) == "Instance") and IsDescendantOf(data, MainScreenGui) then
					elseif MayGC <= 0 then
						RemoveTable(colored, DataIndex)
					else
						break
					end
				end
				local sigs = library.signals
				local len = sigs and #sigs
				if len then
					local Dyn = math.round(len / 10)
					Dyn = ((Dyn < 1) and 1) or Dyn
					for DataIndex = len, 1, -1 do
						Breathe -= 1
						if Breathe <= 0 then
							Breathe = Dyn
							if wait_check() then
							else
								return
							end
						end
						local data = colored[DataIndex]
						if data and (typeof(data) == "RBXScriptConnection") and data.Connected then
						else
							RemoveTable(colored, DataIndex)
						end
					end
				end
			end
		end)
		local function colortwee(data, tweenit)
			local cclr = colors[data[3]]
			local darkness = data[4]
			tweenService:Create(data[1], TweenInfo.new(tweenit, library.configuration.easingStyle, library.configuration.easingDirection), {
				[data[2]] = (darkness and darkness ~= 1 and darkenColor(cclr, darkness)) or cclr
			}):Play()
		end
		local function colordarktwee(data)
			local cclr = colors[data[3]]
			local darkness = data[4]
			data[1][data[2]] = (darkness and darkness ~= 1 and darkenColor(cclr, darkness)) or cclr
		end
		function updatecolors(tweenit)
			MayGC += 1
			if library.objects and (#library.objects > 0 or next(library.objects)) then
				for _, data in next, colored do
					local x, e
					if tweenit then
						x, e = pcall(colortwee, data, tweenit)
					end
					if not x then
						local x, e = pcall(colordarktwee, data)
						if not x and e then
							warn(debug.traceback(e))
						end
					end
				end
				pcall(function()
					if library.Backdrop then
						library.Backdrop.Visible = library_flags["__Designer.Background.UseBackgroundImage"] and true
						library.Backdrop.Image = resolveid(library_flags["__Designer.Background.ImageAssetID"], "__Designer.Background.ImageAssetID") or ""
						library.Backdrop.ImageColor3 = library_flags["__Designer.Background.ImageColor"] or Color3.new(1, 1, 1)
						library.Backdrop.ImageTransparency = (library_flags["__Designer.Background.ImageTransparency"] or 95) / 100
					end
				end)
			end
			MayGC -= 1
		end
	end
	local function updatecolorsnotween()
		updatecolors()
	end
	library.subs.updatecolors = updatecolors
	library.colors = setmetatable({}, {
		__index = colors,
		__newindex = function(_, k, v)
			if colors[k] ~= v then
				colors[k] = v
				spawn(updatecolorsnotween)
			end
		end
	})
	local elements = library.elements
	shared.libraries = shared.libraries or {}
	local colorpickerconflicts = library.colorpickerconflicts
	local keyHandler = {
		notAllowedKeys = {
			[Enum.KeyCode.Return] = true,
			[Enum.KeyCode.Space] = true,
			[Enum.KeyCode.Tab] = true,
			[Enum.KeyCode.Unknown] = true,
			[Enum.KeyCode.Backspace] = true
		},
		notAllowedMouseInputs = {
			[Enum.UserInputType.MouseMovement] = true,
			[Enum.UserInputType.MouseWheel] = true,
			[Enum.UserInputType.MouseButton1] = true,
			[Enum.UserInputType.MouseButton2] = true,
			[Enum.UserInputType.MouseButton3] = true
		},
		allowedKeys = {
			[Enum.KeyCode.LeftShift] = "LShift",
			[Enum.KeyCode.RightShift] = "RShift",
			[Enum.KeyCode.LeftControl] = "LCtrl",
			[Enum.KeyCode.RightControl] = "RCtrl",
			[Enum.KeyCode.LeftAlt] = "LAlt",
			[Enum.KeyCode.RightAlt] = "RAlt",
			[Enum.KeyCode.CapsLock] = "CAPS",
			[Enum.KeyCode.One] = "1",
			[Enum.KeyCode.Two] = "2",
			[Enum.KeyCode.Three] = "3",
			[Enum.KeyCode.Four] = "4",
			[Enum.KeyCode.Five] = "5",
			[Enum.KeyCode.Six] = "6",
			[Enum.KeyCode.Seven] = "7",
			[Enum.KeyCode.Eight] = "8",
			[Enum.KeyCode.Nine] = "9",
			[Enum.KeyCode.Zero] = "0",
			[Enum.KeyCode.KeypadOne] = "Num-1",
			[Enum.KeyCode.KeypadTwo] = "Num-2",
			[Enum.KeyCode.KeypadThree] = "Num-3",
			[Enum.KeyCode.KeypadFour] = "Num-4",
			[Enum.KeyCode.KeypadFive] = "Num-5",
			[Enum.KeyCode.KeypadSix] = "Num-6",
			[Enum.KeyCode.KeypadSeven] = "Num-7",
			[Enum.KeyCode.KeypadEight] = "Num-8",
			[Enum.KeyCode.KeypadNine] = "Num-9",
			[Enum.KeyCode.KeypadZero] = "Num-0",
			[Enum.KeyCode.Minus] = "-",
			[Enum.KeyCode.Equals] = "=",
			[Enum.KeyCode.Tilde] = "~",
			[Enum.KeyCode.LeftBracket] = "[",
			[Enum.KeyCode.RightBracket] = "]",
			[Enum.KeyCode.RightParenthesis] = ")",
			[Enum.KeyCode.LeftParenthesis] = "(",
			[Enum.KeyCode.Semicolon] = ";",
			[Enum.KeyCode.Quote] = "'",
			[Enum.KeyCode.BackSlash] = "\\",
			[Enum.KeyCode.Comma] = ",",
			[Enum.KeyCode.Period] = ".",
			[Enum.KeyCode.Slash] = "/",
			[Enum.KeyCode.Asterisk] = "*",
			[Enum.KeyCode.Plus] = "+",
			[Enum.KeyCode.Period] = ".",
			[Enum.KeyCode.Backquote] = "`"
		}
	}
	local SeverAllConnections = nil
	function SeverAllConnections(t, cache)
		cache = cache or {}
		for k, v in next, t do
			t[k] = nil
			if v ~= nil then
				if cache[v] then
					continue
				end
				local te = v and typeof(v)
				if te then
					if te == "RBXScriptConnection" then
						v:Disconnect()
					elseif te == "Instance" then
						v:Destroy()
					elseif te == "table" then
						cache[v] = true
						SeverAllConnections(v, cache)
					end
				end
			end
		end
	end
	local function hardunload(library)
		if library.UnloadCallback and (type(library.UnloadCallback) == "function") then
			local x, e = pcall(library.UnloadCallback)
			if not x and e then
				task.spawn(error, e, 2)
			end
		end
		for cflag, data in next, elements do
			if data.Type ~= "Persistence" then
				if data.Set and data.Options.UnloadValue ~= nil then
					data.Set(data.Options.UnloadValue)
				end
				if data.Options.UnloadFunc then
					local y, u = pcall(data.Options.UnloadFunc)
					if not y and u then
						warn(debug.traceback("Error unloading '" .. tostring(cflag) .. "'\n" .. u))
					end
				end
			end
		end
		local hardcache = {}
		SeverAllConnections(library.signals, hardcache)
		SeverAllConnections(library.objects, hardcache)
		hardcache = (table.clear(hardcache) and nil) or nil
		library.signals = nil
		library.objects = nil
	end
	library.Subs.UnloadArg = hardunload
	local function unloadall()
		if shared.libraries then
			local b = 50
			while #shared.libraries > 0 do
				b = b - 1
				if b < 0 then
					b = 50
					wait(warn("Looped 50 times while unloading....?"))
				end
				local v = shared.libraries[1]
				if v and v.unload and (type(v.unload) == "function") then
					if not pcall(v.unload) then
						pcall(hardunload, v)
						for k in next, v do
							v[k] = nil
						end
					end
					if shared.libraries then
						pcall(function()
							table.remove(shared.libraries, 1)
						end)
					else
						return pcall(hardunload, library)
					end
				end
			end
		end
		shared.libraries = nil
	end
	shared.unloadall = unloadall
	library.unloadall = unloadall
	shared.libraries[1 + #shared.libraries] = library
	function library.unload()
		__runscript = nil
		hardunload(library)
		if shared.libraries then
			for k, v in next, shared.libraries or {} do
				if v == library then
					for k in next, table.remove(shared.libraries or {}, k) do
						v[k] = nil
					end
					break
				end
			end
			if shared.libraries and (#shared.libraries == 0) then
				shared.libraries = nil
			end
		end
		warn("Unloaded")
	end
	library.Unload = library.unload
	local Instance_new = (syn and syn.protect_gui and function(...)
		local x = {Instance.new(...)}
		if x[1] then
			library.objects[1 + #library.objects] = x[1]
			pcall(syn.protect_gui, x[1])
		end
		return unpack(x)
	end) or function(...)
		local x = {Instance.new(...)}
		if x[1] then
			library.objects[1 + #library.objects] = x[1]
		end
		return unpack(x)
	end
	library.subs.Instance_new = Instance_new
	local playersservice = game:GetService("Players")
	local function getresolver(listt, filter, method, _)
		local huo, args = type(filter), {}
		local hou = typeof(listt)
		return ((hou == "function") and function(...)
			return listt(...)
		end) or ((hou == "table") and function()
			return listt
		end) or function()
			local hardtype = nil
			local g = listt
			for _ = 1, 5 do
				hardtype = typeof(g)
				if hardtype == "function" then
					local x, e = pcall(listt)
					if x and e then
						g = e
					end
					hardtype = typeof(g)
				end
				if hardtype == "Instance" then
					local lastg = g
					if method == nil and listt == playersservice then
						g = listt:GetPlayers()
					end
					if method then
						local metype = type(method)
						if metype == "table" then
							method = method.Method or method[1]
							args = method.Args or method.Arguments or unpack(method, (method.Method ~= nil and 1) or 2)
							metype = type(method)
						end
						local y, u = nil, nil
						if metype == "function" then
							y, u = pcall(method, listt, unpack(args))
						elseif metype == "string" then
							local y, u = pcall(function()
								return listt[method](listt, unpack(args))
							end)
						else
							warn("Idk how to handle method type of", metype, debug.traceback(""))
						end
						if u then
							if y then
								g = u
							else
								warn("Error trying method", method, "on", listt, debug.traceback(u))
							end
						end
					end
					if g == lastg then
						g = listt:GetChildren()
					end
				end
				if hardtype == "Enum" then
					g = listt:GetEnumItems()
				end
				hardtype = typeof(g)
				if hardtype == "table" then
					break
				end
			end
			hardtype = typeof(g)
			if hardtype ~= "table" then
				warn("Could not resolve " .. hou .. " type to a list.")
				return {}
			end
			if filter then
				if huo == "function" then
					local accept = {}
					for _, v in next, g do
						local x, e = pcall(filter, v)
						if x and e then
							accept[1 + #accept] = (e == true and v) or e
						end
					end
					g = accept
				elseif huo == "string" then
					local accept = {}
					for _, v in next, g do
						if tostring(v):lower():find(huo) then
							accept[1 + #accept] = v
						end
					end
					g = accept
				elseif huo == "table" then
					local accept = {}
					if type(filter[1]) == "string" then
						for _, v in next, g do
							if tostring(v):lower():find(huo) then
								accept[1 + #accept] = v
							elseif filter[0] then
								accept[1 + #accept] = v
							end
						end
					else
						for _, v in next, g do
							if not table.find(filter, v) and not table.find(filter, tostring(v)) then
								accept[1 + #accept] = v
							elseif not filter[0] then
								accept[1 + #accept] = v
							end
						end
					end
					g = accept
				end
			end
			return g
		end
	end
	library.subs.GetResolver = getresolver
	local function resetall()
		destroyrainbowsg = true
		pcall(function()
			for k, v in next, elements do
				if v and k and v.Set and (v.Default ~= nil) and (library_flags[k] ~= v.Default) and (string.sub(k, 1, 11) ~= "__Designer.") then
					v:Set(v.Default)
				end
			end
		end)
	end
	library.ResetAll = resetall
	local textService = game:GetService("TextService")
	local userInputService = game:GetService("UserInputService")
	local runService = game:GetService("RunService")
	local LP = playersservice.LocalPlayer
	library.LP = LP
	library.Players = playersservice
	library.UserInputService = userInputService
	library.RunService = runService
	local mouse = LP and LP:GetMouse()
	if not mouse and PluginManager and runService:IsStudio() then
		shared.library_plugin = shared.library_plugin or print("Creating Studio Test-Plugin...") or PluginManager():CreatePlugin()
		mouse = shared.library_plugin:GetMouse()
		library.plugin = shared.library_plugin
	end
	library.Mouse = mouse
	local textToSize = nil
	do
		local textService = game:GetService("TextService")
		local bigv2 = Vector2.one * math.huge
		function textToSize(object)
			return textService:GetTextSize(object.Text, object.TextSize, object.Font, bigv2)
		end
	end
	library.subs.textToSize = textToSize
	local function removeSpaces(str)
		if str then
			local newStr = str:gsub(" ", "")
			return newStr
		end
	end
	library.subs.removeSpaces = removeSpaces
	local function Color3FromHex(hex)
		hex = hex:gsub("#", ""):upper():gsub("0X", "")
		return Color3.fromRGB(tonumber(hex:sub(1, 2), 16), tonumber(hex:sub(3, 4), 16), tonumber(hex:sub(5, 6), 16))
	end
	library.subs.Color3FromHex = Color3FromHex
	local floor = math.floor
	local function Color3ToHex(color)
		local r, g, b = string.format("%X", floor(color.R * 255)), string.format("%X", floor(color.G * 255)), string.format("%X", floor(color.B * 255))
		if #r < 2 then
			r = "0" .. r
		end
		if #g < 2 then
			g = "0" .. g
		end
		if #b < 2 then
			b = "0" .. b
		end
		return string.format("%s%s%s", r, g, b)
	end
	if Color3.ToHex and not shared.overridecolortohex then
		local x, e = pcall(Color3.ToHex, Color3.new())
		if x and type(e) == "string" and #e == 6 then
			Color3ToHex = Color3.ToHex
		end
	end
	library.subs.Color3ToHex = Color3ToHex
	local isDraggingSomething = false
	local function makeDraggable(topBarObject, object)
		local dragging = nil
		local dragInput = nil
		local dragStart = nil
		local startPosition = nil
		library.signals[1 + #library.signals] = topBarObject.InputBegan:Connect(function(input)
			if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
				dragging = true
				dragStart = input.Position
				startPosition = object.Position
				input.Changed:Connect(function()
					if input.UserInputState == Enum.UserInputState.End then
						dragging = false
					end
				end)
			end
		end)
		library.signals[1 + #library.signals] = topBarObject.InputChanged:Connect(function(input)
			if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
				dragInput = input
			end
		end)
		library.signals[1 + #library.signals] = userInputService.InputChanged:Connect(function(input)
			if input == dragInput and dragging then
				local delta = input.Position - dragStart
				if not isDraggingSomething and library.configuration.smoothDragging then
					tweenService:Create(object, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
						Position = UDim2.new(startPosition.X.Scale, startPosition.X.Offset + delta.X, startPosition.Y.Scale, startPosition.Y.Offset + delta.Y)
					}):Play()
				elseif not isDraggingSomething and not library.configuration.smoothDragging then
					object.Position = UDim2.new(startPosition.X.Scale, startPosition.X.Offset + delta.X, startPosition.Y.Scale, startPosition.Y.Offset + delta.Y)
				end
			end
		end)
	end
	library.subs.makeDraggable = makeDraggable
	local JSONEncode, JSONDecode = nil, nil
	do
		local temp_http = game:FindService("HttpService") or game:GetService("HttpService")
		local httpservice = temp_http
		if cloneref and (type(cloneref) == "function") then
			httpservice, temp_http = cloneref(httpservice), nil
		end
		library.Http = httpservice
		local JSONEncodeFunc = httpservice.JSONEncode
		function JSONEncode(...)
			return pcall(JSONEncodeFunc, httpservice, ...)
		end
		library.JSONEncode = JSONEncode
		local JSONDecodeFunc = httpservice.JSONDecode
		function JSONDecode(...)
			return pcall(JSONDecodeFunc, httpservice, ...)
		end
		library.JSONDecode = JSONDecode
	end
	local convertfilename
	do
		local string_gsub = string.gsub
		function convertfilename(str, default, replace)
			replace = replace or "_"
			local corrections = 0
			local predname = string_gsub(str, "%W", function(c)
				local byt = c:byte()
				if ((byt == 0) or (byt == 32) or (byt == 33) or (byt == 59) or (byt == 61) or ((byt >= 35) and (byt <= 41)) or ((byt >= 43) and (byt <= 57)) or ((byt >= 64) and (byt <= 123)) or ((byt >= 125) and (byt <= 127))) then
				else
					corrections = 1 + corrections
					return replace
				end
			end)
			return (default and corrections == #predname and tostring(default)) or predname
		end
		library.subs.ConvertFilename = convertfilename
	end
	do
		do
			local function NewOption(TextStr, Order, Parent)
				local Option = Instance_new("Frame")
				local BBorder = Instance_new("Frame")
				local Inner_2 = Instance_new("Frame")
				local Border_2 = Instance_new("Frame")
				local Text = Instance_new("TextLabel")
				local Button = Instance_new("TextButton")
				Option.AnchorPoint = Vector2.new(0, 0.5)
				Option.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {Option, "BackgroundColor3", "background"}
				Option.BorderColor3 = Color3.fromRGB(27, 27, 27)
				Option.LayoutOrder = Order or #Parent:GetChildren()
				Option.Name = "Option"
				Option.Position = UDim2.new(0, 5, 0.5, 0)
				Option.Size = UDim2.new(0, 35, 0, 25)
				BBorder.AnchorPoint = Vector2.new(0.5, 0.5)
				BBorder.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {BBorder, "BackgroundColor3", "background"}
				BBorder.BorderColor3 = Color3.fromRGB(50, 43, 50)
				BBorder.BorderMode = Enum.BorderMode.Inset
				BBorder.Name = "BBorder"
				BBorder.Parent = Option
				BBorder.Position = UDim2.new(0.5, 0, 0.5, 0)
				BBorder.Size = UDim2.new(1, 0, 1, 0)
				Inner_2.AnchorPoint = Vector2.new(0.5, 0.5)
				Inner_2.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {Inner_2, "BackgroundColor3", "background"}
				Inner_2.BorderColor3 = Color3.fromRGB(27, 27, 27)
				Inner_2.Name = "Inner"
				Inner_2.Parent = Option
				Inner_2.Position = UDim2.new(0.5, 0, 0.5, 0)
				Inner_2.Size = UDim2.new(1, -6, 1, -6)
				Border_2.AnchorPoint = Vector2.new(0.5, 0.5)
				Border_2.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {Border_2, "BackgroundColor3", "background"}
				Border_2.BorderColor3 = Color3.fromRGB(50, 43, 50)
				Border_2.BorderMode = Enum.BorderMode.Inset
				Border_2.Name = "Border"
				Border_2.Parent = Inner_2
				Border_2.Position = UDim2.new(0.5, 0, 0.5, 0)
				Border_2.Size = UDim2.new(1, 0, 1, 0)
				Text.AnchorPoint = Vector2.new(0.5, 0.5)
				Text.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
				Text.BackgroundTransparency = 1
				Text.Font = Enum.Font.Code
				Text.FontSize = Enum.FontSize.Size14
				Text.Name = "Text"
				Text.Parent = Border_2
				Text.Position = UDim2.new(0.5, 0, 0.5, 0)
				Text.Size = UDim2.new(1, 0, 1, 0)
				Text.TextColor3 = library.colors.elementText
				colored[1 + #colored] = {Text, "TextColor3", "elementText"}
				Text.TextSize = 14
				Text.TextStrokeTransparency = 0.75
				Button.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
				Button.BackgroundTransparency = 1
				Button.BorderSizePixel = 0
				Button.Font = Enum.Font.SourceSans
				Button.FontSize = Enum.FontSize.Size14
				Button.Name = "Button"
				Button.Parent = Option
				Button.Size = UDim2.new(1, 0, 1, 0)
				Button.Text = ""
				Button.TextColor3 = Color3.fromRGB(0, 0, 0)
				Button.TextSize = 14
				Button.TextTransparency = 1
				Text.Text = TextStr
				local siz = textToSize(Text)
				Option.Size = UDim2.new(0, math.max(siz.X, 28) + 12, 0, 25)
				Option.Parent = Parent
				return Option, Button, Text
			end
			local function AddOption(OptionData, Key, OptionCount, Parent, Close, PromptEvent, KeepOpen)
				local Enabled = OptionData.Enabled
				if OptionData.Disabled then
					Enabled = false
				else
					Enabled = Enabled or (Enabled == nil)
				end
				local OptionText = OptionData.Text or OptionData.String or OptionData.Message or OptionData.Value or OptionData.Name or Key
				local Callback = OptionData.Callback or OptionData.OnPressed or OptionData.Function or nil
				local Order = tonumber(OptionData.Slot or OptionData.Order or OptionData.LayoutOrder or OptionData.Index or OptionCount)
				local OptionIns, OptionButton, OptionTxt = NewOption(tostring(OptionText), Order, Parent)
				local OptionObj = {
					Text = OptionText,
					Callback = Callback,
					ButtonObject = OptionIns,
					Pressed = OptionButton.MouseButton1Click,
					PressedRight = OptionButton.MouseButton2Click,
					Activated = OptionButton.Activated,
					TextButton = OptionButton,
					Order = Order,
					Enabled = Enabled
				}
				function OptionObj.Remove()
					do
						local Btn = OptionObj.ButtonObject
						if Btn then
							Btn:Destroy()
						end
					end
					for k in next, OptionObj do
						rawset(OptionObj, k, nil)
					end
					return true
				end
				local Proxy = nil
				local function Clicked(f)
					return function(...)
						if f then
							task.spawn(f, ...)
						end
						PromptEvent:Fire(Key, OptionButton.Text, ...)
						if KeepOpen then
						else
							Close()
						end
					end
				end
				function OptionObj.Press(...)
					OptionObj.Update()
					Proxy = Proxy or Clicked(Callback)
					Proxy(...)
				end
				function OptionObj.Lock()
					OptionObj.Enabled = false
					OptionObj.Update()
				end
				function OptionObj.Unlock()
					OptionObj.Enabled = true
					OptionObj.Update()
				end
				function OptionObj.SetLocked(self, state)
					if type(self) == "boolean" then
						state = self
					end
					OptionObj.Enabled = state
					OptionObj.Update()
				end
				function OptionObj.SetCondition(self, Condition)
					if type(self) ~= "table" then
						Condition = self
					end
					OptionObj.Condition = Condition
					OptionObj.Update()
				end
				function OptionObj.Update()
					do
						local OptionText = OptionObj.Text or OptionData.Text or OptionData.String or OptionData.Message or OptionData.Value or OptionData.Name or OptionButton.Text or Key
						OptionButton.Text = tostring(OptionText)
					end
					OptionIns.LayoutOrder = tonumber(OptionObj.Order or OptionData.Slot or OptionData.Order or OptionData.LayoutOrder or OptionData.Index or OptionIns.LayoutOrder or OptionCount)
					do
						local Enabled = OptionData.Enabled
						local Cond = OptionObj.Condition
						if Cond then
							local x, e = pcall(Cond, OptionObj)
							if x then
								Enabled = e and true
							else
								warn(debug.traceback(string.format("Error in prompt-option %s's Condition function: %s", OptionButton.Text, e), 2))
							end
						else
							if OptionData.Disabled then
								Enabled = false
							else
								Enabled = (Enabled and true) or (Enabled == nil)
							end
						end
						local Proxy = nil
						do
							local nCallback = (Enabled and (OptionData.Callback or OptionData.OnPressed or OptionData.Function)) or nil
							if not Proxy or Callback ~= nCallback then
								Callback = nCallback
								Proxy = Clicked(Callback)
								OptionObj.PressedConnection = (OptionObj.PressedConnection and OptionObj.PressedConnection:Disconnect() and nil) or (Callback and OptionObj.Pressed:Connect(Proxy)) or nil
							end
							local PC = OptionObj.PressedConnection
							if Enabled then
								if PC then
									if Callback then
									else
										OptionObj.PressedConnection = (PC:Disconnect() and nil) or nil
									end
								elseif Callback then
									Proxy = Proxy or Clicked(Callback)
									OptionObj.PressedConnection = OptionObj.Pressed:Connect(Proxy)
								end
							elseif PC then
								OptionObj.PressedConnection = (PC:Disconnect() and nil) or nil
							end
						end
						OptionObj.Enabled = Enabled
						OptionTxt.TextTransparency = (Enabled and 0) or 0.5
					end
					return OptionObj
				end
				OptionObj.Update()
				return OptionObj
			end
			local function SortByLayoutOrder(a, b)
				return a.Order < b.Order
			end
			local DefaultSelections = {
				Ok = true
			}
			function library.Prompt(self, PromptData, ...)
				if rawequal(self, library) then
				else
					PromptData, self = self, library
				end
				local PromptEvent = Instance_new("BindableEvent")
				local PromptObj = {
					OnSelect = PromptEvent.Event,
					Active = true,
					SelectedEvent = PromptEvent
				}
				local ChoicePopup = Instance_new("Frame")
				local Buttons = Instance_new("ScrollingFrame")
				local Title = Instance_new("TextLabel")
				local Description = Instance_new("TextLabel")
				local DoClose = PromptData.CloseButton
				DoClose = (DoClose == nil) or (DoClose == true) or DoClose or nil
				local Close = DoClose and Instance_new("ImageButton")
				do
					local Border = Instance_new("Frame")
					local Inner = Instance_new("Frame")
					local InnerBorder = Instance_new("Frame")
					local Bar = Instance_new("Frame")
					local Splitter = Instance_new("Frame")
					local ButtonBar = Instance_new("Frame")
					local UIListLayout = Instance_new("UIListLayout")
					ChoicePopup.AnchorPoint = Vector2.new(0.5, 0.5)
					ChoicePopup.BackgroundColor3 = library.colors.background
					colored[1 + #colored] = {ChoicePopup, "BackgroundColor3", "background"}
					ChoicePopup.BorderColor3 = library.colors.outerBorder
					colored[1 + #colored] = {ChoicePopup, "BorderColor3", "outerBorder"}
					ChoicePopup.Name = "ChoicePopup"
					ChoicePopup.Position = UDim2.new(0.5, 0, 0.5, 0)
					ChoicePopup.Size = UDim2.new(0, 325, 0, 100)
					Border.AnchorPoint = Vector2.new(0.5, 0.5)
					Border.BackgroundColor3 = library.colors.background
					colored[1 + #colored] = {Border, "BackgroundColor3", "background"}
					Border.BorderColor3 = library.colors.innerBorder
					colored[1 + #colored] = {Border, "BorderColor3", "innerBorder"}
					Border.BorderMode = Enum.BorderMode.Inset
					Border.Name = "Border"
					Border.Parent = ChoicePopup
					Border.Position = UDim2.new(0.5, 0, 0.5, 0)
					Border.Size = UDim2.new(1, 0, 1, 0)
					Inner.AnchorPoint = Vector2.new(0.5, 0.5)
					Inner.BackgroundColor3 = library.colors.background
					colored[1 + #colored] = {Inner, "BackgroundColor3", "background"}
					Inner.BorderColor3 = library.colors.outerBorder
					colored[1 + #colored] = {Inner, "BorderColor3", "outerBorder"}
					Inner.Name = "Inner"
					Inner.Parent = ChoicePopup
					Inner.Position = UDim2.new(0.5, 0, 0.5, 0)
					Inner.Size = UDim2.new(1, -8, 1, -8)
					InnerBorder.AnchorPoint = Vector2.new(0.5, 0.5)
					InnerBorder.BackgroundColor3 = library.colors.background
					colored[1 + #colored] = {InnerBorder, "BackgroundColor3", "background"}
					InnerBorder.BorderColor3 = library.colors.innerBorder
					colored[1 + #colored] = {InnerBorder, "BorderColor3", "innerBorder"}
					InnerBorder.BorderMode = Enum.BorderMode.Inset
					InnerBorder.Name = "InnerBorder"
					InnerBorder.Parent = Inner
					InnerBorder.Position = UDim2.new(0.5, 0, 0.5, 0)
					InnerBorder.Size = UDim2.new(1, 0, 1, 0)
					Bar.BackgroundColor3 = library.colors.main
					colored[1 + #colored] = {Bar, "BackgroundColor3", "main"}
					Bar.BorderSizePixel = 0
					Bar.Name = "Bar"
					Bar.Parent = InnerBorder
					Bar.Size = UDim2.new(1, 0, 0, 3)
					Splitter.AnchorPoint = Vector2.new(0, 1)
					Splitter.BackgroundColor3 = Color3.fromRGB(38, 38, 38)
					Splitter.BorderSizePixel = 0
					Splitter.Name = "Splitter"
					Splitter.Parent = InnerBorder
					Splitter.Position = UDim2.new(0, 0, 1, -35)
					Splitter.Size = UDim2.new(1, 0, 0, 1)
					Title.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
					Title.BackgroundTransparency = 1
					Title.Font = Enum.Font.Code
					Title.FontSize = Enum.FontSize.Size18
					Title.Parent = InnerBorder
					Title.Position = UDim2.new(0, 6, 0, 4)
					Title.Size = UDim2.new(0, 300, 0, 25)
					Title.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {Title, "TextColor3", "elementText"}
					Title.TextSize = 15
					Title.TextStrokeTransparency = 0.95
					Title.TextXAlignment = Enum.TextXAlignment.Left
					Description.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
					Description.BackgroundTransparency = 1
					Description.Font = Enum.Font.Code
					Description.FontSize = Enum.FontSize.Size14
					Description.Name = "Description"
					Description.Parent = InnerBorder
					Description.Position = UDim2.new(0, 6, 0, 25)
					Description.Size = UDim2.new(0, 300, 0, 25)
					Description.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {Description, "TextColor3", "elementText"}
					Description.TextSize = 14
					Description.TextStrokeTransparency = 0.95
					Description.TextTruncate = Enum.TextTruncate.AtEnd
					Description.TextWrap = true
					Description.TextWrapped = true
					Description.TextXAlignment = Enum.TextXAlignment.Left
					if Close then
						Close.AnchorPoint = Vector2.new(1)
						Close.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
						Close.BackgroundTransparency = 1
						Close.Image = "http://www.roblox.com/asset/?id=9064270627"
						Close.ImageColor3 = library.colors.section
						colored[1 + #colored] = {Close, "ImageColor3", "section"}
						Close.Name = "Close"
						Close.Parent = InnerBorder
						Close.Position = UDim2.new(1, -9, 0, 11)
						Close.ScaleType = Enum.ScaleType.Fit
						Close.Size = UDim2.new(0, 10, 0, 10)
					end
					ButtonBar.AnchorPoint = Vector2.new(0, 1)
					ButtonBar.BackgroundColor3 = library.colors.sectionBackground
					colored[1 + #colored] = {ButtonBar, "BackgroundColor3", "sectionBackground"}
					ButtonBar.BorderSizePixel = 0
					ButtonBar.Name = "ButtonBar"
					ButtonBar.Parent = InnerBorder
					ButtonBar.Position = UDim2.new(0, 0, 1, 0)
					ButtonBar.Size = UDim2.new(1, 0, 0, 35)
					Buttons.AutomaticCanvasSize = Enum.AutomaticSize.X
					Buttons.BackgroundColor3 = library.colors.sectionBackground
					colored[1 + #colored] = {Buttons, "BackgroundColor3", "sectionBackground"}
					Buttons.BorderColor3 = library.colors.background
					colored[1 + #colored] = {Buttons, "BorderColor3", "background"}
					Buttons.BorderSizePixel = 0
					Buttons.BottomImage = ""
					Buttons.CanvasSize = UDim2.new(0, 0, 0, 0)
					Buttons.MidImage = ""
					Buttons.Name = "Buttons"
					Buttons.Parent = ButtonBar
					Buttons.Position = UDim2.new(0, 6, 0, 0)
					Buttons.ScrollBarImageColor3 = Color3.fromRGB(0, 0, 0)
					Buttons.ScrollBarImageTransparency = 1
					Buttons.ScrollBarThickness = 0
					Buttons.ScrollingDirection = Enum.ScrollingDirection.X
					Buttons.Selectable = false
					Buttons.Size = UDim2.new(1, -12, 1, 0)
					Buttons.TopImage = ""
					UIListLayout.FillDirection = Enum.FillDirection.Horizontal
					UIListLayout.Padding = UDim.new(0, 10)
					UIListLayout.Parent = Buttons
					UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
					UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Center
				end
				PromptObj.FrameInstance = ChoicePopup
				PromptObj.Closed = ChoicePopup.Destroying
				local function ClosePrompt(method)
					PromptObj.Active = (ChoicePopup and ChoicePopup:Destroy() and nil) or (PromptEvent:Fire("Close", method and (method == "timeout_")) and nil) or nil
				end
				PromptObj.Close = ClosePrompt
				if Close then
					Close.MouseButton1Click:Connect(((DoClose ~= true) and DoClose) or ClosePrompt)
				end
				do
					local NameTxt = PromptData.Name
					Title.Text = ((NameTxt ~= nil) and tostring(NameTxt)) or "Untitled Prompt"
				end
				do
					local DescriptionTxt = PromptData.Description
					if DescriptionTxt == nil then
						DescriptionTxt = PromptData.Text
						if DescriptionTxt == nil then
							DescriptionTxt = PromptData.String
							if DescriptionTxt == nil then
								DescriptionTxt = PromptData.Message
								if DescriptionTxt == nil then
									DescriptionTxt = PromptData.Value
								end
							end
						end
					end
					Description.Text = ((DescriptionTxt ~= nil) and tostring(DescriptionTxt)) or ""
				end
				do
					local Selections = PromptData.Options or PromptData.Buttons or PromptData.Choices
					if Selections then
						if type(Selections) ~= "table" then
							Selections = {
								OK = Selections
							}
						end
					else
						Selections = DefaultSelections
					end
					local OptionCount = 0
					local ButtonsProxy = {}
					PromptObj.Buttons = ButtonsProxy
					function PromptObj.ReOrder(self, SortFunc)
						if self ~= ButtonsProxy then
							SortFunc, self = self, nil
						end
						if SortFunc and (type(SortFunc) ~= "function") then
							SortFunc = SortByLayoutOrder
						end
						local BtnOrder = {}
						for _, Data in next, ButtonsProxy do
							BtnOrder[1 + #BtnOrder] = Data
						end
						table.sort(BtnOrder, SortFunc or SortByLayoutOrder)
						for Order, Data in next, BtnOrder do
							Data.Order = Order or Data.Order
							local UpdateFunc = Data.Update
							if UpdateFunc then
								spawn(UpdateFunc)
							else
								local Frame = Data.ButtonObject
								if Frame then
									Frame.LayoutOrder = Order
								end
							end
						end
						return ButtonsProxy
					end
					function PromptObj.PressAll(self, ...)
						local isSelf = nil
						if self and rawequal(self, PromptObj) then
							isSelf = true
						end
						for _, Data in next, ButtonsProxy do
							local f = Data.Callback
							if f then
								if isSelf then
									f(...)
								else
									f(self, ...)
								end
							end
						end
					end
					local KeepOpen = PromptData.KeepOpen
					for Key, OptionData in next, Selections do
						OptionCount += 1
						do
							local typ = OptionData and type(OptionData)
							if typ ~= "table" then
								OptionData = {
									Name = ((typ == "string") and (type(Key) == "number") and OptionData) or Key,
									Callback = ((typ == "function") and OptionData) or ClosePrompt
								}
							end
						end
						ButtonsProxy[Key] = AddOption(OptionData, Key, OptionCount, Buttons, ClosePrompt, PromptEvent, KeepOpen)
					end
				end
				do
					local to = PromptData.Timeout
					to = to and tonumber(to)
					if to then
						task.delay(to, ClosePrompt, "timeout_")
					end
				end
				ChoicePopup.Parent = MainScreenGui
				makeDraggable(ChoicePopup, ChoicePopup)
				return PromptObj, ChoicePopup
			end
		end
		do
			local Popups = Instance.new("Frame")
			local UIListLayout = Instance.new("UIListLayout")
			library.NotifyLayout = UIListLayout
			Popups.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
			Popups.BackgroundTransparency = 1
			Popups.Name = "Popups"
			Popups.Position = UDim2.new(0, 10, 0, 10)
			Popups.Size = UDim2.new(1, -20, 1, -20)
			UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Right
			UIListLayout.Padding = UDim.new(0, 5)
			UIListLayout.Parent = Popups
			UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
			UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Bottom
			Popups.Parent = MainScreenGui
			library.NotificationsFrame = Popups
			local Inverse = true
			local os_clock = os.clock
			local Notifications = {}
			library.Notifications = Notifications
			spawn(function()
				local v1, vtop, htop = Enum.FillDirection.Vertical, Enum.VerticalAlignment.Top, Enum.HorizontalAlignment.Center
				while wait_check() do
					local Len = #Notifications
					while wait_check() and (Len <= 0) do
						Len = #Notifications
						if wait_check(0.25) then
						else
							return
						end
					end
					Inverse = ((UIListLayout.FillDirection == v1) and (UIListLayout.VerticalAlignment ~= vtop)) or (UIListLayout.HorizontalAlignment ~= htop)
					local BreathingRoom, now = 10, os_clock()
					local Order = 0
					for Index = Len, 1, -1 do
						BreathingRoom -= 1
						if BreathingRoom <= 0 then
							if wait_check() then
								BreathingRoom, now = 10, os_clock()
							else
								return
							end
						end
						local Noti = Notifications[Index]
						local Obj = Noti and Noti.Object
						if Obj and Noti.Active and (Noti.Paused or ((now - Noti.Expires) < Noti.Duration)) then
							if Noti.TextLabel.Text ~= Noti.Text then
								Noti:SetText(Noti.Text)
							end
							if Obj.Visible then
								Order += 1
								Obj.LayoutOrder = Order * ((Inverse and -1) or 1)
							end
							continue
						end
						Noti.Object = ((Noti.Object and Noti.Object:Destroy()) and nil) or (Noti.Destroy() and nil) or (table.remove(Notifications, Index) and nil) or nil
					end
				end
			end)
			function library.Notify(self, NotificationData, ...)
				if rawequal(self, library) then
				else
					NotificationData, self = self, library
				end
				local now = os_clock()
				local dur = NotificationData.Time or 6
				local TextStr = NotificationData.Text or NotificationData.String or NotificationData.Value or NotificationData.Message or NotificationData.Msg
				TextStr = ((TextStr == nil) and "No text given") or tostring(TextStr)
				local NotificationObj = {
					InitTime = now,
					Active = true,
					Forced = false,
					Duration = dur,
					Expires = now + dur,
					Paused = (NotificationData.Paused and true) or false,
					Text = TextStr,
					Arguments = NotificationData
				}
				NotificationObj.Forced = NotificationObj.Paused
				local Notification = Instance.new("Frame")
				NotificationObj.Object = Notification
				local Border = Instance.new("Frame")
				local Inner = Instance.new("Frame")
				local Border_2 = Instance.new("Frame")
				local Text = Instance.new("TextLabel")
				NotificationObj.TextLabel = Text
				local Bar = Instance.new("Frame")
				local Close = Instance.new("ImageButton")
				Notification.AnchorPoint = Vector2.one
				Notification.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {Notification, "BackgroundColor3", "background"}
				Notification.BorderColor3 = library.colors.outerBorder
				colored[1 + #colored] = {Notification, "BorderColor3", "outerBorder"}
				Notification.Name = "Notification"
				Notification.Position = UDim2.new(1, -10, 1, -10)
				Notification.Size = UDim2.new(0, 5e4, 0, 32)
				Border.AnchorPoint = Vector2.new(0.5, 0.5)
				Border.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {Border, "BackgroundColor3", "background"}
				Border.BorderColor3 = library.colors.innerBorder
				colored[1 + #colored] = {Border, "BorderColor3", "innerBorder"}
				Border.BorderMode = Enum.BorderMode.Inset
				Border.Name = "Border"
				Border.Parent = Notification
				Border.Position = UDim2.new(0.5, 0, 0.5, 0)
				Border.Size = UDim2.new(1, 0, 1, 0)
				Inner.AnchorPoint = Vector2.one / 2
				Inner.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {Inner, "BackgroundColor3", "background"}
				Inner.BorderColor3 = library.colors.outerBorder
				colored[1 + #colored] = {Inner, "BorderColor3", "outerBorder"}
				Inner.Name = "Inner"
				Inner.Parent = Notification
				Inner.Position = UDim2.new(0.5, 0, 0.5, 0)
				Inner.Size = UDim2.new(1, -8, 1, -8)
				Border_2.AnchorPoint = Vector2.one / 2
				Border_2.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {Border_2, "BackgroundColor3", "background"}
				Border_2.BorderColor3 = library.colors.innerBorder
				colored[1 + #colored] = {Border_2, "BorderColor3", "innerBorder"}
				Border_2.BorderMode = Enum.BorderMode.Inset
				Border_2.Name = "Border"
				Border_2.Parent = Inner
				Border_2.Position = UDim2.new(0.5, 0, 0.5, 0)
				Border_2.Size = UDim2.new(1, 0, 1, 0)
				Text.AnchorPoint = Vector2.new(0, 0.5)
				Text.BackgroundTransparency = 1
				Text.Font = Enum.Font.Code
				Text.FontSize = Enum.FontSize.Size14
				Text.Name = "Text"
				Text.Parent = Border_2
				Text.Position = UDim2.new(0, 8, 0.5, 0)
				Text.Size = UDim2.new(1, -8, 1, -7)
				Text.Text = TextStr
				Text.TextColor3 = library.colors.elementText
				colored[1 + #colored] = {Text, "TextColor3", "elementText"}
				Text.TextSize = 14
				Text.TextScaled = true
				Text.TextStrokeTransparency = 0.75
				Text.TextWrap = true
				Text.TextWrapped = true
				Text.TextXAlignment = Enum.TextXAlignment.Left
				Bar.BackgroundColor3 = library.colors.main
				colored[1 + #colored] = {Bar, "BackgroundColor3", "main"}
				Bar.BorderSizePixel = 0
				Bar.Name = "Bar"
				Bar.Parent = Border_2
				Bar.Size = UDim2.new(0, 3, 1, 0)
				Close.AnchorPoint = Vector2.new(1, 0.5)
				Close.BackgroundTransparency = 1
				Close.Image = "rbxassetid://5492252477"
				Close.ImageColor3 = library.colors.elementText
				colored[1 + #colored] = {Close, "ImageColor3", "elementText"}
				Close.Name = "Close"
				Close.Parent = Border_2
				Close.Position = UDim2.new(1, -6, 0.5, 0)
				Close.ScaleType = Enum.ScaleType.Fit
				Close.Size = UDim2.new(0, 10, 0, 10)
				Notification.Size = UDim2.new(0, 64 + textToSize(Text).X, 0, 32)
				Notification.Parent = Popups
				Notification.LayoutOrder = #Notification.Parent:GetChildren() * ((Inverse and 1) or -1)
				if Popups.Parent then
				else
					Popups.Parent = MainScreenGui
				end
				NotificationObj.OnClose = Close.Activated
				NotificationObj.InputBegan = Notification.InputBegan
				NotificationObj.Destroying = Notification.Destroying
				NotificationObj.MouseEnter = Notification.MouseEnter
				NotificationObj.MouseLeave = Notification.MouseLeave
				function NotificationObj.SetText(self, Str)
					if rawequal(self, NotificationObj) then
					else
						Str = self
					end
					Str = ((Str == nil) and "No text given") or tostring(Str)
					Text.Text, NotificationObj.Text = Str, Str
					Notification.Size = UDim2.new(0, 44 + Text.TextBounds.X, 0, 32)
					return Str, Text
				end
				local function Pause(self, Set, NoForce)
					if rawequal(self, NotificationObj) then
					else
						Set, NoForce = self, Set
					end
					local IsPaused = NotificationObj.Paused
					if Set == nil then
						Set = not IsPaused
					else
						Set = Set or false
					end
					if Set or (IsPaused == Set) then
					else
						NotificationObj.Expires = math.max(NotificationObj.Expires, os_clock() + math.clamp(NotificationObj.Duration / 2.5, 1, 3))
					end
					NotificationObj.Paused = Set
					if NoForce then
					else
						NotificationObj.Forced = Set
					end
					return Set
				end
				NotificationObj.SetPaused = Pause
				function NotificationObj.AddTime(self, Extension)
					if rawequal(self, NotificationObj) then
					else
						Extension = self
					end
					NotificationObj.Expires += Extension
				end
				function NotificationObj.Hide(self, SetPause)
					if Notification and NotificationObj.Active then
						if rawequal(self, NotificationObj) then
						else
							SetPause = self
						end
						if SetPause then
							Pause(true)
						end
						Notification.Visible = false
					end
				end
				function NotificationObj.Show(self, SetPause)
					if Notification and NotificationObj.Active then
						if rawequal(self, NotificationObj) then
						else
							SetPause = self
						end
						if SetPause then
							Pause(false)
						end
						Notification.Visible = true
					end
				end
				function NotificationObj.SetVisible(self, Visible, SetPause)
					if Notification and NotificationObj.Active then
						if rawequal(self, NotificationObj) then
						else
							Visible, SetPause = self, Visible
						end
						if Visible == nil then
							Notification.Visible = not Notification.Visible
						else
							Notification.Visible = (Visible and true) or false
						end
						if SetPause then
							Pause(Notification.Visible)
						end
					end
				end
				Notification.MouseEnter:Connect(function()
					if NotificationObj.Forced then
						return
					end
					Pause(true, true)
				end)
				Notification.MouseLeave:Connect(function()
					if NotificationObj.Forced then
						return
					end
					Pause(false, true)
				end)
				local function Destroy()
					if Notification then
						Notification:Destroy()
					end
					NotificationObj.Active = false
					NotificationObj.Object = nil
				end
				NotificationObj.Destroy = Destroy
				Close.Activated:Connect(Destroy)
				Notifications[1 + #Notifications] = NotificationObj
				return NotificationObj, Notification, Text
			end
		end
	end
	function library:CreateWindow(options, ...)
		options = (options and type(options) == "string" and resolvevararg("Window", options, ...)) or options
		local homepage = nil
		local windowoptions = options
		local windowName = options.Name or "Unnamed Window"
		options.Name = windowName
		if windowName and #windowName > 0 and library.WorkspaceName == "Pepsi Lib" then
			library.WorkspaceName = convertfilename(windowName, "Pepsi Lib")
		end
		local pepsiLibrary = Instance_new("ScreenGui")
		library.MainScreenGui, MainScreenGui = pepsiLibrary, pepsiLibrary
		local main = Instance_new("Frame")
		local mainBorder = Instance_new("Frame")
		local tabSlider = Instance_new("Frame")
		local innerMain = Instance_new("Frame")
		local innerMainBorder = Instance_new("Frame")
		local innerBackdrop = Instance_new("ImageLabel")
		local innerMainHolder = Instance_new("Frame")
		local tabsHolder = Instance_new("ImageLabel")
		local tabHolderList = Instance_new("UIListLayout")
		local tabHolderPadding = Instance_new("UIPadding")
		local headline = Instance_new("TextLabel")
		local splitter = Instance_new("TextLabel")
		local submenuOpen = nil
		library.globals["__Window" .. options.Name] = {
			submenuOpen = submenuOpen
		}
		pepsiLibrary.Name = "     "
		pepsiLibrary.Parent = library.gui_parent
		pepsiLibrary.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
		pepsiLibrary.DisplayOrder = 10
		pepsiLibrary.ResetOnSpawn = false
		main.Name = "main"
		main.Parent = pepsiLibrary
		main.AnchorPoint = Vector2.new(0.5, 0.5)
		main.BackgroundColor3 = library.colors.background
		colored[1 + #colored] = {main, "BackgroundColor3", "background"}
		main.BorderColor3 = library.colors.outerBorder
		colored[1 + #colored] = {main, "BorderColor3", "outerBorder"}
		main.Position = UDim2.fromScale(0.5, 0.5)
		
		if _G.SizxPc == true then
			X = 550
			Y = 350
		else
			X = 550
			Y = 350
		end
		
		main.Size = UDim2.fromOffset(X, Y)
	
		makeDraggable(main, main)
		mainBorder.Name = "mainBorder"
		mainBorder.Parent = main
		mainBorder.AnchorPoint = Vector2.new(0.5, 0.5)
		mainBorder.BackgroundColor3 = library.colors.background
		colored[1 + #colored] = {mainBorder, "BackgroundColor3", "background"}
		mainBorder.BorderColor3 = library.colors.innerBorder
		colored[1 + #colored] = {mainBorder, "BorderColor3", "innerBorder"}
		mainBorder.BorderMode = Enum.BorderMode.Inset
		mainBorder.Position = UDim2.fromScale(0.5, 0.5)
		mainBorder.Size = UDim2.fromScale(1, 1)
		innerMain.Name = "innerMain"
		innerMain.Parent = main
		innerMain.AnchorPoint = Vector2.new(0.5, 0.5)
		innerMain.BackgroundColor3 = library.colors.background
		colored[1 + #colored] = {innerMain, "BackgroundColor3", "background"}
		innerMain.BorderColor3 = library.colors.outerBorder
		colored[1 + #colored] = {innerMain, "BorderColor3", "outerBorder"}
		innerMain.Position = UDim2.fromScale(0.5, 0.5)
		innerMain.Size = UDim2.new(1, -14, 1, -14)
		innerMainBorder.Name = "innerMainBorder"
		innerMainBorder.Parent = innerMain
		innerMainBorder.AnchorPoint = Vector2.new(0.5, 0.5)
		innerMainBorder.BackgroundColor3 = library.colors.background
		colored[1 + #colored] = {innerMainBorder, "BackgroundColor3", "background"}
		innerMainBorder.BorderColor3 = library.colors.innerBorder
		colored[1 + #colored] = {innerMainBorder, "BorderColor3", "innerBorder"}
		innerMainBorder.BorderMode = Enum.BorderMode.Inset
		innerMainBorder.Position = UDim2.fromScale(0.5, 0.5)
		innerMainBorder.Size = UDim2.fromScale(1, 1)
		innerMainHolder.Name = "innerMainHolder"
		innerMainHolder.Parent = innerMain
		innerMainHolder.BackgroundColor3 = Color3.new(1, 1, 1)
		innerMainHolder.BackgroundTransparency = 1
		innerMainHolder.Position = UDim2:fromOffset(25)
		innerMainHolder.Size = UDim2.new(1, 0, 1, -25)
		innerBackdrop.Name = "innerBackdrop"
		innerBackdrop.Parent = innerMainHolder
		innerBackdrop.BackgroundColor3 = Color3.new(1, 1, 1)
		innerBackdrop.BackgroundTransparency = 1
		innerBackdrop.Size = UDim2.fromScale(1, 1)
		innerBackdrop.ZIndex = -1
		innerBackdrop.Visible = library_flags["__Designer.Background.UseBackgroundImage"] and true
		innerBackdrop.ImageColor3 = library_flags["__Designer.Background.ImageColor"] or Color3.new(1, 1, 1)
		innerBackdrop.ImageTransparency = (library_flags["__Designer.Background.ImageTransparency"] or 95) / 100
		innerBackdrop.Image = resolveid(library_flags["__Designer.Background.ImageAssetID"], "__Designer.Background.ImageAssetID") or ""
		library.Backdrop = innerBackdrop
		tabsHolder.Name = "tabsHolder"
		tabsHolder.Parent = innerMain
		tabsHolder.BackgroundColor3 = library.colors.topGradient
		colored[1 + #colored] = {tabsHolder, "BackgroundColor3", "topGradient"}
		tabsHolder.BorderSizePixel = 0
		tabsHolder.Position = UDim2.fromOffset(1, 1)
		tabsHolder.Size = UDim2.new(1, -2, 0, 23)
		tabsHolder.Image = "rbxassetid://2454009026"
		tabsHolder.ImageColor3 = library.colors.bottomGradient
		colored[1 + #colored] = {tabsHolder, "ImageColor3", "bottomGradient"}
		tabHolderList.Name = "tabHolderList"
		tabHolderList.Parent = tabsHolder
		tabHolderList.FillDirection = Enum.FillDirection.Horizontal
		tabHolderList.SortOrder = Enum.SortOrder.LayoutOrder
		tabHolderList.VerticalAlignment = Enum.VerticalAlignment.Center
		tabHolderList.Padding = UDim:new(3)
		tabHolderPadding.Name = "tabHolderPadding"
		tabHolderPadding.Parent = tabsHolder
		tabHolderPadding.PaddingLeft = UDim:new(7)
		headline.Name = "headline"
		headline.Parent = tabsHolder
		headline.BackgroundColor3 = Color3.new(1, 1, 1)
		headline.BackgroundTransparency = 1
		headline.LayoutOrder = 1
		headline.Font = Enum.Font.Code
		headline.Text = (windowName and tostring(windowName)) or "???"
		headline.TextColor3 = library.colors.main
		colored[1 + #colored] = {headline, "TextColor3", "main"}
		headline.TextSize = 14
		headline.TextStrokeColor3 = library.colors.outerBorder
		colored[1 + #colored] = {headline, "TextStrokeColor3", "outerBorder"}
		headline.TextStrokeTransparency = 0.75
		headline.Size = UDim2:new(textToSize(headline).X + 4, 1)
		splitter.Name = "splitter"
		splitter.Parent = tabsHolder
		splitter.BackgroundColor3 = Color3.new(1, 1, 1)
		splitter.BackgroundTransparency = 1
		splitter.LayoutOrder = 2
		splitter.Size = UDim2:new(6, 1)
		splitter.Font = Enum.Font.Code
		splitter.Text = "|"
		splitter.TextColor3 = library.colors.tabText
		colored[1 + #colored] = {splitter, "TextColor3", "tabText"}
		splitter.TextSize = 14
		splitter.TextStrokeColor3 = library.colors.tabText
		colored[1 + #colored] = {splitter, "TextStrokeColor3", "tabText"}
		splitter.TextStrokeTransparency = 0.75
		tabSlider.Name = "tabSlider"
		tabSlider.Parent = main
		tabSlider.BackgroundColor3 = library.colors.main
		colored[1 + #colored] = {tabSlider, "BackgroundColor3", "main"}
		tabSlider.BorderSizePixel = 0
		tabSlider.Position = UDim2.fromOffset(100, 30)
		tabSlider.Size = UDim2:fromOffset(1)
		tabSlider.Visible = false
		local IgnoreCoreInputs = nil
		do
			local os_clock = os.clock
			library.signals[1 + #library.signals] = userInputService.InputBegan:Connect(function(keyCode)
				if IgnoreCoreInputs or userInputService:GetFocusedTextBox() then
					return
				elseif keyCode.KeyCode == library.configuration.hideKeybind then
					if not lasthidebing or ((os_clock() - lasthidebing) > 12) then
						main.Visible = not main.Visible
					end
					lasthidebing = nil
				end
			end)
		end
		local windowFunctions = {
			tabCount = 0,
			selected = {},
			Flags = elements
		}
		library.globals["__Window" .. windowName].windowFunctions = windowFunctions
		function windowFunctions:Show(x)
			main.Visible = (x == nil) or (x == true) or (x == 1)
			return main.Visible
		end
		function windowFunctions:Hide(x)
			main.Visible = (x == false) or (x == 0)
			return main.Visible
		end
		function windowFunctions:Visibility(x)
			if x == nil then
				main.Visible = not main.Visible
			else
				main.Visible = x and true
			end
			return main.Visible
		end
		function windowFunctions:MoveTabSlider(tabObject)
			spawn(function()
				tabSlider.Visible = true
				tweenService:Create(tabSlider, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
					Size = UDim2.fromOffset(tabObject.AbsoluteSize.X, 1),
					Position = UDim2.fromOffset(tabObject.AbsolutePosition.X, tabObject.AbsolutePosition.Y + tabObject.AbsoluteSize.Y) - UDim2.fromOffset(main.AbsolutePosition.X, main.AbsolutePosition.Y)
				}):Play()
			end)
		end
		windowFunctions.LastTab = nil
		function windowFunctions:CreateTab(options, ...)
			options = (options and (type(options) == "string") and resolvevararg("Tab", options, ...)) or options or {
				Name = "Pepsi Style: Elite Lego Hax"
			}
			local image = options.Image
			if image then
				image = resolveid(image)
			end
			local tabName = options.Name or "Unnamed Tab"
			options.Name = tabName
			windowFunctions.tabCount = windowFunctions.tabCount + 1
			local newTab = Instance_new((image and "ImageButton") or "TextButton")
			local newTabHolder = Instance_new("Frame")
			library.globals["__Window" .. windowName].newTabHolder = newTabHolder
			local left = Instance_new("ScrollingFrame")
			local leftList = Instance_new("UIListLayout")
			local leftPadding = Instance_new("UIPadding")
			local right = Instance_new("ScrollingFrame")
			local rightList = Instance_new("UIListLayout")
			local rightPadding = Instance_new("UIPadding")
			newTab.Name = removeSpaces((tabName and tostring(tabName):lower() or "???") .. "Tab")
			newTab.Parent = tabsHolder
			newTab.BackgroundTransparency = 1
			newTab.LayoutOrder = (options.LastTab and 99999) or tonumber(options.TabOrder or options.LayoutOrder) or (2 + windowFunctions.tabCount)
			local colored_newTab_TextColor3 = nil
			if image then
				newTab.Image = image
				newTab.ImageColor3 = options.ImageColor or options.Color or Color3.new(1, 1, 1)
				newTab.Size = UDim2:new(tabsHolder.AbsoluteSize.Y, 1)
			else
				colored_newTab_TextColor3 = {newTab, "TextColor3", "tabText"}
				colored[1 + #colored] = colored_newTab_TextColor3
				newTab.Font = Enum.Font.Code
				newTab.Text = (tabName and tostring(tabName)) or "???"
				if windowFunctions.tabCount ~= 1 then
					colored_newTab_TextColor3[4] = 1.35
					newTab.TextColor3 = darkenColor(library.colors.tabText, 1.35)
				else
					newTab.TextColor3 = library.colors.tabText
				end
				newTab.TextSize = 14
				newTab.TextStrokeColor3 = Color3.fromRGB(42, 42, 42)
				newTab.TextStrokeTransparency = 0.75
				newTab.Size = UDim2:new(textToSize(newTab).X + 4, 1)
			end
			local function goto()
				if not library.colorpicker and not submenuOpen and (windowFunctions.selected.button ~= newTab) and newTab.Parent and newTabHolder.Parent then
					pcall(function()
						for _, e in next, library.elements do
							if e and (type(e) == "table") then
								if e.Update then
									pcall(e.Update)
								end
								if e.UpdateAll then
									pcall(e.UpdateAll)
								end
							end
						end
					end)
					if windowFunctions.LastTab then
						windowFunctions.LastTab[4] = 1.35
					end
					windowFunctions:MoveTabSlider(newTab)
					if windowFunctions.selected.button.ClassName == "TextButton" then
						tweenService:Create(windowFunctions.selected.button, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							TextColor3 = darkenColor(library.colors.tabText, 1.35)
						}):Play()
					end
					if colored_newTab_TextColor3 then
						colored_newTab_TextColor3[4] = nil
					end
					windowFunctions.selected.holder.Visible = false
					windowFunctions.selected.button = newTab
					windowFunctions.selected.holder = newTabHolder
					if windowFunctions.selected.button.ClassName == "TextButton" then
						tweenService:Create(windowFunctions.selected.button, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							TextColor3 = library.colors.tabText
						}):Play()
					end
					windowFunctions.selected.holder.Visible = true
					windowFunctions.LastTab = colored_newTab_TextColor3
				end
			end
			if not homepage and newTab.LayoutOrder <= 4 then
				homepage = goto
			end
			library.signals[1 + #library.signals] = newTab.MouseButton1Click:Connect(goto)
			if windowFunctions.tabCount == 1 then
				tabSlider.Size = UDim2.fromOffset(newTab.AbsoluteSize.X, 1)
				tabSlider.Position = UDim2.fromOffset(newTab.AbsolutePosition.X, newTab.AbsolutePosition.Y + newTab.AbsoluteSize.Y) - UDim2.fromOffset(main.AbsolutePosition.X, main.AbsolutePosition.Y)
				tabSlider.Visible = true
				windowFunctions.selected.holder = newTabHolder
				windowFunctions.selected.button = newTab
			end
			newTabHolder.Name = removeSpaces((tabName and tabName:lower()) or "???") .. "TabHolder"
			newTabHolder.Parent = innerMainHolder
			newTabHolder.BackgroundColor3 = Color3.new(1, 1, 1)
			newTabHolder.BackgroundTransparency = 1
			newTabHolder.Size = UDim2.fromScale(1, 1)
			newTabHolder.Visible = windowFunctions.tabCount == 1
			left.Name = "left"
			left.Parent = newTabHolder
			left.BackgroundColor3 = Color3.new(1, 1, 1)
			left.BackgroundTransparency = 1
			left.Size = UDim2.fromScale(0.5, 1)
			left.CanvasSize = UDim2.new()
			left.ScrollBarThickness = 0
			leftList.Name = "leftList"
			leftList.Parent = left
			leftList.HorizontalAlignment = Enum.HorizontalAlignment.Center
			leftList.SortOrder = Enum.SortOrder.LayoutOrder
			leftList.Padding = UDim:new(14)
			leftPadding.Name = "leftPadding"
			leftPadding.Parent = left
			leftPadding.PaddingTop = UDim:new(12)
			right.Name = "right"
			right.Parent = newTabHolder
			right.BackgroundColor3 = Color3.new(1, 1, 1)
			right.BackgroundTransparency = 1
			right.Size = UDim2.fromScale(0.5, 1)
			right.CanvasSize = UDim2.new()
			right.ScrollBarThickness = 0
			right.Position = UDim2.new(0.5)
			rightList.Name = "rightList"
			rightList.Parent = right
			rightList.HorizontalAlignment = Enum.HorizontalAlignment.Center
			rightList.SortOrder = Enum.SortOrder.LayoutOrder
			rightList.Padding = UDim:new(14)
			rightPadding.Name = "rightPadding"
			rightPadding.Parent = right
			rightPadding.PaddingTop = UDim:new(12)
			local tabFunctions = {
				Flags = {},
				Remove = function()
					local relod = nil
					if newTab then
						newTab.Parent = nil
						relod = true
					end
					if newTabHolder then
						newTabHolder.Parent = nil
						relod = true
					end
					if relod then
						windowFunctions:UpdateAll()
					end
				end,
				Select = goto
			}
			function tabFunctions:CreateSection(options, ...)
				options = (options and type(options) == "string" and resolvevararg("Tab", options, ...)) or options
				local sectionName, holderSide = options.Name or "Unnamed Section", options.Side
				options.Name = sectionName
				local newSection = Instance_new("Frame")
				local newSectionBorder = Instance_new("Frame")
				local insideBorderHider = Instance_new("Frame")
				local outsideBorderHider = Instance_new("Frame")
				local sectionHolder = Instance_new("Frame")
				local sectionList = Instance_new("UIListLayout")
				local sectionPadding = Instance_new("UIPadding")
				local sectionHeadline = Instance_new("TextLabel")
				colorpickerconflicts[1 + #colorpickerconflicts] = insideBorderHider
				colorpickerconflicts[1 + #colorpickerconflicts] = outsideBorderHider
				colorpickerconflicts[1 + #colorpickerconflicts] = sectionHeadline
				newSection.Name = removeSpaces((sectionName and sectionName:lower() or "???") .. "Section")
				newSection.Parent = (holderSide and (((holderSide:lower() == "left") and left) or right)) or left
				newSection.BackgroundColor3 = library.colors.sectionBackground
				colored[1 + #colored] = {newSection, "BackgroundColor3", "sectionBackground"}
				newSection.BorderColor3 = library.colors.outerBorder
				colored[1 + #colored] = {newSection, "BorderColor3", "outerBorder"}
				newSection.Size = UDim2.new(1, -20)
				newSection.Visible = false
				newSectionBorder.Name = "newSectionBorder"
				newSectionBorder.Parent = newSection
				newSectionBorder.BackgroundColor3 = library.colors.sectionBackground
				colored[1 + #colored] = {newSectionBorder, "BackgroundColor3", "sectionBackground"}
				newSectionBorder.BorderColor3 = library.colors.innerBorder
				colored[1 + #colored] = {newSectionBorder, "BorderColor3", "innerBorder"}
				newSectionBorder.BorderMode = Enum.BorderMode.Inset
				newSectionBorder.Size = UDim2.fromScale(1, 1)
				sectionHolder.Name = "sectionHolder"
				sectionHolder.Parent = newSection
				sectionHolder.BackgroundColor3 = Color3.new(1, 1, 1)
				sectionHolder.BackgroundTransparency = 1
				sectionHolder.Size = UDim2.fromScale(1, 1)
				sectionList.Name = "sectionList"
				sectionList.Parent = sectionHolder
				sectionList.HorizontalAlignment = Enum.HorizontalAlignment.Center
				sectionList.SortOrder = Enum.SortOrder.LayoutOrder
				sectionList.Padding = UDim:new(1)
				sectionPadding.Name = "sectionPadding"
				sectionPadding.Parent = sectionHolder
				sectionPadding.PaddingTop = UDim:new(9)
				sectionHeadline.Name = "sectionHeadline"
				sectionHeadline.Parent = newSection
				sectionHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
				sectionHeadline.BackgroundTransparency = 1
				sectionHeadline.Position = UDim2.fromOffset(18, -8)
				sectionHeadline.ZIndex = 2
				sectionHeadline.Font = Enum.Font.Code
				sectionHeadline.LineHeight = 1.15
				sectionHeadline.Text = (sectionName and sectionName or "???")
				sectionHeadline.TextColor3 = library.colors.section
				colored[1 + #colored] = {sectionHeadline, "TextColor3", "section"}
				sectionHeadline.TextSize = 14
				sectionHeadline.Size = UDim2.fromOffset(textToSize(sectionHeadline).X + 4, 12)
				insideBorderHider.Name = "insideBorderHider"
				insideBorderHider.Parent = newSection
				insideBorderHider.BackgroundColor3 = library.colors.sectionBackground
				colored[1 + #colored] = {insideBorderHider, "BackgroundColor3", "sectionBackground"}
				insideBorderHider.BorderSizePixel = 0
				insideBorderHider.Position = UDim2.fromOffset(15)
				insideBorderHider.Size = UDim2.fromOffset(sectionHeadline.AbsoluteSize.X + 3, 1)
				outsideBorderHider.Name = "outsideBorderHider"
				outsideBorderHider.Parent = newSection
				outsideBorderHider.BackgroundColor3 = library.colors.background
				colored[1 + #colored] = {outsideBorderHider, "BackgroundColor3", "background"}
				outsideBorderHider.BorderSizePixel = 0
				outsideBorderHider.Position = UDim2.fromOffset(15, -1)
				outsideBorderHider.Size = UDim2.fromOffset(sectionHeadline.AbsoluteSize.X + 3, 1)
				local sectionFunctions = {
					Flags = {},
					Remove = function()
						if newSection then
							newSection.Parent = nil
							windowFunctions:UpdateAll()
						end
					end
				}
				function sectionFunctions:Update(extra)
					local currentHolder = newSection.Parent
					if not newSection.Visible then
						newSection.Visible = true
					end
					newSection.Size = UDim2.new(1, -20, 0, (15 + sectionList.AbsoluteContentSize.Y))
					if currentHolder then
						currentHolder.CanvasSize = UDim2:fromOffset(currentHolder:FindFirstChildOfClass("UIListLayout").AbsoluteContentSize.Y + 22 + (tonumber(extra) or 0))
					end
				end
				function sectionFunctions:UpdateAll(...)
					for _, obj in next, sectionFunctions.Flags do
						if obj then
							if obj.Update then
								pcall(obj.Update)
							end
							if obj.UpdateAll then
								pcall(obj.UpdateAll)
							end
						end
					end
					sectionFunctions:Update(...)
				end
				function sectionFunctions:AddToggle(options, ...)
					options = (options and type(options) == "string" and resolvevararg("Tab", options, ...)) or options
					local toggleName, alreadyEnabled, callback, flagName = assert(options.Name, "Missing Name for new toggle."), options.Value or options.Enabled, options.Callback, options.Flag or (function()
						library.unnamedtoggles = 1 + (library.unnamedtoggles or 0)
						return "Toggle" .. tostring(library.unnamedtoggles)
					end)()
					if elements[flagName] ~= nil then
						warn(debug.traceback("Warning! Re-used flag '" .. flagName .. "'", 3))
					end
					local newToggle = Instance_new("Frame")
					local toggle = Instance_new("ImageLabel")
					local toggleInner = Instance_new("ImageLabel")
					local toggleButton = Instance_new("TextButton")
					local toggleHeadline = Instance_new("TextLabel")
					local keybindPositioner = Instance_new("Frame")
					local keybindList = Instance_new("UIListLayout")
					local keybindButton = Instance_new("TextButton")
					local lockedup = options.Locked
					newToggle.Name = removeSpaces((toggleName and toggleName:lower() or "???") .. "Toggle")
					newToggle.Parent = sectionHolder
					newToggle.BackgroundColor3 = Color3.new(1, 1, 1)
					newToggle.BackgroundTransparency = 1
					newToggle.Size = UDim2.new(1, 0, 0, 19)
					toggle.Name = "toggle"
					toggle.Parent = newToggle
					toggle.Active = true
					toggle.BackgroundColor3 = library.colors.topGradient
					local colored_toggle_BackgroundColor3 = {toggle, "BackgroundColor3", "topGradient"}
					colored[1 + #colored] = colored_toggle_BackgroundColor3
					toggle.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {toggle, "BorderColor3", "elementBorder"}
					toggle.Position = UDim2.fromScale(0.0308237672, 0.165842205)
					toggle.Selectable = true
					toggle.Size = UDim2.fromOffset(12, 12)
					toggle.Image = "rbxassetid://2454009026"
					toggle.ImageColor3 = library.colors.bottomGradient
					local colored_toggle_ImageColor3 = {toggle, "ImageColor3", "bottomGradient"}
					colored[1 + #colored] = colored_toggle_ImageColor3
					toggleInner.Name = "toggleInner"
					toggleInner.Parent = toggle
					toggleInner.Active = true
					toggleInner.AnchorPoint = Vector2.new(0.5, 0.5)
					toggleInner.BackgroundColor3 = library.colors.topGradient
					local colored_toggleInner_BackgroundColor3 = {toggleInner, "BackgroundColor3", "topGradient"}
					colored[1 + #colored] = colored_toggleInner_BackgroundColor3
					toggleInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {toggleInner, "BorderColor3", "elementBorder"}
					toggleInner.Position = UDim2.fromScale(0.5, 0.5)
					toggleInner.Selectable = true
					toggleInner.Size = UDim2.new(1, -4, 1, -4)
					toggleInner.Image = "rbxassetid://2454009026"
					toggleInner.ImageColor3 = library.colors.bottomGradient
					local colored_toggleInner_ImageColor3 = {toggleInner, "ImageColor3", "bottomGradient"}
					colored[1 + #colored] = colored_toggleInner_ImageColor3
					toggleButton.Name = "toggleButton"
					toggleButton.Parent = newToggle
					toggleButton.BackgroundColor3 = Color3.new(1, 1, 1)
					toggleButton.BackgroundTransparency = 1
					toggleButton.Size = UDim2.fromScale(1, 1)
					toggleButton.ZIndex = 5
					toggleButton.Font = Enum.Font.SourceSans
					toggleButton.Text = ""
					toggleButton.TextColor3 = Color3.new()
					toggleButton.TextSize = 14
					toggleButton.TextTransparency = 1
					toggleHeadline.Name = "toggleHeadline"
					toggleHeadline.Parent = newToggle
					toggleHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
					toggleHeadline.BackgroundTransparency = 1
					toggleHeadline.Position = UDim2.fromScale(0.123, 0.165842161)
					toggleHeadline.Size = UDim2.fromOffset(170, 11)
					toggleHeadline.Font = Enum.Font.Code
					toggleHeadline.Text = toggleName or "???"
					toggleHeadline.TextColor3 = library.colors.elementText
					local colored_toggleHeadline_TextColor3 = {toggleHeadline, "TextColor3", "elementText", (lockedup and 0.5) or nil}
					colored[1 + #colored] = colored_toggleHeadline_TextColor3
					toggleHeadline.TextSize = 14
					toggleHeadline.TextXAlignment = Enum.TextXAlignment.Left
					local last_v = nil
					local function Set(t, newStatus)
						if nil == newStatus and t ~= nil then
							newStatus = t
						end
						last_v = library_flags[flagName]
						if options.Condition ~= nil then
							if type(options.Condition) == "function" then
								local v, e = pcall(options.Condition, newStatus, last_v)
								if e then
									if not v then
										warn(debug.traceback(string.format("Error in toggle %s's Condition function: %s", flagName, e), 2))
									end
								else
									return last_v
								end
							end
						end
						if newStatus ~= nil and type(newStatus) == "boolean" then
							library_flags[flagName] = newStatus
							if options.Location then
								options.Location[options.LocationFlag or flagName] = newStatus
							end
							if callback and (last_v ~= newStatus or options.AllowDuplicateCalls) then
								colored_toggleInner_BackgroundColor3[3] = (newStatus and "main") or "topGradient"
								colored_toggleInner_BackgroundColor3[4] = (newStatus and 1.5) or nil
								colored_toggleInner_ImageColor3[3] = (newStatus and "main") or "bottomGradient"
								colored_toggleInner_ImageColor3[4] = (newStatus and 2.5) or nil
								tweenService:Create(toggleInner, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
									BackgroundColor3 = (newStatus and darkenColor(library.colors.main, 1.5)) or library.colors.topGradient,
									ImageColor3 = (newStatus and darkenColor(library.colors.main, 2.5)) or library.colors.bottomGradient
								}):Play()
								task.spawn(callback, newStatus, last_v)
							end
						end
						return newStatus
					end
					options.Keybind = options.Keybind or options.Key or options.KeyBind
					local haskbflag, kbUpdate, kbData = nil, nil, nil
					if options.Keybind then
						local options = options.Keybind
						local htyp = typeof(options)
						if htyp == "EnumItem" then
							options = {
								Value = options
							}
						elseif htyp ~= "table" then
							options = {}
						end
						local presetKeybind, callback, kbpresscallback, kbflag = options.Value or options.Key, options.Callback, options.Pressed, options.Flag or (function()
							if flagName then
								return flagName .. "_ToggleKeybind"
							end
							library.unnamedkeybinds = 1 + (library.unnamedkeybinds or 0)
							return "Keybind" .. tostring(library.unnamedkeybinds)
						end)()
						if elements[kbflag] ~= nil or kbflag == flagName then
							warn(debug.traceback("Warning! Re-used flag '" .. kbflag .. "'", 3))
						end
						haskbflag = kbflag
						library.keyHandler = keyHandler
						local keyHandler = options.KeyNames or keyHandler
						local bindedKey = presetKeybind
						local justBinded = false
						local keyName = keyHandler.allowedKeys[bindedKey] or (bindedKey and (bindedKey.Name or tostring(bindedKey):gsub("Enum.KeyCode.", ""))) or "NONE"
						local newKeybind = newToggle
						keybindPositioner.Name = "keybindPositioner"
						keybindPositioner.Parent = newKeybind
						keybindPositioner.BackgroundColor3 = Color3.new(1, 1, 1)
						keybindPositioner.BackgroundTransparency = 1
						keybindPositioner.Position = UDim2.new(0.00448430516)
						keybindPositioner.Size = UDim2.fromOffset(214, 19)
						keybindPositioner.ZIndex = 1 + toggleButton.ZIndex
						keybindList.Name = "keybindList"
						keybindList.Parent = keybindPositioner
						keybindList.FillDirection = Enum.FillDirection.Horizontal
						keybindList.HorizontalAlignment = Enum.HorizontalAlignment.Right
						keybindList.SortOrder = Enum.SortOrder.LayoutOrder
						keybindList.VerticalAlignment = Enum.VerticalAlignment.Center
						keybindButton.Name = "keybindButton"
						keybindButton.Parent = keybindPositioner
						keybindButton.Active = false
						keybindButton.BackgroundColor3 = Color3.new(1, 1, 1)
						keybindButton.BackgroundTransparency = 1
						keybindButton.Position = UDim2.fromScale(0.598130822, 0.184210524)
						keybindButton.Selectable = false
						keybindButton.Size = UDim2.fromOffset(46, 12)
						keybindButton.Font = Enum.Font.Code
						keybindButton.Text = keyName or (presetKeybind and tostring(presetKeybind):gsub("Enum.KeyCode.", "")) or "[NONE]"
						keybindButton.TextColor3 = library.colors.otherElementText
						local colored_keybindButton_TextColor3 = {keybindButton, "TextColor3", "otherElementText"}
						colored[1 + #colored] = colored_keybindButton_TextColor3
						keybindButton.TextSize = 14
						keybindButton.TextXAlignment = Enum.TextXAlignment.Right
						keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
						local klast_v = bindedKey or presetKeybind
						local function newkey()
							if lockedup then
								return
							end
							local old_texts = keybindButton.Text
							colored_keybindButton_TextColor3[3] = "main"
							colored_keybindButton_TextColor3[4] = nil
							tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								TextColor3 = library.colors.main
							}):Play()
							if klast_v then
								keybindButton.Text = "(Was " .. (klast_v and tostring(klast_v):gsub("Enum.KeyCode.", "") or "[NONE]") .. ") [...]"
							else
								keybindButton.Text = "[...]"
							end
							local receivingKey = nil
							receivingKey = userInputService.InputBegan:Connect(function(key)
								if lockedup then
									return receivingKey:Disconnect()
								end
								klast_v = library_flags[kbflag]
								if not keyHandler.notAllowedKeys[key.KeyCode] then
									if key.KeyCode ~= Enum.KeyCode.Unknown then
										bindedKey = (key.KeyCode ~= Enum.KeyCode.Escape and key.KeyCode) or library_flags[kbflag]
										library_flags[kbflag] = bindedKey
										if options.Location then
											options.Location[options.LocationFlag or kbflag] = bindedKey
										end
										if bindedKey then
											keyName = keyHandler.allowedKeys[bindedKey] or (bindedKey and (bindedKey.Name or tostring(bindedKey):gsub("Enum.KeyCode.", ""))) or "NONE"
											keybindButton.Text = "[" .. (keyName or (bindedKey and bindedKey.Name) or "NONE") .. "]"
											keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
											justBinded = true
											colored_keybindButton_TextColor3[3] = "otherElementText"
											colored_keybindButton_TextColor3[4] = nil
											tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
												TextColor3 = library.colors.otherElementText
											}):Play()
											receivingKey:Disconnect()
										end
										if callback and klast_v ~= bindedKey then
											task.spawn(callback, bindedKey, klast_v)
										end
										return
									elseif key.KeyCode == Enum.KeyCode.Unknown and not keyHandler.notAllowedMouseInputs[key.UserInputType] then
										bindedKey = key.UserInputType
										library_flags[kbflag] = bindedKey
										if options.Location then
											options.Location[options.LocationFlag or kbflag] = bindedKey
										end
										keyName = keyHandler.allowedKeys[bindedKey]
										keybindButton.Text = "[" .. (keyName or (bindedKey and bindedKey.Name) or tostring(bindedKey.KeyCode):gsub("Enum.KeyCode.", "")) .. "]"
										keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
										justBinded = true
										colored_keybindButton_TextColor3[3] = "otherElementText"
										colored_keybindButton_TextColor3[4] = nil
										tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
											TextColor3 = library.colors.otherElementText
										}):Play()
										receivingKey:Disconnect()
										if callback and klast_v ~= bindedKey then
											task.spawn(callback, bindedKey, klast_v)
										end
										return
									end
								end
								if key.KeyCode == Enum.KeyCode.Backspace or Enum.KeyCode.Escape == key.KeyCode then
									old_texts, bindedKey = "[NONE]", nil
								end
								keybindButton.Text = old_texts
								colored_keybindButton_TextColor3[3] = "otherElementText"
								colored_keybindButton_TextColor3[4] = nil
								tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
									TextColor3 = library.colors.otherElementText
								}):Play()
								receivingKey:Disconnect()
								if callback and klast_v ~= bindedKey then
									task.spawn(callback, bindedKey, klast_v)
								end
							end)
							library.signals[1 + #library.signals] = receivingKey
						end
						library.signals[1 + #library.signals] = keybindButton.MouseButton1Click:Connect(newkey)
						if kbpresscallback and not justBinded then
							library.signals[1 + #library.signals] = userInputService.InputBegan:Connect(function(key, chatting)
								chatting = chatting or (userInputService:GetFocusedTextBox() and true)
								if not chatting and not justBinded then
									if not keyHandler.notAllowedKeys[key.KeyCode] and not keyHandler.notAllowedMouseInputs[key.UserInputType] then
										if bindedKey == key.UserInputType or not justBinded and bindedKey == key.KeyCode then
											if kbpresscallback then
												task.spawn(kbpresscallback, key, chatting)
											end
										end
										justBinded = false
									end
								end
							end)
						end
						options.Mode = (options.Mode and string.lower(tostring(options.Mode))) or "dynamic"
						local modes = {
							dynamic = 1,
							hold = 1,
							toggle = 1
						}
						library.signals[1 + #library.signals] = userInputService.InputBegan:Connect(function(input, chatting)
							if justBinded then
								wait(0.1)
								justBinded = false
								return
							elseif lockedup then
								return
							end
							chatting = chatting or userInputService:GetFocusedTextBox()
							if not chatting then
								local key = library_flags[kbflag]
								local mode = options.Mode
								if not modes[mode] then
									mode = "dynamic"
									options.Mode = mode
								end
								if key == input.KeyCode or key == input.UserInputType then
									if mode == "dynamic" or mode == "both" or mode == "hold" then
										if mode == "dynamic" and library_flags[flagName] then
											return Set(false)
										end
										Set(true)
										local now = os.clock()
										local waittil = nil
										if mode == "dynamic" then
											waittil = Instance.new("BindableEvent")
										end
										local xconnection = nil
										xconnection = userInputService.InputEnded:Connect(function(input, chatting)
											chatting = chatting or userInputService:GetFocusedTextBox()
											if not chatting and (key == input.KeyCode or key == input.UserInputType) then
												xconnection = (xconnection and xconnection:Disconnect() and nil) or nil
												if mode == "hold" or os.clock() - now > math.clamp(tonumber(options.DynamicTime) or 0.65, 0.05, 20) then
													Set(false)
												end
											end
										end)
										library.signals[1 + #library.signals] = xconnection
									else
										Set(not library_flags[flagName])
									end
								end
							end
						end)
						local function kbset(t, key)
							if nil == key and t ~= nil then
								key = t
							end
							if key == "nil" or key == "NONE" or key == "none" then
								key = nil
							end
							last_v = library_flags[kbflag]
							bindedKey = key
							library_flags[kbflag] = key
							if options.Location then
								options.Location[options.LocationFlag or kbflag] = key
							end
							keyName = (key == nil and "NONE") or keyHandler.allowedKeys[key]
							keybindButton.Text = "[" .. (keyName or key.Name or tostring(key):gsub("Enum.KeyCode.", "")) .. "]"
							keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
							justBinded = true
							colored_keybindButton_TextColor3[3] = "otherElementText"
							colored_keybindButton_TextColor3[4] = nil
							tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								TextColor3 = library.colors.otherElementText
							}):Play()
							if callback and (last_v ~= key or options.AllowDuplicateCalls) then
								task.spawn(callback, key, last_v)
							end
							return key
						end
						if presetKeybind ~= nil then
							kbset(presetKeybind)
						else
							library_flags[kbflag] = bindedKey
							if options.Location then
								options.Location[options.LocationFlag or kbflag] = bindedKey
							end
						end
						local default = library_flags[kbflag]
						local function UpdateKb()
							callback, kbpresscallback = options.Callback, options.Pressed
							local key = library_flags[kbflag]
							bindedKey = key
							keyName = keyHandler.allowedKeys[bindedKey] or (bindedKey and (bindedKey.Name or tostring(bindedKey):gsub("Enum.KeyCode.", ""))) or "NONE"
							keybindButton.Text = "[" .. (keyName or (key and key.Name) or tostring(key):gsub("Enum.KeyCode.", "")) .. "]"
							keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
							colored_keybindButton_TextColor3[3] = "otherElementText"
							colored_keybindButton_TextColor3[4] = (lockedup and 2.5) or nil
							tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								TextColor3 = (lockedup and darkenColor(library.colors.otherElementText, colored_keybindButton_TextColor3[4])) or library.colors.otherElementText
							}):Play()
							return key
						end
						kbUpdate = UpdateKb
						local objectdata = {
							Options = options,
							Name = kbflag,
							Flag = kbflag,
							Type = "Keybind",
							Default = default,
							Parent = sectionFunctions,
							Instance = keybindButton,
							Get = function()
								return library_flags[kbflag]
							end,
							Set = kbset,
							RawSet = function(t, key)
								if t ~= nil and key == nil then
									key = t
								end
								library_flags[kbflag] = key
								UpdateKb()
								return key
							end,
							Update = UpdateKb,
							Reset = function()
								return kbset(nil, default)
							end
						}
						kbData = objectdata
						tabFunctions.Flags[kbflag], sectionFunctions.Flags[kbflag], elements[kbflag] = objectdata, objectdata, objectdata
					end
					sectionFunctions:Update()
					library.signals[1 + #library.signals] = toggleButton.MouseButton1Click:Connect(function()
						if not library.colorpicker and not submenuOpen and not lockedup then
							local newval = not library_flags[flagName]
							if options.Condition ~= nil then
								if type(options.Condition) == "function" then
									local v, e = pcall(options.Condition, newval, not newval)
									if e then
										if not v then
											warn(debug.traceback(string.format("Error in toggle %s's Condition function: %s", flagName, e), 2))
										end
									else
										return last_v
									end
								end
							end
							library_flags[flagName] = newval
							if options.Location then
								options.Location[options.LocationFlag or flagName] = newval
							end
							colored_toggleInner_BackgroundColor3[3] = (newval and "main") or "topGradient"
							colored_toggleInner_BackgroundColor3[4] = (newval and 1.5) or nil
							colored_toggleInner_ImageColor3[3] = (newval and "main") or "bottomGradient"
							colored_toggleInner_ImageColor3[4] = (newval and 2.5) or nil
							tweenService:Create(toggleInner, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = (newval and darkenColor(library.colors.main, 1.5)) or library.colors.topGradient,
								ImageColor3 = (newval and darkenColor(library.colors.main, 2.5)) or library.colors.bottomGradient
							}):Play()
							if callback then
								task.spawn(callback, newval)
							end
						end
					end)
					library.signals[1 + #library.signals] = newToggle.MouseEnter:Connect(function()
						colored_toggle_BackgroundColor3[3] = "main"
						colored_toggle_BackgroundColor3[4] = 1.5
						colored_toggle_ImageColor3[3] = "main"
						colored_toggle_ImageColor3[4] = 2.5
						tweenService:Create(toggle, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = darkenColor(library.colors.main, 1.5),
							ImageColor3 = darkenColor(library.colors.main, 2.5)
						}):Play()
					end)
					library.signals[1 + #library.signals] = newToggle.MouseLeave:Connect(function()
						colored_toggle_BackgroundColor3[3] = "topGradient"
						colored_toggle_BackgroundColor3[4] = nil
						colored_toggle_ImageColor3[3] = "bottomGradient"
						colored_toggle_ImageColor3[4] = nil
						tweenService:Create(toggle, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = library.colors.topGradient,
							ImageColor3 = library.colors.bottomGradient
						}):Play()
					end)
					if library_flags[flagName] then
						colored_toggleInner_BackgroundColor3[3] = "main"
						colored_toggleInner_BackgroundColor3[4] = 1.5
						colored_toggleInner_ImageColor3[3] = "main"
						colored_toggleInner_ImageColor3[4] = 2.5
						tweenService:Create(toggleInner, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = darkenColor(library.colors.main, 1.5),
							ImageColor3 = darkenColor(library.colors.main, 2.5)
						}):Play()
					end
					local function Update()
						toggleName, callback = options.Name or toggleName, options.Callback
						local boolstatus = library_flags[flagName]
						colored_toggleInner_BackgroundColor3[3] = (boolstatus and "main") or "topGradient"
						colored_toggleInner_BackgroundColor3[4] = (boolstatus and 1.5) or nil
						colored_toggleInner_ImageColor3[3] = (boolstatus and "main") or "bottomGradient"
						colored_toggleInner_ImageColor3[4] = (boolstatus and 2.5) or nil
						if lockedup then
							colored_toggleInner_BackgroundColor3[4] = 1 + (colored_toggleInner_BackgroundColor3[4] or 1)
							colored_toggleInner_ImageColor3[4] = 1 + (colored_toggleInner_ImageColor3[4] or 1)
						end
						tweenService:Create(toggleInner, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = (boolstatus and darkenColor(library.colors.main, colored_toggleInner_BackgroundColor3[4])) or library.colors.topGradient,
							ImageColor3 = (boolstatus and darkenColor(library.colors.main, colored_toggleInner_ImageColor3[4])) or library.colors.bottomGradient
						}):Play()
						colored_toggleHeadline_TextColor3[4] = (lockedup and 2.5) or nil
						tweenService:Create(toggleHeadline, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							TextColor3 = (lockedup and darkenColor(library.colors.elementText, colored_toggleHeadline_TextColor3[4])) or library.colors.elementText
						}):Play()
						toggleHeadline.Text = toggleName or "???"
						return boolstatus
					end
					if alreadyEnabled ~= nil then
						Set(alreadyEnabled)
					else
						library_flags[flagName] = alreadyEnabled and true
						if options.Location then
							options.Location[options.LocationFlag or flagName] = alreadyEnabled and true
						end
					end
					local default = library_flags[flagName] and true
					Update()
					if kbUpdate then
						kbUpdate()
					end
					local objectdata = {
						Options = options,
						Type = "Toggle",
						Name = flagName,
						Flag = flagName,
						Default = default,
						Parent = sectionFunctions,
						Instance = toggleButton,
						Set = Set,
						Remove = function()
							if newToggle then
								newToggle.Parent = nil
								sectionFunctions:Update()
							end
						end,
						RawSet = function(t, newStatus, condition)
							if t ~= nil and type(t) ~= "table" then
								newStatus, condition = t, newStatus
							end
							last_v = library_flags[flagName]
							if (condition ~= false) and (condition ~= 0) then
								local overridecondition = condition and (type(condition) == "function") and condition
								if overridecondition or (options.Condition ~= nil) then
									if type(overridecondition or options.Condition) == "function" then
										local v, e = pcall(overridecondition or options.Condition, newStatus, last_v)
										if e then
											if not v then
												warn(debug.traceback(string.format("Error in toggle (RawSet) %s's Condition function: %s", flagName, e), 2))
											end
										else
											return last_v
										end
									end
								end
							end
							library_flags[flagName] = newStatus
							if options.Location then
								options.Location[options.LocationFlag or flagName] = newStatus
							end
							Update()
							return newStatus
						end,
						KeybindData = kbData,
						Get = function()
							return library_flags[flagName]
						end,
						Update = Update,
						Reset = function()
							return Set(nil, default)
						end,
						SetLocked = function(t, state)
							if type(t) ~= "table" then
								state = t
							end
							local last_v = lockedup
							if state == nil then
								lockedup = not lockedup
							else
								lockedup = state
							end
							if lockedup ~= last_v then
								colored_toggleHeadline_TextColor3[4] = (lockedup and 2.5) or nil
								Update()
								if kbUpdate then
									kbUpdate()
								end
							end
							return lockedup
						end,
						Lock = function()
							if not lockedup then
								lockedup = true
								colored_toggleHeadline_TextColor3[4] = 2.5
								Update()
								if kbUpdate then
									kbUpdate()
								end
							end
							return lockedup
						end,
						Unlock = function()
							if lockedup then
								lockedup = false
								colored_toggleHeadline_TextColor3[4] = nil
								Update()
								if kbUpdate then
									kbUpdate()
								end
							end
							return lockedup
						end,
						SetCondition = function(t, condition)
							if type(t) ~= "table" and condition == nil then
								condition = t
							end
							options.Condition = condition
							return condition
						end
					}
					if kbData then
						kbData.ToggleData = objectdata
					end
					tabFunctions.Flags[flagName], sectionFunctions.Flags[flagName], elements[flagName] = objectdata, objectdata, objectdata
					return objectdata
				end
				sectionFunctions.CreateToggle = sectionFunctions.AddToggle
				sectionFunctions.NewToggle = sectionFunctions.AddToggle
				sectionFunctions.Toggle = sectionFunctions.AddToggle
				sectionFunctions.Tog = sectionFunctions.AddToggle
				function sectionFunctions:AddButton(...)
					local args = nil
					if ... and not select(2, ...) and type(...) == "table" and #... > 0 and type((...)[1]) == "table" and (...)[1].Name then
						args = ...
					else
						args = {...}
					end
					local buttons, offset = {}, 0
					local frames = {}
					local fram = nil
					for _, options in next, args do
						options = (options and options[1] and type(options[1]) == "string" and resolvevararg("Button", unpack(options))) or options
						local buttonName, callback = assert(options.Name, "Missing Name for new button."), options.Callback or (warn("AddButton missing callback. Name:", options.Name or "No Name", debug.traceback("")) and nil) or function()
						end
						local lockedup = options.Locked
						local realButton = Instance_new("TextButton")
						realButton.Name = "realButton"
						realButton.BackgroundColor3 = Color3.new(1, 1, 1)
						realButton.BackgroundTransparency = 1
						realButton.Size = UDim2.fromScale(1, 1)
						realButton.ZIndex = 5
						realButton.Font = Enum.Font.Code
						realButton.Text = (buttonName and tostring(buttonName)) or "???"
						realButton.TextColor3 = library.colors.elementText
						local colored_realButton_TextColor3 = {realButton, "TextColor3", "elementText"}
						colored[1 + #colored] = colored_realButton_TextColor3
						realButton.TextSize = 14
						local textsize = textToSize(realButton).X + 14
						if newSection.Parent and (newSection.Parent.AbsoluteSize.X < (offset + textsize + 8)) then
							offset, fram = 0, nil
						end
						local newButton = fram or Instance_new("Frame")
						fram = newButton
						local framButtons = frames[fram] or {}
						frames[fram] = framButtons
						local button = Instance_new("ImageLabel")
						newButton.Name = removeSpaces((buttonName and buttonName:lower() or "???") .. "Holder")
						newButton.Parent = sectionHolder
						newButton.BackgroundColor3 = Color3.new(1, 1, 1)
						newButton.BackgroundTransparency = 1
						newButton.Size = UDim2.new(1, 0, 0, 24)
						button.Name = "button"
						button.Parent = newButton
						button.Active = true
						button.BackgroundColor3 = library.colors.topGradient
						local colored_button_BackgroundColor3 = {button, "BackgroundColor3", "topGradient"}
						colored[1 + #colored] = colored_button_BackgroundColor3
						button.BorderColor3 = library.colors.elementBorder
						colored[1 + #colored] = {button, "BorderColor3", "elementBorder"}
						button.Position = UDim2.new(0.031, offset, 0.166)
						button.Selectable = true
						button.Size = UDim2.fromOffset(28, 18)
						button.Image = "rbxassetid://2454009026"
						button.ImageColor3 = library.colors.bottomGradient
						local colored_button_ImageColor3 = {button, "ImageColor3", "bottomGradient"}
						colored[1 + #colored] = colored_button_ImageColor3
						local buttonInner = Instance_new("ImageLabel")
						buttonInner.Name = "buttonInner"
						buttonInner.Parent = button
						buttonInner.Active = true
						buttonInner.AnchorPoint = Vector2.new(0.5, 0.5)
						buttonInner.BackgroundColor3 = library.colors.topGradient
						local colored_buttonInner_BackgroundColor3 = {buttonInner, "BackgroundColor3", "topGradient"}
						colored[1 + #colored] = colored_buttonInner_BackgroundColor3
						buttonInner.BorderColor3 = library.colors.elementBorder
						colored[1 + #colored] = {buttonInner, "BorderColor3", "elementBorder"}
						buttonInner.Position = UDim2.fromScale(0.5, 0.5)
						buttonInner.Selectable = true
						buttonInner.Size = UDim2.new(1, -4, 1, -4)
						buttonInner.Image = "rbxassetid://2454009026"
						buttonInner.ImageColor3 = library.colors.bottomGradient
						local colored_buttonInner_ImageColor3 = {buttonInner, "ImageColor3", "bottomGradient"}
						colored[1 + #colored] = colored_buttonInner_ImageColor3
						button.Size = UDim2.fromOffset(textsize, 18)
						realButton.Parent = button
						offset = offset + textsize + 6
						sectionFunctions:Update()
						local presses = 0
						library.signals[1 + #library.signals] = realButton.MouseButton1Click:Connect(function()
							if lockedup then
								return
							end
							if options.Condition ~= nil and type(options.Condition) == "function" then
								local v, e = pcall(options.Condition, presses)
								if e then
									if not v then
										warn(debug.traceback(string.format("Error in button %s's Condition function: %s", buttonName, e), 2))
									end
								else
									return
								end
							end
							if not library.colorpicker and not submenuOpen then
								presses = 1 + presses
								task.spawn(callback, presses)
							end
						end)
						local imin = nil
						library.signals[1 + #library.signals] = button.MouseEnter:Connect(function()
							imin = 1
							colored_button_BackgroundColor3[3] = "main"
							colored_button_BackgroundColor3[4] = 1.5
							colored_button_ImageColor3[3] = "main"
							colored_button_ImageColor3[4] = 2.5
							tweenService:Create(button, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = darkenColor(library.colors.main, 1.5),
								ImageColor3 = darkenColor(library.colors.main, 2.5)
							}):Play()
						end)
						library.signals[1 + #library.signals] = button.MouseLeave:Connect(function()
							imin = nil
							colored_button_BackgroundColor3[3] = "topGradient"
							colored_button_BackgroundColor3[4] = nil
							colored_button_ImageColor3[3] = "bottomGradient"
							colored_button_ImageColor3[4] = nil
							tweenService:Create(button, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = library.colors.topGradient,
								ImageColor3 = library.colors.bottomGradient
							}):Play()
						end)
						local function Update(Recursive)
							buttonName, callback = options.Name or buttonName, options.Callback or (warn(debug.traceback("AddButton missing callback. Name:" .. (options.Name or buttonName or "No Name"), 2)) and nil) or function()
							end
							colored_button_BackgroundColor3[3] = (imin and "main") or "topGradient"
							colored_button_BackgroundColor3[4] = (imin and 1.5) or nil
							colored_button_ImageColor3[3] = (imin and "main") or "bottomGradient"
							colored_button_ImageColor3[4] = (imin and 2.5) or nil
							colored_buttonInner_BackgroundColor3[4] = nil
							colored_buttonInner_ImageColor3[4] = nil
							colored_realButton_TextColor3[4] = nil
							if lockedup then
								colored_button_BackgroundColor3[4] = 1.25
								colored_button_ImageColor3[4] = 1.25
								colored_buttonInner_BackgroundColor3[4] = 1.25
								colored_buttonInner_ImageColor3[4] = 1.25
								colored_realButton_TextColor3[4] = 1.75
							end
							tweenService:Create(button, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = (imin and darkenColor(library.colors.main, colored_button_BackgroundColor3[4])) or darkenColor(library.colors.topGradient, colored_button_BackgroundColor3[4]),
								ImageColor3 = (imin and darkenColor(library.colors.main, colored_button_ImageColor3[4])) or darkenColor(library.colors.bottomGradient, colored_button_ImageColor3[4])
							}):Play()
							tweenService:Create(buttonInner, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = darkenColor(library.colors.topGradient, colored_buttonInner_BackgroundColor3[4]),
								ImageColor3 = darkenColor(library.colors.bottomGradient, colored_buttonInner_ImageColor3[4])
							}):Play()
							tweenService:Create(realButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								TextColor3 = darkenColor(library.colors.elementText, colored_realButton_TextColor3[4])
							}):Play()
							realButton.Text = (buttonName and tostring(buttonName)) or "???"
							local newtextsize = textToSize(realButton).X + 14
							if textsize ~= newtextsize then
								textsize = newtextsize
								button.Size = UDim2.fromOffset(textsize, 18)
								if Recursive ~= "Recursive" then
									if buttons and buttons.UpdateAll then
										buttons.UpdateAll()
									end
								end
							end
							return presses, textsize
						end
						Update()
						local objectdata = {
							Options = options,
							Name = buttonName,
							Flag = buttonName,
							Type = "Button",
							Parent = sectionFunctions,
							Instance = realButton,
							Frame = fram or newButton,
							ButtonFrame = button,
							Remove = function()
								if button then
									button.Parent = nil
									buttons.UpdateAll()
									sectionFunctions.UpdateAll()
								end
							end,
							Press = function(...)
								if lockedup then
									return presses
								end
								if options.Condition ~= nil and type(options.Condition) == "function" then
									local v, e = pcall(options.Condition, presses)
									if e then
										if v then
										else
											warn(debug.traceback(string.format("Error in button %s's Condition function: %s", buttonName, e), 2))
										end
									else
										return presses
									end
								end
								local args = {...}
								local a1 = args[1]
								if a1 and type(a1) == "table" then
									table.remove(args, 1)
								end
								presses = 1 + presses
								task.spawn(callback, presses, ...)
								return presses
							end,
							RawPress = function(...)
								local args = {...}
								local a1 = args[1]
								if a1 and type(a1) == "table" then
									table.remove(args, 1)
								end
								task.spawn(callback, presses, ...)
								return presses
							end,
							Get = function()
								return callback, presses
							end,
							SetLocked = function(t, state)
								if type(t) ~= "table" then
									state = t
								end
								local last_v = lockedup
								if state == nil then
									lockedup = not lockedup
								else
									lockedup = state
								end
								if lockedup ~= last_v then
									Update()
								end
								return lockedup
							end,
							Lock = function()
								if not lockedup then
									lockedup = true
									Update()
								end
								return lockedup
							end,
							Unlock = function()
								if lockedup then
									lockedup = false
									Update()
								end
								return lockedup
							end,
							SetCondition = function(t, condition)
								if type(t) ~= "table" and condition == nil then
									condition = t
								end
								options.Condition = condition
								return condition
							end,
							Update = Update,
							SetText = function(t, str)
								if type(t) ~= "table" and str == nil then
									str = t
								end
								buttonName = str
								options.Name = str
								realButton.Text = (buttonName and tostring(buttonName)) or "???"
								return str
							end,
							SetCallback = function(t, call)
								if type(t) ~= "table" and call == nil then
									call = t
								end
								options.Callback = call
								callback = call
								return call
							end
						}
						tabFunctions.Flags[buttonName], sectionFunctions.Flags[buttonName], elements[buttonName] = objectdata, objectdata, objectdata
						buttons[1 + #buttons] = objectdata
						framButtons[1 + #framButtons] = objectdata
					end
					function buttons.PressAll()
						for _, v in next, buttons do
							v.Press()
						end
					end
					function buttons.UpdateAll()
						for fram, Fram in next, frames do
							if Fram and fram then
								local offset = 0
								for _, v in next, Fram do
									if v and v.Instance and fram:IsAncestorOf(v.Instance) then
										local _, textsize = v.Update("Recursive")
										textsize = tonumber(textsize) or (textToSize(v.Instance).X + 14)
										v.ButtonFrame.Position = UDim2.new(0.031, offset, 0.166)
										offset = offset + textsize + 6
									end
								end
							end
						end
					end
					buttons.Update = buttons.UpdateAll
					function buttons.RemoveAll()
						for _, v in next, buttons do
							v.Remove()
						end
					end
					function buttons.Remove()
						for fram in next, frames do
							if fram then
								fram.Parent = nil
							end
						end
					end
					if #buttons == 1 then
						for k, v in next, buttons[1] do
							if buttons[k] == nil then
								buttons[k] = v
							end
						end
					end
					return buttons
				end
				sectionFunctions.CreateButton = sectionFunctions.AddButton
				sectionFunctions.NewButton = sectionFunctions.AddButton
				sectionFunctions.Button = sectionFunctions.AddButton
				function sectionFunctions:AddTextbox(options, ...)
					options = (options and type(options) == "string" and resolvevararg("Textbox", options, ...)) or options
					local textboxName, presetValue, placeholder, callback, flagName = assert(options.Name, "Missing Name for new textbox."), options.Value, options.Placeholder, options.Callback, options.Flag or (function()
						library.unnamedtextboxes = 1 + (library.unnamedtextboxes or 0)
						return "Textbox" .. tostring(library.unnamedtextboxes)
					end)()
					if elements[flagName] ~= nil then
						warn(debug.traceback("Warning! Re-used flag '" .. flagName .. "'", 3))
					end
					local requiredtype = options.Type
					local newTextbox = Instance_new("Frame")
					local textbox = Instance_new("ImageLabel")
					local textboxInner = Instance_new("ImageLabel")
					local realTextbox = Instance_new("TextBox")
					local textboxHeadline = Instance_new("TextLabel")
					newTextbox.Name = removeSpaces((textboxName and textboxName:lower()) or "???") .. "Holder"
					newTextbox.Parent = sectionHolder
					newTextbox.BackgroundColor3 = Color3.new(1, 1, 1)
					newTextbox.BackgroundTransparency = 1
					newTextbox.Size = UDim2.new(1, 0, 0, 42)
					textbox.Name = "textbox"
					textbox.Parent = newTextbox
					textbox.Active = true
					textbox.BackgroundColor3 = library.colors.topGradient
					local colored_textbox_BackgroundColor3 = {textbox, "BackgroundColor3", "topGradient"}
					colored[1 + #colored] = colored_textbox_BackgroundColor3
					textbox.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {textbox, "BorderColor3", "elementBorder"}
					textbox.Position = UDim2.fromScale(0.031, 0.48)
					textbox.Selectable = true
					textbox.Size = UDim2.fromOffset(206, 18)
					textbox.Image = "rbxassetid://2454009026"
					textbox.ImageColor3 = library.colors.bottomGradient
					local colored_textbox_ImageColor3 = {textbox, "ImageColor3", "bottomGradient"}
					colored[1 + #colored] = colored_textbox_ImageColor3
					textboxInner.Name = "textboxInner"
					textboxInner.Parent = textbox
					textboxInner.Active = true
					textboxInner.AnchorPoint = Vector2.new(0.5, 0.5)
					textboxInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {textboxInner, "BackgroundColor3", "topGradient"}
					textboxInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {textboxInner, "BorderColor3", "elementBorder"}
					textboxInner.Position = UDim2.fromScale(0.5, 0.5)
					textboxInner.Selectable = true
					textboxInner.Size = UDim2.new(1, -4, 1, -4)
					textboxInner.Image = "rbxassetid://2454009026"
					textboxInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {textboxInner, "ImageColor3", "bottomGradient"}
					realTextbox.Name = "realTextbox"
					if options.Rich or options.RichText or options.RichTextBox then
						realTextbox.RichText = true
					end
					if options.MultiLine or options.Lines then
						realTextbox.MultiLine = true
					end
					if options.Font or options.TextFont then
						realTextbox.Font = options.Font
					end
					if options.TextScaled or options.Scaled then
						realTextbox.TextScaled = true
					end
					realTextbox.BackgroundColor3 = Color3.new(1, 1, 1)
					realTextbox.BackgroundTransparency = 1
					realTextbox.Position = UDim2.new(0.0295485705)
					realTextbox.Size = UDim2.fromScale(0.97, 1)
					realTextbox.ZIndex = 5
					realTextbox.Font = Enum.Font.Code
					realTextbox.LineHeight = 1.15
					realTextbox.Text = tostring(presetValue)
					realTextbox.TextColor3 = library.colors.otherElementText
					colored[1 + #colored] = {realTextbox, "TextColor3", "otherElementText"}
					realTextbox.TextSize = 14
					if options.ClearTextOnFocus or options.ClearText then
						realTextbox.ClearTextOnFocus = true
					else
						realTextbox.ClearTextOnFocus = false
					end
					realTextbox.PlaceholderText = (placeholder ~= nil and tostring(placeholder)) or (presetValue ~= nil and tostring(presetValue)) or ""
					realTextbox.TextXAlignment = Enum.TextXAlignment.Left
					if options.CustomProperties and type(options.CustomProperties) == "table" then
						for k, v in next, options.CustomProperties do
							local oof, e = pcall(function()
								realTextbox[k] = v
							end)
							if not oof and e then
								warn("Error setting Textbox", flagName, "|", e, debug.traceback(""))
							end
						end
					end
					realTextbox.Parent = textbox
					textboxHeadline.Name = "textboxHeadline"
					textboxHeadline.Parent = newTextbox
					textboxHeadline.Active = true
					textboxHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
					textboxHeadline.BackgroundTransparency = 1
					textboxHeadline.Position = UDim2.new(0.031)
					textboxHeadline.Selectable = true
					textboxHeadline.Size = UDim2.fromOffset(206, 20)
					textboxHeadline.ZIndex = 5
					textboxHeadline.Font = Enum.Font.Code
					textboxHeadline.LineHeight = 1.15
					textboxHeadline.Text = (textboxName and tostring(textboxName)) or "???"
					textboxHeadline.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {textboxHeadline, "TextColor3", "elementText"}
					textboxHeadline.TextSize = 14
					textboxHeadline.TextXAlignment = Enum.TextXAlignment.Left
					sectionFunctions:Update()
					local last_v = presetValue
					local function resolvevalue(val)
						if options.PreFormat then
							local typ = type(options.PreFormat)
							if typ == "function" then
								local x, e = pcall(options.PreFormat, val)
								if not x and e then
									warn("Error in Pre-Format (Textbox " .. flagName .. "):", e)
								else
									val = e
								end
							end
						end
						if requiredtype == "number" then
							if not options.Hex and not options.Binary and not options.Base then
								val = tonumber(val) or tonumber(val:gsub("%D", ""), 10) or 0
							else
								val = tonumber(val, (options.Hex and 16) or (options.Binary and 2) or options.Base or 10) or 0
							end
							if options.Max or options.Min then
								val = math.clamp(val, options.Min or -math.huge, options.Max or math.huge)
							end
							local decimalprecision = tonumber(options.Decimals or options.Precision or options.Precise)
							if decimalprecision then
								val = tonumber(string.format("%0." .. tostring(decimalprecision) .. "f", val))
							end
						end
						if options.PostFormat then
							local typ = type(options.PostFormat)
							if typ == "function" then
								local x, e = pcall(options.PostFormat, val)
								if not x and e then
									warn("Error in Post-Format (Textbox " .. flagName .. "):", e)
								else
									val = e
								end
							end
						end
						return (val and tonumber(val)) or val
					end
					library.signals[1 + #library.signals] = realTextbox.FocusLost:Connect(function()
						last_v = library_flags[flagName]
						local val = resolvevalue(realTextbox.Text)
						library_flags[flagName] = val
						if options.Location then
							options.Location[options.LocationFlag or flagName] = val
						end
						if callback and last_v ~= val then
							task.spawn(callback, tostring(val), last_v, realTextbox)
						end
					end)
					library.signals[1 + #library.signals] = newTextbox.MouseEnter:Connect(function()
						colored_textbox_BackgroundColor3[3] = "main"
						colored_textbox_BackgroundColor3[4] = 1.5
						colored_textbox_ImageColor3[3] = "main"
						colored_textbox_ImageColor3[4] = 2.5
						tweenService:Create(textbox, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = darkenColor(library.colors.main, 1.5),
							ImageColor3 = darkenColor(library.colors.main, 2.5)
						}):Play()
					end)
					library.signals[1 + #library.signals] = newTextbox.MouseLeave:Connect(function()
						colored_textbox_BackgroundColor3[3] = "topGradient"
						colored_textbox_BackgroundColor3[4] = nil
						colored_textbox_ImageColor3[3] = "bottomGradient"
						colored_textbox_ImageColor3[4] = nil
						tweenService:Create(textbox, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = library.colors.topGradient,
							ImageColor3 = library.colors.bottomGradient
						}):Play()
					end)
					local function set(t, str)
						if nil == str and t ~= nil then
							str = t
						end
						last_v = library_flags[flagName]
						library_flags[flagName] = str
						if options.Location then
							options.Location[options.LocationFlag or flagName] = str
						end
						local sstr = (str ~= nil and tostring(str)) or "Empty Text"
						if realTextbox.Text ~= sstr then
							realTextbox.Text = sstr
						end
						if callback and (last_v ~= str or options.AllowDuplicateCalls) then
							task.spawn(callback, str, last_v, realTextbox)
						end
						return str
					end
					if presetValue ~= nil then
						set(presetValue)
					else
						library_flags[flagName] = presetValue
						if options.Location then
							options.Location[options.LocationFlag or flagName] = presetValue
						end
					end
					local default = library_flags[flagName]
					local function update()
						textboxName, placeholder, callback = options.Name or textboxName, options.Placeholder or placeholder, options.Callback
						local str = library_flags[flagName]
						str = (str ~= nil and tostring(str)) or "Empty Text"
						if realTextbox.Text ~= str then
							realTextbox.Text = str
						end
						textboxHeadline.Text = (textboxName and tostring(textboxName)) or "???"
						return str
					end
					local objectdata = {
						Options = options,
						Name = flagName,
						Flag = flagName,
						Type = "Textbox",
						Default = default,
						Parent = sectionFunctions,
						Instance = realTextbox,
						Get = function()
							return library_flags[flagName]
						end,
						Set = set,
						Update = update,
						Remove = function()
							if newTextbox then
								newTextbox.Parent = nil
								sectionFunctions:Update()
							end
						end,
						RawSet = function(t, str)
							if t ~= nil and str == nil then
								str = t
							end
							last_v = library_flags[flagName]
							library_flags[flagName] = str
							if options.Location then
								options.Location[options.LocationFlag or flagName] = str
							end
							update()
							return str
						end,
						Reset = function()
							return set(nil, default)
						end
					}
					tabFunctions.Flags[flagName], sectionFunctions.Flags[flagName], elements[flagName] = objectdata, objectdata, objectdata
					return objectdata
				end
				sectionFunctions.AddTextBox = sectionFunctions.AddTextbox
				sectionFunctions.NewTextBox = sectionFunctions.AddTextbox
				sectionFunctions.CreateTextBox = sectionFunctions.AddTextbox
				sectionFunctions.TextBox = sectionFunctions.AddTextbox
				sectionFunctions.NewTextbox = sectionFunctions.AddTextbox
				sectionFunctions.CreateTextbox = sectionFunctions.AddTextbox
				sectionFunctions.Textbox = sectionFunctions.AddTextbox
				sectionFunctions.Box = sectionFunctions.AddTextbox
				function sectionFunctions:AddKeybind(options, ...)
					options = (options and type(options) == "string" and resolvevararg("Keybind", options, ...)) or options
					local keybindName, presetKeybind, callback, presscallback, flag = assert(options.Name, "Missing Name for new keybind."), options.Value, options.Callback, options.Pressed, options.Flag or (function()
						library.unnamedkeybinds = 1 + (library.unnamedkeybinds or 0)
						return "Keybind" .. tostring(library.unnamedkeybinds)
					end)()
					local IsCore = (options.CoreBinding and true) or nil
					if elements[flag] ~= nil then
						warn(debug.traceback("Warning! Re-used flag '" .. flag .. "'", 3))
					end
					library.keyHandler = keyHandler
					local keyHandler = options.KeyNames or keyHandler
					local newKeybind = Instance_new("Frame")
					local keybindHeadline = Instance_new("TextLabel")
					local keybindPositioner = Instance_new("Frame")
					local keybindList = Instance_new("UIListLayout")
					local keybindButton = Instance_new("TextButton")
					local bindedKey = presetKeybind
					local justBinded = false
					local keyName = (presetKeybind and tostring(presetKeybind):gsub("Enum.KeyCode.", "") or "")
					newKeybind.Name = "newKeybind"
					newKeybind.Parent = sectionHolder
					newKeybind.BackgroundColor3 = Color3.new(1, 1, 1)
					newKeybind.BackgroundTransparency = 1
					newKeybind.Size = UDim2.new(1, 0, 0, 19)
					keybindHeadline.Name = "keybindHeadline"
					keybindHeadline.Parent = newKeybind
					keybindHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
					keybindHeadline.BackgroundTransparency = 1
					keybindHeadline.Position = UDim2.fromScale(0.031, 0.165842161)
					keybindHeadline.Size = UDim2.fromOffset(215, 12)
					keybindHeadline.Font = Enum.Font.Code
					keybindHeadline.Text = (keybindName and tostring(keybindName)) or "???"
					keybindHeadline.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {keybindHeadline, "TextColor3", "elementText"}
					keybindHeadline.TextSize = 14
					keybindHeadline.TextXAlignment = Enum.TextXAlignment.Left
					keybindPositioner.Name = "keybindPositioner"
					keybindPositioner.Parent = newKeybind
					keybindPositioner.BackgroundColor3 = Color3.new(1, 1, 1)
					keybindPositioner.BackgroundTransparency = 1
					keybindPositioner.Position = UDim2.new(0.00448430516)
					keybindPositioner.Size = UDim2.fromOffset(214, 19)
					keybindList.Name = "keybindList"
					keybindList.Parent = keybindPositioner
					keybindList.FillDirection = Enum.FillDirection.Horizontal
					keybindList.HorizontalAlignment = Enum.HorizontalAlignment.Right
					keybindList.SortOrder = Enum.SortOrder.LayoutOrder
					keybindList.VerticalAlignment = Enum.VerticalAlignment.Center
					keybindButton.Name = "keybindButton"
					keybindButton.Parent = keybindPositioner
					keybindButton.Active = false
					keybindButton.BackgroundColor3 = Color3.new(1, 1, 1)
					keybindButton.BackgroundTransparency = 1
					keybindButton.Position = UDim2.fromScale(0.598130822, 0.184210524)
					keybindButton.Selectable = false
					keybindButton.Size = UDim2.fromOffset(46, 12)
					keybindButton.Font = Enum.Font.Code
					keybindButton.Text = (presetKeybind and tostring(presetKeybind):gsub("Enum.KeyCode.", "") or "[NONE]")
					keybindButton.TextColor3 = library.colors.otherElementText
					local colored_keybindButton_TextColor3 = {keybindButton, "TextColor3", "otherElementText"}
					colored[1 + #colored] = colored_keybindButton_TextColor3
					keybindButton.TextSize = 14
					keybindButton.TextXAlignment = Enum.TextXAlignment.Right
					keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
					sectionFunctions:Update()
					local last_v = bindedKey or presetKeybind
					local function newkey()
						local IgnoreKey = nil
						if IsCore then
							IgnoreKey = tick()
							IgnoreCoreInputs = IgnoreKey
						end
						local old_texts = keybindButton.Text
						colored_keybindButton_TextColor3[3] = "main"
						colored_keybindButton_TextColor3[4] = nil
						tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							TextColor3 = library.colors.main
						}):Play()
						if last_v then
							keybindButton.Text = "(Was " .. (last_v and tostring(last_v):gsub("Enum.KeyCode.", "") or "[NONE]") .. ") [...]"
						else
							keybindButton.Text = "[...]"
						end
						local receivingKey = nil
						receivingKey = userInputService.InputBegan:Connect(function(key)
							last_v = library_flags[flag]
							if not keyHandler.notAllowedKeys[key.KeyCode] then
								if key.KeyCode ~= Enum.KeyCode.Unknown then
									bindedKey = (key.KeyCode ~= Enum.KeyCode.Escape and key.KeyCode) or library_flags[flag]
									library_flags[flag] = bindedKey
									if options.Location then
										options.Location[options.LocationFlag or flag] = bindedKey
									end
									if bindedKey then
										keyName = keyHandler.allowedKeys[bindedKey]
										keybindButton.Text = "[" .. (keyName or bindedKey.Name or tostring(key.KeyCode):gsub("Enum.KeyCode.", "")) .. "]"
										keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
										justBinded = true
										colored_keybindButton_TextColor3[3] = "otherElementText"
										colored_keybindButton_TextColor3[4] = nil
										tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
											TextColor3 = library.colors.otherElementText
										}):Play()
										receivingKey:Disconnect()
									end
									if callback and last_v ~= bindedKey then
										task.spawn(callback, bindedKey, last_v)
									end
									if IsCore then
										delay(0.1, function()
											if IgnoreCoreInputs and (IgnoreCoreInputs == IgnoreKey) then
												IgnoreCoreInputs = nil
											end
										end)
									end
									return
								elseif key.KeyCode == Enum.KeyCode.Unknown and not keyHandler.notAllowedMouseInputs[key.UserInputType] then
									bindedKey = key.UserInputType
									library_flags[flag] = bindedKey
									if options.Location then
										options.Location[options.LocationFlag or flag] = bindedKey
									end
									keyName = keyHandler.allowedKeys[bindedKey]
									keybindButton.Text = "[" .. (keyName or bindedKey.Name or tostring(key.KeyCode):gsub("Enum.KeyCode.", "")) .. "]"
									keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
									justBinded = true
									colored_keybindButton_TextColor3[3] = "otherElementText"
									colored_keybindButton_TextColor3[4] = nil
									tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
										TextColor3 = library.colors.otherElementText
									}):Play()
									receivingKey:Disconnect()
									if callback and last_v ~= bindedKey then
										task.spawn(callback, bindedKey, last_v)
									end
									if IsCore then
										delay(0.1, function()
											if IgnoreCoreInputs and (IgnoreCoreInputs == IgnoreKey) then
												IgnoreCoreInputs = nil
											end
										end)
									end
									return
								end
							end
							if key.KeyCode == Enum.KeyCode.Backspace or Enum.KeyCode.Escape == key.KeyCode then
								old_texts, bindedKey = "[NONE]", nil
							end
							keybindButton.Text = old_texts
							colored_keybindButton_TextColor3[3] = "otherElementText"
							colored_keybindButton_TextColor3[4] = nil
							tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								TextColor3 = library.colors.otherElementText
							}):Play()
							receivingKey:Disconnect()
							if callback and last_v ~= bindedKey then
								task.spawn(callback, bindedKey, last_v)
							end
						end)
						library.signals[1 + #library.signals] = receivingKey
					end
					library.signals[1 + #library.signals] = keybindButton.MouseButton1Click:Connect(newkey)
					library.signals[1 + #library.signals] = newKeybind.InputEnded:Connect(function(input)
						if not library.colorpicker and not submenuOpen and input.UserInputType == Enum.UserInputType.MouseButton1 then
							newkey()
						end
					end)
					if presscallback then
						library.signals[1 + #library.signals] = userInputService.InputBegan:Connect(function(key, chatting)
							if not keyHandler.notAllowedKeys[key.KeyCode] and not keyHandler.notAllowedMouseInputs[key.UserInputType] then
								if not justBinded and bindedKey == key.UserInputType or not justBinded and bindedKey == key.KeyCode and not chatting then
									if presscallback then
										task.spawn(presscallback, key, chatting)
									end
								end
								if justBinded then
									justBinded = false
								end
							end
						end)
					end
					local function set(t, key)
						if (nil == key) and (t ~= nil) then
							key = t
						end
						if (key == "nil") or (key == "NONE") or (key == "none") then
							key = nil
						end
						last_v = library_flags[flag]
						bindedKey = key
						library_flags[flag] = key
						if options.Location then
							options.Location[options.LocationFlag or flag] = key
						end
						keyName = (key == nil and "NONE") or keyHandler.allowedKeys[key]
						keybindButton.Text = "[" .. (keyName or key.Name or tostring(key):gsub("Enum.KeyCode.", "")) .. "]"
						keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
						justBinded = true
						colored_keybindButton_TextColor3[3] = "otherElementText"
						colored_keybindButton_TextColor3[4] = nil
						tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							TextColor3 = library.colors.otherElementText
						}):Play()
						if callback and ((last_v ~= key) or options.AllowDuplicateCalls) then
							task.spawn(callback, key, last_v)
						end
						return key
					end
					if presetKeybind ~= nil then
						set(presetKeybind)
					else
						library_flags[flag] = bindedKey
						if options.Location then
							options.Location[options.LocationFlag or flag] = bindedKey
						end
					end
					local default = library_flags[flag]
					local function update()
						keybindName, callback, presscallback = options.Name or keybindName, options.Callback, options.Pressed
						local key = library_flags[flag]
						keyName = (key == nil and "NONE") or keyHandler.allowedKeys[key]
						keybindButton.Text = "[" .. (keyName or key.Name or tostring(key):gsub("Enum.KeyCode.", "")) .. "]"
						keybindButton.Size = UDim2.fromOffset(textToSize(keybindButton).X + 4, 12)
						colored_keybindButton_TextColor3[3] = "otherElementText"
						colored_keybindButton_TextColor3[4] = nil
						tweenService:Create(keybindButton, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							TextColor3 = library.colors.otherElementText
						}):Play()
						keybindHeadline.Text = (keybindName and tostring(keybindName)) or "???"
						return key
					end
					local objectdata = {
						Options = options,
						Name = flag,
						Flag = flag,
						Type = "Keybind",
						Default = default,
						Parent = sectionFunctions,
						Instance = keybindButton,
						Get = function()
							return library_flags[flag]
						end,
						Set = set,
						Remove = function()
							if newKeybind then
								newKeybind.Parent = nil
								sectionFunctions:Update()
							end
						end,
						RawSet = function(t, key)
							if nil == key and t ~= nil then
								key = t
							end
							if key == "nil" or key == "NONE" or key == "none" then
								key = nil
							end
							last_v = library_flags[flag]
							bindedKey = key
							library_flags[flag] = key
							if options.Location then
								options.Location[options.LocationFlag or flag] = key
							end
							justBinded = true
							return key
						end,
						Update = update,
						Reset = function()
							return set(nil, default)
						end
					}
					tabFunctions.Flags[flag], sectionFunctions.Flags[flag], elements[flag] = objectdata, objectdata, objectdata
					return objectdata
				end
				sectionFunctions.NewKeybind = sectionFunctions.AddKeybind
				sectionFunctions.CreateKeybind = sectionFunctions.AddKeybind
				sectionFunctions.Keybind = sectionFunctions.AddKeybind
				sectionFunctions.Bind = sectionFunctions.AddKeybind
				function sectionFunctions:AddLabel(options, ...)
					options = (options and type(options) == "string" and resolvevararg("Label", options, ...)) or options
					local labelName, flag = options.Text or options.Value or options.Name, options.Flag or (function()
						library.unnamedlabels = 1 + (library.unnamedlabels or 0)
						return "Label" .. tostring(library.unnamedlabels)
					end)()
					if elements[flag] ~= nil then
						warn(debug.traceback("Warning! Re-used flag '" .. flag .. "'", 3))
					end
					local newLabel = Instance_new("Frame")
					local labelHeadline = Instance_new("TextLabel")
					local labelPositioner = Instance_new("Frame")
					local labelButton = Instance_new("TextButton")
					newLabel.Name = "newLabel"
					newLabel.Parent = sectionHolder
					newLabel.BackgroundColor3 = Color3.new(1, 1, 1)
					newLabel.BackgroundTransparency = 1
					newLabel.Size = UDim2.new(1, 0, 0, 19)
					labelHeadline.Name = "labelHeadline"
					labelHeadline.Parent = newLabel
					labelHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
					labelHeadline.BackgroundTransparency = 1
					labelHeadline.Position = UDim2.fromScale(0.031, 0.165842161)
					labelHeadline.Size = UDim2.fromOffset(215, 12)
					labelHeadline.Font = Enum.Font.Code
					labelHeadline.Text = (labelName and tostring(labelName)) or "Empty Text"
					labelHeadline.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {labelHeadline, "TextColor3", "elementText"}
					labelHeadline.TextSize = 14
					labelHeadline.TextXAlignment = Enum.TextXAlignment.Left
					labelPositioner.Name = "labelPositioner"
					labelPositioner.Parent = newLabel
					labelPositioner.BackgroundColor3 = Color3.new(1, 1, 1)
					labelPositioner.BackgroundTransparency = 1
					labelPositioner.Position = UDim2.new(0.00448430516)
					labelPositioner.Size = UDim2.fromOffset(214, 19)
					sectionFunctions:Update()
					local function set(t, str)
						if nil == str and t ~= nil then
							str = t
						end
						labelHeadline.Text = (nil ~= str and tostring(str)) or "Empty Text"
						return str
					end
					local default = labelHeadline.Text
					local objectdata = {
						Options = options,
						Name = flag,
						Flag = flag,
						Type = "Label",
						Default = default,
						Parent = sectionFunctions,
						Instance = labelHeadline,
						Get = function()
							return labelHeadline.Text, labelHeadline
						end,
						Set = set,
						SetText = set,
						Remove = function()
							if newLabel then
								newLabel.Parent = nil
								sectionFunctions:Update()
							end
						end,
						RawSet = set,
						Update = function()
							return labelHeadline.Text
						end,
						Reset = function()
							return set(nil, default)
						end
					}
					tabFunctions.Flags[flag], sectionFunctions.Flags[flag], elements[flag] = objectdata, objectdata, objectdata
					return objectdata
				end
				sectionFunctions.NewLabel = sectionFunctions.AddLabel
				sectionFunctions.CreateLabel = sectionFunctions.AddLabel
				sectionFunctions.Label = sectionFunctions.AddLabel
				sectionFunctions.Text = sectionFunctions.AddLabel
				function sectionFunctions:AddSlider(options, ...)
					options = (options and type(options) == "string" and resolvevararg("Slider", options, ...)) or options
					local sliderName, maxValue, minValue, presetValue, callback, flagName = assert(options.Name, "Missing Name for new slider."), assert(options.Max, "Missing Max for new slider."), assert(options.Min, "Missing Min for new slider."), options.Value, options.Callback, options.Flag or (function()
						library.unnamedsliders = 1 + (library.unnamedsliders or 0)
						return "Slider" .. tostring(library.unnamedsliders)
					end)()
					if elements[flagName] ~= nil then
						warn(debug.traceback("Warning! Re-used flag '" .. flagName .. "'", 3))
					end
					local decimalprecision = tonumber(options.Decimals or options.Precision or options.Precise)
					if not decimalprecision and options.Max - options.Min <= 1 then
						decimalprecision = 1
					end
					if decimalprecision then
						decimalprecision = math.clamp(decimalprecision, 0, 99)
						if decimalprecision <= 0 then
							decimalprecision = nil
						end
						decimalprecision = tostring(decimalprecision)
					end
					local formattyp = options.Format and type(options.Format)
					local function resolvedisplay(val, was)
						local str = nil
						if decimalprecision then
							str = string.format("%0." .. decimalprecision .. "f", val)
						end
						str = str or tostring(val)
						if formattyp == "string" then
							return string.format(options.Format, val)
						elseif formattyp == "function" then
							local oof, g = pcall(options.Format, val, was)
							if not oof or not g then
								warn("Your format function for", sliderName, "Slider:", flagName, "has returned nothing. It should return a string to display.", debug.traceback(""))
								return "Format Function Errored"
							end
							return tostring(g)
						end
						return (sliderName or "???") .. ": " .. str
					end
					local usetextbox = options.Textbox or options.InputBox or options.CustomInput
					local newSlider = Instance_new("Frame")
					local slider = Instance_new("ImageLabel")
					local sliderInner = Instance_new("ImageLabel")
					local sliderColored = Instance_new("ImageLabel")
					local sliderHeadline = Instance_new("TextLabel")
					local startingValue = presetValue or minValue
					local sliderDragging = false
					newSlider.Name = "newSlider"
					newSlider.Parent = sectionHolder
					newSlider.BackgroundColor3 = Color3.new(1, 1, 1)
					newSlider.BackgroundTransparency = 1
					newSlider.Size = UDim2.new(1, 0, 0, 42)
					slider.Name = "slider"
					slider.Parent = newSlider
					slider.Active = true
					slider.BackgroundColor3 = library.colors.topGradient
					local colored_slider_BackgroundColor3 = {slider, "BackgroundColor3", "topGradient"}
					colored[1 + #colored] = colored_slider_BackgroundColor3
					slider.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {slider, "BorderColor3", "elementBorder"}
					slider.Position = UDim2.fromScale(0.031, 0.48)
					slider.Selectable = true
					slider.Size = (usetextbox and UDim2.fromOffset(156, 18)) or UDim2.fromOffset(206, 18)
					slider.Image = "rbxassetid://2454009026"
					slider.ImageColor3 = library.colors.bottomGradient
					local colored_slider_ImageColor3 = {slider, "ImageColor3", "bottomGradient"}
					colored[1 + #colored] = colored_slider_ImageColor3
					sliderInner.Name = "sliderInner"
					sliderInner.Parent = slider
					sliderInner.Active = true
					sliderInner.AnchorPoint = Vector2.new(0.5, 0.5)
					sliderInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {sliderInner, "BackgroundColor3", "topGradient"}
					sliderInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {sliderInner, "BorderColor3", "elementBorder"}
					sliderInner.Position = UDim2.fromScale(0.5, 0.5)
					sliderInner.Selectable = true
					sliderInner.Size = UDim2.new(1, -4, 1, -4)
					sliderInner.Image = "rbxassetid://2454009026"
					sliderInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {sliderInner, "ImageColor3", "bottomGradient"}
					sliderColored.Name = "sliderColored"
					sliderColored.Parent = sliderInner
					sliderColored.Active = true
					sliderColored.BackgroundColor3 = darkenColor(library.colors.main, 1.5)
					colored[1 + #colored] = {sliderColored, "BackgroundColor3", "main", 1.5}
					sliderColored.BorderSizePixel = 0
					sliderColored.Selectable = true
					sliderColored.Size = UDim2.fromScale(((startingValue or minValue) - minValue) / (maxValue - minValue), 1)
					sliderColored.Image = "rbxassetid://2454009026"
					sliderColored.ImageColor3 = darkenColor(library.colors.main, 2.5)
					colored[1 + #colored] = {sliderColored, "ImageColor3", "main", 2.5}
					sliderHeadline.Name = "sliderHeadline"
					sliderHeadline.Parent = newSlider
					sliderHeadline.Active = true
					sliderHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
					sliderHeadline.BackgroundTransparency = 1
					sliderHeadline.Position = UDim2.new(0.031)
					sliderHeadline.Selectable = true
					sliderHeadline.Size = UDim2.fromOffset(206, 20)
					sliderHeadline.ZIndex = 5
					sliderHeadline.Font = Enum.Font.Code
					sliderHeadline.LineHeight = 1.15
					sliderHeadline.Text = resolvedisplay(startingValue)
					sliderHeadline.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {sliderHeadline, "TextColor3", "elementText"}
					sliderHeadline.TextSize = 14
					sliderHeadline.TextXAlignment = Enum.TextXAlignment.Left
					local realTextbox = nil
					local function Set(t, newValue)
						if nil == newValue and t ~= nil then
							newValue = t
						end
						minValue, maxValue = options.Min, options.Max
						if newValue and (options.IllegalInput or ((not minValue or newValue >= minValue) and (not maxValue or newValue <= maxValue))) then
							local last_val = library_flags[flagName]
							library_flags[flagName] = newValue
							if options.Location then
								options.Location[options.LocationFlag or flagName] = newValue
							end
							do
								local newValue = (options.IllegalInput and math.clamp(newValue, minValue or -math.huge, maxValue or math.huge)) or newValue
								tweenService:Create(sliderColored, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
									Size = UDim2.fromScale(((newValue or minValue) - minValue) / (maxValue - minValue), 1)
								}):Play()
							end
							sliderHeadline.Text = resolvedisplay(newValue, last_val)
							if usetextbox and realTextbox then
								realTextbox.Text = tostring(newValue)
							end
							if callback and (last_val ~= newValue or options.AllowDuplicateCalls) then
								task.spawn(callback, newValue, last_val)
							end
						end
						return newValue
					end
					if presetValue ~= nil then
						Set(presetValue)
					else
						library_flags[flagName] = startingValue
						if options.Location then
							options.Location[options.LocationFlag or flagName] = startingValue
						end
					end
					if usetextbox then
						if type(usetextbox) ~= "table" then
							usetextbox = options
						end
						local textbox = Instance_new("ImageLabel")
						local textboxInner = Instance_new("ImageLabel")
						realTextbox = Instance_new("TextBox")
						textbox.Name = "textbox"
						textbox.Parent = newSlider
						textbox.Active = true
						textbox.BackgroundColor3 = library.colors.topGradient
						local colored_textbox_BackgroundColor3 = {textbox, "BackgroundColor3", "topGradient"}
						colored[1 + #colored] = colored_textbox_BackgroundColor3
						textbox.BorderColor3 = library.colors.elementBorder
						colored[1 + #colored] = {textbox, "BorderColor3", "elementBorder"}
						textbox.Position = UDim2.new(1, -54, 0.48)
						textbox.Selectable = true
						textbox.Size = UDim2.fromOffset(43, 18)
						textbox.Image = "rbxassetid://2454009026"
						textbox.ImageColor3 = library.colors.bottomGradient
						local colored_textbox_ImageColor3 = {textbox, "ImageColor3", "bottomGradient"}
						colored[1 + #colored] = colored_textbox_ImageColor3
						textboxInner.Name = "textboxInner"
						textboxInner.Parent = textbox
						textboxInner.Active = true
						textboxInner.AnchorPoint = Vector2.new(0.5, 0.5)
						textboxInner.BackgroundColor3 = library.colors.topGradient
						colored[1 + #colored] = {textboxInner, "BackgroundColor3", "topGradient"}
						textboxInner.BorderColor3 = library.colors.elementBorder
						colored[1 + #colored] = {textboxInner, "BorderColor3", "elementBorder"}
						textboxInner.Position = UDim2.fromScale(0.5, 0.5)
						textboxInner.Selectable = true
						textboxInner.Size = UDim2.new(1, -4, 1, -4)
						textboxInner.Image = "rbxassetid://2454009026"
						textboxInner.ImageColor3 = library.colors.bottomGradient
						colored[1 + #colored] = {textboxInner, "ImageColor3", "bottomGradient"}
						realTextbox.Name = "realTextbox"
						realTextbox.Parent = textbox
						realTextbox.BackgroundColor3 = Color3.new(1, 1, 1)
						realTextbox.BackgroundTransparency = 1
						realTextbox.Position = UDim2.new(0.0295485705)
						realTextbox.Size = UDim2.fromScale(0.97, 1)
						realTextbox.ZIndex = 5
						realTextbox.ClearTextOnFocus = false
						realTextbox.Font = Enum.Font.Code
						realTextbox.LineHeight = 1.15
						realTextbox.Text = tostring(presetValue)
						realTextbox.TextColor3 = library.colors.otherElementText
						colored[1 + #colored] = {realTextbox, "TextColor3", "otherElementText"}
						realTextbox.TextSize = 14
						realTextbox.PlaceholderText = (presetValue ~= nil and tostring(presetValue)) or ""
						library.signals[1 + #library.signals] = realTextbox.FocusLost:Connect(function()
							local val = realTextbox.Text
							if usetextbox.PreFormat then
								local typ = type(usetextbox.PreFormat)
								if typ == "function" then
									local x, e = pcall(usetextbox.PreFormat, val)
									if not x and e then
										warn("Error in Pre-Format (Textbox " .. flagName .. "):", e)
									else
										val = e
									end
								end
							end
							val = (not usetextbox.Hex and not usetextbox.Binary and not usetextbox.Base and (tonumber(val) or tonumber(val:gsub("%D", ""), 10) or 0)) or tonumber(val, (usetextbox.Hex and 16) or (usetextbox.Binary and 2) or usetextbox.Base or 10) or 0
							if not options.IllegalInput and (options.Max or options.Min) then
								val = math.clamp(val, options.Min or -math.huge, options.Max or math.huge)
							end
							local decimalprecision = tonumber(options.Decimals or options.Precision or options.Precise)
							if decimalprecision then
								val = tonumber(string.format("%0." .. tostring(decimalprecision) .. "f", val))
							end
							if usetextbox.PostFormat then
								local typ = type(usetextbox.PostFormat)
								if typ == "function" then
									local x, e = pcall(usetextbox.PostFormat, val)
									if not x and e then
										warn("Error in Post-Format (Textbox " .. flagName .. "):", e)
									else
										val = e
									end
								end
							end
							Set((val and tonumber(val)) or presetValue or 0)
						end)
						library.signals[1 + #library.signals] = textbox.MouseEnter:Connect(function()
							colored_textbox_BackgroundColor3[3] = "main"
							colored_textbox_BackgroundColor3[4] = 1.5
							colored_textbox_ImageColor3[3] = "main"
							colored_textbox_ImageColor3[4] = 2.5
							tweenService:Create(textbox, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = darkenColor(library.colors.main, 1.5),
								ImageColor3 = darkenColor(library.colors.main, 2.5)
							}):Play()
						end)
						library.signals[1 + #library.signals] = textbox.MouseLeave:Connect(function()
							colored_textbox_BackgroundColor3[3] = "topGradient"
							colored_textbox_BackgroundColor3[4] = nil
							colored_textbox_ImageColor3[3] = "bottomGradient"
							colored_textbox_ImageColor3[4] = nil
							tweenService:Create(textbox, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = library.colors.topGradient,
								ImageColor3 = library.colors.bottomGradient
							}):Play()
						end)
					end
					sectionFunctions:Update()
					library.signals[1 + #library.signals] = slider.MouseEnter:Connect(function()
						colored_slider_BackgroundColor3[3] = "main"
						colored_slider_BackgroundColor3[4] = 1.5
						colored_slider_ImageColor3[3] = "main"
						colored_slider_ImageColor3[4] = 2.5
						tweenService:Create(slider, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = darkenColor(library.colors.main, 1.5),
							ImageColor3 = darkenColor(library.colors.main, 2.5)
						}):Play()
					end)
					library.signals[1 + #library.signals] = slider.MouseLeave:Connect(function()
						colored_slider_BackgroundColor3[3] = "topGradient"
						colored_slider_BackgroundColor3[4] = nil
						colored_slider_ImageColor3[3] = "bottomGradient"
						colored_slider_ImageColor3[4] = nil
						tweenService:Create(slider, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = library.colors.topGradient,
							ImageColor3 = library.colors.bottomGradient
						}):Play()
					end)
					local function sliding(input, sb, sc)
						local last_val = library_flags[flagName]
						local pos = UDim2.fromScale(math.clamp((input.Position.X - sb.AbsolutePosition.X) / sb.AbsoluteSize.X, 0, 1), 1)
						tweenService:Create(sc, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
							Size = pos
						}):Play()
						local sliderValue = nil
						if decimalprecision then
							sliderValue = tonumber(string.format("%0." .. decimalprecision .. "f", ((pos.X.Scale * maxValue) / maxValue) * (maxValue - minValue) + minValue))
						end
						sliderValue = sliderValue or tonumber(string.format("%0.2f", (floor(((pos.X.Scale * maxValue) / maxValue) * (maxValue - minValue) + minValue))))
						library_flags[flagName] = sliderValue
						if options.Location then
							options.Location[options.LocationFlag or flagName] = sliderValue
						end
						sliderHeadline.Text = resolvedisplay(sliderValue, last_val)
						if usetextbox and realTextbox then
							realTextbox.Text = tostring(sliderValue)
						end
						if callback and last_val ~= sliderValue then
							task.spawn(callback, sliderValue, last_val)
						end
						last_val = sliderValue
					end
					library.signals[1 + #library.signals] = newSlider.InputBegan:Connect(function(input)
						if not library.colorpicker and input.UserInputType == Enum.UserInputType.MouseButton1 then
							sliderDragging = true
							isDraggingSomething = true
						end
					end)
					library.signals[1 + #library.signals] = newSlider.InputEnded:Connect(function(input)
						if not library.colorpicker and input.UserInputType == Enum.UserInputType.MouseButton1 then
							sliderDragging = false
							isDraggingSomething = false
						end
					end)
					library.signals[1 + #library.signals] = newSlider.InputBegan:Connect(function(input)
						if not library.colorpicker and not isDraggingSomething and input.UserInputType == Enum.UserInputType.MouseButton1 then
							isDraggingSomething = true
							sliding(input, sliderInner, sliderColored)
						end
					end)
					library.signals[1 + #library.signals] = userInputService.InputChanged:Connect(function(input)
						if not library.colorpicker and sliderDragging and input.UserInputType == Enum.UserInputType.MouseMovement then
							sliding(input, sliderInner, sliderColored)
						end
					end)
					local default = library_flags[flagName]
					local function Update(t, last_val)
						if last_val == nil and t ~= nil and type(t) ~= "table" then
							last_val = t
						end
						sliderName, maxValue, minValue, callback = options.Name or sliderName, options.Max or maxValue, options.Min or minValue, options.Callback
						local newValue = library_flags[flagName]
						do
							local newValue = math.clamp(newValue, options.Min or -math.huge, options.Max or math.huge)
							tweenService:Create(sliderColored, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
								Size = UDim2.fromScale(((newValue or minValue) - minValue) / (maxValue - minValue), 1)
							}):Play()
						end
						sliderHeadline.Text = resolvedisplay(newValue, last_val)
						if usetextbox and realTextbox then
							realTextbox.Text = tostring(newValue)
						end
						return newValue
					end
					local objectdata = {
						Options = options,
						Name = flagName,
						Flag = flagName,
						Type = "Slider",
						Default = default,
						Parent = sectionFunctions,
						Instance = sliderHeadline,
						Set = Set,
						Get = function()
							return library_flags[flagName]
						end,
						SetConstraints = function(t, min, max)
							if t and type(t) ~= "table" then
								min, max = t, min
							end
							if min then
								options.Min = min
							end
							if max then
								options.Max = max
							end
							Update()
						end,
						SetMin = function(t, min)
							if min == nil and t ~= nil then
								min = t
							end
							if min and min ~= options.Min then
								options.Min = min
								Update()
							end
						end,
						SetMax = function(t, max)
							if max == nil and t ~= nil then
								max = t
							end
							if max and max ~= options.Max then
								options.Max = max
								Update()
							end
						end,
						Update = Update,
						Remove = function()
							if newSlider then
								newSlider.Parent = nil
								sectionFunctions:Update()
							end
						end,
						RawSet = function(t, newValue)
							if nil == newValue and t ~= nil then
								newValue = t
							end
							local last_val = library_flags[flagName]
							library_flags[flagName] = newValue
							if options.Location then
								options.Location[options.LocationFlag or flagName] = newValue
							end
							Update(nil, last_val)
						end,
						Reset = function()
							return Set(nil, default)
						end
					}
					tabFunctions.Flags[flagName], sectionFunctions.Flags[flagName], elements[flagName] = objectdata, objectdata, objectdata
					return objectdata
				end
				sectionFunctions.NewSlider = sectionFunctions.AddSlider
				sectionFunctions.CreateSlider = sectionFunctions.AddSlider
				sectionFunctions.NumberConstraint = sectionFunctions.AddSlider
				sectionFunctions.Slider = sectionFunctions.AddSlider
				sectionFunctions.Slide = sectionFunctions.AddSlider
				function sectionFunctions:AddSearchBox(options, ...)
					options = (options and type(options) == "string" and resolvevararg("SearchBox", options, ...)) or options
					local dropdownName, listt, val, callback, flagName = assert(options.Name, "Missing Name for new searchbox."), assert(options.List, "Missing List for new searchbox."), options.Value, options.Callback, options.Flag or (function()
						library.unnamedsearchbox = 1 + (library.unnamedsearchbox or 0)
						return "SearchBox" .. tostring(library.unnamedsearchbox)
					end)()
					if elements[flagName] ~= nil then
						warn(debug.traceback("Warning! Re-used flag '" .. flagName .. "'", 3))
					end
					local newDropdown = Instance_new("Frame")
					local dropdown = Instance_new("ImageLabel")
					local dropdownInner = Instance_new("ImageLabel")
					local dropdownToggle = Instance_new("ImageButton")
					local dropdownSelection = Instance_new("TextBox")
					local dropdownHeadline = Instance_new("TextLabel")
					local dropdownHolderFrame = Instance_new("ImageLabel")
					local dropdownHolderInner = Instance_new("ImageLabel")
					local realDropdownHolder = Instance_new("ScrollingFrame")
					local realDropdownHolderList = Instance_new("UIListLayout")
					local dropdownEnabled = false
					local resolvelist = getresolver(listt, options.Filter)
					local list = resolvelist()
					local multiselect = options.MultiSelect or options.Multi or options.Multiple
					local passed_multiselect = multiselect and type(multiselect)
					local blankstring = not multiselect and (options.BlankValue or options.NoValueString or options.Nothing)
					local selectedOption = val or list[1] or blankstring
					local addcallback = options.ItemAdded or options.AddedCallback
					local delcallback = options.ItemRemoved or options.RemovedCallback
					local clrcallback = options.ItemsCleared or options.ClearedCallback
					local modcallback = options.ItemChanged or options.ChangedCallback
					if blankstring and val == nil then
						val = blankstring
					end
					if val ~= nil then
						selectedOption = val
					end
					if multiselect and (not selectedOption or type(selectedOption) ~= "table") then
						selectedOption = {}
					end
					local selectedObjects = {}
					local optionCount = 0
					newDropdown.Name = "newDropdown"
					newDropdown.Parent = sectionHolder
					newDropdown.BackgroundColor3 = Color3.new(1, 1, 1)
					newDropdown.BackgroundTransparency = 1
					newDropdown.Size = UDim2.new(1, 0, 0, 42)
					dropdown.Name = "dropdown"
					dropdown.Parent = newDropdown
					dropdown.Active = true
					dropdown.BackgroundColor3 = library.colors.topGradient
					local colored_dropdown_BackgroundColor3 = {dropdown, "BackgroundColor3", "topGradient"}
					colored[1 + #colored] = colored_dropdown_BackgroundColor3
					dropdown.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {dropdown, "BorderColor3", "elementBorder"}
					dropdown.Position = UDim2.fromScale(0.027, 0.45)
					dropdown.Selectable = true
					dropdown.Size = UDim2.fromOffset(206, 18)
					dropdown.Image = "rbxassetid://2454009026"
					dropdown.ImageColor3 = library.colors.bottomGradient
					local colored_dropdown_ImageColor3 = {dropdown, "ImageColor3", "bottomGradient"}
					colored[1 + #colored] = colored_dropdown_ImageColor3
					dropdownInner.Name = "dropdownInner"
					dropdownInner.Parent = dropdown
					dropdownInner.Active = true
					dropdownInner.AnchorPoint = Vector2.new(0.5, 0.5)
					dropdownInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {dropdownInner, "BackgroundColor3", "topGradient"}
					dropdownInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {dropdownInner, "BorderColor3", "elementBorder"}
					dropdownInner.Position = UDim2.fromScale(0.5, 0.5)
					dropdownInner.Selectable = true
					dropdownInner.Size = UDim2.new(1, -4, 1, -4)
					dropdownInner.Image = "rbxassetid://2454009026"
					dropdownInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {dropdownInner, "ImageColor3", "bottomGradient"}
					dropdownToggle.Name = "dropdownToggle"
					dropdownToggle.Parent = dropdown
					dropdownToggle.BackgroundColor3 = Color3.new(1, 1, 1)
					dropdownToggle.BackgroundTransparency = 1
					dropdownToggle.Position = UDim2.fromScale(0.9, 0.17)
					dropdownToggle.Rotation = 90
					dropdownToggle.Size = UDim2.fromOffset(12, 12)
					dropdownToggle.ZIndex = 6
					dropdownToggle.Image = "rbxassetid://71659683"
					dropdownToggle.ImageColor3 = Color3.fromRGB(171, 171, 171)
					dropdownSelection.Name = "dropdownSelection"
					dropdownSelection.Parent = dropdown
					dropdownSelection.BackgroundColor3 = Color3.new(1, 1, 1)
					dropdownSelection.BackgroundTransparency = 1
					dropdownSelection.Position = UDim2.new(0.0295485705)
					dropdownSelection.Size = UDim2.fromScale(0.85, 1)
					dropdownSelection.ZIndex = 5
					dropdownSelection.Font = Enum.Font.Code
					dropdownSelection.LineHeight = 1.15
					dropdownSelection.Text = (passed_multiselect == "string" and multiselect) or tostring((multiselect and (blankstring or "Select Item(s)")) or (selectedOption and tostring(selectedOption)) or blankstring or "No Blank String")
					dropdownSelection.TextColor3 = library.colors.otherElementText
					colored[1 + #colored] = {dropdownSelection, "TextColor3", "otherElementText"}
					dropdownSelection.TextSize = 14
					dropdownSelection.TextXAlignment = Enum.TextXAlignment.Left
					dropdownSelection.ClearTextOnFocus = true
					dropdownHeadline.Name = "dropdownHeadline"
					dropdownHeadline.Parent = newDropdown
					dropdownHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
					dropdownHeadline.BackgroundTransparency = 1
					dropdownHeadline.Position = UDim2.fromScale(0.034, 0.03)
					dropdownHeadline.Size = UDim2.fromOffset(167, 11)
					dropdownHeadline.Font = Enum.Font.Code
					dropdownHeadline.Text = (dropdownName and tostring(dropdownName)) or "???"
					dropdownHeadline.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {dropdownHeadline, "TextColor3", "elementText"}
					dropdownHeadline.TextSize = 14
					dropdownHeadline.TextXAlignment = Enum.TextXAlignment.Left
					dropdownHolderFrame.Name = "dropdownHolderFrame"
					dropdownHolderFrame.Parent = newDropdown
					dropdownHolderFrame.Active = true
					dropdownHolderFrame.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {dropdownHolderFrame, "BackgroundColor3", "topGradient"}
					dropdownHolderFrame.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {dropdownHolderFrame, "BorderColor3", "elementBorder"}
					dropdownHolderFrame.Position = UDim2.fromScale(0.025, 1.012)
					dropdownHolderFrame.Selectable = true
					dropdownHolderFrame.Size = UDim2.fromOffset(206, 22)
					dropdownHolderFrame.Visible = false
					dropdownHolderFrame.Image = "rbxassetid://2454009026"
					dropdownHolderFrame.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {dropdownHolderFrame, "ImageColor3", "bottomGradient"}
					dropdownHolderInner.Name = "dropdownHolderInner"
					dropdownHolderInner.Parent = dropdownHolderFrame
					dropdownHolderInner.Active = true
					dropdownHolderInner.AnchorPoint = Vector2.new(0.5, 0.5)
					dropdownHolderInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {dropdownHolderInner, "BackgroundColor3", "topGradient"}
					dropdownHolderInner.BorderColor3 = library.colors.elementBorder
					dropdownHolderInner.Position = UDim2.fromScale(0.5, 0.5)
					dropdownHolderInner.Selectable = true
					dropdownHolderInner.Size = UDim2.new(1, -4, 1, -4)
					dropdownHolderInner.Image = "rbxassetid://2454009026"
					dropdownHolderInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {dropdownHolderInner, "ImageColor3", "bottomGradient"}
					realDropdownHolder.Name = "realDropdownHolder"
					realDropdownHolder.Parent = dropdownHolderInner
					realDropdownHolder.BackgroundColor3 = Color3.new(1, 1, 1)
					realDropdownHolder.BackgroundTransparency = 1
					realDropdownHolder.Selectable = false
					realDropdownHolder.Size = UDim2.fromScale(1, 1)
					realDropdownHolder.CanvasSize = UDim2.new()
					realDropdownHolder.ScrollBarThickness = 5
					realDropdownHolder.ScrollingDirection = Enum.ScrollingDirection.Y
					realDropdownHolder.AutomaticCanvasSize = Enum.AutomaticSize.Y
					realDropdownHolder.ScrollBarImageTransparency = 0.5
					realDropdownHolder.ScrollBarImageColor3 = library.colors.section
					colored[1 + #colored] = {realDropdownHolder, "ScrollBarImageColor3", "section"}
					realDropdownHolderList.Name = "realDropdownHolderList"
					realDropdownHolderList.Parent = realDropdownHolder
					realDropdownHolderList.HorizontalAlignment = Enum.HorizontalAlignment.Center
					realDropdownHolderList.SortOrder = Enum.SortOrder.LayoutOrder
					sectionFunctions:Update()
					local restorezindex = {}
					library.signals[1 + #library.signals] = newDropdown.MouseEnter:Connect(function()
						colored_dropdown_BackgroundColor3[3] = "main"
						colored_dropdown_BackgroundColor3[4] = 1.5
						colored_dropdown_ImageColor3[3] = "main"
						colored_dropdown_ImageColor3[4] = 2.5
						tweenService:Create(dropdown, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = darkenColor(library.colors.main, 1.5),
							ImageColor3 = darkenColor(library.colors.main, 2.5)
						}):Play()
					end)
					library.signals[1 + #library.signals] = newDropdown.MouseLeave:Connect(function()
						if not dropdownEnabled then
							colored_dropdown_BackgroundColor3[3] = "topGradient"
							colored_dropdown_BackgroundColor3[4] = nil
							colored_dropdown_ImageColor3[3] = "bottomGradient"
							colored_dropdown_ImageColor3[4] = nil
							tweenService:Create(dropdown, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = library.colors.topGradient,
								ImageColor3 = library.colors.bottomGradient
							}):Play()
						end
					end)
					local function UpdateDropdownHolder()
						if optionCount >= 6 then
							realDropdownHolder.CanvasSize = UDim2:fromOffset(realDropdownHolderList.AbsoluteContentSize.Y + 2)
						elseif optionCount <= 5 then
							dropdownHolderFrame.Size = UDim2.fromOffset(206, realDropdownHolderList.AbsoluteContentSize.Y + 4)
						end
					end
					local function AddOptions(optionsTable, filter)
						if options.Sort then
							local didstuff, dosort = nil, options.Sort
							if type(dosort) == "function" then
								local g, h = pcall(table.sort, optionsTable, dosort)
								if g then
									didstuff = true
								elseif h then
									warn("Error sorting list:", h, debug.traceback(""))
								end
							end
							if not didstuff then
								table.sort(optionsTable, library.defaultSort)
							end
						end
						if blankstring and (optionsTable[1] ~= blankstring or table.find(optionsTable, blankstring, 2)) then
							local exists = table.find(optionsTable, blankstring)
							if exists then
								for _ = 1, 35 do
									table.remove(optionsTable, exists)
									exists = table.find(optionsTable, blankstring)
									if not exists then
										break
									end
								end
							end
							table.insert(optionsTable, 1, blankstring)
						end
						optionCount = 0
						realDropdownHolderList.Parent = nil
						realDropdownHolder:ClearAllChildren()
						realDropdownHolderList.Parent = realDropdownHolder
						for _, v in next, optionsTable do
							if not filter or tostring(v):lower():find(dropdownSelection.Text:lower(), 1, not options.RegEx) then
								optionCount = optionCount + 1
								UpdateDropdownHolder()
								local newOption = Instance_new("ImageLabel")
								local optionButton = Instance_new("TextButton")
								if selectedOption == v then
									selectedObjects[1] = newOption
									selectedObjects[2] = optionButton
								end
								newOption.Name = "Frame"
								newOption.Parent = realDropdownHolder
								local togged = (not multiselect and selectedOption == v) or (multiselect and table.find(selectedOption, v))
								newOption.BackgroundColor3 = (togged and library.colors.selectedOption) or library.colors.topGradient
								newOption.BorderSizePixel = 0
								newOption.Size = UDim2.fromOffset(202, 18)
								newOption.Image = "rbxassetid://2454009026"
								newOption.ImageColor3 = (togged and library.colors.unselectedOption) or library.colors.bottomGradient
								local stringed = tostring(v)
								optionButton.Name = stringed
								optionButton.Parent = newOption
								optionButton.Active = true
								optionButton.AnchorPoint = Vector2.new(0.5, 0.5)
								optionButton.BackgroundColor3 = Color3.new(1, 1, 1)
								optionButton.BackgroundTransparency = 1
								optionButton.Position = UDim2.fromScale(0.5, 0.5)
								optionButton.Selectable = true
								optionButton.Size = UDim2.new(1, -10, 1)
								optionButton.ZIndex = 5
								optionButton.Font = Enum.Font.Code
								optionButton.Text = (togged and (" " .. stringed)) or stringed
								optionButton.TextColor3 = (togged and library.colors.main) or library.colors.otherElementText
								optionButton.TextSize = 14
								optionButton.TextXAlignment = Enum.TextXAlignment.Left
								library.signals[1 + #library.signals] = optionButton[(multiselect and "MouseButton1Click") or "MouseButton1Down"]:Connect(function()
									if not library.colorpicker then
										dropdownSelection.Text = (passed_multiselect == "string" and multiselect) or tostring(blankstring or "Select Item(s)")
										restorezindex[newSection] = restorezindex[newSection] or newSection.ZIndex
										restorezindex[newDropdown] = restorezindex[newDropdown] or newDropdown.ZIndex
										restorezindex[sectionHolder] = restorezindex[sectionHolder] or sectionHolder.ZIndex
										if multiselect then
											local cloned = {unpack(selectedOption)}
											local togged = table.find(selectedOption, v)
											if togged then
												table.remove(selectedOption, togged)
											else
												selectedOption[1 + #selectedOption] = v
											end
											togged = table.find(selectedOption, v)
											optionButton.Text = (togged and (" " .. stringed)) or stringed
											newOption.BackgroundColor3 = (togged and library.colors.selectedOption) or library.colors.topGradient
											newOption.ImageColor3 = (togged and library.colors.unselectedOption) or library.colors.bottomGradient
											optionButton.TextColor3 = (togged and library.colors.main) or library.colors.otherElementText
											if callback then
												task.spawn(callback, selectedOption, cloned)
											end
											if togged then
												if addcallback then
													task.spawn(addcallback, v, selectedOption)
												end
											elseif delcallback then
												task.spawn(delcallback, v, selectedOption)
											end
											if modcallback then
												task.spawn(modcallback, v, togged, selectedOption)
											end
											if #selectedOption == 0 and clrcallback then
												task.spawn(clrcallback, selectedOption, cloned)
											end
											return
										else
											dropdownSelection.Text = stringed
											if selectedOption ~= v then
												local last_v = library_flags[flagName]
												selectedObjects[1].BackgroundColor3 = library.colors.topGradient
												selectedObjects[1].ImageColor3 = library.colors.bottomGradient
												selectedObjects[2].Text = selectedObjects[2].Name
												selectedObjects[2].TextColor3 = library.colors.otherElementText
												selectedOption = v
												selectedObjects[1] = newOption
												selectedObjects[2] = optionButton
												newOption.BackgroundColor3 = library.colors.selectedOption
												newOption.ImageColor3 = library.colors.unselectedOption
												optionButton.TextColor3 = library.colors.main
												dropdownHolderFrame.Visible = false
												dropdownToggle.Rotation = 90
												dropdownEnabled = false
												newDropdown.ZIndex = 1
												colored_dropdown_BackgroundColor3[3] = "topGradient"
												colored_dropdown_BackgroundColor3[4] = nil
												colored_dropdown_ImageColor3[3] = "bottomGradient"
												colored_dropdown_ImageColor3[4] = nil
												tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
													BackgroundColor3 = library.colors.topGradient,
													ImageColor3 = library.colors.bottomGradient
												}):Play()
												library_flags[flagName] = selectedOption
												if options.Location then
													options.Location[options.LocationFlag or flagName] = selectedOption
												end
												dropdownSelection.Text = tostring(selectedOption)
												if submenuOpen then
													submenuOpen = nil
												end
												if callback then
													task.spawn(callback, selectedOption, last_v)
												end
											else
												submenuOpen = nil
												dropdownToggle.Rotation = 90
												colored_dropdown_BackgroundColor3[3] = "topGradient"
												colored_dropdown_BackgroundColor3[4] = nil
												colored_dropdown_ImageColor3[3] = "bottomGradient"
												colored_dropdown_ImageColor3[4] = nil
												tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
													BackgroundColor3 = library.colors.topGradient,
													ImageColor3 = library.colors.bottomGradient
												}):Play()
												dropdownHolderFrame.Visible = false
											end
										end
										for ins, z in next, restorezindex do
											ins.ZIndex = z
										end
									end
								end)
								library.signals[1 + #library.signals] = optionButton.MouseEnter:Connect(function()
									tweenService:Create(newOption, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
										BackgroundColor3 = library.colors.hoveredOptionTop,
										ImageColor3 = library.colors.hoveredOptionBottom
									}):Play()
								end)
								library.signals[1 + #library.signals] = optionButton.MouseLeave:Connect(function()
									local togged = (not multiselect and selectedOption == v) or (multiselect and table.find(selectedOption, v))
									tweenService:Create(newOption, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
										BackgroundColor3 = (togged and library.colors.selectedOption) or library.colors.topGradient,
										ImageColor3 = (togged and library.colors.unselectedOption) or library.colors.bottomGradient
									}):Play()
								end)
								UpdateDropdownHolder()
							end
						end
					end
					local precisionscrolling, update = nil
					local showing = false
					local function display(dropdownEnabled, f)
						if submenuOpen == dropdown or submenuOpen == nil then
							if dropdownEnabled then
								list = resolvelist()
								AddOptions(list, f)
								submenuOpen = dropdown
								dropdownToggle.Rotation = 270
								restorezindex[newSection] = restorezindex[newSection] or newSection.ZIndex
								restorezindex[newDropdown] = restorezindex[newDropdown] or newDropdown.ZIndex
								restorezindex[sectionHolder] = restorezindex[sectionHolder] or sectionHolder.ZIndex
								newSection.ZIndex = 50 + newSection.ZIndex
								newDropdown.ZIndex = 2
								sectionHolder.ZIndex = 2
								colored_dropdown_BackgroundColor3[3] = "main"
								colored_dropdown_BackgroundColor3[4] = 1.5
								colored_dropdown_ImageColor3[3] = "main"
								colored_dropdown_ImageColor3[4] = 2.5
								tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
									BackgroundColor3 = darkenColor(library.colors.main, 1.5),
									ImageColor3 = darkenColor(library.colors.main, 2.5)
								}):Play()
								dropdownHolderFrame.Visible = true
								if not options.DisablePrecisionScrolling then
									local scrollrate = tonumber(options.ScrollButtonRate or options.ScrollRate) or 5
									local upkey = options.ScrollUpButton or library.scrollupbutton or shared.scrollupbutton or Enum.KeyCode.Up
									local downkey = options.ScrollDownButton or library.scrolldownbutton or shared.scrolldownbutton or Enum.KeyCode.Down
									precisionscrolling = (precisionscrolling and precisionscrolling:Disconnect() and nil) or userInputService.InputBegan:Connect(function(input)
										if input.UserInputType == Enum.UserInputType.Keyboard then
											local code = input.KeyCode
											local isup = code == upkey
											local isdown = code == downkey
											if isup or isdown then
												local txt = userInputService:GetFocusedTextBox()
												if not txt or txt == dropdownSelection then
													while wait_check() and userInputService:IsKeyDown(code) do
														realDropdownHolder.CanvasPosition = Vector2:new(math.clamp(realDropdownHolder.CanvasPosition.Y + ((isup and -scrollrate) or scrollrate), 0, realDropdownHolder.AbsoluteCanvasSize.Y))
													end
												end
											end
										end
									end)
									library.signals[1 + #library.signals] = precisionscrolling
								end
							else
								submenuOpen = nil
								dropdownToggle.Rotation = 90
								colored_dropdown_BackgroundColor3[3] = "topGradient"
								colored_dropdown_BackgroundColor3[4] = nil
								colored_dropdown_ImageColor3[3] = "bottomGradient"
								colored_dropdown_ImageColor3[4] = nil
								tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
									BackgroundColor3 = library.colors.topGradient,
									ImageColor3 = library.colors.bottomGradient
								}):Play()
								dropdownHolderFrame.Visible = false
								for ins, z in next, restorezindex do
									ins.ZIndex = z
								end
								precisionscrolling = (precisionscrolling and precisionscrolling:Disconnect() and nil) or nil
							end
						end
						showing = dropdownEnabled
						if showing or dropdownEnabled then
						else
							delay(0.01, update)
						end
					end
					local Set = (multiselect and function(t, dat)
						if nil == dat and t ~= nil then
							dat = t
						end
						local lastv = library_flags[flagName]
						if not lastv or selectedOption ~= lastv then
							if lastv and type(lastv) == "table" then
								selectedOption = library_flags[flagName]
							else
								library_flags[flagName] = selectedOption
							end
							warn("Attempting to use new table for", flagName, " Please use :Set(), because setting it through flags table may cause errors", debug.traceback(""))
							lastv = library_flags[flagName]
						end
						local cloned = {unpack(selectedOption)}
						if not dat then
							if #selectedOption ~= 0 then
								table.clear(selectedOption)
								if callback then
									task.spawn(callback, selectedOption, cloned)
								end
							end
							return selectedOption
						elseif type(dat) ~= "table" then
							warn("Expected table for argument #1 on Set for MultiSelect searchbox. Got", dat, debug.traceback(""))
							return selectedOption
						end
						for k = table.pack(unpack(dat)).n, 1, -1 do
							if dat[k] == nil then
								table.remove(dat, k)
							end
						end
						local proceed = #cloned ~= #dat
						table.clear(selectedOption)
						for k, v in next, dat do
							selectedOption[k] = v
							if not proceed and cloned[k] ~= v then
								proceed = 1
							end
						end
						dropdownSelection.Text = (passed_multiselect == "string" and multiselect) or tostring(blankstring or "Select Item(s)")
						if proceed and callback then
							task.spawn(callback, selectedOption, cloned)
						end
						return selectedOption
					end) or function(t, str)
						if nil == str and t then
							str = t
						end
						local last_v = library_flags[flagName]
						selectedOption = str
						library_flags[flagName] = str
						if options.Location then
							options.Location[options.LocationFlag or flagName] = str
						end
						local sstr = (selectedOption and tostring(selectedOption)) or blankstring or "No Blank String"
						if dropdownSelection.Text ~= sstr then
							dropdownSelection.Text = sstr
						end
						if callback and (last_v ~= str or options.AllowDuplicateCalls) then
							task.spawn(callback, str, last_v)
						end
						return str
					end
					if val ~= nil then
						Set(val)
					else
						library_flags[flagName] = selectedOption
						if options.Location then
							options.Location[options.LocationFlag or flagName] = selectedOption
						end
					end
					library.signals[1 + #library.signals] = dropdownToggle.MouseButton1Click:Connect(function()
						showing = not showing
						display(showing)
					end)
					library.signals[1 + #library.signals] = dropdownSelection.Focused:Connect(function()
						showing = true
						display(true)
					end)
					library.signals[1 + #library.signals] = dropdownSelection:GetPropertyChangedSignal("Text"):Connect(function()
						if showing then
							display(true, #dropdownSelection.Text > 0)
						end
					end)
					if not multiselect then
						library.signals[1 + #library.signals] = dropdownSelection.FocusLost:Connect(function(b)
							if showing then
								wait()
							end
							showing = false
							display(false)
							if b then
								Set(dropdownSelection.Text)
							end
						end)
					end
					AddOptions(list)
					local default = library_flags[flagName]
					function update()
						dropdownName, callback = options.Name or dropdownName, options.Callback
						local sstr = (passed_multiselect == "string" and multiselect) or (not multiselect and library_flags[flagName] and tostring(library_flags[flagName])) or (not multiselect and selectedOption and tostring(selectedOption)) or blankstring or "Nothing"
						if dropdownSelection.Text ~= sstr then
							dropdownSelection.Text = sstr
						end
						dropdownHeadline.Text = (dropdownName and tostring(dropdownName)) or "???"
						return sstr
					end
					local function validate(fallbackValue)
						if list and table.find(list, library_flags[flagName]) then
							update()
							return true
						end
						if fallbackValue ~= nil then
							if fallbackValue == "__DEFAULT" then
								fallbackValue = default
							end
						else
							fallbackValue = list[1]
						end
						if multiselect and type(fallbackValue) ~= "table" then
							fallbackValue = {fallbackValue}
						end
						return Set(fallbackValue)
					end
					local objectdata = {
						Options = options,
						Name = flagName,
						Flag = flagName,
						Type = "SearchBox",
						Default = default,
						Parent = sectionFunctions,
						Instance = dropdownSelection,
						Validate = validate,
						Set = Set,
						Remove = function()
							if newDropdown then
								newDropdown.Parent = nil
								sectionFunctions:Update()
							end
						end,
						RawSet = ((multiselect and function(t, dat)
							if nil == dat and t ~= nil then
								dat = t
							end
							local lastv = library_flags[flagName]
							if not lastv or selectedOption ~= lastv then
								if lastv and type(lastv) == "table" then
									selectedOption = library_flags[flagName]
								else
									library_flags[flagName] = selectedOption
								end
								warn("Attempting to use new table for", flagName, " Please use :Set(), as setting through flags table may cause errors", debug.traceback(""))
								lastv = library_flags[flagName]
							end
							local cloned = {unpack(selectedOption)}
							if not dat then
								if #selectedOption ~= 0 then
									table.clear(selectedOption)
									if callback then
										task.spawn(callback, selectedOption, cloned)
									end
								end
								return selectedOption
							elseif type(dat) ~= "table" then
								warn("Expected table for argument #1 on Set for MultiSelect searchbox. Got", dat, debug.traceback(""))
								return selectedOption
							end
							for k = table.pack(unpack(dat)).n, 1, -1 do
								if dat[k] == nil then
									table.remove(dat, k)
								end
							end
							local proceed = #cloned ~= #dat
							table.clear(selectedOption)
							for k, v in next, dat do
								selectedOption[k] = v
								if not proceed and cloned[k] ~= v then
									proceed = 1
								end
							end
							update()
							return selectedOption
						end) or function(t, str)
							if nil == str and t then
								str = t
							end
							selectedOption = str
							library_flags[flagName] = str
							if options.Location then
								options.Location[options.LocationFlag or flagName] = str
							end
							update()
							return str
						end),
						Get = function()
							return library_flags[flagName]
						end,
						Update = update,
						Reset = function()
							return Set(nil, default)
						end
					}
					function objectdata.UpdateList(t, listt, updateValues)
						if (nil == listt and t ~= nil) or (type(t) == "table" and type(listt) ~= "table") then
							listt, updateValues = t, listt
						end
						if listt == objectdata then
							listt = nil
						end
						resolvelist = getresolver(listt or options.List, options.Filter, options.Method)
						list = resolvelist()
						if updateValues then
							validate()
						end
						if showing then
							display(false)
							display(true)
						end
						return list
					end
					tabFunctions.Flags[flagName], sectionFunctions.Flags[flagName], elements[flagName] = objectdata, objectdata, objectdata
					return objectdata
				end
				sectionFunctions.NewSearchBox = sectionFunctions.AddSearchBox
				sectionFunctions.CreateSearchBox = sectionFunctions.AddSearchBox
				sectionFunctions.SearchBox = sectionFunctions.AddSearchBox
				sectionFunctions.CreateSearchbox = sectionFunctions.AddSearchBox
				sectionFunctions.NewSearchbox = sectionFunctions.AddSearchBox
				sectionFunctions.Searchbox = sectionFunctions.AddSearchBox
				sectionFunctions.Sbox = sectionFunctions.AddSearchBox
				sectionFunctions.SBox = sectionFunctions.AddSearchBox
				if isfolder and makefolder and listfiles and readfile and writefile then
					function sectionFunctions:AddPersistence(options, ...)
						options = (options and type(options) == "string" and resolvevararg("Tab", options, ...)) or options
						local dropdownName, custom_workspace, val, persistiveflags, suffix, callback, loadcallback, savecallback, postload, postsave, flagName = assert(options.Name, "Missing Name for new persistence."), options.Workspace or library.WorkspaceName, options.Value, options.Persistive or options.Flags or "all", options.Suffix, options.Callback, options.LoadCallback, options.SaveCallback, options.PostLoadCallback, options.PostSaveCallback, options.Flag or (function()
							library.unnamedpersistence = 1 + (library.unnamedpersistence or 0)
							return "Persistence" .. tostring(library.unnamedpersistence)
						end)()
						if elements[flagName] ~= nil then
							warn(debug.traceback("Warning! Re-used flag '" .. flagName .. "'", 3))
						end
						local designerpersists = options.Desginer
						local newDropdown = Instance_new("Frame")
						local dropdown = Instance_new("ImageLabel")
						local dropdownInner = Instance_new("ImageLabel")
						local dropdownToggle = Instance_new("ImageButton")
						local dropdownSelection = Instance_new("TextBox")
						local dropdownHeadline = Instance_new("TextLabel")
						local dropdownHolderFrame = Instance_new("ImageLabel")
						local dropdownHolderInner = Instance_new("ImageLabel")
						local realDropdownHolder = Instance_new("ScrollingFrame")
						local realDropdownHolderList = Instance_new("UIListLayout")
						local dropdownEnabled = false
						if not isfolder("./Pepsi Lib") then
							makefolder("./Pepsi Lib")
						end
						local common_string = "./Pepsi Lib/" .. tostring(custom_workspace or library.WorkspaceName)
						local function resolvelist(nofold)
							if custom_workspace ~= options.Workspace then
								custom_workspace = options.Workspace
								common_string = "./Pepsi Lib/" .. tostring(custom_workspace or library.WorkspaceName)
							end
							if not isfolder or not makefolder or not listfiles then
								return {}
							end
							if not isfolder(common_string) then
								if nofold then
									return {}
								end
								makefolder(common_string)
							end
							assert(isfolder(common_string), "Couldn't create folder: " .. tostring(library.WorkspaceName or "No workspace name?"))
							local names, files = {}, listfiles(common_string)
							if #files > 0 then
								local len = #common_string + 2
								for _, f in next, files do
									names[1 + #names] = string.sub(f, len, -5)
								end
								table.sort(names)
							end
							return names
						end
						local list = resolvelist(true)
						local blankstring = options.BlankValue or options.NoValueString or options.Nothing
						local selectedObjects = {}
						local optionCount = 0
						if blankstring and val == nil then
							val = blankstring
						end
						local selectedOption = val or blankstring or list[1]
						newDropdown.Name = "newDropdown"
						newDropdown.Parent = sectionHolder
						newDropdown.BackgroundColor3 = Color3.new(1, 1, 1)
						newDropdown.BackgroundTransparency = 1
						newDropdown.Size = UDim2.new(1, 0, 0, 42)
						dropdown.Name = "dropdown"
						dropdown.Parent = newDropdown
						dropdown.Active = true
						dropdown.BackgroundColor3 = library.colors.topGradient
						local colored_dropdown_BackgroundColor3 = {dropdown, "BackgroundColor3", "topGradient"}
						colored[1 + #colored] = colored_dropdown_BackgroundColor3
						dropdown.BorderColor3 = library.colors.elementBorder
						colored[1 + #colored] = {dropdown, "BorderColor3", "elementBorder"}
						dropdown.Position = UDim2.fromScale(0.027, 0.45)
						dropdown.Selectable = true
						dropdown.Size = UDim2.fromOffset(206, 18)
						dropdown.Image = "rbxassetid://2454009026"
						dropdown.ImageColor3 = library.colors.bottomGradient
						local colored_dropdown_ImageColor3 = {dropdown, "ImageColor3", "bottomGradient"}
						colored[1 + #colored] = colored_dropdown_ImageColor3
						dropdownInner.Name = "dropdownInner"
						dropdownInner.Parent = dropdown
						dropdownInner.Active = true
						dropdownInner.AnchorPoint = Vector2.new(0.5, 0.5)
						dropdownInner.BackgroundColor3 = library.colors.topGradient
						colored[1 + #colored] = {dropdownInner, "BackgroundColor3", "topGradient"}
						dropdownInner.BorderColor3 = library.colors.elementBorder
						colored[1 + #colored] = {dropdownInner, "BorderColor3", "elementBorder"}
						dropdownInner.Position = UDim2.fromScale(0.5, 0.5)
						dropdownInner.Selectable = true
						dropdownInner.Size = UDim2.new(1, -4, 1, -4)
						dropdownInner.Image = "rbxassetid://2454009026"
						dropdownInner.ImageColor3 = library.colors.bottomGradient
						colored[1 + #colored] = {dropdownInner, "ImageColor3", "bottomGradient"}
						dropdownToggle.Name = "dropdownToggle"
						dropdownToggle.Parent = dropdown
						dropdownToggle.BackgroundColor3 = Color3.new(1, 1, 1)
						dropdownToggle.BackgroundTransparency = 1
						dropdownToggle.Position = UDim2.fromScale(0.9, 0.17)
						dropdownToggle.Rotation = 90
						dropdownToggle.Size = UDim2.fromOffset(12, 12)
						dropdownToggle.ZIndex = 2
						dropdownToggle.Image = "rbxassetid://71659683"
						dropdownToggle.ImageColor3 = Color3.fromRGB(171, 171, 171)
						dropdownSelection.Name = "dropdownSelection"
						dropdownSelection.Parent = dropdown
						dropdownSelection.BackgroundColor3 = Color3.new(1, 1, 1)
						dropdownSelection.BackgroundTransparency = 1
						dropdownSelection.Position = UDim2.new(0.0295485705)
						dropdownSelection.Size = UDim2.fromScale(0.97, 1)
						dropdownSelection.ZIndex = 5
						dropdownSelection.Font = Enum.Font.Code
						dropdownSelection.LineHeight = 1.15
						dropdownSelection.Text = (selectedOption and tostring(selectedOption)) or "nil"
						dropdownSelection.TextColor3 = library.colors.otherElementText
						colored[1 + #colored] = {dropdownSelection, "TextColor3", "otherElementText"}
						dropdownSelection.TextSize = 14
						dropdownSelection.TextXAlignment = Enum.TextXAlignment.Left
						dropdownHeadline.Name = "dropdownHeadline"
						dropdownHeadline.Parent = newDropdown
						dropdownHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
						dropdownHeadline.BackgroundTransparency = 1
						dropdownHeadline.Position = UDim2.fromScale(0.034, 0.03)
						dropdownHeadline.Size = UDim2.fromOffset(167, 11)
						dropdownHeadline.Font = Enum.Font.Code
						dropdownHeadline.Text = (dropdownName and tostring(dropdownName)) or "???"
						dropdownHeadline.TextColor3 = library.colors.elementText
						colored[1 + #colored] = {dropdownHeadline, "TextColor3", "elementText"}
						dropdownHeadline.TextSize = 14
						dropdownHeadline.TextXAlignment = Enum.TextXAlignment.Left
						dropdownHolderFrame.Name = "dropdownHolderFrame"
						dropdownHolderFrame.Parent = newDropdown
						dropdownHolderFrame.Active = true
						dropdownHolderFrame.BackgroundColor3 = library.colors.topGradient
						colored[1 + #colored] = {dropdownHolderFrame, "BackgroundColor3", "topGradient"}
						dropdownHolderFrame.BorderColor3 = library.colors.elementBorder
						colored[1 + #colored] = {dropdownHolderFrame, "BorderColor3", "elementBorder"}
						dropdownHolderFrame.Position = UDim2.fromScale(0.025, 1.012)
						dropdownHolderFrame.Selectable = true
						dropdownHolderFrame.Size = UDim2.fromOffset(206, 22)
						dropdownHolderFrame.Visible = false
						dropdownHolderFrame.Image = "rbxassetid://2454009026"
						dropdownHolderFrame.ImageColor3 = library.colors.bottomGradient
						colored[1 + #colored] = {dropdownHolderFrame, "ImageColor3", "bottomGradient"}
						dropdownHolderInner.Name = "dropdownHolderInner"
						dropdownHolderInner.Parent = dropdownHolderFrame
						dropdownHolderInner.Active = true
						dropdownHolderInner.AnchorPoint = Vector2.new(0.5, 0.5)
						dropdownHolderInner.BackgroundColor3 = library.colors.topGradient
						colored[1 + #colored] = {dropdownHolderInner, "BackgroundColor3", "topGradient"}
						dropdownHolderInner.BorderColor3 = library.colors.elementBorder
						colored[1 + #colored] = {dropdownHolderInner, "BorderColor3", "elementBorder"}
						dropdownHolderInner.Position = UDim2.fromScale(0.5, 0.5)
						dropdownHolderInner.Selectable = true
						dropdownHolderInner.Size = UDim2.new(1, -4, 1, -4)
						dropdownHolderInner.Image = "rbxassetid://2454009026"
						dropdownHolderInner.ImageColor3 = library.colors.bottomGradient
						colored[1 + #colored] = {dropdownHolderInner, "ImageColor3", "bottomGradient"}
						realDropdownHolder.Name = "realDropdownHolder"
						realDropdownHolder.Parent = dropdownHolderInner
						realDropdownHolder.BackgroundColor3 = Color3.new(1, 1, 1)
						realDropdownHolder.BackgroundTransparency = 1
						realDropdownHolder.Selectable = false
						realDropdownHolder.Size = UDim2.fromScale(1, 1)
						realDropdownHolder.CanvasSize = UDim2.new()
						realDropdownHolder.ScrollBarThickness = 5
						realDropdownHolder.ScrollingDirection = Enum.ScrollingDirection.Y
						realDropdownHolder.AutomaticCanvasSize = Enum.AutomaticSize.Y
						realDropdownHolder.ScrollBarImageTransparency = 0.5
						realDropdownHolder.ScrollBarImageColor3 = library.colors.section
						colored[1 + #colored] = {realDropdownHolder, "ScrollBarImageColor3", "section"}
						realDropdownHolderList.Name = "realDropdownHolderList"
						realDropdownHolderList.Parent = realDropdownHolder
						realDropdownHolderList.HorizontalAlignment = Enum.HorizontalAlignment.Center
						realDropdownHolderList.SortOrder = Enum.SortOrder.LayoutOrder
						sectionFunctions:Update()
						library.signals[1 + #library.signals] = newDropdown.MouseEnter:Connect(function()
							colored_dropdown_BackgroundColor3[3] = "main"
							colored_dropdown_BackgroundColor3[4] = 1.5
							colored_dropdown_ImageColor3[3] = "main"
							colored_dropdown_ImageColor3[4] = 2.5
							tweenService:Create(dropdown, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = darkenColor(library.colors.main, 1.5),
								ImageColor3 = darkenColor(library.colors.main, 2.5)
							}):Play()
						end)
						library.signals[1 + #library.signals] = newDropdown.MouseLeave:Connect(function()
							if not dropdownEnabled then
								colored_dropdown_BackgroundColor3[3] = "topGradient"
								colored_dropdown_BackgroundColor3[4] = nil
								colored_dropdown_ImageColor3[3] = "bottomGradient"
								colored_dropdown_ImageColor3[4] = nil
								tweenService:Create(dropdown, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
									BackgroundColor3 = library.colors.topGradient,
									ImageColor3 = library.colors.bottomGradient
								}):Play()
							end
						end)
						local restorezindex = {}
						local function UpdateDropdownHolder()
							if optionCount >= 6 then
								realDropdownHolder.CanvasSize = UDim2:fromOffset(realDropdownHolderList.AbsoluteContentSize.Y + 2)
							elseif optionCount <= 5 then
								dropdownHolderFrame.Size = UDim2.fromOffset(206, (realDropdownHolderList.AbsoluteContentSize.Y + 4))
							end
						end
						local function AddOptions(optionsTable, filter)
							if options.Sort then
								local didstuff, dosort = nil, options.Sort
								if type(dosort) == "function" then
									local g, h = pcall(table.sort, optionsTable, dosort)
									if g then
										didstuff = true
									elseif h then
										warn("Error sorting list:", h, debug.traceback(""))
									end
								end
								if not didstuff then
									table.sort(optionsTable, library.defaultSort)
								end
							end
							if blankstring and (optionsTable[1] ~= blankstring or table.find(optionsTable, blankstring, 2)) then
								local exists = table.find(optionsTable, blankstring)
								if exists then
									for _ = 1, 35 do
										table.remove(optionsTable, exists)
										exists = table.find(optionsTable, blankstring)
										if not exists then
											break
										end
									end
								end
								table.insert(optionsTable, 1, blankstring)
							end
							optionCount = 0
							realDropdownHolderList.Parent = nil
							realDropdownHolder:ClearAllChildren()
							realDropdownHolderList.Parent = realDropdownHolder
							for _, v in next, optionsTable do
								if not filter or tostring(v):lower():find(dropdownSelection.Text:lower(), 1, true) then
									optionCount = optionCount + 1
									UpdateDropdownHolder()
									local newOption = Instance_new("ImageLabel")
									local optionButton = Instance_new("TextButton")
									if selectedOption == v or not selectedObjects[1] or not selectedObjects[2] then
										selectedObjects[1] = newOption
										selectedObjects[2] = optionButton
									end
									newOption.Name = "Frame"
									newOption.Parent = realDropdownHolder
									newOption.BackgroundColor3 = (selectedOption == v and library.colors.selectedOption or library.colors.topGradient)
									newOption.BorderSizePixel = 0
									newOption.Size = UDim2.fromOffset(202, 18)
									newOption.Image = "rbxassetid://2454009026"
									newOption.ImageColor3 = (selectedOption == v and library.colors.unselectedOption or library.colors.bottomGradient)
									optionButton.Name = tostring(v)
									optionButton.Parent = newOption
									optionButton.AnchorPoint = Vector2.new(0.5, 0.5)
									optionButton.BackgroundColor3 = Color3.new(1, 1, 1)
									optionButton.BackgroundTransparency = 1
									optionButton.Position = UDim2.fromScale(0.5, 0.5)
									optionButton.Size = UDim2.new(1, -10, 1)
									optionButton.ZIndex = 5
									optionButton.Font = Enum.Font.Code
									optionButton.Text = (selectedOption == v and " " .. tostring(v)) or tostring(v)
									optionButton.TextColor3 = (selectedOption == v and library.colors.main or library.colors.otherElementText)
									optionButton.TextSize = 14
									optionButton.TextXAlignment = Enum.TextXAlignment.Left
									library.signals[1 + #library.signals] = optionButton.MouseButton1Down:Connect(function()
										dropdownSelection.Text = tostring(v)
										restorezindex[newSection] = restorezindex[newSection] or newSection.ZIndex
										restorezindex[newDropdown] = restorezindex[newDropdown] or newDropdown.ZIndex
										restorezindex[sectionHolder] = restorezindex[sectionHolder] or sectionHolder.ZIndex
										if selectedOption ~= v then
											local last_v = library_flags[flagName]
											selectedObjects[1].BackgroundColor3 = library.colors.topGradient
											selectedObjects[1].ImageColor3 = library.colors.bottomGradient
											selectedObjects[2].Text = selectedObjects[2].Name
											selectedObjects[2].TextColor3 = library.colors.otherElementText
											selectedOption = v
											selectedObjects[1] = newOption
											selectedObjects[2] = optionButton
											newOption.BackgroundColor3 = library.colors.selectedOption
											newOption.ImageColor3 = library.colors.unselectedOption
											optionButton.TextColor3 = library.colors.main
											dropdownHolderFrame.Visible = false
											dropdownToggle.Rotation = 90
											dropdownEnabled = false
											colored_dropdown_BackgroundColor3[3] = "topGradient"
											colored_dropdown_BackgroundColor3[4] = nil
											colored_dropdown_ImageColor3[3] = "bottomGradient"
											colored_dropdown_ImageColor3[4] = nil
											tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
												BackgroundColor3 = library.colors.topGradient,
												ImageColor3 = library.colors.bottomGradient
											}):Play()
											library_flags[flagName] = selectedOption
											if options.Location then
												options.Location[options.LocationFlag or flagName] = selectedOption
											end
											dropdownSelection.Text = tostring(selectedOption)
											if submenuOpen then
												submenuOpen = nil
											end
											if callback then
												task.spawn(callback, selectedOption, last_v)
											end
										else
											submenuOpen = nil
											dropdownToggle.Rotation = 90
											newDropdown.ZIndex = 1
											sectionHolder.ZIndex = 1
											colored_dropdown_BackgroundColor3[3] = "topGradient"
											colored_dropdown_BackgroundColor3[4] = nil
											colored_dropdown_ImageColor3[3] = "bottomGradient"
											colored_dropdown_ImageColor3[4] = nil
											tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
												BackgroundColor3 = library.colors.topGradient,
												ImageColor3 = library.colors.bottomGradient
											}):Play()
											dropdownHolderFrame.Visible = false
										end
										for ins, z in next, restorezindex do
											ins.ZIndex = z
										end
									end)
									library.signals[1 + #library.signals] = optionButton.MouseEnter:Connect(function()
										tweenService:Create(newOption, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
											BackgroundColor3 = library.colors.hoveredOptionTop,
											ImageColor3 = library.colors.hoveredOptionBottom
										}):Play()
									end)
									library.signals[1 + #library.signals] = optionButton.MouseLeave:Connect(function()
										tweenService:Create(newOption, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
											BackgroundColor3 = library.colors.unhoveredOptionTop,
											ImageColor3 = library.colors.unhoveredOptionBottom
										}):Play()
									end)
									UpdateDropdownHolder()
								end
							end
						end
						local precisionscrolling, update = nil
						local showing = false
						local function display(dropdownEnabled, f)
							if submenuOpen == dropdown or submenuOpen == nil then
								if dropdownEnabled then
									list = resolvelist(true)
									AddOptions(list, f)
									submenuOpen = dropdown
									restorezindex[newSection] = restorezindex[newSection] or newSection.ZIndex
									restorezindex[newDropdown] = restorezindex[newDropdown] or newDropdown.ZIndex
									restorezindex[sectionHolder] = restorezindex[sectionHolder] or sectionHolder.ZIndex
									newSection.ZIndex = 50 + newSection.ZIndex
									dropdownToggle.Rotation = 270
									newDropdown.ZIndex = 2
									sectionHolder.ZIndex = 2
									colored_dropdown_BackgroundColor3[3] = "main"
									colored_dropdown_BackgroundColor3[4] = 1.5
									colored_dropdown_ImageColor3[3] = "main"
									colored_dropdown_ImageColor3[4] = 2.5
									tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
										BackgroundColor3 = darkenColor(library.colors.main, 1.5),
										ImageColor3 = darkenColor(library.colors.main, 2.5)
									}):Play()
									dropdownHolderFrame.Visible = true
									if not options.DisablePrecisionScrolling then
										local upkey = options.ScrollUpButton or library.scrollupbutton or shared.scrollupbutton or Enum.KeyCode.Up
										local downkey = options.ScrollDownButton or library.scrolldownbutton or shared.scrolldownbutton or Enum.KeyCode.Down
										precisionscrolling = (precisionscrolling and precisionscrolling:Disconnect() and nil) or userInputService.InputBegan:Connect(function(input)
											if input.UserInputType == Enum.UserInputType.Keyboard then
												local code = input.KeyCode
												local isup = code == upkey
												local isdown = code == downkey
												if isup or isdown then
													local txt = userInputService:GetFocusedTextBox()
													if not txt then
														while wait_check() and userInputService:IsKeyDown(code) do
															realDropdownHolder.CanvasPosition = Vector2:new(math.clamp(realDropdownHolder.CanvasPosition.Y + ((isup and -5) or 5), 0, realDropdownHolder.AbsoluteCanvasSize.Y))
														end
													end
												end
											end
										end)
										library.signals[1 + #library.signals] = precisionscrolling
									end
								else
									submenuOpen = nil
									dropdownToggle.Rotation = 90
									colored_dropdown_BackgroundColor3[3] = "topGradient"
									colored_dropdown_BackgroundColor3[4] = nil
									colored_dropdown_ImageColor3[3] = "bottomGradient"
									colored_dropdown_ImageColor3[4] = nil
									tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
										BackgroundColor3 = library.colors.topGradient,
										ImageColor3 = library.colors.bottomGradient
									}):Play()
									dropdownHolderFrame.Visible = false
									for ins, z in next, restorezindex do
										ins.ZIndex = z
									end
									precisionscrolling = (precisionscrolling and precisionscrolling:Disconnect() and nil) or nil
								end
								showing = dropdownEnabled
								if showing or dropdownEnabled then
								else
									delay(0.01, update)
								end
							end
						end
						local last_v = nil
						local function Set(t, str)
							if nil == str and t then
								str = t
							end
							selectedOption = str
							last_v = library_flags[flagName]
							library_flags[flagName] = str
							if options.Location then
								options.Location[options.LocationFlag or flagName] = str
							end
							local sstr = (selectedOption and tostring(selectedOption)) or blankstring or "No Blank String"
							if dropdownSelection.Text ~= sstr then
								dropdownSelection.Text = sstr
							end
							if callback and (last_v ~= str or options.AllowDuplicateCalls) then
								task.spawn(callback, str, last_v)
							end
							return str
						end
						if val ~= nil then
							Set(val)
						else
							Set("Filename")
						end
						library.signals[1 + #library.signals] = dropdownSelection.Focused:Connect(function()
							showing = true
							display(true)
						end)
						library.signals[1 + #library.signals] = dropdownSelection:GetPropertyChangedSignal("Text"):Connect(function()
							if showing then
								display(true, #dropdownSelection.Text > 0)
							end
						end)
						library.signals[1 + #library.signals] = dropdownSelection.FocusLost:Connect(function(b)
							if showing then
								wait()
							end
							showing = false
							display(false)
							if b then
								Set(dropdownSelection.Text)
							end
						end)
						AddOptions(list)
						local function savestuff(s, get)
							if not s or type(s) ~= "string" then
								s = nil
							end
							local rawfile = "json__save"
							if not get then
								local filenameddst = string.gsub(s or dropdownSelection.Text or "", "%W", "")
								if #filenameddst == 0 then
									return
								end
								rawfile = string.format("%s/%s.txt", common_string, filenameddst)
							end
							if savecallback then
								local x, e = pcall(savecallback, rawfile, library_flags[flagName])
								if not x and e then
									warn("Error while calling the Pre-Save callback:", e, debug.traceback(""))
								end
							end
							local working_with = {}
							if persistiveflags == 1 or persistiveflags == true or persistiveflags == "*" then
								persistiveflags = "all"
							elseif persistiveflags == 2 then
								persistiveflags = "tab"
							elseif persistiveflags == 3 then
								persistiveflags = "section"
							end
							if persistiveflags == "all" or persistiveflags == "tab" or persistiveflags == "section" then
								for cflag, data in next, (persistiveflags == "all" and elements) or (persistiveflags == "tab" and tabFunctions.Flags) or (persistiveflags == "section" and sectionFunctions.Flags) do
									if data.Type ~= "Persistence" and (designerpersists or string.sub(cflag, 1, 11) ~= "__Designer.") then
										working_with[cflag] = data
									end
								end
							elseif type(persistiveflags) == "table" then
								if #persistiveflags > 0 then
									local inverted = persistiveflags[0] == false or persistiveflags.Inverted
									for k, cflag in next, persistiveflags do
										if k > 0 then
											local data = elements[cflag]
											if data and data.Type ~= "Persistence" and (designerpersists or string.sub(cflag, 1, 11) ~= "__Designer.") then
												working_with[cflag] = (not inverted and data) or nil
											end
										end
									end
								else
									for cflag, persists in next, elements do
										if persists and (designerpersists or string.sub(cflag, 1, 11) ~= "__Designer.") then
											local data = elements[cflag]
											if data then
												working_with[cflag] = data
											end
										end
									end
								end
							end
							local saving = {}
							for cflag in next, working_with do
								local value = library_flags[cflag]
								local good, jval = nil, nil
								if value ~= nil then
									good, jval = JSONEncode(value)
								else
									good, jval = true, "null"
								end
								if not good or (jval == "null" and value ~= nil) then
									local typ = typeof(value)
									if typ == "Color3" then
										value = (library.rainbowflags[cflag] and "rainbow") or Color3ToHex(value)
									end
									value = tostring(value)
									good, jval = JSONEncode(value)
									if not good or (jval == "null" and value ~= nil) then
										warn("Could not save value:", value, debug.traceback(""))
									end
								end
								if good and jval then
									saving[cflag] = value
								end
							end
							local ret = nil
							local good, content = JSONEncode(saving)
							if good and content then
								if not get then
									if not isfolder(common_string) then
										makefolder(common_string)
									end
									writefile(rawfile, content)
								else
									ret = content
								end
							end
							if postsave then
								local x, e = pcall(postsave, rawfile, library_flags[flagName])
								if not x and e then
									warn("Error while calling the Post-Save callback:", e, debug.traceback(""))
								end
							end
							return ret
						end
						local function loadstuff(s, jsonmode, silent)
							if not s or type(s) ~= "string" then
								s = nil
							end
							local filename = "json__load"
							if not jsonmode then
								local filenameddst = convertfilename(s or dropdownSelection.Text, nil, "")
								if #filenameddst == 0 then
									return
								end
								filename = string.format("%s/%s.txt", common_string, filenameddst)
							end
							if loadcallback then
								local x, e = pcall(loadcallback, (jsonmode and s) or filename, library_flags[flagName])
								if not x and e then
									warn("Error while calling the Pre-Load callback:", e, debug.traceback(""))
								end
							end
							if jsonmode or not isfile or isfile(filename) then
								local content = (jsonmode and s) or (not jsonmode and readfile(filename))
								if content and #content > 1 then
									local good, jcontent = JSONDecode(content)
									if good and jcontent then
										for cflag, val in next, jcontent do
											if val and type(val) == "string" and #val > 7 and #val < 64 and string.sub(val, 1, 5) == "Enum." then
												local e = string.find(val, ".", 6, true)
												if e then
													local en = Enum[string.sub(val, 6, e - 1)]
													en = en and en[string.sub(val, e + 1)]
													if en then
														val = en
													else
														warn("Tried & failed to convert '" .. val .. "' to EnumItem")
													end
												end
											end
											local data = elements[cflag]
											if data and data.Type ~= "Persistence" then
												if silent and data.RawSet then
													data:RawSet(val)
												elseif data.Set then
													data:Set(val)
												else
													library_flags[cflag] = val
												end
											end
										end
									end
								end
							end
							if postload then
								local x, e = pcall(postload, filename, library_flags[flagName])
								if not x and e then
									warn("Error while calling the Post-Load callback:", e, debug.traceback(""))
								end
							end
						end
						local fram = nil
						do
							local buttons, offset = {}, 0
							for _, options in next, {{
								Name = "Save" .. ((suffix and (" " .. tostring(suffix))) or ""),
								Callback = savestuff
								}, {
									Name = "Load" .. ((suffix and (" " .. tostring(suffix))) or ""),
									Callback = loadstuff
								}} do
								local buttonName, callback = options.Name, options.Callback
								local realButton = Instance_new("TextButton")
								realButton.Name = "realButton"
								realButton.BackgroundColor3 = Color3.new(1, 1, 1)
								realButton.BackgroundTransparency = 1
								realButton.Size = UDim2.fromScale(1, 1)
								realButton.ZIndex = 5
								realButton.Font = Enum.Font.Code
								realButton.Text = (buttonName and tostring(buttonName)) or "???"
								realButton.TextColor3 = library.colors.elementText
								colored[1 + #colored] = {realButton, "TextColor3", "elementText"}
								realButton.TextSize = 14
								local textsize = textToSize(realButton).X + 14
								if newSection.Parent and (newSection.Parent.AbsoluteSize.X < (offset + textsize + 8)) then
									offset, fram = 0, nil
								end
								local newButton = fram or Instance_new("Frame")
								fram = newButton
								local button = Instance_new("ImageLabel")
								newButton.Name = removeSpaces((buttonName and buttonName:lower() or "???") .. "Holder")
								newButton.Parent = sectionHolder
								newButton.BackgroundColor3 = Color3.new(1, 1, 1)
								newButton.BackgroundTransparency = 1
								newButton.Size = UDim2.new(1, 0, 0, 24)
								button.Name = "button"
								button.Parent = newButton
								button.Active = true
								button.BackgroundColor3 = library.colors.topGradient
								local colored_button_BackgroundColor3 = {button, "BackgroundColor3", "topGradient"}
								colored[1 + #colored] = colored_button_BackgroundColor3
								button.BorderColor3 = library.colors.elementBorder
								colored[1 + #colored] = {button, "BorderColor3", "elementBorder"}
								button.Position = UDim2.new(0.031, offset, 0.166)
								button.Selectable = true
								button.Size = UDim2.fromOffset(28, 18)
								button.Image = "rbxassetid://2454009026"
								button.ImageColor3 = library.colors.bottomGradient
								local colored_button_ImageColor3 = {button, "ImageColor3", "bottomGradient"}
								colored[1 + #colored] = colored_button_ImageColor3
								local buttonInner = Instance_new("ImageLabel")
								buttonInner.Name = "buttonInner"
								buttonInner.Parent = button
								buttonInner.Active = true
								buttonInner.AnchorPoint = Vector2.new(0.5, 0.5)
								buttonInner.BackgroundColor3 = library.colors.topGradient
								colored[1 + #colored] = {buttonInner, "BackgroundColor3", "topGradient"}
								buttonInner.BorderColor3 = library.colors.elementBorder
								colored[1 + #colored] = {buttonInner, "BorderColor3", "elementBorder"}
								buttonInner.Position = UDim2.fromScale(0.5, 0.5)
								buttonInner.Selectable = true
								buttonInner.Size = UDim2.new(1, -4, 1, -4)
								buttonInner.Image = "rbxassetid://2454009026"
								buttonInner.ImageColor3 = library.colors.bottomGradient
								colored[1 + #colored] = {buttonInner, "ImageColor3", "bottomGradient"}
								button.Size = UDim2.fromOffset(textsize, 18)
								realButton.Parent = button
								offset = offset + textsize + 6
								sectionFunctions:Update()
								local presses = 0
								library.signals[1 + #library.signals] = realButton.MouseButton1Click:Connect(function()
									if not library.colorpicker and not submenuOpen then
										presses = 1 + presses
										task.spawn(callback, presses)
									end
								end)
								library.signals[1 + #library.signals] = button.MouseEnter:Connect(function()
									colored_button_BackgroundColor3[3] = "main"
									colored_button_BackgroundColor3[4] = 1.5
									colored_button_ImageColor3[3] = "main"
									colored_button_ImageColor3[4] = 2.5
									tweenService:Create(button, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
										BackgroundColor3 = darkenColor(library.colors.main, 1.5),
										ImageColor3 = darkenColor(library.colors.main, 2.5)
									}):Play()
								end)
								library.signals[1 + #library.signals] = button.MouseLeave:Connect(function()
									colored_button_BackgroundColor3[3] = "topGradient"
									colored_button_BackgroundColor3[4] = nil
									colored_button_ImageColor3[3] = "bottomGradient"
									colored_button_ImageColor3[4] = nil
									tweenService:Create(button, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
										BackgroundColor3 = library.colors.topGradient,
										ImageColor3 = library.colors.bottomGradient
									}):Play()
								end)
							end
						end
						local default = library_flags[flagName]
						function update()
							dropdownName, custom_workspace, persistiveflags, suffix, callback, loadcallback, savecallback, postload, postsave = options.Name or dropdownName, options.Workspace or library.WorkspaceName, options.Persistive or options.Flags or "all", options.Suffix, options.Callback, options.LoadCallback, options.SaveCallback, options.PostLoadCallback, options.PostSaveCallback
							local sstr = tostring(library_flags[flagName])
							if dropdownSelection.Text ~= sstr then
								dropdownSelection.Text = sstr
							end
							dropdownHeadline.Text = (dropdownName and tostring(dropdownName)) or "???"
							return sstr
						end
						local objectdata = {
							Options = options,
							Name = flagName,
							Flag = flagName,
							Type = "Persistence",
							Default = default,
							Parent = sectionFunctions,
							Instance = dropdownSelection,
							Set = Set,
							Remove = function()
								local relod = nil
								if newDropdown then
									newDropdown.Parent = nil
									relod = true
								end
								if fram then
									fram.Parent = nil
									relod = true
								end
								if relod then
									sectionFunctions:Update()
								end
							end,
							SaveFile = function(t, str, ret)
								if t ~= nil and type(t) ~= "table" then
									str, ret = t, str
								end
								if type(str) == "string" then
									str = str:match("(.+)%..+$") or str
								end
								return savestuff(str, ret)
							end,
							LoadFile = function(t, str, jsonmode)
								if t ~= nil and type(t) ~= "table" then
									str, jsonmode = t, str
								end
								if isfile and isfile(str) then
									return loadstuff(readfile(str), true)
								elseif not jsonmode and type(str) == "string" then
									str = str:match("(.+)%..+$") or str
								end
								return loadstuff(str, jsonmode)
							end,
							LoadJSON = function(_, json)
								return loadstuff(json, true)
							end,
							LoadFileRaw = function(t, str, jsonmode)
								if t ~= nil and type(t) ~= "table" then
									str, jsonmode = t, str
								end
								if isfile and isfile(str) then
									return loadstuff(readfile(str), true, true)
								elseif not jsonmode and type(str) == "string" then
									str = str:match("(.+)%..+$") or str
								end
								return loadstuff(str, jsonmode, true)
							end,
							LoadJSONRaw = function(_, json)
								return loadstuff(json, true, true)
							end,
							GetJSON = function(t, clipboard)
								if nil == clipboard and t ~= nil then
									clipboard = t
								end
								local json = savestuff(nil, true)
								local clipfunc = (clipboard and type(clipboard) == "function" and clipboard) or setclipboard
								if clipboard and clipfunc then
									clipfunc(json)
								end
								return json
							end,
							RawSet = function(t, str)
								if nil == str and t ~= nil then
									str = t
								end
								selectedOption = str
								last_v = library_flags[flagName]
								library_flags[flagName] = str
								if options.Location then
									options.Location[options.LocationFlag or flagName] = str
								end
								update()
								return str
							end,
							Get = function()
								return library_flags[flagName]
							end,
							Update = update,
							Reset = function()
								return Set(nil, default)
							end
						}
						tabFunctions.Flags[flagName], sectionFunctions.Flags[flagName], elements[flagName] = objectdata, objectdata, objectdata
						return objectdata
					end
				else
					function sectionFunctions.AddPersistence()
						if not library.warnedpersistance then
							library.warnedpersistance = 1
							warn(debug.traceback("Persistance not supported"))
						end
						function sectionFunctions.AddPersistence()
						end
					end
				end
				sectionFunctions.NewPersistence = sectionFunctions.AddPersistence
				sectionFunctions.CreatePersistence = sectionFunctions.AddPersistence
				sectionFunctions.Persistence = sectionFunctions.AddPersistence
				sectionFunctions.CreateSaveLoad = sectionFunctions.AddPersistence
				sectionFunctions.SaveLoad = sectionFunctions.AddPersistence
				sectionFunctions.SL = sectionFunctions.AddPersistence
				function sectionFunctions:AddDropdown(options, ...)
					options = (options and type(options) == "string" and resolvevararg("Dropdown", options, ...)) or options
					local dropdownName, listt, val, callback, flagName = assert(options.Name, "Missing Name for new searchbox."), assert(options.List, "Missing List for new searchbox."), options.Value, options.Callback, options.Flag or (function()
						library.unnameddropdown = 1 + (library.unnameddropdown or 0)
						return "Dropdown" .. tostring(library.unnameddropdown)
					end)()
					if elements[flagName] ~= nil then
						warn(debug.traceback("Warning! Re-used flag '" .. flagName .. "'", 3))
					end
					local newDropdown = Instance_new("Frame")
					local dropdown = Instance_new("ImageLabel")
					local dropdownInner = Instance_new("ImageLabel")
					local dropdownToggle = Instance_new("ImageButton")
					local dropdownSelection = Instance_new("TextLabel")
					local dropdownHeadline = Instance_new("TextLabel")
					local dropdownHolderFrame = Instance_new("ImageLabel")
					local dropdownHolderInner = Instance_new("ImageLabel")
					local realDropdownHolder = Instance_new("ScrollingFrame")
					local realDropdownHolderList = Instance_new("UIListLayout")
					local dropdownEnabled = false
					local multiselect = options.MultiSelect or options.Multi or options.Multiple
					local addcallback = options.ItemAdded or options.AddedCallback
					local delcallback = options.ItemRemoved or options.RemovedCallback
					local clrcallback = options.ItemsCleared or options.ClearedCallback
					local modcallback = options.ItemChanged or options.ChangedCallback
					local blankstring = not multiselect and (options.BlankValue or options.NoValueString or options.Nothing)
					local resolvelist = getresolver(listt, options.Filter, options.Method)
					local list = resolvelist()
					local selectedOption = list[1]
					local passed_multiselect = multiselect and type(multiselect)
					if blankstring and val == nil then
						val = blankstring
					end
					if val ~= nil then
						selectedOption = val
					end
					if multiselect and (not selectedOption or type(selectedOption) ~= "table") then
						selectedOption = {}
					end
					local selectedObjects = {}
					local optionCount = 0
					newDropdown.Name = "newDropdown"
					newDropdown.Parent = sectionHolder
					newDropdown.BackgroundColor3 = Color3.new(1, 1, 1)
					newDropdown.BackgroundTransparency = 1
					newDropdown.Size = UDim2.new(1, 0, 0, 42)
					dropdown.Name = "dropdown"
					dropdown.Parent = newDropdown
					dropdown.Active = true
					dropdown.BackgroundColor3 = library.colors.topGradient
					local colored_dropdown_BackgroundColor3 = {dropdown, "BackgroundColor3", "topGradient"}
					colored[1 + #colored] = colored_dropdown_BackgroundColor3
					dropdown.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {dropdown, "BorderColor3", "elementBorder"}
					dropdown.Position = UDim2.fromScale(0.027, 0.45)
					dropdown.Selectable = true
					dropdown.Size = UDim2.fromOffset(206, 18)
					dropdown.Image = "rbxassetid://2454009026"
					dropdown.ImageColor3 = library.colors.bottomGradient
					local colored_dropdown_ImageColor3 = {dropdown, "ImageColor3", "bottomGradient"}
					colored[1 + #colored] = colored_dropdown_ImageColor3
					dropdownInner.Name = "dropdownInner"
					dropdownInner.Parent = dropdown
					dropdownInner.Active = true
					dropdownInner.AnchorPoint = Vector2.new(0.5, 0.5)
					dropdownInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {dropdownInner, "BackgroundColor3", "topGradient"}
					dropdownInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {dropdownInner, "BorderColor3", "elementBorder"}
					dropdownInner.Position = UDim2.fromScale(0.5, 0.5)
					dropdownInner.Selectable = true
					dropdownInner.Size = UDim2.new(1, -4, 1, -4)
					dropdownInner.Image = "rbxassetid://2454009026"
					dropdownInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {dropdownInner, "ImageColor3", "bottomGradient"}
					dropdownToggle.Name = "dropdownToggle"
					dropdownToggle.Parent = dropdown
					dropdownToggle.BackgroundColor3 = Color3.new(1, 1, 1)
					dropdownToggle.BackgroundTransparency = 1
					dropdownToggle.Position = UDim2.fromScale(0.9, 0.17)
					dropdownToggle.Rotation = 90
					dropdownToggle.Size = UDim2.fromOffset(12, 12)
					dropdownToggle.ZIndex = 2
					dropdownToggle.Image = "rbxassetid://71659683"
					dropdownToggle.ImageColor3 = Color3.fromRGB(171, 171, 171)
					dropdownSelection.Name = "dropdownSelection"
					dropdownSelection.Parent = dropdown
					dropdownSelection.Active = true
					dropdownSelection.BackgroundColor3 = Color3.new(1, 1, 1)
					dropdownSelection.BackgroundTransparency = 1
					dropdownSelection.Position = UDim2.new(0.0295)
					dropdownSelection.Selectable = true
					dropdownSelection.Size = UDim2.fromScale(0.97, 1)
					dropdownSelection.ZIndex = 5
					dropdownSelection.Font = Enum.Font.Code
					dropdownSelection.Text = (passed_multiselect == "string" and multiselect) or (multiselect and tostring(blankstring or "Select Item(s)")) or (selectedOption and tostring(selectedOption)) or tostring(blankstring or "No Blank String")
					dropdownSelection.TextColor3 = library.colors.otherElementText
					colored[1 + #colored] = {dropdownSelection, "TextColor3", "otherElementText"}
					dropdownSelection.TextSize = 14
					dropdownSelection.TextXAlignment = Enum.TextXAlignment.Left
					dropdownHeadline.Name = "dropdownHeadline"
					dropdownHeadline.Parent = newDropdown
					dropdownHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
					dropdownHeadline.BackgroundTransparency = 1
					dropdownHeadline.Position = UDim2.fromScale(0.034, 0.03)
					dropdownHeadline.Size = UDim2.fromOffset(167, 11)
					dropdownHeadline.Font = Enum.Font.Code
					dropdownHeadline.Text = (dropdownName and tostring(dropdownName)) or "???"
					dropdownHeadline.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {dropdownHeadline, "TextColor3", "elementText"}
					dropdownHeadline.TextSize = 14
					dropdownHeadline.TextXAlignment = Enum.TextXAlignment.Left
					dropdownHolderFrame.Name = "dropdownHolderFrame"
					dropdownHolderFrame.Parent = newDropdown
					dropdownHolderFrame.Active = true
					dropdownHolderFrame.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {dropdownHolderFrame, "BackgroundColor3", "topGradient"}
					dropdownHolderFrame.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {dropdownHolderFrame, "BorderColor3", "elementBorder"}
					dropdownHolderFrame.Position = UDim2.fromScale(0.025, 1.012)
					dropdownHolderFrame.Selectable = true
					dropdownHolderFrame.Size = UDim2.fromOffset(206, 22)
					dropdownHolderFrame.Visible = false
					dropdownHolderFrame.Image = "rbxassetid://2454009026"
					dropdownHolderFrame.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {dropdownHolderFrame, "ImageColor3", "bottomGradient"}
					dropdownHolderInner.Name = "dropdownHolderInner"
					dropdownHolderInner.Parent = dropdownHolderFrame
					dropdownHolderInner.Active = true
					dropdownHolderInner.AnchorPoint = Vector2.new(0.5, 0.5)
					dropdownHolderInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {dropdownHolderInner, "BackgroundColor3", "topGradient"}
					dropdownHolderInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {dropdownHolderInner, "BorderColor3", "elementBorder"}
					dropdownHolderInner.Position = UDim2.fromScale(0.5, 0.5)
					dropdownHolderInner.Selectable = true
					dropdownHolderInner.Size = UDim2.new(1, -4, 1, -4)
					dropdownHolderInner.Image = "rbxassetid://2454009026"
					dropdownHolderInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {dropdownHolderInner, "ImageColor3", "bottomGradient"}
					realDropdownHolder.Name = "realDropdownHolder"
					realDropdownHolder.Parent = dropdownHolderInner
					realDropdownHolder.BackgroundColor3 = Color3.new(1, 1, 1)
					realDropdownHolder.BackgroundTransparency = 1
					realDropdownHolder.Selectable = false
					realDropdownHolder.Size = UDim2.fromScale(1, 1)
					realDropdownHolder.CanvasSize = UDim2.new()
					realDropdownHolder.ScrollBarThickness = 5
					realDropdownHolder.ScrollingDirection = Enum.ScrollingDirection.Y
					realDropdownHolder.AutomaticCanvasSize = Enum.AutomaticSize.Y
					realDropdownHolder.ScrollBarImageTransparency = 0.5
					realDropdownHolder.ScrollBarImageColor3 = library.colors.section
					colored[1 + #colored] = {realDropdownHolder, "ScrollBarImageColor3", "section"}
					realDropdownHolderList.Name = "realDropdownHolderList"
					realDropdownHolderList.Parent = realDropdownHolder
					realDropdownHolderList.HorizontalAlignment = Enum.HorizontalAlignment.Center
					realDropdownHolderList.SortOrder = Enum.SortOrder.LayoutOrder
					sectionFunctions:Update()
					local showing = false
					local function UpdateDropdownHolder()
						if optionCount >= 6 then
							realDropdownHolder.CanvasSize = UDim2:fromOffset(realDropdownHolderList.AbsoluteContentSize.Y + 2)
						elseif optionCount <= 5 then
							dropdownHolderFrame.Size = UDim2.fromOffset(206, realDropdownHolderList.AbsoluteContentSize.Y + 4)
						end
					end
					local restorezindex = {}
					local Set = (multiselect and function(t, dat)
						if nil == dat and t ~= nil then
							dat = t
						end
						local lastv = library_flags[flagName]
						if not lastv or selectedOption ~= lastv then
							if lastv and type(lastv) == "table" then
								selectedOption = library_flags[flagName]
							else
								library_flags[flagName] = selectedOption
							end
							warn("Attempting to use new table for", flagName, " Please use :Set(), as setting through flags table may cause errors", debug.traceback(""))
							lastv = library_flags[flagName]
						end
						local cloned = {unpack(selectedOption)}
						if not dat then
							if #selectedOption ~= 0 then
								table.clear(selectedOption)
								if callback then
									task.spawn(callback, selectedOption, cloned)
								end
							end
							return selectedOption
						elseif type(dat) ~= "table" then
							warn("Expected table for argument #1 on Set for MultiSelect dropdown. Got", dat, debug.traceback(""))
							return selectedOption
						end
						for k = table.pack(unpack(dat)).n, 1, -1 do
							if dat[k] == nil then
								table.remove(dat, k)
							end
						end
						local proceed = #cloned ~= #dat
						table.clear(selectedOption)
						for k, v in next, dat do
							selectedOption[k] = v
							if not proceed and cloned[k] ~= v then
								proceed = 1
							end
						end
						dropdownSelection.Text = (passed_multiselect == "string" and multiselect) or tostring(blankstring or "Select Item(s)")
						if proceed and callback then
							task.spawn(callback, selectedOption, cloned)
						end
						return selectedOption
					end) or function(t, str)
						if nil == str and t ~= nil then
							str = t
						end
						local last_v = library_flags[flagName]
						selectedOption = str
						library_flags[flagName] = str
						if options.Location then
							options.Location[options.LocationFlag or flagName] = str
						end
						local sstr = (selectedOption and tostring(selectedOption)) or blankstring or "No Blank String"
						if dropdownSelection.Text ~= sstr then
							dropdownSelection.Text = sstr
						end
						if callback and (last_v ~= str or options.AllowDuplicateCalls) then
							task.spawn(callback, str, last_v)
						end
						return str
					end
					if val ~= nil then
						Set(val)
					else
						library_flags[flagName] = selectedOption
						if options.Location then
							options.Location[options.LocationFlag or flagName] = selectedOption
						end
					end
					local function AddOptions(optionsTable)
						if options.Sort then
							local didstuff, dosort = nil, options.Sort
							if type(dosort) == "function" then
								local g, h = pcall(table.sort, optionsTable, dosort)
								if g then
									didstuff = true
								elseif h then
									warn("Error sorting list:", h, debug.traceback(""))
								end
							elseif dosort ~= 1 and dosort ~= true then
								warn("Potential mistake for passed Sort argument:", dosort, debug.traceback(""))
							end
							if not didstuff then
								table.sort(optionsTable, library.defaultSort)
							end
						end
						if blankstring and (optionsTable[1] ~= blankstring or table.find(optionsTable, blankstring, 2)) then
							local exists = table.find(optionsTable, blankstring)
							if exists then
								for _ = 1, 35 do
									table.remove(optionsTable, exists)
									exists = table.find(optionsTable, blankstring)
									if not exists then
										break
									end
								end
							end
							table.insert(optionsTable, 1, blankstring)
						end
						optionCount = 0
						realDropdownHolderList.Parent = nil
						realDropdownHolder:ClearAllChildren()
						realDropdownHolderList.Parent = realDropdownHolder
						for _, v in next, optionsTable do
							optionCount = optionCount + 1
							local newOption = Instance_new("ImageLabel")
							local optionButton = Instance_new("TextButton")
							if selectedOption == v then
								selectedObjects[1] = newOption
								selectedObjects[2] = optionButton
							end
							newOption.Name = "Frame"
							newOption.Parent = realDropdownHolder
							local togged = (not multiselect and selectedOption == v) or (multiselect and table.find(selectedOption, v))
							newOption.BackgroundColor3 = (togged and library.colors.selectedOption) or library.colors.topGradient
							newOption.BorderSizePixel = 0
							newOption.Size = UDim2.fromOffset(202, 18)
							newOption.Image = "rbxassetid://2454009026"
							newOption.ImageColor3 = (togged and library.colors.unselectedOption) or library.colors.bottomGradient
							local stringed = tostring(v)
							optionButton.Name = stringed
							optionButton.Parent = newOption
							optionButton.AnchorPoint = Vector2.new(0.5, 0.5)
							optionButton.BackgroundColor3 = Color3.new(1, 1, 1)
							optionButton.BackgroundTransparency = 1
							optionButton.Position = UDim2.fromScale(0.5, 0.5)
							optionButton.Size = UDim2.new(1, -10, 1)
							optionButton.ZIndex = 5
							optionButton.Font = Enum.Font.Code
							optionButton.Text = (togged and (" " .. stringed)) or stringed
							optionButton.TextColor3 = (togged and library.colors.main) or library.colors.otherElementText
							optionButton.TextSize = 14
							optionButton.TextXAlignment = Enum.TextXAlignment.Left
							library.signals[1 + #library.signals] = optionButton.MouseButton1Click:Connect(function()
								if not library.colorpicker then
									restorezindex[newSection] = restorezindex[newSection] or newSection.ZIndex
									restorezindex[newDropdown] = restorezindex[newDropdown] or newDropdown.ZIndex
									restorezindex[sectionHolder] = restorezindex[sectionHolder] or sectionHolder.ZIndex
									if multiselect then
										local cloned = {unpack(selectedOption)}
										local togged = table.find(selectedOption, v)
										if togged then
											table.remove(selectedOption, togged)
										else
											selectedOption[1 + #selectedOption] = v
										end
										togged = table.find(selectedOption, v)
										optionButton.Text = (togged and (" " .. stringed)) or stringed
										newOption.BackgroundColor3 = (togged and library.colors.selectedOption) or library.colors.topGradient
										newOption.ImageColor3 = (togged and library.colors.unselectedOption) or library.colors.bottomGradient
										optionButton.TextColor3 = (togged and library.colors.main) or library.colors.otherElementText
										dropdownSelection.Text = (passed_multiselect == "string" and multiselect) or tostring(blankstring or "Select Item(s)")
										if callback then
											task.spawn(callback, selectedOption, cloned)
										end
										if togged then
											if addcallback then
												task.spawn(addcallback, v, selectedOption)
											end
										elseif delcallback then
											task.spawn(delcallback, v, selectedOption)
										end
										if modcallback then
											task.spawn(modcallback, v, togged, selectedOption)
										end
										if #selectedOption == 0 and clrcallback then
											task.spawn(clrcallback, selectedOption, cloned)
										end
										return
									else
										if selectedOption ~= v then
											local last_v = library_flags[flagName]
											selectedObjects[1].BackgroundColor3 = library.colors.topGradient
											selectedObjects[1].ImageColor3 = library.colors.bottomGradient
											selectedObjects[2].Text = selectedObjects[2].Name
											selectedObjects[2].TextColor3 = library.colors.otherElementText
											selectedOption = v
											dropdownSelection.Text = stringed
											selectedObjects[1] = newOption
											selectedObjects[2] = optionButton
											newOption.BackgroundColor3 = library.colors.selectedOption
											newOption.ImageColor3 = library.colors.unselectedOption
											optionButton.Text = " " .. stringed
											optionButton.TextColor3 = library.colors.main
											dropdownHolderFrame.Visible = false
											dropdownToggle.Rotation = 90
											dropdownEnabled = false
											newDropdown.ZIndex = 1
											colored_dropdown_BackgroundColor3[3] = "topGradient"
											colored_dropdown_BackgroundColor3[4] = nil
											colored_dropdown_ImageColor3[3] = "bottomGradient"
											colored_dropdown_ImageColor3[4] = nil
											tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
												BackgroundColor3 = library.colors.topGradient,
												ImageColor3 = library.colors.bottomGradient
											}):Play()
											library_flags[flagName] = selectedOption
											if options.Location then
												options.Location[options.LocationFlag or flagName] = selectedOption
											end
											submenuOpen = nil
											showing = false
											if callback then
												task.spawn(callback, selectedOption, last_v)
											end
										else
											showing = false
											submenuOpen = nil
											dropdownToggle.Rotation = 90
											newDropdown.ZIndex = 1
											sectionHolder.ZIndex = 1
											colored_dropdown_BackgroundColor3[3] = "topGradient"
											colored_dropdown_BackgroundColor3[4] = nil
											colored_dropdown_ImageColor3[3] = "bottomGradient"
											colored_dropdown_ImageColor3[4] = nil
											tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
												BackgroundColor3 = library.colors.topGradient,
												ImageColor3 = library.colors.bottomGradient
											}):Play()
											dropdownHolderFrame.Visible = false
										end
									end
									for ins, z in next, restorezindex do
										ins.ZIndex = z
									end
								end
							end)
							library.signals[1 + #library.signals] = optionButton.MouseEnter:Connect(function()
								tweenService:Create(newOption, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
									BackgroundColor3 = library.colors.hoveredOptionTop,
									ImageColor3 = library.colors.hoveredOptionBottom
								}):Play()
							end)
							library.signals[1 + #library.signals] = optionButton.MouseLeave:Connect(function()
								local togged = (not multiselect and selectedOption == v) or (multiselect and table.find(selectedOption, v))
								tweenService:Create(newOption, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
									BackgroundColor3 = (togged and library.colors.selectedOption) or library.colors.topGradient,
									ImageColor3 = (togged and library.colors.unselectedOption) or library.colors.bottomGradient
								}):Play()
							end)
							UpdateDropdownHolder()
						end
					end
					local precisionscrolling, update = nil
					local function display(dropdownEnabled)
						list = resolvelist()
						if dropdownEnabled then
							AddOptions(list)
							submenuOpen = dropdown
							dropdownToggle.Rotation = 270
							restorezindex[newSection] = restorezindex[newSection] or newSection.ZIndex
							restorezindex[newDropdown] = restorezindex[newDropdown] or newDropdown.ZIndex
							restorezindex[sectionHolder] = restorezindex[sectionHolder] or sectionHolder.ZIndex
							newSection.ZIndex = 50 + newSection.ZIndex
							newDropdown.ZIndex = 2
							sectionHolder.ZIndex = 2
							colored_dropdown_BackgroundColor3[3] = "main"
							colored_dropdown_BackgroundColor3[4] = 1.5
							colored_dropdown_ImageColor3[3] = "main"
							colored_dropdown_ImageColor3[4] = 2.5
							tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = darkenColor(library.colors.main, 1.5),
								ImageColor3 = darkenColor(library.colors.main, 2.5)
							}):Play()
							dropdownHolderFrame.Visible = true
							if not options.DisablePrecisionScrolling then
								local upkey = options.ScrollUpButton or library.scrollupbutton or shared.scrollupbutton or Enum.KeyCode.Up
								local downkey = options.ScrollDownButton or library.scrolldownbutton or shared.scrolldownbutton or Enum.KeyCode.Down
								precisionscrolling = (precisionscrolling and precisionscrolling:Disconnect() and nil) or userInputService.InputBegan:Connect(function(input)
									if input.UserInputType == Enum.UserInputType.Keyboard then
										local code = input.KeyCode
										local isup = code == upkey
										local isdown = code == downkey
										if isup or isdown then
											local txt = userInputService:GetFocusedTextBox()
											if not txt or txt == dropdownSelection then
												while wait_check() and userInputService:IsKeyDown(code) do
													realDropdownHolder.CanvasPosition = Vector2:new(math.clamp(realDropdownHolder.CanvasPosition.Y + ((isup and -5) or 5), 0, realDropdownHolder.AbsoluteCanvasSize.Y))
												end
											end
										end
									end
								end)
								library.signals[1 + #library.signals] = precisionscrolling
							end
						else
							submenuOpen = nil
							dropdownToggle.Rotation = 90
							colored_dropdown_BackgroundColor3[3] = "topGradient"
							colored_dropdown_BackgroundColor3[4] = nil
							colored_dropdown_ImageColor3[3] = "bottomGradient"
							colored_dropdown_ImageColor3[4] = nil
							tweenService:Create(dropdown, TweenInfo.new(0.35, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = library.colors.topGradient,
								ImageColor3 = library.colors.bottomGradient
							}):Play()
							dropdownHolderFrame.Visible = false
							for ins, z in next, restorezindex do
								ins.ZIndex = z
							end
							precisionscrolling = (precisionscrolling and precisionscrolling:Disconnect() and nil) or nil
						end
						if not multiselect and (not next(list) or not table.find(list, library_flags[flagName])) then
							Set(list[1])
						end
						showing = dropdownEnabled
						if showing or dropdownEnabled then
						else
							delay(0.01, update)
						end
					end
					library.signals[1 + #library.signals] = newDropdown.InputEnded:Connect(function(input)
						if not library.colorpicker and input.UserInputType == Enum.UserInputType.MouseButton1 then
							showing = not showing
							display(showing)
						end
					end)
					library.signals[1 + #library.signals] = newDropdown.MouseEnter:Connect(function()
						colored_dropdown_BackgroundColor3[3] = "main"
						colored_dropdown_BackgroundColor3[4] = 1.5
						colored_dropdown_ImageColor3[3] = "main"
						colored_dropdown_ImageColor3[4] = 2.5
						tweenService:Create(dropdown, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = darkenColor(library.colors.main, 1.5),
							ImageColor3 = darkenColor(library.colors.main, 2.5)
						}):Play()
					end)
					library.signals[1 + #library.signals] = newDropdown.MouseLeave:Connect(function()
						if not dropdownEnabled then
							colored_dropdown_BackgroundColor3[3] = "topGradient"
							colored_dropdown_BackgroundColor3[4] = nil
							colored_dropdown_ImageColor3[3] = "bottomGradient"
							colored_dropdown_ImageColor3[4] = nil
							tweenService:Create(dropdown, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = library.colors.topGradient,
								ImageColor3 = library.colors.bottomGradient
							}):Play()
						end
					end)
					library.signals[1 + #library.signals] = dropdownToggle.MouseButton1Click:Connect(function()
						if not library.colorpicker then
							showing = not showing
							display(showing)
						end
					end)
					AddOptions(list)
					local default = library_flags[flagName]
					function update()
						dropdownName, callback = options.Name or dropdownName, options.Callback
						local sstr = (passed_multiselect == "string" and multiselect) or (library_flags[flagName] and tostring(library_flags[flagName])) or (selectedOption and tostring(selectedOption)) or blankstring or "nil"
						if dropdownSelection.Text ~= sstr then
							dropdownSelection.Text = sstr
						end
						dropdownHeadline.Text = (dropdownName and tostring(dropdownName)) or "???"
						return sstr
					end
					local function validate(fallbackValue)
						if list and table.find(list, library_flags[flagName]) then
							update()
							return true
						end
						if fallbackValue ~= nil then
							if fallbackValue == "__DEFAULT" then
								fallbackValue = fallbackValue
							end
						else
							fallbackValue = list[1]
						end
						return Set((multiselect and {fallbackValue}) or fallbackValue)
					end
					local objectdata = {
						Options = options,
						Name = flagName,
						Flag = flagName,
						Type = "Dropdown",
						Default = default,
						Parent = sectionFunctions,
						Instance = dropdownSelection,
						Get = function()
							return library_flags[flagName]
						end,
						Set = Set,
						Remove = function()
							if newDropdown then
								newDropdown.Parent = nil
								sectionFunctions:Update()
							end
						end,
						RawSet = ((multiselect and function(t, dat)
							if nil == dat and t ~= nil then
								dat = t
							end
							local lastv = library_flags[flagName]
							if not lastv or selectedOption ~= lastv then
								if lastv and type(lastv) == "table" then
									selectedOption = library_flags[flagName]
								else
									library_flags[flagName] = selectedOption
								end
								warn("Attempting to use new table for", flagName, " Please use :Set(), as setting through flags table may cause errors", debug.traceback(""))
								lastv = library_flags[flagName]
							end
							local cloned = {unpack(selectedOption)}
							if not dat then
								if #selectedOption ~= 0 then
									table.clear(selectedOption)
								end
								return selectedOption
							elseif type(dat) ~= "table" then
								warn("Expected table for argument #1 on Set for MultiSelect dropdown. Got", dat, debug.traceback(""))
								return selectedOption
							end
							for k = table.pack(unpack(dat)).n, 1, -1 do
								if dat[k] == nil then
									table.remove(dat, k)
								end
							end
							table.clear(selectedOption)
							for k, v in next, dat do
								selectedOption[k] = v
							end
							return selectedOption
						end) or function(t, str)
							if nil == str and t ~= nil then
								str = t
							end
							selectedOption = str
							library_flags[flagName] = str
							if options.Location then
								options.Location[options.LocationFlag or flagName] = str
							end
							update()
							return str
						end),
						Update = update,
						Reset = function()
							return Set(nil, default)
						end
					}
					function objectdata.UpdateList(t, listt, updateValues)
						if (nil == listt and t ~= nil) or (type(t) == "table" and type(listt) ~= "table") then
							listt, updateValues = t, listt
						end
						if listt == objectdata then
							listt = nil
						end
						resolvelist = getresolver(listt or options.List, options.Filter, options.Method)
						list = resolvelist()
						if updateValues then
							validate()
						end
						if showing then
							display(false)
							display(true)
						end
						return list
					end
					tabFunctions.Flags[flagName], sectionFunctions.Flags[flagName], elements[flagName] = objectdata, objectdata, objectdata
					return objectdata
				end
				sectionFunctions.AddDropDown = sectionFunctions.AddDropdown
				sectionFunctions.NewDropDown = sectionFunctions.AddDropdown
				sectionFunctions.NewDropdown = sectionFunctions.AddDropdown
				sectionFunctions.CreateDropdown = sectionFunctions.AddDropdown
				sectionFunctions.CreateDropdown = sectionFunctions.AddDropdown
				sectionFunctions.DropDown = sectionFunctions.AddDropdown
				sectionFunctions.Dropdown = sectionFunctions.AddDropdown
				sectionFunctions.DD = sectionFunctions.AddDropdown
				sectionFunctions.Dd = sectionFunctions.AddDropdown
				function sectionFunctions:AddColorpicker(options, ...)
					options = (options and type(options) == "string" and resolvevararg("Colorpicker", options, ...)) or options
					if options.Random == true then
						options.Value = "random"
					elseif options.Rainbow == true then
						options.Value = "rainbow"
					end
					local colorPickerName, presetColor, callback, flagName = assert(options.Name, "Missing Name for new colorpicker."), options.Value, options.Callback, options.Flag or (function()
						library.unnamedcolorpicker = 1 + (library.unnamedcolorpicker or 0)
						return "Colorpicker" .. tostring(library.unnamedcolorpicker)
					end)()
					if elements[flagName] ~= nil then
						warn(debug.traceback("Warning! Re-used flag '" .. flagName .. "'", 3))
					end
					local designers = options.__designer
					options.__designer = nil
					local rainbowColorMode = false
					if presetColor == "random" then
						presetColor = Color3.new(math.random(), math.random(), math.random())
					elseif presetColor == "rainbow" then
						presetColor = Color3.new(1, 1, 1)
						rainbowColorMode = true
					end
					local newColorPicker = Instance_new("Frame")
					local colorPicker = Instance_new("ImageLabel")
					local colorPickerInner = Instance_new("ImageLabel")
					local colorPickerHeadline = Instance_new("TextLabel")
					local colorPickerButton = Instance_new("TextButton")
					local colorPickerHolderFrame = Instance_new("ImageLabel")
					local colorPickerHolderInner = Instance_new("ImageLabel")
					local color = Instance_new("ImageLabel")
					local selectorColor = Instance_new("Frame")
					local hue = Instance_new("ImageLabel")
					local hueGradient = Instance_new("UIGradient")
					local selectorHue = Instance_new("Frame")
					local randomColor = Instance_new("ImageLabel")
					local randomColorInner = Instance_new("ImageLabel")
					local randomColorButton = Instance_new("ImageButton")
					local hexInputBox = Instance_new("TextBox")
					local hexInput = Instance_new("ImageLabel")
					local hexInputInner = Instance_new("ImageLabel")
					local rainbow = Instance_new("ImageLabel")
					local rainbowInner = Instance_new("ImageLabel")
					local rainbowButton = Instance_new("ImageButton")
					local startingColor = presetColor or Color3.new(1, 1, 1)
					local colorPickerEnabled = false
					local colorH, colorS, colorV = 1, 1, 1
					local colorInput, hueInput = nil, nil
					local oldBackgroundColor = Color3.new()
					local oldImageColor = oldBackgroundColor
					local oldColor = oldBackgroundColor
					local rainbowColorValue = 0
					newColorPicker.Name = "newColorPicker"
					newColorPicker.Parent = sectionHolder
					newColorPicker.BackgroundColor3 = Color3.new(1, 1, 1)
					newColorPicker.BackgroundTransparency = 1
					newColorPicker.Size = UDim2.new(1, 0, 0, 19)
					colorPicker.Name = "colorPicker"
					colorPicker.Parent = newColorPicker
					colorPicker.Active = true
					colorPicker.BackgroundColor3 = library.colors.topGradient
					local colored_colorPicker_BackgroundColor3 = {colorPicker, "BackgroundColor3", "topGradient"}
					colored[1 + #colored] = colored_colorPicker_BackgroundColor3
					colorPicker.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {colorPicker, "BorderColor3", "elementBorder"}
					colorPicker.Position = UDim2.fromScale(0.842, 0.113)
					colorPicker.Selectable = true
					colorPicker.Size = UDim2.fromOffset(24, 12)
					colorPicker.Image = "rbxassetid://2454009026"
					colorPicker.ImageColor3 = library.colors.bottomGradient
					local colored_colorPicker_ImageColor3 = {colorPicker, "ImageColor3", "bottomGradient"}
					colored[1 + #colored] = colored_colorPicker_ImageColor3
					colorPickerInner.Name = "colorPickerInner"
					colorPickerInner.Parent = colorPicker
					colorPickerInner.Active = true
					colorPickerInner.AnchorPoint = Vector2.new(0.5, 0.5)
					colorPickerInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {colorPickerInner, "BorderColor3", "elementBorder"}
					colorPickerInner.Position = UDim2.fromScale(0.5, 0.5)
					colorPickerInner.Selectable = true
					colorPickerInner.Size = UDim2.new(1, -4, 1, -4)
					colorPickerInner.Image = "rbxassetid://2454009026"
					colorPickerInner.BackgroundColor3 = darkenColor(startingColor, 1.5)
					colorPickerInner.ImageColor3 = darkenColor(startingColor, 2.5)
					colorPickerHeadline.Name = "colorPickerHeadline"
					colorPickerHeadline.Parent = newColorPicker
					colorPickerHeadline.BackgroundColor3 = Color3.new(1, 1, 1)
					colorPickerHeadline.BackgroundTransparency = 1
					colorPickerHeadline.Position = UDim2.fromScale(0.034, 0.113)
					colorPickerHeadline.Size = UDim2.fromOffset(173, 11)
					colorPickerHeadline.Font = Enum.Font.Code
					colorPickerHeadline.Text = colorPickerName or "???"
					colorPickerHeadline.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {colorPickerHeadline, "TextColor3", "elementText"}
					colorPickerHeadline.TextSize = 14
					colorPickerHeadline.TextXAlignment = Enum.TextXAlignment.Left
					colorPickerButton.Name = "colorPickerButton"
					colorPickerButton.Parent = newColorPicker
					colorPickerButton.BackgroundColor3 = Color3.new(1, 1, 1)
					colorPickerButton.BackgroundTransparency = 1
					colorPickerButton.Size = UDim2.fromScale(1, 1)
					colorPickerButton.ZIndex = 5
					colorPickerButton.Font = Enum.Font.SourceSans
					colorPickerButton.Text = ""
					colorPickerButton.TextColor3 = Color3.new()
					colorPickerButton.TextSize = 14
					colorPickerButton.TextTransparency = 1
					colorPickerButton.BorderColor3 = library.colors.elementBorder
					local colored_colorPickerButton_BorderColor3 = {colorPickerButton, "BorderColor3", "elementBorder"}
					colored[1 + #colored] = colored_colorPickerButton_BorderColor3
					local function UpdateColorPicker(force, rainbsow)
						local last_vv = library_flags[flagName]
						local newColor = force or Color3.fromHSV(colorH, colorS, colorV)
						if not force then
							colorH, colorS, colorV = newColor:ToHSV()
						end
						colorPickerInner.BackgroundColor3 = darkenColor(newColor, 1.5)
						colorPickerInner.ImageColor3 = darkenColor(newColor, 2.5)
						color.BackgroundColor3 = Color3.fromHSV(colorH, 1, 1)
						library_flags[flagName] = newColor
						if options.Location then
							options.Location[options.LocationFlag or flagName] = newColor
						end
						hexInputBox.Text = Color3ToHex(newColor)
						if force then
							color.BackgroundColor3 = force
							selectorColor.Position = UDim2.new(force and select(3, Color3.toHSV(force)))
						end
						local pos = 1 - (Color3.toHSV(newColor))
						local scalex = selectorHue.Position.X.Scale
						if scalex ~= pos and not (((pos == 0) or (pos == 1)) and ((scalex == 1) or (scalex == 0))) then
							selectorHue.Position = UDim2.new(pos)
						end
						if callback and last_vv ~= newColor then
							task.spawn(callback, newColor, last_vv, rainbsow)
						end
					end
					library.signals[1 + #library.signals] = colorPickerButton.MouseButton1Click:Connect(function()
						if submenuOpen == colorPicker or submenuOpen == nil then
							colorPickerEnabled = not colorPickerEnabled
							library.colorpicker = colorPickerEnabled
							colorPickerHolderFrame.Visible = colorPickerEnabled
							if colorPickerEnabled then
								for _, v in next, colorpickerconflicts do
									v.Visible = false
								end
								submenuOpen = colorPicker
								newColorPicker.ZIndex = 2
								newSection.ZIndex = 100 + newSection.ZIndex
								colorPickerButton.BorderColor3 = library.colors.main
								colored_colorPickerButton_BorderColor3[3] = "main"
								UpdateColorPicker()
							else
								for _, v in next, colorpickerconflicts do
									v.Visible = true
								end
								submenuOpen = nil
								newColorPicker.ZIndex = 0
								newSection.ZIndex = newSection.ZIndex - 100
								colorPickerButton.BorderColor3 = library.colors.elementBorder
								colored_colorPickerButton_BorderColor3[3] = "elementBorder"
							end
						end
					end)
					colorPickerHolderFrame.Name = "colorPickerHolderFrame"
					colorPickerHolderFrame.Parent = newColorPicker
					colorPickerHolderFrame.Active = true
					colorPickerHolderFrame.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {colorPickerHolderFrame, "BackgroundColor3", "topGradient"}
					colorPickerHolderFrame.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {colorPickerHolderFrame, "BorderColor3", "elementBorder"}
					colorPickerHolderFrame.Selectable = true
					colorPickerHolderFrame.Position = UDim2.fromScale(0.025, 1.012)
					colorPickerHolderFrame.Size = UDim2.fromOffset(206, 250)
					if math.ceil(colorPickerHolderFrame.AbsolutePosition.Y + colorPickerHolderFrame.AbsoluteSize.Y) > floor(newTabHolder.AbsoluteSize.Y + newTabHolder.AbsolutePosition.Y) then
						colorPickerHolderFrame.Position = UDim2.new(0.025, 0, 1.012, -colorPickerHolderFrame.AbsoluteSize.Y - colorPickerButton.AbsoluteSize.Y - 2)
					end
					colorPickerHolderFrame.Visible = false
					colorPickerHolderFrame.Image = "rbxassetid://2454009026"
					colorPickerHolderFrame.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {colorPickerHolderFrame, "ImageColor3", "bottomGradient"}
					colorPickerHolderInner.Name = "colorPickerHolderInner"
					colorPickerHolderInner.Parent = colorPickerHolderFrame
					colorPickerHolderInner.Active = true
					colorPickerHolderInner.AnchorPoint = Vector2.new(0.5, 0.5)
					colorPickerHolderInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {colorPickerHolderInner, "BackgroundColor3", "topGradient"}
					colorPickerHolderInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {colorPickerHolderInner, "BorderColor3", "elementBorder"}
					colorPickerHolderInner.Position = UDim2.fromScale(0.5, 0.5)
					colorPickerHolderInner.Selectable = true
					colorPickerHolderInner.Size = UDim2.new(1, -4, 1, -4)
					colorPickerHolderInner.Image = "rbxassetid://2454009026"
					colorPickerHolderInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {colorPickerHolderInner, "ImageColor3", "bottomGradient"}
					color.Name = "color"
					color.Parent = colorPickerHolderInner
					color.BackgroundColor3 = startingColor
					color.BorderSizePixel = 0
					color.Position = UDim2.fromOffset(5, 5)
					color.Size = UDim2.new(1, -10, 0, 192)
					color.Image = "rbxassetid://4155801252"
					selectorColor.Name = "selectorColor"
					selectorColor.Parent = color
					selectorColor.AnchorPoint = Vector2.new(0.5, 0.5)
					selectorColor.BackgroundColor3 = Color3.fromRGB(144, 144, 144)
					selectorColor.BorderColor3 = Color3.fromRGB(69, 65, 70)
					selectorColor.Position = UDim2.new(startingColor and select(3, Color3.toHSV(startingColor)))
					selectorColor.Size = UDim2.fromOffset(4, 4)
					hue.Name = "hue"
					hue.Parent = colorPickerHolderInner
					hue.BackgroundColor3 = Color3.new(1, 1, 1)
					hue.BorderSizePixel = 0
					hue.Position = UDim2.fromOffset(5, 202)
					hue.Size = UDim2.new(1, -10, 0, 14)
					hue.Image = "rbxassetid://3570695787"
					hue.ScaleType = Enum.ScaleType.Slice
					hue.SliceScale = 0.01
					hueGradient.Color = ColorSequence.new({ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 4)), ColorSequenceKeypoint.new(0.17, Color3.fromRGB(235, 7, 255)), ColorSequenceKeypoint.new(0.33, Color3:fromRGB(9, 189)), ColorSequenceKeypoint.new(0.5, Color3:fromRGB(193, 196)), ColorSequenceKeypoint.new(0.66, Color3:new(1)), ColorSequenceKeypoint.new(0.84, Color3.fromRGB(255, 247)), ColorSequenceKeypoint.new(1, Color3.new(1))})
					hueGradient.Name = "hueGradient"
					hueGradient.Parent = hue
					selectorHue.Name = "selectorHue"
					selectorHue.Parent = hue
					selectorHue.BackgroundColor3 = Color3:fromRGB(125, 255)
					selectorHue.BackgroundTransparency = 0.2
					selectorHue.BorderColor3 = Color3:fromRGB(84, 91)
					selectorHue.Position = UDim2.new(1 - (Color3.toHSV(startingColor)))
					selectorHue.Size = UDim2:new(2, 1)
					hexInput.Name = "hexInput"
					hexInput.Parent = colorPickerHolderInner
					hexInput.Active = true
					hexInput.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {hexInput, "BackgroundColor3", "topGradient"}
					hexInput.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {hexInput, "BorderColor3", "elementBorder"}
					hexInput.Position = UDim2.fromOffset(5, 223)
					hexInput.Selectable = true
					hexInput.Size = UDim2.fromOffset(150, 18)
					hexInput.Image = "rbxassetid://2454009026"
					hexInput.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {hexInput, "ImageColor3", "bottomGradient"}
					hexInputInner.Name = "hexInputInner"
					hexInputInner.Parent = hexInput
					hexInputInner.Active = true
					hexInputInner.AnchorPoint = Vector2.new(0.5, 0.5)
					hexInputInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {hexInputInner, "BackgroundColor3", "topGradient"}
					hexInputInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {hexInputInner, "BorderColor3", "elementBorder"}
					hexInputInner.Position = UDim2.fromScale(0.5, 0.5)
					hexInputInner.Selectable = true
					hexInputInner.Size = UDim2.new(1, -4, 1, -4)
					hexInputInner.Image = "rbxassetid://2454009026"
					hexInputInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {hexInputInner, "ImageColor3", "bottomGradient"}
					hexInputBox.Name = "hexInputBox"
					hexInputBox.Parent = hexInput
					hexInputBox.BackgroundColor3 = Color3.new(1, 1, 1)
					hexInputBox.BackgroundTransparency = 1
					hexInputBox.Size = UDim2.fromScale(1, 1)
					hexInputBox.ZIndex = 5
					hexInputBox.Font = Enum.Font.Code
					hexInputBox.PlaceholderText = "Hex Input"
					hexInputBox.Text = Color3ToHex(startingColor)
					hexInputBox.TextColor3 = library.colors.elementText
					colored[1 + #colored] = {hexInputBox, "TextColor3", "elementText"}
					hexInputBox.TextSize = 14
					hexInputBox.ClearTextOnFocus = false
					randomColor.Name = "randomColor"
					randomColor.Parent = colorPickerHolderInner
					randomColor.Active = true
					randomColor.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {randomColor, "BackgroundColor3", "topGradient"}
					randomColor.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {randomColor, "BorderColor3", "elementBorder"}
					randomColor.Position = UDim2.fromOffset(158, 223)
					randomColor.Selectable = true
					randomColor.Size = UDim2.fromOffset(18, 18)
					randomColor.Image = "rbxassetid://2454009026"
					randomColor.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {randomColor, "ImageColor3", "bottomGradient"}
					randomColorInner.Name = "randomColorInner"
					randomColorInner.Parent = randomColor
					randomColorInner.Active = true
					randomColorInner.AnchorPoint = Vector2.new(0.5, 0.5)
					randomColorInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {randomColorInner, "BackgroundColor3", "topGradient"}
					randomColorInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {randomColorInner, "BorderColor3", "elementBorder"}
					randomColorInner.Position = UDim2.fromScale(0.5, 0.5)
					randomColorInner.Selectable = true
					randomColorInner.Size = UDim2.new(1, -4, 1, -4)
					randomColorInner.Image = "rbxassetid://2454009026"
					randomColorInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {randomColorInner, "ImageColor3", "bottomGradient"}
					randomColorButton.Name = "randomColorButton"
					randomColorButton.Parent = randomColor
					randomColorButton.BackgroundColor3 = Color3.new(1, 1, 1)
					randomColorButton.BackgroundTransparency = 1
					randomColorButton.Size = UDim2.fromScale(1, 1)
					randomColorButton.ZIndex = 5
					randomColorButton.Image = ""
					rainbow.Name = "rainbow"
					rainbow.Parent = colorPickerHolderInner
					rainbow.Active = true
					rainbow.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {rainbow, "BackgroundColor3", "topGradient"}
					rainbow.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {rainbow, "BorderColor3", "elementBorder"}
					rainbow.Position = UDim2.fromOffset(158 + 18 + 4, 223)
					rainbow.Selectable = true
					rainbow.Size = UDim2.fromOffset(18, 18)
					rainbow.Image = ""
					rainbow.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {rainbow, "ImageColor3", "bottomGradient"}
					rainbowInner.Name = "rainbowInner"
					rainbowInner.Parent = randomColor
					rainbowInner.Active = true
					rainbowInner.AnchorPoint = Vector2.new(0.5, 0.5)
					rainbowInner.BackgroundColor3 = library.colors.topGradient
					colored[1 + #colored] = {rainbowInner, "BackgroundColor3", "topGradient"}
					rainbowInner.BorderColor3 = library.colors.elementBorder
					colored[1 + #colored] = {rainbowInner, "BorderColor3", "elementBorder"}
					rainbowInner.Position = UDim2.fromScale(0.5, 0.5)
					rainbowInner.Selectable = true
					rainbowInner.Size = UDim2.new(1, -4, 1, -4)
					rainbowInner.Image = ""
					rainbowInner.ImageColor3 = library.colors.bottomGradient
					colored[1 + #colored] = {rainbowInner, "ImageColor3", "bottomGradient"}
					rainbowButton.Name = "rainbowButton"
					rainbowButton.Parent = rainbow
					rainbowButton.BackgroundColor3 = Color3.new(1, 1, 1)
					rainbowButton.BackgroundTransparency = 1
					rainbowButton.Size = UDim2.fromScale(1, 1)
					rainbowButton.ZIndex = 5
					rainbowButton.Image = ""
					local indexwith = (designers and "rainbows") or "rainbowsg"
					local function setrainbow(t, rainbowColorMod)
						if nil == rainbowColorMod and t ~= nil then
							rainbowColorMod = t
						end
						if rainbowColorMod == nil or type(rainbowColorMod) ~= "boolean" then
							rainbowColorMode = not rainbowColorMode
						else
							rainbowColorMode = rainbowColorMod
						end
						if colorInput then
							colorInput = (colorInput:Disconnect() and nil) or nil
						end
						if hueInput then
							hueInput = (hueInput:Disconnect() and nil) or nil
						end
						pcall(function()
							if destroyrainbows and library.rainbows <= 0 then
								destroyrainbows = nil
							end
							if destroyrainbowsg and library.rainbowsg <= 0 then
								destroyrainbowsg = nil
							end
						end)
						if rainbowColorMode then
							pcall(function()
								if not library.rainbowflags[flagName] then
									library[indexwith] = 1 + library[indexwith]
								end
								library.rainbowflags[flagName] = true
								oldImageColor = colorPickerInner.ImageColor3
								oldBackgroundColor = colorPickerInner.BackgroundColor3
								oldColor = color.BackgroundColor3
								pcall(function()
									local common_float = 1 / 255
									while wait_check() and rainbowColorMode and (options.Value == "rainbow" or ((not designers and not destroyrainbowsg) or (designers and not destroyrainbows))) do
										rainbowColorValue = common_float + rainbowColorValue
										if rainbowColorValue > 1 then
											rainbowColorValue = 0
										end
										colorH = rainbowColorValue
										UpdateColorPicker(Color3.fromHSV(rainbowColorValue, 1, 1), true)
									end
								end)
							end)
							pcall(function()
								rainbowColorMode = nil
								if library.rainbowflags[flagName] then
									library[indexwith] = library[indexwith] - 1
								end
								library.rainbowflags[flagName] = nil
							end)
						end
						UpdateColorPicker(library_flags[flagName])
					end
					library.signals[1 + #library.signals] = randomColorButton.MouseButton1Click:Connect(function()
						if rainbowColorMode then
							setrainbow(false)
						end
						UpdateColorPicker(Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255)))
					end)
					library.signals[1 + #library.signals] = rainbowButton.MouseButton1Click:Connect(setrainbow)
					sectionFunctions:Update()
					library.signals[1 + #library.signals] = newColorPicker.MouseEnter:Connect(function()
						tweenService:Create(colorPicker, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
							BackgroundColor3 = darkenColor(library.colors.main, 1.5),
							ImageColor3 = darkenColor(library.colors.main, 2.5)
						}):Play()
						colored_colorPicker_BackgroundColor3[3] = "main"
						colored_colorPicker_BackgroundColor3[4] = 1.5
						colored_colorPicker_ImageColor3[3] = "main"
						colored_colorPicker_ImageColor3[4] = 2.5
					end)
					library.signals[1 + #library.signals] = newColorPicker.MouseLeave:Connect(function()
						if not colorPickerEnabled then
							tweenService:Create(colorPicker, TweenInfo.new(0.25, library.configuration.easingStyle, library.configuration.easingDirection), {
								BackgroundColor3 = library.colors.topGradient,
								ImageColor3 = library.colors.bottomGradient
							}):Play()
							colored_colorPicker_BackgroundColor3[3] = "topGradient"
							colored_colorPicker_BackgroundColor3[4] = nil
							colored_colorPicker_ImageColor3[3] = "bottomGradient"
							colored_colorPicker_ImageColor3[4] = nil
						end
					end)
					hexInputBox.FocusLost:Connect(function()
						if #hexInputBox.Text > 5 then
							local last_vv = library_flags[flagName]
							local not_fucked, clr = pcall(Color3FromHex, hexInputBox.Text)
							UpdateColorPicker((not_fucked and clr) or last_vv)
						end
					end)
					colorH = 1 - (math.clamp(selectorHue.AbsolutePosition.X - hue.AbsolutePosition.X, 0, hue.AbsoluteSize.X) / hue.AbsoluteSize.X)
					colorS = (math.clamp(selectorColor.AbsolutePosition.X - color.AbsolutePosition.X, 0, color.AbsoluteSize.X) / color.AbsoluteSize.X)
					colorV = 1 - (math.clamp(selectorColor.AbsolutePosition.Y - color.AbsolutePosition.Y, 0, color.AbsoluteSize.Y) / color.AbsoluteSize.Y)
					library.signals[1 + #library.signals] = color.InputBegan:Connect(function(input)
						if input.UserInputType == Enum.UserInputType.MouseButton1 then
							isDraggingSomething = true
							colorInput = (colorInput and colorInput:Disconnect() and nil) or runService.RenderStepped:Connect(function()
								local colorX = (math.clamp(mouse.X - color.AbsolutePosition.X, 0, color.AbsoluteSize.X) / color.AbsoluteSize.X)
								local colorY = (math.clamp(mouse.Y - color.AbsolutePosition.Y, 0, color.AbsoluteSize.Y) / color.AbsoluteSize.Y)
								selectorColor.Position = UDim2.fromScale(colorX, colorY)
								colorS = colorX
								colorV = 1 - colorY
								UpdateColorPicker()
							end)
							library.signals[1 + #library.signals] = colorInput
						end
					end)
					library.signals[1 + #library.signals] = color.InputEnded:Connect(function(input)
						if input.UserInputType == Enum.UserInputType.MouseButton1 then
							if colorInput then
								isDraggingSomething = false
								colorInput:Disconnect()
							end
						end
					end)
					library.signals[1 + #library.signals] = hue.InputBegan:Connect(function(input)
						if input.UserInputType == Enum.UserInputType.MouseButton1 then
							if hueInput then
								hueInput:Disconnect()
							end
							isDraggingSomething = true
							hueInput = runService.RenderStepped:Connect(function()
								local hueX = math.clamp(mouse.X - hue.AbsolutePosition.X, 0, hue.AbsoluteSize.X) / hue.AbsoluteSize.X
								selectorHue.Position = UDim2.new(hueX)
								colorH = 1 - hueX
								UpdateColorPicker()
							end)
							library.signals[1 + #library.signals] = hueInput
						end
					end)
					library.signals[1 + #library.signals] = hue.InputEnded:Connect(function(input)
						if hueInput and input.UserInputType == Enum.UserInputType.MouseButton1 then
							isDraggingSomething = false
							hueInput:Disconnect()
						end
					end)
					if rainbowColorMode then
						spawn(function()
							rainbowColorMode = nil
							setrainbow(true)
						end)
					end
					local function Set(t, clr)
						if clr == nil and t ~= nil then
							clr = t
						end
						if clr == "rainbow" then
							if not rainbowColorMode then
								task.spawn(setrainbow, true)
							end
							return
						elseif clr == "random" then
							clr = Color3.new(math.random(), math.random(), math.random())
						elseif type(clr) == "string" and tonumber(clr, 16) then
							clr = Color3FromHex(clr)
						end
						task.spawn(setrainbow, false)
						local last_v = library_flags[flagName]
						library_flags[flagName] = clr
						if options.Location then
							options.Location[options.LocationFlag or flagName] = clr
						end
						color.BackgroundColor3 = clr
						selectorColor.Position = UDim2.new(clr and select(3, Color3.toHSV(clr)))
						selectorHue.Position = UDim2.new(1 - (Color3.toHSV(clr)))
						colorPickerInner.BackgroundColor3 = darkenColor(clr, 1.5)
						colorPickerInner.ImageColor3 = darkenColor(clr, 2.5)
						hexInputBox.Text = Color3ToHex(clr)
						colorH, colorS, colorV = Color3.toHSV(clr)
						if callback and (last_v ~= clr or options.AllowDuplicateCalls) then
							task.spawn(callback, clr, last_v)
						end
						return clr
					end
					if presetColor ~= nil then
						Set(presetColor)
					else
						library_flags[flagName] = startingColor
						if options.Location then
							options.Location[options.LocationFlag or flagName] = startingColor
						end
					end
					local default = options.Value or startingColor or library_flags[flagName]
					local function update()
						colorPickerName, callback = options.Name or colorPickerName, options.Callback
						local clr = library_flags[flagName]
						color.BackgroundColor3 = clr
						selectorColor.Position = UDim2.new(clr and select(3, Color3.toHSV(clr)))
						selectorHue.Position = UDim2.new(1 - (Color3.toHSV(clr)))
						colorPickerInner.BackgroundColor3 = darkenColor(clr, 1.5)
						colorPickerInner.ImageColor3 = darkenColor(clr, 2.5)
						hexInputBox.Text = Color3ToHex(clr)
						colorPickerHeadline.Text = colorPickerName or "???"
						return clr
					end
					local objectdata = {
						Options = options,
						Name = flagName,
						Flag = flagName,
						Type = "Colorpicker",
						Default = default,
						Parent = sectionFunctions,
						Instance = newColorPicker,
						SetRainbow = setrainbow,
						Get = function()
							return library_flags[flagName]
						end,
						GetRainbow = function()
							return rainbowColorMode
						end,
						Set = Set,
						Remove = function()
							if newColorPicker then
								newColorPicker.Parent = nil
								sectionFunctions:Update()
							end
						end,
						RawSet = function(t, clr)
							if clr == nil and t ~= nil then
								clr = t
							end
							if clr == "rainbow" then
								if not rainbowColorMode then
									task.spawn(setrainbow, true)
								end
								return clr
							elseif clr == "random" then
								clr = Color3.new(math.random(), math.random(), math.random())
							elseif clr and type(clr) == "string" and tonumber(clr, 16) then
								clr = Color3FromHex(clr)
							end
							task.spawn(setrainbow, false)
							library_flags[flagName] = clr
							if options.Location then
								options.Location[options.LocationFlag or flagName] = clr
							end
							return clr
						end,
						Update = update,
						Reset = function()
							return Set(nil, default)
						end
					}
					tabFunctions.Flags[flagName], sectionFunctions.Flags[flagName], elements[flagName] = objectdata, objectdata, objectdata
					return objectdata
				end
				sectionFunctions.AddColorPicker = sectionFunctions.AddColorpicker
				sectionFunctions.NewColorpicker = sectionFunctions.AddColorpicker
				sectionFunctions.NewColorPicker = sectionFunctions.AddColorpicker
				sectionFunctions.CreateColorPicker = sectionFunctions.AddColorpicker
				sectionFunctions.CreateColorpicker = sectionFunctions.AddColorpicker
				sectionFunctions.ColorPicker = sectionFunctions.AddColorpicker
				sectionFunctions.Colorpicker = sectionFunctions.AddColorpicker
				sectionFunctions.Cp = sectionFunctions.AddColorpicker
				sectionFunctions.CP = sectionFunctions.AddColorpicker
				return sectionFunctions
			end
			tabFunctions.AddSection = tabFunctions.CreateSection
			tabFunctions.NewSection = tabFunctions.CreateSection
			tabFunctions.Section = tabFunctions.CreateSection
			tabFunctions.Sec = tabFunctions.CreateSection
			tabFunctions.S = tabFunctions.CreateSection
			function tabFunctions:UpdateAll()
				local target = self or tabFunctions
				if target and type(target) == "table" and target.Flags then
					for _, e in next, target.Flags do
						if e and type(e) == "table" and e.Update then
							pcall(e.Update)
						end
					end
				end
			end
			return tabFunctions
		end
		windowFunctions.AddTab = windowFunctions.CreateTab
		windowFunctions.NewTab = windowFunctions.CreateTab
		windowFunctions.Tab = windowFunctions.CreateTab
		windowFunctions.T = windowFunctions.CreateTab
		function windowFunctions:CreateDesigner(options, ...)
			options = (options and type(options) == "string" and resolvevararg("Tab", options, ...)) or options
			assert(shared.bypasstablimit or (library.Designer == nil), "Designer already exists")
			options = options or {}
			options.Image = options.Image or 7483871523
			options.LastTab = true
			local designer = windowFunctions:CreateTab(options)
			local colorsection = designer:CreateSection({
				Name = "Colors"
			})
			local backgroundsection = designer:CreateSection({
				Name = "Background",
				Side = "right"
			})
			local detailssection = designer:CreateSection({
				Name = "More Info"
			})
			local filessection = designer:CreateSection({
				Name = "Profiles",
				Side = "right"
			})
			local settingssection = designer:CreateSection({
				Name = "Settings",
				Side = "right"
			})
			local designerelements = {}
			library.designerelements = designerelements
			for _, v in next, {{"Main", "main"}, {"Background", "background"}, {"Outer Border", "outerBorder"}, {"Inner Border", "innerBorder"}, {"Top Gradient", "topGradient"}, {"Bottom Gradient", "bottomGradient"}, {"Section Background", "sectionBackground"}, {"Section", "section"}, {"Element Text", "elementText"}, {"Other Element Text", "otherElementText"}, {"Tab Text", "tabText"}, {"Element Border", "elementBorder"}, {"Selected Option", "selectedOption"}, {"Unselected Option", "unselectedOption"}, {"Hovered Option Top", "hoveredOptionTop"}, {"Unhovered Option Top", "unhoveredOptionTop"}, {"Hovered Option Bottom", "hoveredOptionBottom"}, {"Unhovered Option Bottom", "unhoveredOptionBottom"}} do
				local nam, codename = v[1], v[2]
				local cflag = "__Designer.Colors." .. codename
				designerelements[codename] = {
					Return = colorsection:AddColorpicker({
						Name = nam,
						Flag = cflag,
						Value = library.colors[codename],
						Callback = function(v, y)
							library.colors[codename] = v or y
						end,
						__designer = 1
					}),
					Flag = cflag
				}
			end
			local flags = {}
			local persistoptions = {
				Name = "Workspace Profile",
				Flag = "__Designer.Background.WorkspaceProfile",
				Flags = true,
				Suffix = "Config",
				Workspace = library.WorkspaceName or "Unnamed Workspace",
				Desginer = true
			}
			local daaata = {{"AddTextbox", "__Designer.Textbox.ImageAssetID", backgroundsection, {
				Name = "Image Asset ID",
				Placeholder = "",
				Flag = "__Designer.Background.ImageAssetID",
				Value = "",
				Callback = updatecolorsnotween
			}}, {"AddColorpicker", "__Designer.Colorpicker.ImageColor", backgroundsection, {
				Name = "Image Color",
				Flag = "__Designer.Background.ImageColor",
				Value = Color3.new(1, 1, 1),
				Callback = updatecolorsnotween,
				__designer = 1
			}}, {"AddSlider", "__Designer.Slider.ImageTransparency", backgroundsection, {
				Name = "Image Transparency",
				Flag = "__Designer.Background.ImageTransparency",
				Value = 95,
				Min = 0,
				Max = 100,
				Format = "Image Transparency: %s%%",
				Textbox = true,
				Callback = updatecolorsnotween
			}}, {"AddToggle", "__Designer.Toggle.UseBackgroundImage", backgroundsection, {
				Name = "Use Background Image",
				Flag = "__Designer.Background.UseBackgroundImage",
				Value = true,
				Callback = updatecolorsnotween
			}}, {"AddPersistence", "__Designer.Persistence.ThemeFile", filessection, {
				Name = "Theme Profile",
				Flag = "__Designer.Files.ThemeFile",
				Workspace = "Pepsi Lib Themes",
				Flags = flags,
				Suffix = "Theme",
				Desginer = true
			}}, {"AddTextbox", "__Designer.Textbox.WorkspaceName", filessection, {
				Name = "Workspace Name",
				Value = library.WorkspaceName or "Unnamed Workspace",
				Flag = "__Designer.Files.WorkspaceFile",
				Callback = function(n, o)
					persistoptions.Workspace = n or o
				end
			}}, {"AddPersistence", "__Designer.Persistence.WorkspaceProfile", filessection, persistoptions}, {"AddButton", "__Designer.Button.TerminateGUI", settingssection, {{
				Name = "Terminate GUI",
				Callback = library.unload
			}, {
				Name = "Reset GUI",
				Callback = resetall
			}, {
				Name = "Reset Designer",
				Callback = function()
					destroyrainbows = true
					pcall(function()
						for k, v in next, elements do
							if v and k and v.Set and (v.Default ~= nil) and (library_flags[k] ~= v.Default) and (string.sub(k, 1, 11) == "__Designer.") then
								v:Set(v.Default)
							end
						end
					end)
				end
			}}}, {"AddKeybind", "__Designer.Keybind.ShowHideKey", settingssection, {
				Name = "Show/Hide Key",
				Location = library.configuration,
				Flag = "__Designer.Settings.ShowHideKey",
				LocationFlag = "hideKeybind",
				Value = library.configuration.hideKeybind,
				CoreBinding = true,
				Callback = function()
					lasthidebing = os.clock()
				end
			}}, {"AddLabel", "__Designer.Label.Version", settingssection, {
				Name = "Library Version: " .. tostring(library.Version or "?")
			}}}
			if setclipboard and daaata[8] then
				local common_table = daaata[8][4]
				if common_table then
					common_table[1 + #common_table] = {
						Name = "Copy Theme",
						Callback = function()
							local working_with = {}
							if #flags > 0 then
								for k, cflag in next, flags do
									if k > 0 then
										local data = elements[cflag]
										if data and (data.Type ~= "Persistence") and (string.sub(cflag, 1, 11) == "__Designer.") then
											working_with[cflag] = data
										end
									end
								end
							end
							local saving = {}
							for cflag in next, working_with do
								local value = library_flags[cflag]
								local good, jval = nil, nil
								if value ~= nil then
									good, jval = JSONEncode(value)
								else
									good, jval = true, "null"
								end
								if not good or ((jval == "null") and (value ~= nil)) then
									local typ = typeof(value)
									if typ == "Color3" then
										value = (library.rainbowflags[cflag] and "rainbow") or Color3ToHex(value)
									end
									value = tostring(value)
									good, jval = JSONEncode(value)
									if not good or ((jval == "null") and (value ~= nil)) then
										warn("Could not save value:", value, debug.traceback(""))
									end
								end
								if good and jval then
									saving[cflag] = value
								end
							end
							local good, content = JSONEncode(saving)
							if good and content then
								setclipboard(content)
							end
						end
					}
					common_table = nil
				end
			end
			if options.Credit ~= false then
				daaata[1 + #daaata] = {"AddLabel", "__Designer.Label.Creator", detailssection, {
					Text = "Library by Pepsi UI"
				}}
			elseif "Gee, thanks for your support." then
			end
			if options.Info then
				local typ = type(options.Info)
				if typ == "string" then
					daaata[1 + #daaata] = {"AddLabel", "__Designer.Label.Creator", detailssection, {
						Text = options.Info
					}}
				elseif typ == "table" and #options.Info > 0 then
					for _, v in next, options.Info do
						daaata[1 + #daaata] = {"AddLabel", "__Designer.Label.Creator", detailssection, {
							Text = tostring(v)
						}}
					end
				end
			end
			for _, v in next, daaata do
				designerelements[v[2]] = v[3][v[1]](v[3], v[4])
			end
			designerelements["__Designer.Textbox.WorkspaceName"]:Set(library.WorkspaceName or "Unnamed Workspace")
			for k, v in next, elements do
				if v and k and string.sub(k, 1, 11) == "__Designer." and v.Type and v.Type ~= "Persistence" then
					flags[1 + #flags] = k
				end
			end
			if library.Backdrop then
				library.Backdrop.Image = resolveid(library_flags["__Designer.Background.ImageAssetID"], "__Designer.Background.ImageAssetID") or ""
				library.Backdrop.Visible = library_flags["__Designer.Background.UseBackgroundImage"] and true
				library.Backdrop.ImageTransparency = (library_flags["__Designer.Background.ImageTransparency"] or 95) / 100
				library.Backdrop.ImageColor3 = library_flags["__Designer.Background.ImageColor"] or Color3.new(1, 1, 1)
			end
			local function setbackground(t, Asset, Transparency, Visible)
				if Visible == nil and t ~= nil and type(t) ~= "table" then
					Asset, Transparency, Visible = t, Transparency, Visible
				end
				if Visible == 0 or ((Asset == 0 or Asset == false) and Visible == nil and Transparency == nil) then
					Visible = false
				elseif Visible == 1 or ((Asset == 1 or Asset == true) and Visible == nil and Transparency == nil) then
					Visible = true
				elseif Asset == nil and Transparency == nil and Visible == nil then
					Visible = not library_flags["__Designer.Background.UseBackgroundImage"]
				end
				local temp = Asset and type(Asset)
				if Transparency == nil and Visible == nil and temp == "number" and ((Asset ~= 1 and Asset ~= 0) or (Asset > 0 and Asset <= 100)) then
					Transparency, Asset, temp = Asset, nil
				end
				if temp and ((temp == "number" and Asset > 1) or temp == "string") then
					designerelements["__Designer.Textbox.ImageAssetID"]:Set(Asset)
				end
				temp = tonumber(Transparency)
				temp = (temp >= 0 and temp <= 1 and temp * 100) or temp
				if temp then
					designerelements["__Designer.Slider.ImageTransparency"]:Set(temp)
				end
				if Visible ~= nil then
					designerelements["__Designer.Toggle.UseBackgroundImage"]:Set(Visible and true)
				end
				return Asset, Transparency, Visible
			end
			local bk = options.Background or options.Backdrop or options.Grahpic
			if bk then
				if type(bk) == "table" then
					setbackground(bk.Asset or bk[1], bk.Transparency or bk[2], bk.Visible or bk[3])
				else
					setbackground(bk, 0, 1)
				end
			end
			library.Designer = {
				Options = options,
				Parent = windowFunctions,
				Name = "Designer",
				Flag = "Designer",
				Type = "Designer",
				Instance = designer,
				SetBackground = setbackground,
				Remove = designer.Remove
			}
			library.SetBackground = setbackground
			local savestuff = library.elements["__Designer.Background.WorkspaceProfile"]
			if savestuff then
				library.LoadFile = savestuff.LoadFile
				library.LoadFileRaw = savestuff.LoadFileRaw
				library.LoadJSON = savestuff.LoadJSON
				library.LoadJSONRaw = savestuff.LoadJSONRaw
				library.SaveFile = savestuff.SaveFile
				library.GetJSON = savestuff.GetJSON
			end
			spawn(updatecolorsnotween)
			local dorlod = nil
			if options.HideTheme then
				designer.Remove()
				dorlod = true
			elseif options.LockTheme then
				if colorsection then
					colorsection.Remove()
					dorlod = true
				end
				if backgroundsection then
					backgroundsection.Remove()
					dorlod = true
				end
				if designerelements then
					local thing = designerelements["__Designer.Persistence.ThemeFile"]
					if thing then
						thing.Remove()
						dorlod = true
					end
					thing = designerelements["__Designer.Button.TerminateGUI"]
					thing = thing and thing[3]
					if thing then
						thing.Remove()
						dorlod = true
					end
				end
			end
			if dorlod then
				windowFunctions:UpdateAll()
			end
			return library.Designer
		end
		windowFunctions.AddDesigner = windowFunctions.CreateDesigner
		windowFunctions.NewDesigner = windowFunctions.CreateDesigner
		windowFunctions.Designer = windowFunctions.CreateDesigner
		windowFunctions.D = windowFunctions.CreateDesigner
		function windowFunctions:UpdateAll()
			local target = self or windowFunctions
			if target and type(target) == "table" and target.Flags then
				for _, e in next, target.Flags do
					if e and type(e) == "table" then
						if e.Update then
							pcall(e.Update)
						end
						if e.UpdateAll then
							pcall(e.Update)
						end
					end
				end
				pcall(function()
					if library.Backdrop then
						library.Backdrop.Visible = library_flags["__Designer.Background.UseBackgroundImage"] and true
						library.Backdrop.Image = resolveid(library_flags["__Designer.Background.ImageAssetID"], "__Designer.Background.ImageAssetID") or ""
						library.Backdrop.ImageColor3 = library_flags["__Designer.Background.ImageColor"] or Color3.new(1, 1, 1)
						library.Backdrop.ImageTransparency = (library_flags["__Designer.Background.ImageTransparency"] or 95) / 100
					end
				end)
			end
		end
		library.UpdateAll = windowFunctions.UpdateAll
		if options.Themeable or options.DefaultTheme or options.Theme then
			spawn(function()
				local os_clock = os.clock
				local starttime = os_clock()
				while os_clock() - starttime < 12 do
					if homepage then
						windowFunctions.GoHome = homepage
						local x, e = pcall(homepage)
						if not x and e then
							warn("Error going to Homepage:", e)
						end
						x, e = nil
						break
					end
					task.wait()
				end
				local whatDoILookLike = options.Themeable or options.DefaultTheme or options.Theme
				if type(whatDoILookLike) == "table" then
					whatDoILookLike.LockTheme = whatDoILookLike.LockTheme or options.LockTheme or nil
					whatDoILookLike.HideTheme = whatDoILookLike.HideTheme or options.HideTheme or nil
				else
					whatDoILookLike = nil
				end
				windowFunctions:CreateDesigner(whatDoILookLike)
				if options.DefaultTheme or options.Theme then
					spawn(function()
						local content = options.DefaultTheme or options.Theme or options.JSON or options.ThemeJSON
						if content and type(content) == "string" and #content > 1 then
							local good, jcontent = JSONDecode(content)
							if good and jcontent then
								for cflag, val in next, jcontent do
									local data = elements[cflag]
									if data and data.Type ~= "Persistence" then
										if data.Set then
											data:Set(val)
										elseif data.RawSet then
											data:RawSet(val)
										else
											library.flags[cflag] = val
										end
									end
								end
							end
						end
					end)
				end
				os_clock, starttime = nil
			end)
		end
		return windowFunctions
	end
	library.NewWindow = library.CreateWindow
	library.AddWindow = library.CreateWindow
	library.Window = library.CreateWindow
	library.W = library.CreateWindow

local Wait = library.subs.Wait
if game.PlaceId == 2753915549 then
	World1 = true
elseif game.PlaceId == 4442272183 then
	World2 = true
elseif game.PlaceId == 7449423635 then
	World3 = true
end

function CheckQuest() 
	MyLevel = game:GetService("Players").LocalPlayer.Data.Level.Value
	if World1 then
		if MyLevel == 1 or MyLevel <= 9 then
			Mon = "Bandit [Lv. 5]"
			LevelQuest = 1
			NameQuest = "BanditQuest1"
			NameMon = "Bandit"
			CFrameQuest = CFrame.new(1059.37195, 15.4495068, 1550.4231, 0.939700544, -0, -0.341998369, 0, 1, -0, 0.341998369, 0, 0.939700544)
			CFrameMon = CFrame.new(1045.962646484375, 27.00250816345215, 1560.8203125)
		elseif MyLevel == 10 or MyLevel <= 14 then
			Mon = "Monkey [Lv. 14]"
			LevelQuest = 1
			NameQuest = "JungleQuest"
			NameMon = "Monkey"
			CFrameQuest = CFrame.new(-1598.08911, 35.5501175, 153.377838, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			CFrameMon = CFrame.new(-1448.51806640625, 67.85301208496094, 11.46579647064209)
		elseif MyLevel == 15 or MyLevel <= 29 then
			Mon = "Gorilla [Lv. 20]"
			LevelQuest = 2
			NameQuest = "JungleQuest"
			NameMon = "Gorilla"
			CFrameQuest = CFrame.new(-1598.08911, 35.5501175, 153.377838, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			CFrameMon = CFrame.new(-1129.8836669921875, 40.46354675292969, -525.4237060546875)
		elseif MyLevel == 30 or MyLevel <= 39 then
			Mon = "Pirate [Lv. 35]"
			LevelQuest = 1
			NameQuest = "BuggyQuest1"
			NameMon = "Pirate"
			CFrameQuest = CFrame.new(-1141.07483, 4.10001802, 3831.5498, 0.965929627, -0, -0.258804798, 0, 1, -0, 0.258804798, 0, 0.965929627)
			CFrameMon = CFrame.new(-1103.513427734375, 13.752052307128906, 3896.091064453125)
		elseif MyLevel == 40 or MyLevel <= 59 then
			Mon = "Brute [Lv. 45]"
			LevelQuest = 2
			NameQuest = "BuggyQuest1"
			NameMon = "Brute"
			CFrameQuest = CFrame.new(-1141.07483, 4.10001802, 3831.5498, 0.965929627, -0, -0.258804798, 0, 1, -0, 0.258804798, 0, 0.965929627)
			CFrameMon = CFrame.new(-1140.083740234375, 14.809885025024414, 4322.92138671875)
		elseif MyLevel == 60 or MyLevel <= 74 then
			Mon = "Desert Bandit [Lv. 60]"
			LevelQuest = 1
			NameQuest = "DesertQuest"
			NameMon = "Desert Bandit"
			CFrameQuest = CFrame.new(894.488647, 5.14000702, 4392.43359, 0.819155693, -0, -0.573571265, 0, 1, -0, 0.573571265, 0, 0.819155693)
			CFrameMon = CFrame.new(924.7998046875, 6.44867467880249, 4481.5859375)
		elseif MyLevel == 75 or MyLevel <= 89 then
			Mon = "Desert Officer [Lv. 70]"
			LevelQuest = 2
			NameQuest = "DesertQuest"
			NameMon = "Desert Officer"
			CFrameQuest = CFrame.new(894.488647, 5.14000702, 4392.43359, 0.819155693, -0, -0.573571265, 0, 1, -0, 0.573571265, 0, 0.819155693)
			CFrameMon = CFrame.new(1608.2822265625, 8.614224433898926, 4371.00732421875)
		elseif MyLevel == 90 or MyLevel <= 99 then
			Mon = "Snow Bandit [Lv. 90]"
			LevelQuest = 1
			NameQuest = "SnowQuest"
			NameMon = "Snow Bandit"
			CFrameQuest = CFrame.new(1389.74451, 88.1519318, -1298.90796, -0.342042685, 0, 0.939684391, 0, 1, 0, -0.939684391, 0, -0.342042685)
			CFrameMon = CFrame.new(1354.347900390625, 87.27277374267578, -1393.946533203125)
		elseif MyLevel == 100 or MyLevel <= 119 then
			Mon = "Snowman [Lv. 100]"
			LevelQuest = 2
			NameQuest = "SnowQuest"
			NameMon = "Snowman"
			CFrameQuest = CFrame.new(1389.74451, 88.1519318, -1298.90796, -0.342042685, 0, 0.939684391, 0, 1, 0, -0.939684391, 0, -0.342042685)
			CFrameMon = CFrame.new(1201.6412353515625, 144.57958984375, -1550.0670166015625)
		elseif MyLevel == 120 or MyLevel <= 149 then
			Mon = "Chief Petty Officer [Lv. 120]"
			LevelQuest = 1
			NameQuest = "MarineQuest2"
			NameMon = "Chief Petty Officer"
			CFrameQuest = CFrame.new(-5039.58643, 27.3500385, 4324.68018, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			CFrameMon = CFrame.new(-4881.23095703125, 22.65204429626465, 4273.75244140625)
		elseif MyLevel == 150 or MyLevel <= 174 then
			Mon = "Sky Bandit [Lv. 150]"
			LevelQuest = 1
			NameQuest = "SkyQuest"
			NameMon = "Sky Bandit"
			CFrameQuest = CFrame.new(-4839.53027, 716.368591, -2619.44165, 0.866007268, 0, 0.500031412, 0, 1, 0, -0.500031412, 0, 0.866007268)
			CFrameMon = CFrame.new(-4953.20703125, 295.74420166015625, -2899.22900390625)
		elseif MyLevel == 175 or MyLevel <= 189 then
			Mon = "Dark Master [Lv. 175]"
			LevelQuest = 2
			NameQuest = "SkyQuest"
			NameMon = "Dark Master"
			CFrameQuest = CFrame.new(-4839.53027, 716.368591, -2619.44165, 0.866007268, 0, 0.500031412, 0, 1, 0, -0.500031412, 0, 0.866007268)
			CFrameMon = CFrame.new(-5259.8447265625, 391.3976745605469, -2229.035400390625)
		elseif MyLevel == 190 or MyLevel <= 209 then
			Mon = "Prisoner [Lv. 190]"
			LevelQuest = 1
			NameQuest = "PrisonerQuest"
			NameMon = "Prisoner"
			CFrameQuest = CFrame.new(5308.93115, 1.65517521, 475.120514, -0.0894274712, -5.00292918e-09, -0.995993316, 1.60817859e-09, 1, -5.16744869e-09, 0.995993316, -2.06384709e-09, -0.0894274712)
			CFrameMon = CFrame.new(5098.9736328125, -0.3204058110713959, 474.2373352050781)
		elseif MyLevel == 210 or MyLevel <= 249 then
			Mon = "Dangerous Prisoner [Lv. 210]"
			LevelQuest = 2
			NameQuest = "PrisonerQuest"
			NameMon = "Dangerous Prisoner"
			CFrameQuest = CFrame.new(5308.93115, 1.65517521, 475.120514, -0.0894274712, -5.00292918e-09, -0.995993316, 1.60817859e-09, 1, -5.16744869e-09, 0.995993316, -2.06384709e-09, -0.0894274712)
			CFrameMon = CFrame.new(5654.5634765625, 15.633401870727539, 866.2991943359375)
		elseif MyLevel == 250 or MyLevel <= 274 then
			Mon = "Toga Warrior [Lv. 250]"
			LevelQuest = 1
			NameQuest = "ColosseumQuest"
			NameMon = "Toga Warrior"
			CFrameQuest = CFrame.new(-1580.04663, 6.35000277, -2986.47534, -0.515037298, 0, -0.857167721, 0, 1, 0, 0.857167721, 0, -0.515037298)
			CFrameMon = CFrame.new(-1820.21484375, 51.68385696411133, -2740.6650390625)
		elseif MyLevel == 275 or MyLevel <= 299 then
			Mon = "Gladiator [Lv. 275]"
			LevelQuest = 2
			NameQuest = "ColosseumQuest"
			NameMon = "Gladiator"
			CFrameQuest = CFrame.new(-1580.04663, 6.35000277, -2986.47534, -0.515037298, 0, -0.857167721, 0, 1, 0, 0.857167721, 0, -0.515037298)
			CFrameMon = CFrame.new(-1292.838134765625, 56.380882263183594, -3339.031494140625)
		elseif MyLevel == 300 or MyLevel <= 324 then
			Mon = "Military Soldier [Lv. 300]"
			LevelQuest = 1
			NameQuest = "MagmaQuest"
			NameMon = "Military Soldier"
			CFrameQuest = CFrame.new(-5313.37012, 10.9500084, 8515.29395, -0.499959469, 0, 0.866048813, 0, 1, 0, -0.866048813, 0, -0.499959469)
			CFrameMon = CFrame.new(-5411.16455078125, 11.081554412841797, 8454.29296875)
		elseif MyLevel == 325 or MyLevel <= 374 then
			Mon = "Military Spy [Lv. 325]"
			LevelQuest = 2
			NameQuest = "MagmaQuest"
			NameMon = "Military Spy"
			CFrameQuest = CFrame.new(-5313.37012, 10.9500084, 8515.29395, -0.499959469, 0, 0.866048813, 0, 1, 0, -0.866048813, 0, -0.499959469)
			CFrameMon = CFrame.new(-5802.8681640625, 86.26241302490234, 8828.859375)
		elseif MyLevel == 375 or MyLevel <= 399 then
			Mon = "Fishman Warrior [Lv. 375]"
			LevelQuest = 1
			NameQuest = "FishmanQuest"
			NameMon = "Fishman Warrior"
			CFrameQuest = CFrame.new(61122.65234375, 18.497442245483, 1569.3997802734)
			CFrameMon = CFrame.new(60878.30078125, 18.482830047607422, 1543.7574462890625)
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(61163.8515625, 11.6796875, 1819.7841796875))
			end
		elseif MyLevel == 400 or MyLevel <= 449 then
			Mon = "Fishman Commando [Lv. 400]"
			LevelQuest = 2
			NameQuest = "FishmanQuest"
			NameMon = "Fishman Commando"
			CFrameQuest = CFrame.new(61122.65234375, 18.497442245483, 1569.3997802734)
			CFrameMon = CFrame.new(61922.6328125, 18.482830047607422, 1493.934326171875)
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(61163.8515625, 11.6796875, 1819.7841796875))
			end
		elseif MyLevel == 450 or MyLevel <= 474 then
			Mon = "God's Guard [Lv. 450]"
			LevelQuest = 1
			NameQuest = "SkyExp1Quest"
			NameMon = "God's Guard"
			CFrameQuest = CFrame.new(-4721.88867, 843.874695, -1949.96643, 0.996191859, -0, -0.0871884301, 0, 1, -0, 0.0871884301, 0, 0.996191859)
			CFrameMon = CFrame.new(-4710.04296875, 845.2769775390625, -1927.3079833984375)
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-4607.82275, 872.54248, -1667.55688))
			end
		elseif MyLevel == 475 or MyLevel <= 524 then
			Mon = "Shanda [Lv. 475]"
			LevelQuest = 2
			NameQuest = "SkyExp1Quest"
			NameMon = "Shanda"
			CFrameQuest = CFrame.new(-7859.09814, 5544.19043, -381.476196, -0.422592998, 0, 0.906319618, 0, 1, 0, -0.906319618, 0, -0.422592998)
			CFrameMon = CFrame.new(-7678.48974609375, 5566.40380859375, -497.2156066894531)
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-7894.6176757813, 5547.1416015625, -380.29119873047))
			end
		elseif MyLevel == 525 or MyLevel <= 549 then
			Mon = "Royal Squad [Lv. 525]"
			LevelQuest = 1
			NameQuest = "SkyExp2Quest"
			NameMon = "Royal Squad"
			CFrameQuest = CFrame.new(-7906.81592, 5634.6626, -1411.99194, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			CFrameMon = CFrame.new(-7624.25244140625, 5658.13330078125, -1467.354248046875)
		elseif MyLevel == 550 or MyLevel <= 624 then
			Mon = "Royal Soldier [Lv. 550]"
			LevelQuest = 2
			NameQuest = "SkyExp2Quest"
			NameMon = "Royal Soldier"
			CFrameQuest = CFrame.new(-7906.81592, 5634.6626, -1411.99194, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			CFrameMon = CFrame.new(-7836.75341796875, 5645.6640625, -1790.6236572265625)
		elseif MyLevel == 625 or MyLevel <= 649 then
			Mon = "Galley Pirate [Lv. 625]"
			LevelQuest = 1
			NameQuest = "FountainQuest"
			NameMon = "Galley Pirate"
			CFrameQuest = CFrame.new(5259.81982, 37.3500175, 4050.0293, 0.087131381, 0, 0.996196866, 0, 1, 0, -0.996196866, 0, 0.087131381)
			CFrameMon = CFrame.new(5551.02197265625, 78.90135192871094, 3930.412841796875)
		elseif MyLevel >= 650 then
			Mon = "Galley Captain [Lv. 650]"
			LevelQuest = 2
			NameQuest = "FountainQuest"
			NameMon = "Galley Captain"
			CFrameQuest = CFrame.new(5259.81982, 37.3500175, 4050.0293, 0.087131381, 0, 0.996196866, 0, 1, 0, -0.996196866, 0, 0.087131381)
			CFrameMon = CFrame.new(5441.95166015625, 42.50205993652344, 4950.09375)
		end
	elseif World2 then
		if MyLevel == 700 or MyLevel <= 724 then
			Mon = "Raider [Lv. 700]"
			LevelQuest = 1
			NameQuest = "Area1Quest"
			NameMon = "Raider"
			CFrameQuest = CFrame.new(-429.543518, 71.7699966, 1836.18188, -0.22495985, 0, -0.974368095, 0, 1, 0, 0.974368095, 0, -0.22495985)
			CFrameMon = CFrame.new(-728.3267211914062, 52.779319763183594, 2345.7705078125)
		elseif MyLevel == 725 or MyLevel <= 774 then
			Mon = "Mercenary [Lv. 725]"
			LevelQuest = 2
			NameQuest = "Area1Quest"
			NameMon = "Mercenary"
			CFrameQuest = CFrame.new(-429.543518, 71.7699966, 1836.18188, -0.22495985, 0, -0.974368095, 0, 1, 0, 0.974368095, 0, -0.22495985)
			CFrameMon = CFrame.new(-1004.3244018554688, 80.15886688232422, 1424.619384765625)
		elseif MyLevel == 775 or MyLevel <= 799 then
			Mon = "Swan Pirate [Lv. 775]"
			LevelQuest = 1
			NameQuest = "Area2Quest"
			NameMon = "Swan Pirate"
			CFrameQuest = CFrame.new(638.43811, 71.769989, 918.282898, 0.139203906, 0, 0.99026376, 0, 1, 0, -0.99026376, 0, 0.139203906)
			CFrameMon = CFrame.new(1068.664306640625, 137.61428833007812, 1322.1060791015625)
		elseif MyLevel == 800 or MyLevel <= 874 then
			Mon = "Factory Staff [Lv. 800]"
			NameQuest = "Area2Quest"
			LevelQuest = 2
			NameMon = "Factory Staff"
			CFrameQuest = CFrame.new(632.698608, 73.1055908, 918.666321, -0.0319722369, 8.96074881e-10, -0.999488771, 1.36326533e-10, 1, 8.92172336e-10, 0.999488771, -1.07732087e-10, -0.0319722369)
			CFrameMon = CFrame.new(73.07867431640625, 81.86344146728516, -27.470672607421875)
	elseif MyLevel == 875 or MyLevel <= 899 then
			Mon = "Marine Lieutenant [Lv. 875]"
			LevelQuest = 1
			NameQuest = "MarineQuest3"
			NameMon = "Marine Lieutenant"
			CFrameQuest = CFrame.new(-2440.79639, 71.7140732, -3216.06812, 0.866007268, 0, 0.500031412, 0, 1, 0, -0.500031412, 0, 0.866007268)
			CFrameMon = CFrame.new(-2821.372314453125, 75.89727783203125, -3070.089111328125)	
		elseif MyLevel == 900 or MyLevel <= 949 then
			Mon = "Marine Captain [Lv. 900]"
			LevelQuest = 2
			NameQuest = "MarineQuest3"
			NameMon = "Marine Captain"
			CFrameQuest = CFrame.new(-2440.79639, 71.7140732, -3216.06812, 0.866007268, 0, 0.500031412, 0, 1, 0, -0.500031412, 0, 0.866007268)
			CFrameMon = CFrame.new(-1861.2310791015625, 80.17658233642578, -3254.697509765625)
		elseif MyLevel == 950 or MyLevel <= 974 then
			Mon = "Zombie [Lv. 950]"
			LevelQuest = 1
			NameQuest = "ZombieQuest"
			NameMon = "Zombie"
			CFrameQuest = CFrame.new(-5497.06152, 47.5923004, -795.237061, -0.29242146, 0, -0.95628953, 0, 1, 0, 0.95628953, 0, -0.29242146)
			CFrameMon = CFrame.new(-5657.77685546875, 78.96973419189453, -928.68701171875)
		elseif MyLevel == 975 or MyLevel <= 999 then
			Mon = "Vampire [Lv. 975]"
			LevelQuest = 2
			NameQuest = "ZombieQuest"
			NameMon = "Vampire"
			CFrameQuest = CFrame.new(-5497.06152, 47.5923004, -795.237061, -0.29242146, 0, -0.95628953, 0, 1, 0, 0.95628953, 0, -0.29242146)
			CFrameMon = CFrame.new(-6037.66796875, 32.18463897705078, -1340.6597900390625)
		elseif MyLevel == 1000 or MyLevel <= 1049 then
			Mon = "Snow Trooper [Lv. 1000]"
			LevelQuest = 1
			NameQuest = "SnowMountainQuest"
			NameMon = "Snow Trooper"
			CFrameQuest = CFrame.new(609.858826, 400.119904, -5372.25928, -0.374604106, 0, 0.92718488, 0, 1, 0, -0.92718488, 0, -0.374604106)
			CFrameMon = CFrame.new(549.1473388671875, 427.3870544433594, -5563.69873046875)
		elseif MyLevel == 1050 or MyLevel <= 1099 then
			Mon = "Winter Warrior [Lv. 1050]"
			LevelQuest = 2
			NameQuest = "SnowMountainQuest"
			NameMon = "Winter Warrior"
			CFrameQuest = CFrame.new(609.858826, 400.119904, -5372.25928, -0.374604106, 0, 0.92718488, 0, 1, 0, -0.92718488, 0, -0.374604106)
			CFrameMon = CFrame.new(1142.7451171875, 475.6398010253906, -5199.41650390625)
		elseif MyLevel == 1100 or MyLevel <= 1124 then
			Mon = "Lab Subordinate [Lv. 1100]"
			LevelQuest = 1
			NameQuest = "IceSideQuest"
			NameMon = "Lab Subordinate"
			CFrameQuest = CFrame.new(-6064.06885, 15.2422857, -4902.97852, 0.453972578, -0, -0.891015649, 0, 1, -0, 0.891015649, 0, 0.453972578)
			CFrameMon = CFrame.new(-5707.4716796875, 15.951709747314453, -4513.39208984375)
		elseif MyLevel == 1125 or MyLevel <= 1174 then
			Mon = "Horned Warrior [Lv. 1125]"
			LevelQuest = 2
			NameQuest = "IceSideQuest"
			NameMon = "Horned Warrior"
			CFrameQuest = CFrame.new(-6064.06885, 15.2422857, -4902.97852, 0.453972578, -0, -0.891015649, 0, 1, -0, 0.891015649, 0, 0.453972578)
			CFrameMon = CFrame.new(-6341.36669921875, 15.951770782470703, -5723.162109375)
		elseif MyLevel == 1175 or MyLevel <= 1199 then
			Mon = "Magma Ninja [Lv. 1175]"
			LevelQuest = 1
			NameQuest = "FireSideQuest"
			NameMon = "Magma Ninja"
			CFrameQuest = CFrame.new(-5428.03174, 15.0622921, -5299.43457, -0.882952213, 0, 0.469463557, 0, 1, 0, -0.469463557, 0, -0.882952213)
			CFrameMon = CFrame.new(-5449.6728515625, 76.65874481201172, -5808.20068359375)
		elseif MyLevel == 1200 or MyLevel <= 1249 then
			Mon = "Lava Pirate [Lv. 1200]"
			LevelQuest = 2
			NameQuest = "FireSideQuest"
			NameMon = "Lava Pirate"
			CFrameQuest = CFrame.new(-5428.03174, 15.0622921, -5299.43457, -0.882952213, 0, 0.469463557, 0, 1, 0, -0.469463557, 0, -0.882952213)
			CFrameMon = CFrame.new(-5213.33154296875, 49.73788070678711, -4701.451171875)
		elseif MyLevel == 1250 or MyLevel <= 1274 then
			Mon = "Ship Deckhand [Lv. 1250]"
			LevelQuest = 1
			NameQuest = "ShipQuest1"
			NameMon = "Ship Deckhand"
			CFrameQuest = CFrame.new(1037.80127, 125.092171, 32911.6016)         
			CFrameMon = CFrame.new(1212.0111083984375, 150.79205322265625, 33059.24609375)    
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
			end
		elseif MyLevel == 1275 or MyLevel <= 1299 then
			Mon = "Ship Engineer [Lv. 1275]"
			LevelQuest = 2
			NameQuest = "ShipQuest1"
			NameMon = "Ship Engineer"
			CFrameQuest = CFrame.new(1037.80127, 125.092171, 32911.6016)   
			CFrameMon = CFrame.new(919.4786376953125, 43.54401397705078, 32779.96875)   
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
			end             
		elseif MyLevel == 1300 or MyLevel <= 1324 then
			Mon = "Ship Steward [Lv. 1300]"
			LevelQuest = 1
			NameQuest = "ShipQuest2"
			NameMon = "Ship Steward"
			CFrameQuest = CFrame.new(968.80957, 125.092171, 33244.125)         
			CFrameMon = CFrame.new(919.4385375976562, 129.55599975585938, 33436.03515625)      
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
			end
		elseif MyLevel == 1325 or MyLevel <= 1349 then
			Mon = "Ship Officer [Lv. 1325]"
			LevelQuest = 2
			NameQuest = "ShipQuest2"
			NameMon = "Ship Officer"
			CFrameQuest = CFrame.new(968.80957, 125.092171, 33244.125)
			CFrameMon = CFrame.new(1036.0179443359375, 181.4390411376953, 33315.7265625)
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
			end
		elseif MyLevel == 1350 or MyLevel <= 1374 then
			Mon = "Arctic Warrior [Lv. 1350]"
			LevelQuest = 1
			NameQuest = "FrostQuest"
			NameMon = "Arctic Warrior"
			CFrameQuest = CFrame.new(5667.6582, 26.7997818, -6486.08984, -0.933587909, 0, -0.358349502, 0, 1, 0, 0.358349502, 0, -0.933587909)
			CFrameMon = CFrame.new(5966.24609375, 62.97002029418945, -6179.3828125)
			if _G.AutoFarm and (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-6508.5581054688, 5000.034996032715, -132.83953857422))
			end
		elseif MyLevel == 1375 or MyLevel <= 1424 then
			Mon = "Snow Lurker [Lv. 1375]"
			LevelQuest = 2
			NameQuest = "FrostQuest"
			NameMon = "Snow Lurker"
			CFrameQuest = CFrame.new(5667.6582, 26.7997818, -6486.08984, -0.933587909, 0, -0.358349502, 0, 1, 0, 0.358349502, 0, -0.933587909)
			CFrameMon = CFrame.new(5407.07373046875, 69.19437408447266, -6880.88037109375)
		elseif MyLevel == 1425 or MyLevel <= 1449 then
			Mon = "Sea Soldier [Lv. 1425]"
			LevelQuest = 1
			NameQuest = "ForgottenQuest"
			NameMon = "Sea Soldier"
			CFrameQuest = CFrame.new(-3054.44458, 235.544281, -10142.8193, 0.990270376, -0, -0.13915664, 0, 1, -0, 0.13915664, 0, 0.990270376)
			CFrameMon = CFrame.new(-3028.2236328125, 64.67451477050781, -9775.4267578125)
		elseif MyLevel >= 1450 then
			Mon = "Water Fighter [Lv. 1450]"
			LevelQuest = 2
			NameQuest = "ForgottenQuest"
			NameMon = "Water Fighter"
			CFrameQuest = CFrame.new(-3054.44458, 235.544281, -10142.8193, 0.990270376, -0, -0.13915664, 0, 1, -0, 0.13915664, 0, 0.990270376)
			CFrameMon = CFrame.new(-3352.9013671875, 285.01556396484375, -10534.841796875)
		end
	elseif World3 then
		if MyLevel == 1500 or MyLevel <= 1524 then
			Mon = "Pirate Millionaire [Lv. 1500]"
			LevelQuest = 1
			NameQuest = "PiratePortQuest"
			NameMon = "Pirate Millionaire"
			CFrameQuest = CFrame.new(-290.074677, 42.9034653, 5581.58984, 0.965929627, -0, -0.258804798, 0, 1, -0, 0.258804798, 0, 0.965929627)
			CFrameMon = CFrame.new(-245.9963836669922, 47.30615234375, 5584.1005859375)
		elseif MyLevel == 1525 or MyLevel <= 1574 then
			Mon = "Pistol Billionaire [Lv. 1525]"
			LevelQuest = 2
			NameQuest = "PiratePortQuest"
			NameMon = "Pistol Billionaire"
			CFrameQuest = CFrame.new(-290.074677, 42.9034653, 5581.58984, 0.965929627, -0, -0.258804798, 0, 1, -0, 0.258804798, 0, 0.965929627)
			CFrameMon = CFrame.new(-187.3301544189453, 86.23987579345703, 6013.513671875)
		elseif MyLevel == 1575 or MyLevel <= 1599 then
			Mon = "Dragon Crew Warrior [Lv. 1575]"
			LevelQuest = 1
			NameQuest = "AmazonQuest"
			NameMon = "Dragon Crew Warrior"
			CFrameQuest = CFrame.new(5832.83594, 51.6806107, -1101.51563, 0.898790359, -0, -0.438378751, 0, 1, -0, 0.438378751, 0, 0.898790359)
			CFrameMon = CFrame.new(6141.140625, 51.35136413574219, -1340.738525390625)
		elseif MyLevel == 1600 or MyLevel <= 1624 then 
			Mon = "Dragon Crew Archer [Lv. 1600]"
			NameQuest = "AmazonQuest"
			LevelQuest = 2
			NameMon = "Dragon Crew Archer"
			CFrameQuest = CFrame.new(5833.1147460938, 51.60498046875, -1103.0693359375)
			CFrameMon = CFrame.new(6616.41748046875, 441.7670593261719, 446.0469970703125)
		elseif MyLevel == 1625 or MyLevel <= 1649 then
			Mon = "Female Islander [Lv. 1625]"
			NameQuest = "AmazonQuest2"
			LevelQuest = 1
			NameMon = "Female Islander"
			CFrameQuest = CFrame.new(5446.8793945313, 601.62945556641, 749.45672607422)
			CFrameMon = CFrame.new(4685.25830078125, 735.8078002929688, 815.3425903320312)
		elseif MyLevel == 1650 or MyLevel <= 1699 then 
			Mon = "Giant Islander [Lv. 1650]"
			NameQuest = "AmazonQuest2"
			LevelQuest = 2
			NameMon = "Giant Islander"
			CFrameQuest = CFrame.new(5446.8793945313, 601.62945556641, 749.45672607422)
			CFrameMon = CFrame.new(4729.09423828125, 590.436767578125, -36.97627639770508)
		elseif MyLevel == 1700 or MyLevel <= 1724 then
			Mon = "Marine Commodore [Lv. 1700]"
			LevelQuest = 1
			NameQuest = "MarineTreeIsland"
			NameMon = "Marine Commodore"
			CFrameQuest = CFrame.new(2180.54126, 27.8156815, -6741.5498, -0.965929747, 0, 0.258804798, 0, 1, 0, -0.258804798, 0, -0.965929747)
			CFrameMon = CFrame.new(2286.0078125, 73.13391876220703, -7159.80908203125)
		elseif MyLevel == 1725 or MyLevel <= 1774 then
			Mon = "Marine Rear Admiral [Lv. 1725]"
			NameMon = "Marine Rear Admiral"
			NameQuest = "MarineTreeIsland"
			LevelQuest = 2
			CFrameQuest = CFrame.new(2179.98828125, 28.731239318848, -6740.0551757813)
			CFrameMon = CFrame.new(3656.773681640625, 160.52406311035156, -7001.5986328125)
		elseif MyLevel == 1775 or MyLevel <= 1799 then
			Mon = "Fishman Raider [Lv. 1775]"
			LevelQuest = 1
			NameQuest = "DeepForestIsland3"
			NameMon = "Fishman Raider"
			CFrameQuest = CFrame.new(-10581.6563, 330.872955, -8761.18652, -0.882952213, 0, 0.469463557, 0, 1, 0, -0.469463557, 0, -0.882952213)   
			CFrameMon = CFrame.new(-10407.5263671875, 331.76263427734375, -8368.5166015625)
		elseif MyLevel == 1800 or MyLevel <= 1824 then
			Mon = "Fishman Captain [Lv. 1800]"
			LevelQuest = 2
			NameQuest = "DeepForestIsland3"
			NameMon = "Fishman Captain"
			CFrameQuest = CFrame.new(-10581.6563, 330.872955, -8761.18652, -0.882952213, 0, 0.469463557, 0, 1, 0, -0.469463557, 0, -0.882952213)   
			CFrameMon = CFrame.new(-10994.701171875, 352.38140869140625, -9002.1103515625) 
		elseif MyLevel == 1825 or MyLevel <= 1849 then
			Mon = "Forest Pirate [Lv. 1825]"
			LevelQuest = 1
			NameQuest = "DeepForestIsland"
			NameMon = "Forest Pirate"
			CFrameQuest = CFrame.new(-13234.04, 331.488495, -7625.40137, 0.707134247, -0, -0.707079291, 0, 1, -0, 0.707079291, 0, 0.707134247)
			CFrameMon = CFrame.new(-13274.478515625, 332.3781433105469, -7769.58056640625)
		elseif MyLevel == 1850 or MyLevel <= 1899 then
			Mon = "Mythological Pirate [Lv. 1850]"
			LevelQuest = 2
			NameQuest = "DeepForestIsland"
			NameMon = "Mythological Pirate"
			CFrameQuest = CFrame.new(-13234.04, 331.488495, -7625.40137, 0.707134247, -0, -0.707079291, 0, 1, -0, 0.707079291, 0, 0.707134247)   
			CFrameMon = CFrame.new(-13680.607421875, 501.08154296875, -6991.189453125)
		elseif MyLevel == 1900 or MyLevel <= 1924 then
			Mon = "Jungle Pirate [Lv. 1900]"
			LevelQuest = 1
			NameQuest = "DeepForestIsland2"
			NameMon = "Jungle Pirate"
			CFrameQuest = CFrame.new(-12680.3818, 389.971039, -9902.01953, -0.0871315002, 0, 0.996196866, 0, 1, 0, -0.996196866, 0, -0.0871315002)
			CFrameMon = CFrame.new(-12256.16015625, 331.73828125, -10485.8369140625)
		elseif MyLevel == 1925 or MyLevel <= 1974 then
			Mon = "Musketeer Pirate [Lv. 1925]"
			LevelQuest = 2
			NameQuest = "DeepForestIsland2"
			NameMon = "Musketeer Pirate"
			CFrameQuest = CFrame.new(-12680.3818, 389.971039, -9902.01953, -0.0871315002, 0, 0.996196866, 0, 1, 0, -0.996196866, 0, -0.0871315002)
			CFrameMon = CFrame.new(-13457.904296875, 391.545654296875, -9859.177734375)
		elseif MyLevel == 1975 or MyLevel <= 1999 then
			Mon = "Reborn Skeleton [Lv. 1975]"
			LevelQuest = 1
			NameQuest = "HauntedQuest1"
			NameMon = "Reborn Skeleton"
			CFrameQuest = CFrame.new(-9479.2168, 141.215088, 5566.09277, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			CFrameMon = CFrame.new(-8763.7236328125, 165.72299194335938, 6159.86181640625)
		elseif MyLevel == 2000 or MyLevel <= 2024 then
			Mon = "Living Zombie [Lv. 2000]"
			LevelQuest = 2
			NameQuest = "HauntedQuest1"
			NameMon = "Living Zombie"
			CFrameQuest = CFrame.new(-9479.2168, 141.215088, 5566.09277, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			CFrameMon = CFrame.new(-10144.1318359375, 138.62667846679688, 5838.0888671875)
		elseif MyLevel == 2025 or MyLevel <= 2049 then
			Mon = "Demonic Soul [Lv. 2025]"
			LevelQuest = 1
			NameQuest = "HauntedQuest2"
			NameMon = "Demonic Soul"
			CFrameQuest = CFrame.new(-9516.99316, 172.017181, 6078.46533, 0, 0, -1, 0, 1, 0, 1, 0, 0) 
			CFrameMon = CFrame.new(-9505.8720703125, 172.10482788085938, 6158.9931640625)
		elseif MyLevel == 2050 or MyLevel <= 2074 then
			Mon = "Posessed Mummy [Lv. 2050]"
			LevelQuest = 2
			NameQuest = "HauntedQuest2"
			NameMon = "Posessed Mummy"
			CFrameQuest = CFrame.new(-9516.99316, 172.017181, 6078.46533, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			CFrameMon = CFrame.new(-9582.0224609375, 6.251527309417725, 6205.478515625)
		elseif MyLevel == 2075 or MyLevel <= 2099 then
			Mon = "Peanut Scout [Lv. 2075]"
			LevelQuest = 1
			NameQuest = "NutsIslandQuest"
			NameMon = "Peanut Scout"
			CFrameQuest = CFrame.new(-2104.3908691406, 38.104167938232, -10194.21875, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			CFrameMon = CFrame.new(-2143.241943359375, 47.72198486328125, -10029.9951171875)
		elseif MyLevel == 2100 or MyLevel <= 2124 then
			Mon = "Peanut President [Lv. 2100]"
			LevelQuest = 2
			NameQuest = "NutsIslandQuest"
			NameMon = "Peanut President"
			CFrameQuest = CFrame.new(-2104.3908691406, 38.104167938232, -10194.21875, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			CFrameMon = CFrame.new(-1859.35400390625, 38.10316848754883, -10422.4296875)
		elseif MyLevel == 2125 or MyLevel <= 2149 then
			Mon = "Ice Cream Chef [Lv. 2125]"
			LevelQuest = 1
			NameQuest = "IceCreamIslandQuest"
			NameMon = "Ice Cream Chef"
			CFrameQuest = CFrame.new(-820.64825439453, 65.819526672363, -10965.795898438, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			CFrameMon = CFrame.new(-872.24658203125, 65.81957244873047, -10919.95703125)
		elseif MyLevel == 2150 or MyLevel <= 2199 then
			Mon = "Ice Cream Commander [Lv. 2150]"
			LevelQuest = 2
			NameQuest = "IceCreamIslandQuest"
			NameMon = "Ice Cream Commander"
			CFrameQuest = CFrame.new(-820.64825439453, 65.819526672363, -10965.795898438, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			CFrameMon = CFrame.new(-558.06103515625, 112.04895782470703, -11290.7744140625)
		elseif MyLevel == 2200 or MyLevel <= 2224 then
			Mon = "Cookie Crafter [Lv. 2200]"
			LevelQuest = 1
			NameQuest = "CakeQuest1"
			NameMon = "Cookie Crafter"
			CFrameQuest = CFrame.new(-2021.32007, 37.7982254, -12028.7295, 0.957576931, -8.80302053e-08, 0.288177818, 6.9301187e-08, 1, 7.51931211e-08, -0.288177818, -5.2032135e-08, 0.957576931)
			CFrameMon = CFrame.new(-2374.13671875, 37.79826354980469, -12125.30859375)
		elseif MyLevel == 2225 or MyLevel <= 2249 then
			Mon = "Cake Guard [Lv. 2225]"
			LevelQuest = 2
			NameQuest = "CakeQuest1"
			NameMon = "Cake Guard"
			CFrameQuest = CFrame.new(-2021.32007, 37.7982254, -12028.7295, 0.957576931, -8.80302053e-08, 0.288177818, 6.9301187e-08, 1, 7.51931211e-08, -0.288177818, -5.2032135e-08, 0.957576931)
			CFrameMon = CFrame.new(-1598.3070068359375, 43.773197174072266, -12244.5810546875)
		elseif MyLevel == 2250 or MyLevel <= 2274 then
			Mon = "Baking Staff [Lv. 2250]"
			LevelQuest = 1
			NameQuest = "CakeQuest2"
			NameMon = "Baking Staff"
			CFrameQuest = CFrame.new(-1927.91602, 37.7981339, -12842.5391, -0.96804446, 4.22142143e-08, 0.250778586, 4.74911062e-08, 1, 1.49904711e-08, -0.250778586, 2.64211941e-08, -0.96804446)
			CFrameMon = CFrame.new(-1887.8099365234375, 77.6185073852539, -12998.3505859375)
		elseif MyLevel == 2275 or MyLevel <= 2299 then
			Mon = "Head Baker [Lv. 2275]"
			LevelQuest = 2
			NameQuest = "CakeQuest2"
			NameMon = "Head Baker"
			CFrameQuest = CFrame.new(-1927.91602, 37.7981339, -12842.5391, -0.96804446, 4.22142143e-08, 0.250778586, 4.74911062e-08, 1, 1.49904711e-08, -0.250778586, 2.64211941e-08, -0.96804446)
			CFrameMon = CFrame.new(-2216.188232421875, 82.884521484375, -12869.2939453125)
		elseif MyLevel == 2300 or MyLevel <= 2324 then
			Mon = "Cocoa Warrior [Lv. 2300]"
			LevelQuest = 1
			NameQuest = "ChocQuest1"
			NameMon = "Cocoa Warrior"
			CFrameQuest = CFrame.new(233.22836303710938, 29.876001358032227, -12201.2333984375)
			CFrameMon = CFrame.new(-21.55328369140625, 80.57499694824219, -12352.3876953125)
		elseif MyLevel == 2325 or MyLevel <= 2349 then
			Mon = "Chocolate Bar Battler [Lv. 2325]"
			LevelQuest = 2
			NameQuest = "ChocQuest1"
			NameMon = "Chocolate Bar Battler"
			CFrameQuest = CFrame.new(233.22836303710938, 29.876001358032227, -12201.2333984375)
			CFrameMon = CFrame.new(582.590576171875, 77.18809509277344, -12463.162109375)
		elseif MyLevel == 2350 or MyLevel <= 2374 then
			Mon = "Sweet Thief [Lv. 2350]"
			LevelQuest = 1
			NameQuest = "ChocQuest2"
			NameMon = "Sweet Thief"
			CFrameQuest = CFrame.new(150.5066375732422, 30.693693161010742, -12774.5029296875)
			CFrameMon = CFrame.new(165.1884765625, 76.05885314941406, -12600.8369140625)
		elseif MyLevel == 2375 or MyLevel <= 2399 then
			Mon = "Candy Rebel [Lv. 2375]"
			LevelQuest = 2
			NameQuest = "ChocQuest2"
			NameMon = "Candy Rebel"
			CFrameQuest = CFrame.new(150.5066375732422, 30.693693161010742, -12774.5029296875)
			CFrameMon = CFrame.new(134.86563110351562, 77.2476806640625, -12876.5478515625)
		elseif MyLevel == 2400 or MyLevel <= 2449 then
			Mon = "Candy Pirate [Lv. 2400]"
			LevelQuest = 1
			NameQuest = "CandyQuest1"
			NameMon = "Candy Pirate"
			CFrameQuest = CFrame.new(-1150.0400390625, 20.378934860229492, -14446.3349609375)
			CFrameMon = CFrame.new(-1310.5003662109375, 26.016523361206055, -14562.404296875)
		elseif MyLevel <= 2450 then
			Mon = "Snow Demon [Lv. 2425]"
			LevelQuest = 2
			NameQuest = "CandyQuest1"
			NameMon = "Snow Demon"
			CFrameQuest = CFrame.new(-1150.0400390625, 20.378934860229492, -14446.3349609375)
			CFrameMon = CFrame.new(-899.0391235351562, 26.511367797851562, -14430.8681640625)
		end
	end
end
    

function Hop()
	local PlaceID = game.PlaceId
	local AllIDs = {}
	local foundAnything = ""
	local actualHour = os.date("!*t").hour
	local Deleted = false
	function TPReturner()
		local Site;
		if foundAnything == "" then
			Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100'))
		else
			Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100&cursor=' .. foundAnything))
		end
		local ID = ""
		if Site.nextPageCursor and Site.nextPageCursor ~= "null" and Site.nextPageCursor ~= nil then
			foundAnything = Site.nextPageCursor
		end
		local num = 0;
		for i,v in pairs(Site.data) do
			local Possible = true
			ID = tostring(v.id)
			if tonumber(v.maxPlayers) > tonumber(v.playing) then
				for _,Existing in pairs(AllIDs) do
					if num ~= 0 then
						if ID == tostring(Existing) then
							Possible = false
						end
					else
						if tonumber(actualHour) ~= tonumber(Existing) then
							local delFile = pcall(function()
								AllIDs = {}
								table.insert(AllIDs, actualHour)
							end)
						end
					end
					num = num + 1
				end
				if Possible == true then
					table.insert(AllIDs, ID)
					wait()
					pcall(function()
						wait()
						game:GetService("TeleportService"):TeleportToPlaceInstance(PlaceID, ID, game.Players.LocalPlayer)
					end)
					wait(4)
				end
			end
		end
	end
	function Teleport() 
		while wait() do
			pcall(function()
				TPReturner()
				if foundAnything ~= "" then
					TPReturner()
				end
			end)
		end
	end
	Teleport()
end                   
function UpdateIslandESP() 
	for i,v in pairs(game:GetService("Workspace")["_WorldOrigin"].Locations:GetChildren()) do
		pcall(function()
			if IslandESP then 
				if v.Name ~= "Sea" then
					if not v:FindFirstChild('NameEsp') then
						local bill = Instance.new('BillboardGui',v)
						bill.Name = 'NameEsp'
						bill.ExtentsOffset = Vector3.new(0, 1, 0)
						bill.Size = UDim2.new(1,200,1,30)
						bill.Adornee = v
						bill.AlwaysOnTop = true
						local name = Instance.new('TextLabel',bill)
						name.Font = "GothamBold"
						name.FontSize = "Size14"
						name.TextWrapped = true
						name.Size = UDim2.new(1,0,1,0)
						name.TextYAlignment = 'Top'
						name.BackgroundTransparency = 1
						name.TextStrokeTransparency = 0.5
						name.TextColor3 = Color3.fromRGB(7, 236, 240)
					else
						v['NameEsp'].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
					end
				end
			else
				if v:FindFirstChild('NameEsp') then
					v:FindFirstChild('NameEsp'):Destroy()
				end
			end
		end)
	end
end

function isnil(thing)
return (thing == nil)
end
local function round(n)
return math.floor(tonumber(n) + 0.5)
end
Number = math.random(1, 1000000)
function UpdatePlayerChams()
for i,v in pairs(game:GetService'Players':GetChildren()) do
	pcall(function()
		if not isnil(v.Character) then
			if ESPPlayer then
				if not isnil(v.Character.Head) and not v.Character.Head:FindFirstChild('NameEsp'..Number) then
					local bill = Instance.new('BillboardGui',v.Character.Head)
					bill.Name = 'NameEsp'..Number
					bill.ExtentsOffset = Vector3.new(0, 1, 0)
					bill.Size = UDim2.new(1,200,1,30)
					bill.Adornee = v.Character.Head
					bill.AlwaysOnTop = true
					local name = Instance.new('TextLabel',bill)
					name.Font = Enum.Font.GothamSemibold
					name.FontSize = "Size14"
					name.TextWrapped = true
					name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Character.Head.Position).Magnitude/3) ..' Distance')
					name.Size = UDim2.new(1,0,1,0)
					name.TextYAlignment = 'Top'
					name.BackgroundTransparency = 1
					name.TextStrokeTransparency = 0.5
					if v.Team == game.Players.LocalPlayer.Team then
						name.TextColor3 = Color3.new(0,255,0)
					else
						name.TextColor3 = Color3.new(255,0,0)
					end
				else
					v.Character.Head['NameEsp'..Number].TextLabel.Text = (v.Name ..' | '.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Character.Head.Position).Magnitude/3) ..' Distance\nHealth : ' .. round(v.Character.Humanoid.Health*100/v.Character.Humanoid.MaxHealth) .. '%')
				end
			else
				if v.Character.Head:FindFirstChild('NameEsp'..Number) then
					v.Character.Head:FindFirstChild('NameEsp'..Number):Destroy()
				end
			end
		end
	end)
end
end
function UpdateChestChams() 
for i,v in pairs(game.Workspace:GetChildren()) do
	pcall(function()
		if string.find(v.Name,"Chest") then
			if ChestESP then
				if string.find(v.Name,"Chest") then
					if not v:FindFirstChild('NameEsp'..Number) then
						local bill = Instance.new('BillboardGui',v)
						bill.Name = 'NameEsp'..Number
						bill.ExtentsOffset = Vector3.new(0, 1, 0)
						bill.Size = UDim2.new(1,200,1,30)
						bill.Adornee = v
						bill.AlwaysOnTop = true
						local name = Instance.new('TextLabel',bill)
						name.Font = Enum.Font.GothamSemibold
						name.FontSize = "Size14"
						name.TextWrapped = true
						name.Size = UDim2.new(1,0,1,0)
						name.TextYAlignment = 'Top'
						name.BackgroundTransparency = 1
						name.TextStrokeTransparency = 0.5
						if v.Name == "Chest1" then
							name.TextColor3 = Color3.fromRGB(109, 109, 109)
							name.Text = ("Chest 1" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						end
						if v.Name == "Chest2" then
							name.TextColor3 = Color3.fromRGB(173, 158, 21)
							name.Text = ("Chest 2" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						end
						if v.Name == "Chest3" then
							name.TextColor3 = Color3.fromRGB(85, 255, 255)
							name.Text = ("Chest 3" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						end
					else
						v['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
					end
				end
			else
				if v:FindFirstChild('NameEsp'..Number) then
					v:FindFirstChild('NameEsp'..Number):Destroy()
				end
			end
		end
	end)
end
end
function UpdateDevilChams() 
for i,v in pairs(game.Workspace:GetChildren()) do
	pcall(function()
		if DevilFruitESP then
			if string.find(v.Name, "Fruit") then   
				if not v.Handle:FindFirstChild('NameEsp'..Number) then
					local bill = Instance.new('BillboardGui',v.Handle)
					bill.Name = 'NameEsp'..Number
					bill.ExtentsOffset = Vector3.new(0, 1, 0)
					bill.Size = UDim2.new(1,200,1,30)
					bill.Adornee = v.Handle
					bill.AlwaysOnTop = true
					local name = Instance.new('TextLabel',bill)
					name.Font = Enum.Font.GothamSemibold
					name.FontSize = "Size14"
					name.TextWrapped = true
					name.Size = UDim2.new(1,0,1,0)
					name.TextYAlignment = 'Top'
					name.BackgroundTransparency = 1
					name.TextStrokeTransparency = 0.5
					name.TextColor3 = Color3.fromRGB(255, 255, 255)
					name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
				else
					v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
				end
			end
		else
			if v.Handle:FindFirstChild('NameEsp'..Number) then
				v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
			end
		end
	end)
end
end
function UpdateFlowerChams() 
for i,v in pairs(game.Workspace:GetChildren()) do
	pcall(function()
		if v.Name == "Flower2" or v.Name == "Flower1" then
			if FlowerESP then 
				if not v:FindFirstChild('NameEsp'..Number) then
					local bill = Instance.new('BillboardGui',v)
					bill.Name = 'NameEsp'..Number
					bill.ExtentsOffset = Vector3.new(0, 1, 0)
					bill.Size = UDim2.new(1,200,1,30)
					bill.Adornee = v
					bill.AlwaysOnTop = true
					local name = Instance.new('TextLabel',bill)
					name.Font = Enum.Font.GothamSemibold
					name.FontSize = "Size14"
					name.TextWrapped = true
					name.Size = UDim2.new(1,0,1,0)
					name.TextYAlignment = 'Top'
					name.BackgroundTransparency = 1
					name.TextStrokeTransparency = 0.5
					name.TextColor3 = Color3.fromRGB(255, 0, 0)
					if v.Name == "Flower1" then 
						name.Text = ("Blue Flower" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						name.TextColor3 = Color3.fromRGB(0, 0, 255)
					end
					if v.Name == "Flower2" then
						name.Text = ("Red Flower" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						name.TextColor3 = Color3.fromRGB(255, 0, 0)
					end
				else
					v['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
				end
			else
				if v:FindFirstChild('NameEsp'..Number) then
				v:FindFirstChild('NameEsp'..Number):Destroy()
				end
			end
		end   
	end)
end
end
function UpdateRealFruitChams() 
for i,v in pairs(game.Workspace.AppleSpawner:GetChildren()) do
	if v:IsA("Tool") then
		if RealFruitESP then 
			if not v.Handle:FindFirstChild('NameEsp'..Number) then
				local bill = Instance.new('BillboardGui',v.Handle)
				bill.Name = 'NameEsp'..Number
				bill.ExtentsOffset = Vector3.new(0, 1, 0)
				bill.Size = UDim2.new(1,200,1,30)
				bill.Adornee = v.Handle
				bill.AlwaysOnTop = true
				local name = Instance.new('TextLabel',bill)
				name.Font = Enum.Font.GothamSemibold
				name.FontSize = "Size14"
				name.TextWrapped = true
				name.Size = UDim2.new(1,0,1,0)
				name.TextYAlignment = 'Top'
				name.BackgroundTransparency = 1
				name.TextStrokeTransparency = 0.5
				name.TextColor3 = Color3.fromRGB(255, 0, 0)
				name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			else
				v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..' '.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			end
		else
			if v.Handle:FindFirstChild('NameEsp'..Number) then
				v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
			end
		end 
	end
end
for i,v in pairs(game.Workspace.PineappleSpawner:GetChildren()) do
	if v:IsA("Tool") then
		if RealFruitESP then 
			if not v.Handle:FindFirstChild('NameEsp'..Number) then
				local bill = Instance.new('BillboardGui',v.Handle)
				bill.Name = 'NameEsp'..Number
				bill.ExtentsOffset = Vector3.new(0, 1, 0)
				bill.Size = UDim2.new(1,200,1,30)
				bill.Adornee = v.Handle
				bill.AlwaysOnTop = true
				local name = Instance.new('TextLabel',bill)
				name.Font = Enum.Font.GothamSemibold
				name.FontSize = "Size14"
				name.TextWrapped = true
				name.Size = UDim2.new(1,0,1,0)
				name.TextYAlignment = 'Top'
				name.BackgroundTransparency = 1
				name.TextStrokeTransparency = 0.5
				name.TextColor3 = Color3.fromRGB(255, 174, 0)
				name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			else
				v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..' '.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			end
		else
			if v.Handle:FindFirstChild('NameEsp'..Number) then
				v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
			end
		end 
	end
end
for i,v in pairs(game.Workspace.BananaSpawner:GetChildren()) do
	if v:IsA("Tool") then
		if RealFruitESP then 
			if not v.Handle:FindFirstChild('NameEsp'..Number) then
				local bill = Instance.new('BillboardGui',v.Handle)
				bill.Name = 'NameEsp'..Number
				bill.ExtentsOffset = Vector3.new(0, 1, 0)
				bill.Size = UDim2.new(1,200,1,30)
				bill.Adornee = v.Handle
				bill.AlwaysOnTop = true
				local name = Instance.new('TextLabel',bill)
				name.Font = Enum.Font.GothamSemibold
				name.FontSize = "Size14"
				name.TextWrapped = true
				name.Size = UDim2.new(1,0,1,0)
				name.TextYAlignment = 'Top'
				name.BackgroundTransparency = 1
				name.TextStrokeTransparency = 0.5
				name.TextColor3 = Color3.fromRGB(251, 255, 0)
				name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			else
				v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..' '.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			end
		else
			if v.Handle:FindFirstChild('NameEsp'..Number) then
				v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
			end
		end 
	end
end
end

function UpdateIslandESP() 
	for i,v in pairs(game:GetService("Workspace")["_WorldOrigin"].Locations:GetChildren()) do
		pcall(function()
			if IslandESP then 
				if v.Name ~= "Sea" then
					if not v:FindFirstChild('NameEsp') then
						local bill = Instance.new('BillboardGui',v)
						bill.Name = 'NameEsp'
						bill.ExtentsOffset = Vector3.new(0, 1, 0)
						bill.Size = UDim2.new(1,200,1,30)
						bill.Adornee = v
						bill.AlwaysOnTop = true
						local name = Instance.new('TextLabel',bill)
						name.Font = "GothamBold"
						name.FontSize = "Size14"
						name.TextWrapped = true
						name.Size = UDim2.new(1,0,1,0)
						name.TextYAlignment = 'Top'
						name.BackgroundTransparency = 1
						name.TextStrokeTransparency = 0.5
						name.TextColor3 = Color3.fromRGB(7, 236, 240)
					else
						v['NameEsp'].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
					end
				end
			else
				if v:FindFirstChild('NameEsp') then
					v:FindFirstChild('NameEsp'):Destroy()
				end
			end
		end)
	end
end

function isnil(thing)
return (thing == nil)
end
local function round(n)
return math.floor(tonumber(n) + 0.5)
end
Number = math.random(1, 1000000)
function UpdatePlayerChams()
for i,v in pairs(game:GetService'Players':GetChildren()) do
	pcall(function()
		if not isnil(v.Character) then
			if ESPPlayer then
				if not isnil(v.Character.Head) and not v.Character.Head:FindFirstChild('NameEsp'..Number) then
					local bill = Instance.new('BillboardGui',v.Character.Head)
					bill.Name = 'NameEsp'..Number
					bill.ExtentsOffset = Vector3.new(0, 1, 0)
					bill.Size = UDim2.new(1,200,1,30)
					bill.Adornee = v.Character.Head
					bill.AlwaysOnTop = true
					local name = Instance.new('TextLabel',bill)
					name.Font = Enum.Font.GothamSemibold
					name.FontSize = "Size14"
					name.TextWrapped = true
					name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Character.Head.Position).Magnitude/3) ..' Distance')
					name.Size = UDim2.new(1,0,1,0)
					name.TextYAlignment = 'Top'
					name.BackgroundTransparency = 1
					name.TextStrokeTransparency = 0.5
					if v.Team == game.Players.LocalPlayer.Team then
						name.TextColor3 = Color3.new(0,255,0)
					else
						name.TextColor3 = Color3.new(255,0,0)
					end
				else
					v.Character.Head['NameEsp'..Number].TextLabel.Text = (v.Name ..' | '.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Character.Head.Position).Magnitude/3) ..' Distance\nHealth : ' .. round(v.Character.Humanoid.Health*100/v.Character.Humanoid.MaxHealth) .. '%')
				end
			else
				if v.Character.Head:FindFirstChild('NameEsp'..Number) then
					v.Character.Head:FindFirstChild('NameEsp'..Number):Destroy()
				end
			end
		end
	end)
end
end
function UpdateChestChams() 
for i,v in pairs(game.Workspace:GetChildren()) do
	pcall(function()
		if string.find(v.Name,"Chest") then
			if ChestESP then
				if string.find(v.Name,"Chest") then
					if not v:FindFirstChild('NameEsp'..Number) then
						local bill = Instance.new('BillboardGui',v)
						bill.Name = 'NameEsp'..Number
						bill.ExtentsOffset = Vector3.new(0, 1, 0)
						bill.Size = UDim2.new(1,200,1,30)
						bill.Adornee = v
						bill.AlwaysOnTop = true
						local name = Instance.new('TextLabel',bill)
						name.Font = Enum.Font.GothamSemibold
						name.FontSize = "Size14"
						name.TextWrapped = true
						name.Size = UDim2.new(1,0,1,0)
						name.TextYAlignment = 'Top'
						name.BackgroundTransparency = 1
						name.TextStrokeTransparency = 0.5
						if v.Name == "Chest1" then
							name.TextColor3 = Color3.fromRGB(109, 109, 109)
							name.Text = ("Chest 1" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						end
						if v.Name == "Chest2" then
							name.TextColor3 = Color3.fromRGB(173, 158, 21)
							name.Text = ("Chest 2" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						end
						if v.Name == "Chest3" then
							name.TextColor3 = Color3.fromRGB(85, 255, 255)
							name.Text = ("Chest 3" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						end
					else
						v['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
					end
				end
			else
				if v:FindFirstChild('NameEsp'..Number) then
					v:FindFirstChild('NameEsp'..Number):Destroy()
				end
			end
		end
	end)
end
end
function UpdateDevilChams() 
for i,v in pairs(game.Workspace:GetChildren()) do
	pcall(function()
		if DevilFruitESP then
			if string.find(v.Name, "Fruit") then   
				if not v.Handle:FindFirstChild('NameEsp'..Number) then
					local bill = Instance.new('BillboardGui',v.Handle)
					bill.Name = 'NameEsp'..Number
					bill.ExtentsOffset = Vector3.new(0, 1, 0)
					bill.Size = UDim2.new(1,200,1,30)
					bill.Adornee = v.Handle
					bill.AlwaysOnTop = true
					local name = Instance.new('TextLabel',bill)
					name.Font = Enum.Font.GothamSemibold
					name.FontSize = "Size14"
					name.TextWrapped = true
					name.Size = UDim2.new(1,0,1,0)
					name.TextYAlignment = 'Top'
					name.BackgroundTransparency = 1
					name.TextStrokeTransparency = 0.5
					name.TextColor3 = Color3.fromRGB(255, 255, 255)
					name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
				else
					v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
				end
			end
		else
			if v.Handle:FindFirstChild('NameEsp'..Number) then
				v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
			end
		end
	end)
end
end
function UpdateFlowerChams() 
for i,v in pairs(game.Workspace:GetChildren()) do
	pcall(function()
		if v.Name == "Flower2" or v.Name == "Flower1" then
			if FlowerESP then 
				if not v:FindFirstChild('NameEsp'..Number) then
					local bill = Instance.new('BillboardGui',v)
					bill.Name = 'NameEsp'..Number
					bill.ExtentsOffset = Vector3.new(0, 1, 0)
					bill.Size = UDim2.new(1,200,1,30)
					bill.Adornee = v
					bill.AlwaysOnTop = true
					local name = Instance.new('TextLabel',bill)
					name.Font = Enum.Font.GothamSemibold
					name.FontSize = "Size14"
					name.TextWrapped = true
					name.Size = UDim2.new(1,0,1,0)
					name.TextYAlignment = 'Top'
					name.BackgroundTransparency = 1
					name.TextStrokeTransparency = 0.5
					name.TextColor3 = Color3.fromRGB(255, 0, 0)
					if v.Name == "Flower1" then 
						name.Text = ("Blue Flower" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						name.TextColor3 = Color3.fromRGB(0, 0, 255)
					end
					if v.Name == "Flower2" then
						name.Text = ("Red Flower" ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
						name.TextColor3 = Color3.fromRGB(255, 0, 0)
					end
				else
					v['NameEsp'..Number].TextLabel.Text = (v.Name ..'   \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Position).Magnitude/3) ..' Distance')
				end
			else
				if v:FindFirstChild('NameEsp'..Number) then
				v:FindFirstChild('NameEsp'..Number):Destroy()
				end
			end
		end   
	end)
end
end
function UpdateRealFruitChams() 
for i,v in pairs(game.Workspace.AppleSpawner:GetChildren()) do
	if v:IsA("Tool") then
		if RealFruitESP then 
			if not v.Handle:FindFirstChild('NameEsp'..Number) then
				local bill = Instance.new('BillboardGui',v.Handle)
				bill.Name = 'NameEsp'..Number
				bill.ExtentsOffset = Vector3.new(0, 1, 0)
				bill.Size = UDim2.new(1,200,1,30)
				bill.Adornee = v.Handle
				bill.AlwaysOnTop = true
				local name = Instance.new('TextLabel',bill)
				name.Font = Enum.Font.GothamSemibold
				name.FontSize = "Size14"
				name.TextWrapped = true
				name.Size = UDim2.new(1,0,1,0)
				name.TextYAlignment = 'Top'
				name.BackgroundTransparency = 1
				name.TextStrokeTransparency = 0.5
				name.TextColor3 = Color3.fromRGB(255, 0, 0)
				name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			else
				v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..' '.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			end
		else
			if v.Handle:FindFirstChild('NameEsp'..Number) then
				v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
			end
		end 
	end
end
for i,v in pairs(game.Workspace.PineappleSpawner:GetChildren()) do
	if v:IsA("Tool") then
		if RealFruitESP then 
			if not v.Handle:FindFirstChild('NameEsp'..Number) then
				local bill = Instance.new('BillboardGui',v.Handle)
				bill.Name = 'NameEsp'..Number
				bill.ExtentsOffset = Vector3.new(0, 1, 0)
				bill.Size = UDim2.new(1,200,1,30)
				bill.Adornee = v.Handle
				bill.AlwaysOnTop = true
				local name = Instance.new('TextLabel',bill)
				name.Font = Enum.Font.GothamSemibold
				name.FontSize = "Size14"
				name.TextWrapped = true
				name.Size = UDim2.new(1,0,1,0)
				name.TextYAlignment = 'Top'
				name.BackgroundTransparency = 1
				name.TextStrokeTransparency = 0.5
				name.TextColor3 = Color3.fromRGB(255, 174, 0)
				name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			else
				v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..' '.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			end
		else
			if v.Handle:FindFirstChild('NameEsp'..Number) then
				v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
			end
		end 
	end
end
for i,v in pairs(game.Workspace.BananaSpawner:GetChildren()) do
	if v:IsA("Tool") then
		if RealFruitESP then 
			if not v.Handle:FindFirstChild('NameEsp'..Number) then
				local bill = Instance.new('BillboardGui',v.Handle)
				bill.Name = 'NameEsp'..Number
				bill.ExtentsOffset = Vector3.new(0, 1, 0)
				bill.Size = UDim2.new(1,200,1,30)
				bill.Adornee = v.Handle
				bill.AlwaysOnTop = true
				local name = Instance.new('TextLabel',bill)
				name.Font = Enum.Font.GothamSemibold
				name.FontSize = "Size14"
				name.TextWrapped = true
				name.Size = UDim2.new(1,0,1,0)
				name.TextYAlignment = 'Top'
				name.BackgroundTransparency = 1
				name.TextStrokeTransparency = 0.5
				name.TextColor3 = Color3.fromRGB(251, 255, 0)
				name.Text = (v.Name ..' \n'.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			else
				v.Handle['NameEsp'..Number].TextLabel.Text = (v.Name ..' '.. round((game:GetService('Players').LocalPlayer.Character.Head.Position - v.Handle.Position).Magnitude/3) ..' Distance')
			end
		else
			if v.Handle:FindFirstChild('NameEsp'..Number) then
				v.Handle:FindFirstChild('NameEsp'..Number):Destroy()
			end
		end 
	end
end
end

function InfAb()
	if InfAbility then
		if not game:GetService("Players").LocalPlayer.Character.HumanoidRootPart:FindFirstChild("Agility") then
			local inf = Instance.new("ParticleEmitter")
			inf.Acceleration = Vector3.new(0,0,0)
			inf.Archivable = true
			inf.Drag = 20
			inf.EmissionDirection = Enum.NormalId.Top
			inf.Enabled = true
			inf.Lifetime = NumberRange.new(0,0)
			inf.LightInfluence = 0
			inf.LockedToPart = true
			inf.Name = "Agility"
			inf.Rate = 500
			local numberKeypoints2 = {
				NumberSequenceKeypoint.new(0, 0);
				NumberSequenceKeypoint.new(1, 4); 
			}
			inf.Size = NumberSequence.new(numberKeypoints2)
			inf.RotSpeed = NumberRange.new(9999, 99999)
			inf.Rotation = NumberRange.new(0, 0)
			inf.Speed = NumberRange.new(30, 30)
			inf.SpreadAngle = Vector2.new(0,0,0,0)
			inf.Texture = ""
			inf.VelocityInheritance = 0
			inf.ZOffset = 2
			inf.Transparency = NumberSequence.new(0)
			inf.Color = ColorSequence.new(Color3.fromRGB(0,0,0),Color3.fromRGB(0,0,0))
			inf.Parent = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart
		end
	else
		if game:GetService("Players").LocalPlayer.Character.HumanoidRootPart:FindFirstChild("Agility") then
			game:GetService("Players").LocalPlayer.Character.HumanoidRootPart:FindFirstChild("Agility"):Destroy()
		end
	end
end

local LocalPlayer = game:GetService'Players'.LocalPlayer
local originalstam = LocalPlayer.Character.Energy.Value
function infinitestam()
	LocalPlayer.Character.Energy.Changed:connect(function()
		if InfiniteEnergy then
			LocalPlayer.Character.Energy.Value = originalstam
		end 
	end)
end

spawn(function()
	pcall(function()
		while wait(.1) do
			if InfiniteEnergy then
				wait(0.1)
				originalstam = LocalPlayer.Character.Energy.Value
				infinitestam()
			end
		end
	end)
end)

function NoDodgeCool()
	if nododgecool then
		for i,v in next, getgc() do
			if game:GetService("Players").LocalPlayer.Character.Dodge then
				if typeof(v) == "function" and getfenv(v).script == game:GetService("Players").LocalPlayer.Character.Dodge then
					for i2,v2 in next, getupvalues(v) do
						if tostring(v2) == "0.1" then
						repeat wait(.1)
							setupvalue(v,i2,0)
						until not nododgecool
						end
					end
				end
			end
		end
	end
end

function fly()
	local mouse=game:GetService("Players").LocalPlayer:GetMouse''
	localplayer=game:GetService("Players").LocalPlayer
	game:GetService("Players").LocalPlayer.Character:WaitForChild("HumanoidRootPart")
	local torso = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart
	local speedSET=25
	local keys={a=false,d=false,w=false,s=false}
	local e1
	local e2
	local function start()
		local pos = Instance.new("BodyPosition",torso)
		local gyro = Instance.new("BodyGyro",torso)
		pos.Name="EPIXPOS"
		pos.maxForce = Vector3.new(math.huge, math.huge, math.huge)
		pos.position = torso.Position
		gyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
		gyro.CFrame = torso.CFrame
		repeat
				wait()
				localplayer.Character.Humanoid.PlatformStand=true
				local new=gyro.CFrame - gyro.CFrame.p + pos.position
				if not keys.w and not keys.s and not keys.a and not keys.d then
				speed=1
				end
				if keys.w then
				new = new + workspace.CurrentCamera.CoordinateFrame.lookVector * speed
				speed=speed+speedSET
				end
				if keys.s then
				new = new - workspace.CurrentCamera.CoordinateFrame.lookVector * speed
				speed=speed+speedSET
				end
				if keys.d then
				new = new * CFrame.new(speed,0,0)
				speed=speed+speedSET
				end
				if keys.a then
				new = new * CFrame.new(-speed,0,0)
				speed=speed+speedSET
				end
				if speed>speedSET then
				speed=speedSET
				end
				pos.position=new.p
				if keys.w then
				gyro.CFrame = workspace.CurrentCamera.CoordinateFrame*CFrame.Angles(-math.rad(speed*15),0,0)
				elseif keys.s then
				gyro.CFrame = workspace.CurrentCamera.CoordinateFrame*CFrame.Angles(math.rad(speed*15),0,0)
				else
				gyro.CFrame = workspace.CurrentCamera.CoordinateFrame
				end
		until not Fly
		if gyro then 
				gyro:Destroy() 
		end
		if pos then 
				pos:Destroy() 
		end
		flying=false
		localplayer.Character.Humanoid.PlatformStand=false
		speed=0
	end
	e1=mouse.KeyDown:connect(function(key)
		if not torso or not torso.Parent then 
				flying=false e1:disconnect() e2:disconnect() return 
		end
		if key=="w" then
			keys.w=true
		elseif key=="s" then
			keys.s=true
		elseif key=="a" then
			keys.a=true
		elseif key=="d" then
			keys.d=true
		end
	end)
	e2=mouse.KeyUp:connect(function(key)
		if key=="w" then
			keys.w=false
		elseif key=="s" then
			keys.s=false
		elseif key=="a" then
			keys.a=false
		elseif key=="d" then
			keys.d=false
		end
	end)
	start()
end

function Click()
	game:GetService'VirtualUser':CaptureController()
	game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
end

function AutoHaki()
	if not game:GetService("Players").LocalPlayer.Character:FindFirstChild("HasBuso") then
		game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
	end
end

function UnEquipWeapon(Weapon)
	if game.Players.LocalPlayer.Character:FindFirstChild(Weapon) then
		_G.NotAutoEquip = true
		wait(.5)
		game.Players.LocalPlayer.Character:FindFirstChild(Weapon).Parent = game.Players.LocalPlayer.Backpack
		wait(.1)
		_G.NotAutoEquip = false
	end
end

function EquipWeapon(ToolSe)
	if not _G.NotAutoEquip then
		if game.Players.LocalPlayer.Backpack:FindFirstChild(ToolSe) then
			Tool = game.Players.LocalPlayer.Backpack:FindFirstChild(ToolSe)
			wait(.1)
			game.Players.LocalPlayer.Character.Humanoid:EquipTool(Tool)
		end
	end
end


function GetDistance(target)
	return math.floor((target.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude)
end

TP = function(p)
	task.spawn(function()
		pcall(function()
			if game:GetService("Players").LocalPlayer:DistanceFromCharacter(p.Position) <= 250 then 
				game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = p
			elseif not game.Players.LocalPlayer.Character:FindFirstChild("Root")then 
				local K = Instance.new("Part",game.Players.LocalPlayer.Character)
				K.Size = Vector3.new(1,0.5,1)
				K.Name = "Root"
				K.Anchored = true
				K.Transparency = 1
				K.CanCollide = false
				K.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,20,0)
			end
			local U = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position-p.Position).Magnitude
			local z = game:service("TweenService")
			local B = TweenInfo.new((p.Position-game.Players.LocalPlayer.Character.Root.Position).Magnitude/300,Enum.EasingStyle.Linear)
			local S,g = pcall(function()
			local q = z:Create(game.Players.LocalPlayer.Character.Root,B,{CFrame = p})
			q:Play()
		end)
		if not S then 
			return g
		end
		game.Players.LocalPlayer.Character.Root.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
			if S and game.Players.LocalPlayer.Character:FindFirstChild("Root") then 
				pcall(function()
					if (game.Players.LocalPlayer.Character.HumanoidRootPart.Position-p.Position).Magnitude >= 20 then 
						spawn(function()
							pcall(function()
								if (game.Players.LocalPlayer.Character.Root.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 150 then 
									game.Players.LocalPlayer.Character.Root.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
								else 
									game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame=game.Players.LocalPlayer.Character.Root.CFrame
								end
							end)
						end)
					elseif (game.Players.LocalPlayer.Character.HumanoidRootPart.Position-p.Position).Magnitude >= 10 and(game.Players.LocalPlayer.Character.HumanoidRootPart.Position-p.Position).Magnitude < 20 then 
						game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = p
					elseif (game.Players.LocalPlayer.Character.HumanoidRootPart.Position-p.Position).Magnitude < 10 then 
						game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = p
					end
				end)
			end
		end)
	end)
end
	
		function topos(Pos)
		Distance = (Pos.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
		if Distance < 25 then
			Speed = 10000
		elseif Distance < 50 then
			Speed = 5000
		elseif Distance < 250 then
			Speed = 600
		elseif Distance < 500 then
			Speed = 300
		elseif Distance < 750 then
			Speed = 250
		elseif Distance >= 1000 then
			Speed = 200
		end
		game:GetService("TweenService"):Create(
			game:GetService("Players").LocalPlayer.Character.HumanoidRootPart,
			TweenInfo.new(Distance/Speed, Enum.EasingStyle.Linear),
			{CFrame = Pos}
		):Play()
end




spawn(function()
	pcall(function()
		while wait() do
			if _G.AutoFarm or _G.AutoDoughtBoss or _G.Auto_DungeonMobAura or _G.AutoFarmChest or _G.AutoFarmBossHallow or _G.AutoFarmSwanGlasses or _G.AutoLongSword or _G.AutoBlackSpikeycoat or _G.AutoElectricClaw or _G.AutoFarmGunMastery or _G.AutoHolyTorch or _G.AutoLawRaid or _G.AutoFarmBoss or _G.AutoTwinHooks or _G.AutoOpenSwanDoor or _G.AutoDragon_Trident or _G.AutoSaber or _G.AutoFarmFruitMastery or _G.AutoFarmGunMastery or _G.TeleportIsland or _G.Auto_EvoRace or _G.AutoFarmAllMsBypassType or _G.AutoObservationv2 or _G.AutoMusketeerHat or _G.AutoEctoplasm or _G.AutoRengoku or _G.Auto_Rainbow_Haki or _G.AutoObservation or _G.AutoDarkDagger or _G.Safe_Mode or _G.MasteryFruit or _G.AutoBudySword or _G.AutoOderSword or _G.AutoBounty or _G.AutoAllBoss or _G.Auto_Bounty or _G.AutoSharkman or _G.Auto_Mastery_Fruit or _G.Auto_Mastery_Gun or _G.Auto_Dungeon or _G.Auto_Cavender or _G.Auto_Pole or _G.Auto_Kill_Ply or _G.Auto_Factory or _G.AutoSecondSea or _G.TeleportPly or _G.AutoBartilo or _G.Auto_DarkBoss or _G.GrabChest or _G.AutoFarmBounty or _G.Holy_Torch or _G.AutoFarm or _G.Clip or FarmBoss or _G.AutoElitehunter or _G.AutoThirdSea or _G.Auto_Bone or _G.Autoheart or _G.Autodoughking or _G.AutoFarmMaterial or _G.AutoNevaSoulGuitar or _G.Auto_Dragon_Trident or _G.Autotushita or _G.d or _G.Autowaden or _G.Autogay or _G.Autopole or _G.Autosaw or _G.AutoObservationHakiV2 or _G.AutoFarmNearest or AutoFarmChest or _G.AutoCarvender or _G.AutoTwinHook or AutoMobAura or _G.Tweenfruit or _G.AutoFactory or _G.TeleportNPC or AutoGodhuman == true then
				if not game:GetService("Players").LocalPlayer.Character.HumanoidRootPart:FindFirstChild("BodyClip") then
					local Noclip = Instance.new("BodyVelocity")
					Noclip.Name = "BodyClip"
					Noclip.Parent = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart
					Noclip.MaxForce = Vector3.new(100000,100000,100000)
					Noclip.Velocity = Vector3.new(0,0,0)
				end
			end
		end
	end)
end)

spawn(function()
	pcall(function()
		game:GetService("RunService").Stepped:Connect(function()
			if _G.AutoFarm or _G.AutoDoughtBoss or _G.Auto_DungeonMobAura or _G.AutoFarmChest or _G.AutoFarmBossHallow or _G.AutoFarmSwanGlasses or _G.AutoLongSword or _G.AutoBlackSpikeycoat or _G.AutoElectricClaw or _G.AutoFarmGunMastery or _G.AutoHolyTorch or _G.AutoLawRaid or _G.AutoFarmBoss or _G.AutoTwinHooks or _G.AutoOpenSwanDoor or _G.AutoDragon_Trident or _G.AutoSaber or _G.NOCLIP or _G.AutoFarmFruitMastery or _G.AutoFarmGunMastery or _G.TeleportIsland or _G.Auto_EvoRace or _G.AutoFarmAllMsBypassType or _G.AutoObservationv2 or _G.AutoMusketeerHat or _G.AutoEctoplasm or _G.AutoRengoku or _G.Auto_Rainbow_Haki or _G.AutoObservation or _G.AutoDarkDagger or _G.Safe_Mode or _G.MasteryFruit or _G.AutoBudySword or _G.AutoOderSword or _G.AutoBounty or _G.AutoAllBoss or _G.Auto_Bounty or _G.AutoSharkman or _G.Auto_Mastery_Fruit or _G.Auto_Mastery_Gun or _G.Auto_Dungeon or _G.Auto_Cavender or _G.Auto_Pole or _G.Auto_Kill_Ply or _G.Auto_Factory or _G.AutoSecondSea or _G.TeleportPly or _G.AutoBartilo or _G.Auto_DarkBoss or _G.GrabChest or _G.AutoFarmBounty or _G.Holy_Torch or _G.AutoFarm or _G.Clip or _G.AutoElitehunter or _G.AutoThirdSea or _G.Auto_Bone or _G.Autoheart or _G.Autodoughking or _G.AutoFarmMaterial or _G.AutoNevaSoulGuitar or _G.Auto_Dragon_Trident or _G.Autotushita or _G.Autowaden or _G.Autogay or _G.Autopole or _G.Autosaw or _G.AutoObservationHakiV2 or _G.AutoFarmNearest or AutoFarmChest or _G.AutoCarvender or _G.AutoTwinHook or AutoMobAura or _G.Tweenfruit or _G.AutoFactory or _G.TeleportNPC or AutoGodhuman == true then
				for _, v in pairs(game:GetService("Players").LocalPlayer.Character:GetDescendants()) do
					if v:IsA("BasePart") then
						v.CanCollide = false    
					end
				end
			end
		end)
	end)
end)

spawn(function()
	while wait() do
		if _G.AutoFarm or _G.Auto_DungeonMobAura or _G.AutoFarmChest or _G.AutoFarmBossHallow or _G.AutoFarmSwanGlasses or _G.AutoLongSword or _G.AutoBlackSpikeycoat or _G.AutoElectricClaw or _G.AutoFarmGunMastery or _G.AutoHolyTorch or _G.AutoLawRaid or _G.AutoFarmBoss or _G.AutoTwinHooks or _G.AutoOpenSwanDoor or _G.AutoDragon_Trident or _G.AutoSaber or _G.NOCLIP or _G.AutoFarmFruitMastery or _G.AutoFarmGunMastery or _G.TeleportIsland or _G.Auto_EvoRace or _G.AutoFarmAllMsBypassType or _G.AutoObservationv2 or _G.AutoMusketeerHat or _G.AutoEctoplasm or _G.AutoRengoku or _G.Auto_Rainbow_Haki or _G.AutoObservation or _G.AutoDarkDagger or _G.Safe_Mode or _G.MasteryFruit or _G.AutoBudySword or _G.AutoOderSword or _G.AutoAllBoss or _G.Auto_Bounty or _G.AutoSharkman or _G.Auto_Mastery_Fruit or _G.Auto_Mastery_Gun or _G.Auto_Dungeon or _G.Auto_Cavender or _G.Auto_Pole or _G.Auto_Kill_Ply or _G.Auto_Factory or _G.AutoSecondSea or _G.TeleportPly or _G.AutoBartilo or _G.Auto_DarkBoss or _G.AutoFarm or _G.Clip or _G.AutoElitehunter or _G.AutoThirdSea or _G.Auto_Bone or _G.Autoheart or _G.Autodoughking or _G.d or _G.Autowaden or _G.Autogay or _G.AutoObservationHakiV2 or _G.AutoFarmMaterial or _G.AutoFarmNearest or _G.AutoCarvender or _G.AutoTwinHook or AutoMobAura or _G.AutoFactory or AutoGodhuman == true then
			pcall(function()
				game:GetService("ReplicatedStorage").Remotes.CommE:FireServer("Ken",true)
			end)
		end    
	end
end)

spawn(function()
game:GetService("RunService").RenderStepped:Connect(function()
	if _G.AutoClick or Fastattack then
		 pcall(function()
			game:GetService'VirtualUser':CaptureController()
			game:GetService'VirtualUser':Button1Down(Vector2.new(0,1,0,1))
		end)
	end
end)
end)

function StopTween(target)
	if not target then
		_G.StopTween = true
		wait()
		topos(game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame)
		wait()
		if game:GetService("Players").LocalPlayer.Character.HumanoidRootPart:FindFirstChild("BodyClip") then
			game:GetService("Players").LocalPlayer.Character.HumanoidRootPart:FindFirstChild("BodyClip"):Destroy()
		end
		_G.StopTween = false
		_G.Clip = false
	end
end


game:GetService("Players").LocalPlayer.Idled:connect(function()
	game:GetService("VirtualUser"):Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
	wait(1)
	game:GetService("VirtualUser"):Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)

--// Get Weapon Sword
function GetWeaponInventory(Weaponname)
	for i,v in pairs(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("getInventory")) do
	if type(v) == "table" then
	if v.Type == "Sword" then
	if v.Name == Weaponname then
	return true
	end
	end
	end
	end
	return false
	end
--// Equip Gun Mastery
spawn(function()
	pcall(function()
	 while task.wait() do
	 for i,v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
	 if v:IsA("Tool") then
	 if v:FindFirstChild("RemoteFunctionShoot") then
	 CurrentEquipGun = v.Name
	 end
	 end
	 end
	 end
	 end)
	end)

game:GetService("StarterGui"):SetCore("SendNotification",{
	Title = "Namfon Hub",
	Text = "Loading...",
	Icon = "rbxassetid://13984057027"
})

wait(3)

game:GetService("StarterGui"):SetCore("SendNotification",{
	Title = "Namfon Hub" ,
	Text = " ✅ Loading Complete.!",
	Icon = "rbxassetid://13984057027"
})

wait(1)
local PepsisWorld = library:CreateWindow({
    Name = "Namfon Hub Free Script",
    Themeable = {
        Image = "rbxassetid://7483871523",
        }
})

local Page = PepsisWorld:CreateTab({
    Name = "General"
})
local Auto = PepsisWorld:CreateTab({
    Name = "AutoItem/Quest"
})
local PVP = PepsisWorld:CreateTab({
    Name = "Players"
})
local Raid = PepsisWorld:CreateTab({
    Name = "Miscellaneous"
})

local CR = Page:CreateSection({
	Name = "Script",
	Side = "Left"
})
local KUY = CR:AddLabel({
    Name = "Script Make By Namfon Hub"
})

local Time = Page:CreateSection({
	Name = "Time",
	Side = "Left"
})
local Time = Time:AddLabel({
    Name = "Executor Time"
})
spawn(function() getgenv().Time = true;while true do wait(.1) UpdateTime() end end);function UpdateTime() local date = os.date("*t");local hour = (date.hour) % 24;local ampm = hour < 12 and "AM" or "PM";local timezone = string.format("%02i:%02i:%02i %s", ((hour -1) % 12) + 1, date.min, date.sec, ampm);local datetime = string.format("%02d/%02d/%04d", date.day, date.month, date.year);local LocalizationService = game:GetService("LocalizationService");local Players = game:GetService("Players");local player = Players.LocalPlayer;local name = player.Name;local result, code = pcall(function()   return LocalizationService:GetCountryRegionForPlayerAsync(player)  end);Time:Set(" : " .. timezone);Time:Set("Executor Time: " .. datetime .. " [ " .. code .. " ]");spawn(function() if getgenv().Time then pcall(function()  while wait() do  Time()  end end) end end) end

local AutoFarm = Page:CreateSection({
    Name = "AutoFarm",
    Side = "Left"
})

AutoFarm:AddToggle({
    Name = "AutoFarm Level",
	Value = _G.AutoFarm,
    Callback = function(value)
        _G.AutoFarm = value
		StopTween(_G.AutoFarm)
    end
})

spawn(function()
	while wait() do
		if _G.AutoFarm then
			pcall(function()
				local QuestTitle = game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text
				if not string.find(QuestTitle, NameMon) then
					StartMagnet = false
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
				end
				if game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false then
					StartMagnet = false
					CheckQuest()
					repeat wait() TP(CFrameQuest) until (CFrameQuest.Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 3 or not _G.AutoFarm
					if (CFrameQuest.Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 3 then
						wait(0.1)
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest",NameQuest,LevelQuest)
						wait(0.1)
					end
				elseif game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true then
					CheckQuest()
					if game:GetService("Workspace").Enemies:FindFirstChild(Mon) then
						for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
							if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
								if v.Name == Mon then
									if string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, NameMon) then
										repeat task.wait()
											EquipWeapon(_G.SelectWeapon)
											AutoHaki()                                            
											PosMon = v.HumanoidRootPart.CFrame
											TP(v.HumanoidRootPart.CFrame * CFrame.new(0,13,0))
											v.HumanoidRootPart.CanCollide = false
											v.Humanoid.WalkSpeed = 0
											v.Head.CanCollide = false
											v.HumanoidRootPart.Size = Vector3.new(70,70,70)
											StartMagnet = true
											game:GetService'VirtualUser':CaptureController()
											game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
										until not _G.AutoFarm or v.Humanoid.Health <= 0 or not v.Parent or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
									else
										StartMagnet = false
										game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
									end
								end
							end
						end
					else
						TP(CFrameMon)
						StartMagnet = false
						if game:GetService("ReplicatedStorage"):FindFirstChild(Mon) then
						 TP(game:GetService("ReplicatedStorage"):FindFirstChild(Mon).HumanoidRootPart.CFrame * CFrame.new(15,10,2))
						end
					end
				end
			end)
		end
	end
end)
AutoFarm:AddToggle({
    Name = "AutoFarm Nearest",
	Value = _G.AutoFarmNearest,
    Callback = function(value)
        _G.AutoFarmNearest = value
		StopTween(_G.AutoFarmNearest)
    end
})
spawn(function()
    while wait() do
        if _G.AutoFarmNearest then
            for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                if v.Name and v:FindFirstChild("Humanoid") then
                    if v.Humanoid.Health > 0 then
                        repeat wait()
                          EquipWeapon(_G.SelectWeapon)
                            if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
                                local args = {
                                    [1] = "Buso"
                                }
                                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                               end
                            topos(v.HumanoidRootPart.CFrame * CFrame.new(0,40,0))
                            v.HumanoidRootPart.CanCollide = false
                            Fastattack = true
                            v.HumanoidRootPart.Size = Vector3.new(60, 60, 60)
                            game:GetService("VirtualUser"):CaptureController()
                               game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672), game.Workspace.CurrentCamera.CFrame)
                               AutoFarmNearestMagnet = true
                               PosMon = v.HumanoidRootPart.CFrame
                        until not _G.AutoFarmNearest or not v.Parent or v.Humanoid.Health <= 0 
                        AutoFarmNearestMagnet = false
                        Fastattack = false
                    end
                end
            end
        end
    end
    end)
	local World = Page:CreateSection({
		Name = "World",
		Side = "Left"
	})
	World:AddToggle({
    Name = "AutoSecond Sea",
	Value = _G.AutoSecondSea,
    Callback = function(value)
		_G.AutoSecondSea = value
		StopTween(_G.AutoSecondSea)
    end
})
spawn(function()
    while wait() do
        if _G.AutoSecondSea then
            pcall(function()
                if game:GetService("Players").LocalPlayer.Data.Level.Value >= 700 and World1 then
                    _G.AutoFarm = false
                    if game:GetService("Workspace").Map.Ice.Door.Transparency == 1 then
                        if game:GetService("Workspace").Enemies:FindFirstChild("Ice Admiral [Lv. 700] [Boss]") then
                            for i, v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                                if v.Name == "Ice Admiral [Lv. 700] [Boss]" then
                                    if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
                                        repeat
                                            task.wait()
                                            AutoHaki()
                                            EquipWeapon(_G.SelectWeapon)
                                            v.HumanoidRootPart.CanCollide = false
                                            v.Humanoid.WalkSpeed = 0
                                            v.Head.CanCollide = false
                                            topos(v.HumanoidRootPart.CFrame * CFrame.new(0, 35, 0))
                                            game:GetService("VirtualUser"):CaptureController()
                                            game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
                                            sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
                                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelDressrosa")
                                        until not _G.AutoSecondSea or not v.Parent or v.Humanoid.Health <= 0
                                    end
                                end
                            end
                        else
                            if game:GetService("ReplicatedStorage"):FindFirstChild("Ice Admiral [Lv. 700] [Boss]") then
                                topos(game:GetService("ReplicatedStorage"):FindFirstChild("Ice Admiral [Lv. 700] [Boss]").HumanoidRootPart.CFrame * CFrame.new(0, 35, 0))
                            end
                        end
                    else
                        toops(CFrame.new(4851.732421875, 5.651424884796143, 718.1107788085938))
                        wait(1.1)
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("DressrosaQuestProgress", "Detective")
                        wait(0.5)
                        EquipWeapon("Key")
                        toops(CFrame.new(1348.487060546875, 37.34933853149414, -1326.12158203125))
                    end
                end
            end)
        end
    end
end)

World:AddToggle({
		Name = "AutoThird Sea",
		Value = _G.AutoThirdSea,
		Callback = function(value)
			_G.AutoThirdSea = value
			StopTween(_G.AutoThirdSea)
		end
	})
	spawn(function()
		while wait() do
			if _G.AutoThirdSea then
				pcall(function()
					if game:GetService("Players").LocalPlayer.Data.Level.Value >= 1500 and World2 then
						_G.AutoFarm = false
						if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Check") == 0 then
							topos(CFrame.new(-1926.3221435547, 12.819851875305, 1738.3092041016))
							if (CFrame.new(-1926.3221435547, 12.819851875305, 1738.3092041016).Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 10 then
								wait(1.5)
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ZQuestProgress","Begin")
							end
							wait(1.8)
							if game:GetService("Workspace").Enemies:FindFirstChild("rip_indra [Lv. 1500] [Boss]") then
								for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
									if v.Name == "rip_indra [Lv. 1500] [Boss]" then
										repeat task.wait()
											AutoHaki()
											EquipWeapon(_G.SelectWeapon)
											topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
											v.HumanoidRootPart.CanCollide = false
											v.Humanoid.WalkSpeed = 0
											game:GetService'VirtualUser':CaptureController()
											game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
											game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelZou")
											sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
										until _G.AutoThirdSea == false or v.Humanoid.Health <= 0 or not v.Parent
									end
								end
							elseif not game:GetService("Workspace").Enemies:FindFirstChild("rip_indra [Lv. 1500] [Boss]") and (CFrame.new(-26880.93359375, 22.848554611206, 473.18951416016).Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 1000 then
								topos(CFrame.new(-26880.93359375, 22.848554611206, 473.18951416016))
							end
						end
					end
				end)
			end
		end
	end)

	local BossFarm = Page:CreateSection({
		Name = "BossFarm",
		Side = "Left"
	})

	local Boss = {}

for i, v in pairs(game:GetService("ReplicatedStorage"):GetChildren()) do
    if string.find(v.Name, "Boss") then
        if v.Name == "Ice Admiral [Lv. 700] [Boss]" then
            else
            table.insert(Boss, v.Name)
        end
    end
end
	BossFarm:AddDropDown({
		Name = "Select Boss",
		Value = "",
		List = Boss,
		Callback = function(t)
			_G.SelectBoss = t
		end
	})
	BossFarm:AddButton({
		Name = "Refresh Boss",
		Callback = function()
			BossName:Clear()
			for i, v in pairs(game:GetService("ReplicatedStorage"):GetChildren()) do
			if string.find(v.Name, "Boss") then
				BossName:Add(v.Name) 
			end
		end
	end
	})

	BossFarm:AddToggle({
		Name = "Auto Farm Boss",
		Value =  _G.AutoFarmBoss,
		Callback = function(t)
			_G.AutoFarmBoss = t
			StopTween(_G.AutoFarmBoss)
		end
	})
	spawn(function()
		while wait() do
			if _G.AutoFarmBoss then
				pcall(function()
					if game:GetService("Workspace").Enemies:FindFirstChild(_G.SelectBoss) then
						for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
							if v.Name == _G.SelectBoss then
								if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
									repeat task.wait()
										AutoHaki()
										EquipWeapon(_G.SelectWeapon)
										v.HumanoidRootPart.CanCollide = false
										v.Humanoid.WalkSpeed = 0
										v.HumanoidRootPart.Size = Vector3.new(80,80,80)                             
										TP(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
										game:GetService("VirtualUser"):CaptureController()
										game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
										sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
									until not _G.AutoFarmBoss or not v.Parent or v.Humanoid.Health <= 0
								end
							end
						end
					else
						if game:GetService("ReplicatedStorage"):FindFirstChild(_G.SelectBoss) then
							TP(game:GetService("ReplicatedStorage"):FindFirstChild(_G.SelectBoss).HumanoidRootPart.CFrame * CFrame.new(0,35,0))
						end
					end
				end)
			end
		end
	end)
	BossFarm:AddToggle({
		Name = "Auto Farm All Boss",
		Value = _G.AutoAllBoss,
		Callback = function(t)
			_G.AutoAllBoss = t
			StopTween(_G.AutoAllBoss)
		end
	})
	BossFarm:AddToggle({
		Name = "Auto Farm All Boss Hop",
		Value =  _G.AutoAllBossHop,
		Callback = function(t)
			_G.AutoAllBossHop = t
		end
	})


spawn(function()
	while wait() do
		if _G.AutoAllBoss then
			pcall(function()
				for i,v in pairs(game.ReplicatedStorage:GetChildren()) do
					if string.find(v.Name,"Boss") then
						if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 17000 then
							repeat task.wait()
								AutoHaki()
								EquipWeapon(_G.SelectWeapon)
								v.Humanoid.WalkSpeed = 0
								v.HumanoidRootPart.CanCollide = false
								v.Head.CanCollide = false
                                v.HumanoidRootPart.Size = Vector3.new(80,80,80)
								TP(v.HumanoidRootPart.CFrame*CFrame.new(0,35,0))
								game:GetService'VirtualUser':CaptureController()
								game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
								sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
							until v.Humanoid.Health <= 0 or _G.AutoAllBoss == false or not v.Parent
						end
					else
						if _G.AutoAllBossHop then
							Hop()
						end
					end
				end
			end)
		end
	end
end)
	local FightingStyle = Page:CreateSection({
		Name = "Fighting Style",
		Side = "Left"
	})
	FightingStyle:AddToggle({
		Name = "Auto Superhuman",
		Value = _G.AutoSuperhuman,
		Callback = function(t)
			_G.AutoSuperhuman = t
			StopTween(_G.AutoSuperhuman)
		end
	})	
	spawn(function()
		pcall(function()
			while wait() do 
				if _G.AutoSuperhuman then
					if game.Players.LocalPlayer.Backpack:FindFirstChild("Combat") or game.Players.LocalPlayer.Character:FindFirstChild("Combat") and game:GetService("Players")["LocalPlayer"].Data.Beli.Value >= 150000 then
						UnEquipWeapon("Combat")
						wait(.1)
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyBlackLeg")
					end   
					if game.Players.LocalPlayer.Character:FindFirstChild("Superhuman") or game.Players.LocalPlayer.Backpack:FindFirstChild("Superhuman") then
						_G.SelectWeapon = "Superhuman"
					end  
					if game.Players.LocalPlayer.Backpack:FindFirstChild("Black Leg") or game.Players.LocalPlayer.Character:FindFirstChild("Black Leg") or game.Players.LocalPlayer.Backpack:FindFirstChild("Electro") or game.Players.LocalPlayer.Character:FindFirstChild("Electro") or game.Players.LocalPlayer.Backpack:FindFirstChild("Fishman Karate") or game.Players.LocalPlayer.Character:FindFirstChild("Fishman Karate") or game.Players.LocalPlayer.Backpack:FindFirstChild("Dragon Claw") or game.Players.LocalPlayer.Character:FindFirstChild("Dragon Claw") then
						if game.Players.LocalPlayer.Backpack:FindFirstChild("Black Leg") and game.Players.LocalPlayer.Backpack:FindFirstChild("Black Leg").Level.Value <= 299 then
							_G.SelectWeapon = "Black Leg"
						end
						if game.Players.LocalPlayer.Backpack:FindFirstChild("Electro") and game.Players.LocalPlayer.Backpack:FindFirstChild("Electro").Level.Value <= 299 then
							_G.SelectWeapon = "Electro"
						end
						if game.Players.LocalPlayer.Backpack:FindFirstChild("Fishman Karate") and game.Players.LocalPlayer.Backpack:FindFirstChild("Fishman Karate").Level.Value <= 299 then
							_G.SelectWeapon = "Fishman Karate"
						end
						if game.Players.LocalPlayer.Backpack:FindFirstChild("Dragon Claw") and game.Players.LocalPlayer.Backpack:FindFirstChild("Dragon Claw").Level.Value <= 299 then
							_G.SelectWeapon = "Dragon Claw"
						end
						if game.Players.LocalPlayer.Backpack:FindFirstChild("Black Leg") and game.Players.LocalPlayer.Backpack:FindFirstChild("Black Leg").Level.Value >= 300 and game:GetService("Players")["LocalPlayer"].Data.Beli.Value >= 300000 then
							UnEquipWeapon("Black Leg")
							wait(.1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectro")
						end
						if game.Players.LocalPlayer.Character:FindFirstChild("Black Leg") and game.Players.LocalPlayer.Character:FindFirstChild("Black Leg").Level.Value >= 300 and game:GetService("Players")["LocalPlayer"].Data.Beli.Value >= 300000 then
							UnEquipWeapon("Black Leg")
							wait(.1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectro")
						end
						if game.Players.LocalPlayer.Backpack:FindFirstChild("Electro") and game.Players.LocalPlayer.Backpack:FindFirstChild("Electro").Level.Value >= 300 and game:GetService("Players")["LocalPlayer"].Data.Beli.Value >= 750000 then
							UnEquipWeapon("Electro")
							wait(.1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyFishmanKarate")
						end
						if game.Players.LocalPlayer.Character:FindFirstChild("Electro") and game.Players.LocalPlayer.Character:FindFirstChild("Electro").Level.Value >= 300 and game:GetService("Players")["LocalPlayer"].Data.Beli.Value >= 750000 then
							UnEquipWeapon("Electro")
							wait(.1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyFishmanKarate")
						end
						if game.Players.LocalPlayer.Backpack:FindFirstChild("Fishman Karate") and game.Players.LocalPlayer.Backpack:FindFirstChild("Fishman Karate").Level.Value >= 300 and game:GetService("Players")["Localplayer"].Data.Fragments.Value >= 1500 then
							UnEquipWeapon("Fishman Karate")
							wait(.1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BlackbeardReward","DragonClaw","1")
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BlackbeardReward","DragonClaw","2") 
						end
						if game.Players.LocalPlayer.Character:FindFirstChild("Fishman Karate") and game.Players.LocalPlayer.Character:FindFirstChild("Fishman Karate").Level.Value >= 300 and game:GetService("Players")["Localplayer"].Data.Fragments.Value >= 1500 then
							UnEquipWeapon("Fishman Karate")
							wait(.1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BlackbeardReward","DragonClaw","1")
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BlackbeardReward","DragonClaw","2") 
						end
						if game.Players.LocalPlayer.Backpack:FindFirstChild("Dragon Claw") and game.Players.LocalPlayer.Backpack:FindFirstChild("Dragon Claw").Level.Value >= 300 and game:GetService("Players")["LocalPlayer"].Data.Beli.Value >= 3000000 then
							UnEquipWeapon("Dragon Claw")
							wait(.1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySuperhuman")
						end
						if game.Players.LocalPlayer.Character:FindFirstChild("Dragon Claw") and game.Players.LocalPlayer.Character:FindFirstChild("Dragon Claw").Level.Value >= 300 and game:GetService("Players")["LocalPlayer"].Data.Beli.Value >= 3000000 then
							UnEquipWeapon("Dragon Claw")
							wait(.1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySuperhuman")
						end 
					end
				end
			end
		end)
	end)
	FightingStyle:AddToggle({
		Name = "Auto DeathStep",
		Value = _G.AutoDeathStep,
		Callback = function(t)
			_G.AutoDeathStep = t
			StopTween(_G.AutoDeathStep)
		end
	})	
	spawn(function()
		while wait() do wait()
			if _G.AutoDeathStep then
				if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Black Leg") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Black Leg") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Death Step") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Death Step") then
					if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Black Leg") and game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Black Leg").Level.Value >= 450 then
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep")
						_G.SelectWeapon = "Death Step"
					end  
					if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Black Leg") and game:GetService("Players").LocalPlayer.Character:FindFirstChild("Black Leg").Level.Value >= 450 then
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep")
						_G.SelectWeapon = "Death Step"
					end  
					if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Black Leg") and game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Black Leg").Level.Value <= 449 then
						_G.SelectWeapon = "Black Leg"
					end 
				else 
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyBlackLeg")
				end
			end
		end
	end)
	FightingStyle:AddToggle({
		Name = "Auto Sharkman",
		Value = _G.AutoSharkman,
		Callback = function(t)
			_G.AutoSharkman = t
			StopTween(_G.AutoSharkman)
		end
	})	
	spawn(function()
		pcall(function()
			while wait() do
				if _G.AutoSharkman then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyFishmanKarate")
					if string.find(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate"), "keys") then  
						if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Water Key") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Water Key") then
							topos(CFrame.new(-2604.6958, 239.432526, -10315.1982, 0.0425701365, 0, -0.999093413, 0, 1, 0, 0.999093413, 0, 0.0425701365))
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate")
						elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Fishman Karate") and game:GetService("Players").LocalPlayer.Character:FindFirstChild("Fishman Karate").Level.Value >= 400 then
						else 
							Ms = "Tide Keeper [Lv. 1475] [Boss]"
							if game:GetService("Workspace").Enemies:FindFirstChild(Ms) then   
								for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
									if v.Name == Ms then    
										OldCFrameShark = v.HumanoidRootPart.CFrame
										repeat task.wait()
											AutoHaki()
											EquipWeapon(_G.SelectWeapon)
											v.Head.CanCollide = false
											v.Humanoid.WalkSpeed = 0
											v.HumanoidRootPart.CanCollide = false
											v.HumanoidRootPart.CFrame = OldCFrameShark
											topos(v.HumanoidRootPart.CFrame*CFrame.new(0,35,0))
											game:GetService("VirtualUser"):CaptureController()
											game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 670))
											sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
										until not v.Parent or v.Humanoid.Health <= 0 or _G.AutoSharkman == false or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Water Key") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Water Key")
									end
								end
							else
								topos(CFrame.new(-3570.18652, 123.328949, -11555.9072, 0.465199202, -1.3857326e-08, 0.885206044, 4.0332897e-09, 1, 1.35347511e-08, -0.885206044, -2.72606271e-09, 0.465199202))
								wait(3)
							end
						end
					else 
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate")
					end
				end
			end
		end)
	end)	
	FightingStyle:AddToggle({
		Name = "Auto ElectricClaw",
		Value = _G.AutoElectricClaw,
		Callback = function(t)
			_G.AutoElectricClaw = t
			StopTween(_G.AutoElectricClaw)
		end
	})	
	spawn(function()
		pcall(function()
			while wait() do 
				if _G.AutoElectricClaw then
					if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electro") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Electro") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electric Claw") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Electric Claw") then
						if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electro") and game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electro").Level.Value >= 400 then
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw")
							_G.SelectWeapon = "Electric Claw"
						end  
						if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Electro") and game:GetService("Players").LocalPlayer.Character:FindFirstChild("Electro").Level.Value >= 400 then
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw")
							_G.SelectWeapon = "Electric Claw"
						end  
						if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electro") and game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electro").Level.Value <= 399 then
							_G.SelectWeapon = "Electro"
						end 
					else
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectro")
					end
				end
				if _G.AutoElectricClaw then
					if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electro") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Electro") then
						if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electro") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Electro") and game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Electro").Level.Value >= 400 or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Electro").Level.Value >= 400 then
							if _G.AutoFarm == false then
								TP(CFrame.new(-10371.4717, 330.764496, -10131.4199))           
								wait(1.1)                
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw","Start")
								wait(.5)
								TP(CFrame.new(-12550.532226563, 336.22631835938, -7510.4233398438))                            
								wait(.5)
								TP(CFrame.new(-10371.4717, 330.764496, -10131.4199))                           
								wait(1.1)
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw")
							elseif _G.AutoFarm == true then
								_G.AutoFarm = false
								wait(.5)
								TP(CFrame.new(-10371.4717, 330.764496, -10131.4199))           
								wait(1.1)                
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw","Start")
								wait(.5)
								TP(CFrame.new(-12550.532226563, 336.22631835938, -7510.4233398438))                            
								wait(.5)
								TP(CFrame.new(-10371.4717, 330.764496, -10131.4199))                           
								wait(1.1)
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw")
								_G.SelectWeapon = "Electric Claw"
								_G.AutoFarm = true
							end
						end
					end
				end
			end
		end)    
	end)
	FightingStyle:AddToggle({
		Name = "Auto GodHuman",
		Value = _G.Auto_God_Human,
		Callback = function(t)
			_G.Auto_God_Human = t
			StopTween(_G.Auto_God_Human)
		end
	})
	spawn(function()
		while task.wait() do
			if _G.Auto_God_Human then
				pcall(function()
					if game.Players.LocalPlayer.Character:FindFirstChild("Superhuman") or game.Players.LocalPlayer.Backpack:FindFirstChild("Superhuman") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Black Leg") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Black Leg") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Death Step") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Death Step") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Fishman Karate") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Fishman Karate") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Sharkman Karate") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Sharkman Karate") or game.Players.LocalPlayer.Backpack:FindFirstChild("Electro") or game.Players.LocalPlayer.Character:FindFirstChild("Electro") or game.Players.LocalPlayer.Backpack:FindFirstChild("Electric Claw") or game.Players.LocalPlayer.Character:FindFirstChild("Electric Claw") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dragon Claw") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dragon Claw") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dragon Talon") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dragon Talon") or game.Players.LocalPlayer.Character:FindFirstChild("Godhuman") or game.Players.LocalPlayer.Backpack:FindFirstChild("Godhuman") then
						if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySuperhuman",true) == 1 then
							if game.Players.LocalPlayer.Backpack:FindFirstChild("Superhuman") and game.Players.LocalPlayer.Backpack:FindFirstChild("Superhuman").Level.Value >= 400 or game.Players.LocalPlayer.Character:FindFirstChild("Superhuman") and game.Players.LocalPlayer.Character:FindFirstChild("Superhuman").Level.Value >= 400 then
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep")
							end
						else
							game.StarterGui:SetCore("SendNotification", {
								Title = "Notification", 
								Text = "Not Have Superhuman" ,
								Icon = "http://www.roblox.com/asset/?id=7483871523",
								Duration = 2.5
							})
						end
						if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDeathStep",true) == 1 then
							if game.Players.LocalPlayer.Backpack:FindFirstChild("Death Step") and game.Players.LocalPlayer.Backpack:FindFirstChild("Death Step").Level.Value >= 400 or game.Players.LocalPlayer.Character:FindFirstChild("Death Step") and game.Players.LocalPlayer.Character:FindFirstChild("Death Step").Level.Value >= 400 then
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate")
							end
						else
							game.StarterGui:SetCore("SendNotification", {
								Title = "Notification", 
								Text = "Not Have Death Step" ,
								Icon = "http://www.roblox.com/asset/?id=7483871523",
								Duration = 2.5
							})
						end
						if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySharkmanKarate",true) == 1 then
							if game.Players.LocalPlayer.Backpack:FindFirstChild("Sharkman Karate") and game.Players.LocalPlayer.Backpack:FindFirstChild("Sharkman Karate").Level.Value >= 400 or game.Players.LocalPlayer.Character:FindFirstChild("Sharkman Karate") and game.Players.LocalPlayer.Character:FindFirstChild("Sharkman Karate").Level.Value >= 400 then
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw")
							end
						else
							game.StarterGui:SetCore("SendNotification", {
								Title = "Notification", 
								Text = "Not Have SharkMan Karate" ,
								Icon = "http://www.roblox.com/asset/?id=7483871523",
								Duration = 2.5
							})
						end
						if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyElectricClaw",true) == 1 then
							if game.Players.LocalPlayer.Backpack:FindFirstChild("Electric Claw") and game.Players.LocalPlayer.Backpack:FindFirstChild("Electric Claw").Level.Value >= 400 or game.Players.LocalPlayer.Character:FindFirstChild("Electric Claw") and game.Players.LocalPlayer.Character:FindFirstChild("Electric Claw").Level.Value >= 400 then
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon")
							end
						else
							game.StarterGui:SetCore("SendNotification", {
								Title = "Notification", 
								Text = "Not Have Electric Claw" ,
								Icon = "http://www.roblox.com/asset/?id=7483871523",
								Duration = 2.5
							})
						end
						if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyDragonTalon",true) == 1 then
							if game.Players.LocalPlayer.Backpack:FindFirstChild("Dragon Talon") and game.Players.LocalPlayer.Backpack:FindFirstChild("Dragon Talon").Level.Value >= 400 or game.Players.LocalPlayer.Character:FindFirstChild("Dragon Talon") and game.Players.LocalPlayer.Character:FindFirstChild("Dragon Talon").Level.Value >= 400 then
								if string.find(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman",true), "Bring") then
									game.StarterGui:SetCore("SendNotification", {
										Title = "Notification", 
										Text = "Not Have Enough Material" ,
										Icon = "http://www.roblox.com/asset/?id=7483871523",
										Duration = 2.5
									})
								else
									game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuyGodhuman")
								end
							end
						else
							game.StarterGui:SetCore("SendNotification", {
								Title = "Notification", 
								Text = "Not Have Dragon Talon" ,
								Icon = "http://www.roblox.com/asset/?id=7483871523",
								Duration = 2.5
							})
						end
					else
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BuySuperhuman")
					end
				end)
			end
		end
	end)

	local Settings = Page:CreateSection({
		Name = "Settings Auto Farm",
		Side = "Right"
	})
Settings:AddDropdown({
	Name = "Select Weapon",
	Value = "Melee",
	List = {"Melee","Sword","Gun","Fruit"},
	Callback = function(t)
		_G.SelectWeapon = t
	end
})


spawn(function()
	while wait() do
	pcall(function()
	  if _G.SelectWeapon == "Melee" then
	  for i ,v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
	  if v.ToolTip == "Melee" then
	  if game.Players.LocalPlayer.Backpack:FindFirstChild(tostring(v.Name)) then
		_G.SelectWeapon = v.Name
	  end
	  end
	  end
	  elseif _G.SelectWeapon == "Sword" then
	  for i ,v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
	  if v.ToolTip == "Sword" then
	  if game.Players.LocalPlayer.Backpack:FindFirstChild(tostring(v.Name)) then
		_G.SelectWeapon = v.Name
	  end
	  end
	  end
	  elseif _G.SelectWeapon == "Devil Fruit" then
	  for i ,v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
	  if v.ToolTip == "Blox Fruit" then
	  if game.Players.LocalPlayer.Backpack:FindFirstChild(tostring(v.Name)) then
		_G.SelectWeapon = v.Name
	  end
	  end
	  end
	  elseif _G.SelectWeapon == "Gun" then
	  for i ,v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
	  if v.ToolTip == "Gun" then
	  if game.Players.LocalPlayer.Backpack:FindFirstChild(tostring(v.Name)) then
		_G.SelectWeapon = v.Name
	  end
	  end
	  end
	  else
		for i ,v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
	  if v.ToolTip == _G.SelectWeapon then
		_G.SelectWeapon = v.Name
	  end
	  end
	  end
	  end)
	end
  end)

  Settings:AddToggle({
	Name = "Fast Attack",
	Value = true,
	Callback = function(t)
		_G.FastAttack2 = t
		_G.FastAttackNormalSpeed = t 
		_G.FastAttack = t
		FastAttack = t
	end
})

local CameraShaker = require(game.ReplicatedStorage.Util.CameraShaker)
CombatFrameworkR = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework)
y = debug.getupvalues(CombatFrameworkR)[2]
spawn(function()
    game:GetService("RunService").RenderStepped:Connect(function()
        if _G.FastAttack2 then
            if typeof(y) == "table" then
                pcall(function()
                    CameraShaker:Stop()
                    y.activeController.timeToNextAttack = (math.huge^math.huge^math.huge)
                    y.activeController.timeToNextAttack = 0
                    y.activeController.hitboxMagnitude = 60
                    y.activeController.active = false
                    y.activeController.timeToNextBlock = 0
                    y.activeController.focusStart = 1655503339.0980349
                    y.activeController.increment = 1
                    y.activeController.blocking = false
                    y.activeController.attacking = false
                    y.activeController.humanoid.AutoRotate = true
                end)
            end
        end
    end)
end)
function GetBladeHit()
    local CombatFrameworkLib = debug.getupvalues(require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework))
    local CmrFwLib = CombatFrameworkLib[2]
    local p13 = CmrFwLib.activeController
    local weapon = p13.blades[1]
    if not weapon then 
        return weapon
    end
    while weapon.Parent ~= game.Players.LocalPlayer.Character do
        weapon = weapon.Parent 
    end
    return weapon
end
function AttackHit()
    local CombatFrameworkLib = debug.getupvalues(require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework))
    local CmrFwLib = CombatFrameworkLib[2]
    local plr = game.Players.LocalPlayer
    for i = 1, 1 do
        local bladehit = require(game.ReplicatedStorage.CombatFramework.RigLib).getBladeHits(plr.Character,{plr.Character.HumanoidRootPart},60)
        local cac = {}
        local hash = {}
        for k, v in pairs(bladehit) do
            if v.Parent:FindFirstChild("HumanoidRootPart") and not hash[v.Parent] then
                table.insert(cac, v.Parent.HumanoidRootPart)
                hash[v.Parent] = true
            end
        end
        bladehit = cac
        if #bladehit > 0.5 then
            pcall(function()
                CmrFwLib.activeController.timeToNextAttack = (math.huge^math.huge^math.huge)
                CmrFwLib.activeController.attacking = false
                CmrFwLib.activeController.blocking = false
                CmrFwLib.activeController.timeToNextBlock = 0
                CmrFwLib.activeController.increment = 3
                CmrFwLib.activeController.hitboxMagnitude = 50
                CmrFwLib.activeController.focusStart = 0
                game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("weaponChange",tostring(GetBladeHit()))
                game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("hit", bladehit, i, "")
            end)
        end
    end
end
spawn(function()
    while wait(.1) do
        if _G.FastAttack2 then
            pcall(function()
                repeat task.wait(0.175)
                    AttackHit()
                until not _G.FastAttack2
            end)
        end
    end
end)

local SeraphFrame = debug.getupvalues(require(game:GetService("Players").LocalPlayer.PlayerScripts:WaitForChild("CombatFramework")))[2]
local VirtualUser = game:GetService('VirtualUser')
local RigControllerR = debug.getupvalues(require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework.RigController))[2]
local Client = game:GetService("Players").LocalPlayer
local DMG = require(Client.PlayerScripts.CombatFramework.Particle.Damage)

function SeraphFuckWeapon() 
    local p13 = SeraphFrame.activeController
    local wea = p13.blades[1]
    if not wea then return end
    while wea.Parent~=game.Players.LocalPlayer.Character do wea=wea.Parent end
    return wea
end

function getHits(Size)
    local Hits = {}
    local Enemies = workspace.Enemies:GetChildren()
    local Characters = workspace.Characters:GetChildren()
    for i=1,#Enemies do local v = Enemies[i]
        local Human = v:FindFirstChildOfClass("Humanoid")
        if Human and Human.RootPart and Human.Health > 0 and game.Players.LocalPlayer:DistanceFromCharacter(Human.RootPart.Position) < Size+5 then
            table.insert(Hits,Human.RootPart)
        end
    end
    for i=1,#Characters do local v = Characters[i]
        if v ~= game.Players.LocalPlayer.Character then
            local Human = v:FindFirstChildOfClass("Humanoid")
            if Human and Human.RootPart and Human.Health > 0 and game.Players.LocalPlayer:DistanceFromCharacter(Human.RootPart.Position) < Size+5 then
                table.insert(Hits,Human.RootPart)
            end
        end
    end
    return Hits
end

task.spawn(
    function()
    while wait(0.059) do
        if _G.FastAttackNormalSpeed  then
            if SeraphFrame.activeController then
                    SeraphFrame.activeController.timeToNextAttack = (math.huge^math.huge^math.huge)
                    SeraphFrame.activeController.focusStart = 0
                    SeraphFrame.activeController.hitboxMagnitude = 40
                    SeraphFrame.activeController.humanoid.AutoRotate = true
                    SeraphFrame.activeController.increment = 1 + 1 / 1
            end
        end
    end
end)

function Boost()
    spawn(function()
    game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("weaponChange",tostring(SeraphFuckWeapon()))
    end)
end

function Unboost()
    spawn(function()
        game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("unequipWeapon",tostring(SeraphFuckWeapon()))
    end)
end

local cdnormal = 0.175
local Animation = Instance.new("Animation")
local CooldownFastAttack = 0.181
Attack = function()
    local ac = SeraphFrame.activeController
    if ac and ac.equipped then
        task.spawn(
            function()
            if tick() - cdnormal > 0.5 then
                ac:attack()
                cdnormal = tick()
            else
                Animation.AnimationId = ac.anims.basic[1]
                ac.humanoid:LoadAnimation(Animation):Play(1, 1)
                game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("hit", getHits(120), 2, "")
            end
        end)
    end
end

b = tick()
spawn(function()
    while wait(0.059) do
        if _G.FastAttackNormalSpeed then
            if b - tick() > 0.75 then
                wait(0.059)
                b = tick()
            end
            pcall(function()
                for i, v in pairs(game.Workspace.Enemies:GetChildren()) do
                    if v.Humanoid.Health > 0 then
                        if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 40 then
                            Attack()
                            wait(0.059)
                            Boost()
                        end
                    end
                end
            end)
        end
    end
end)

k = tick()
spawn(function()
    while wait(0.059) do
        if _G.FastAttackNormalSpeed  then
            if k - tick() > 0.75 then
                wait(0.059)
                k = tick()
            end
            pcall(function()
                for i, v in pairs(game.Workspace.Enemies:GetChildren()) do
                    if v.Humanoid.Health > 0 then
                        if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 40 then
                        wait(0.059)
                        Unboost()
                        end
                    end
                end
            end)
        end
    end
end)

tjw1 = true
task.spawn(
    function()
        local a = game.Players.LocalPlayer
        local b = require(a.PlayerScripts.CombatFramework.Particle)
        local c = require(game:GetService("ReplicatedStorage").CombatFramework.RigLib)
        if not shared.orl then
            shared.orl = c.wrapAttackAnimationAsync
        end
        if not shared.cpc then
            shared.cpc = b.play
        end
        if tjw1 then
            pcall(
                function()
                    c.wrapAttackAnimationAsync = function(d, e, f, g, h)
                        local i = c.getBladeHits(e, f, g)
                        if i then
                            b.play = function()
                            end
                            d:Play(0.25, 0.25, 0.25)
                            h(i)
                            b.play = shared.cpc
                            wait(.5)
                            d:Stop()
                        end
                    end
                end
            )
        end
    end
)



local CameRA = require(game:GetService("ReplicatedStorage").Util.CameraShaker)CameRA:Stop()
local Client = game.Players.LocalPlayer
local STOP = require(Client.PlayerScripts.CombatFramework.Particle)
local STOPRL = require(game:GetService("ReplicatedStorage").CombatFramework.RigLib)
task.spawn(function()
    pcall(function()
        if not shared.orl then
            shared.orl = STOPRL.wrapAttackAnimationAsync
        end
            if not shared.cpc then
                shared.cpc = STOP.play 
            end
        spawn(function()
            game:GetService("RunService").Stepped:Connect(function()
                STOPRL.wrapAttackAnimationAsync = function(a,b,c,d,func)
                    local Hits = STOPRL.getBladeHits(b,c,d)
                    if Hits then
                        if _G.FastAttackNormalSpeed then
                            STOP.play = function() end
                            a:Play(0.1,0.1,0.1)
                            func(Hits)
                            STOP.play = shared.cpc
                            wait(a.length * 0.5)
                            a:Stop()
                        else
                            func(Hits)
                            STOP.play = shared.cpc
                            wait(a.length * 0.5)
                            a:Stop()
                        end
                    end
                end
            end)
        end)
    end)
    end)


coroutine.wrap(function()
	local StopCamera = require(game.ReplicatedStorage.Util.CameraShaker)StopCamera:Stop()
		for v,v in pairs(getreg()) do
			if typeof(v) == "function" and getfenv(v).script == game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework then
				 for v,v in pairs(debug.getupvalues(v)) do
					if typeof(v) == "table" then
						spawn(function()
							game:GetService("RunService").RenderStepped:Connect(function()
								if _G.FastAttack then
									 pcall(function()
										 v.activeController.timeToNextAttack = -(math.huge^math.huge^math.huge)
										 v.activeController.attacking = false
										 v.activeController.increment = 1
										 v.activeController.blocking = false   
										 v.activeController.hitboxMagnitude = 150
										   v.activeController.currentAttackTrack = 0
										   v.activeController.focusStart = 0
										   v.activeController.blocking = false
										   v.activeController.attacking = false
										   v.activeController.hitboxMagnitude = 40
										   v.activeController.humanoid.AutoRotate = true
										   v.activeController.increment = 1
										 sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRaxNerous", math.huge)
									 end)
								 end
							 end)
						end)
					end
				end
			end
		end
	end)();

	
	spawn(function()
		while wait() do
		pcall(function()
			if FastAttack then
		if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
		game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
		end
		end
		end)
		end
		end) 
		
		require(game.ReplicatedStorage.Util.CameraShaker):Stop()
		
		local CombatFramework = require(game:GetService("Players").LocalPlayer.PlayerScripts:WaitForChild("CombatFramework"));
		local CombatFrameworkR = getupvalues(CombatFramework)[2];
		local RigController = require(game:GetService("Players")['LocalPlayer'].PlayerScripts.CombatFramework.RigController);
		local RigControllerR = getupvalues(RigController)[2];
		local realbhit = require(game.ReplicatedStorage.CombatFramework.RigLib);
		local cooldownfastattack = tick();
		function DisabledDamage()
			task.spawn(function()
				while wait() do
					pcall(function()
						if FastAttack then
							game:GetService("ReplicatedStorage").Assets.GUI.DamageCounter.Enabled = false;
						else
							game:GetService("ReplicatedStorage").Assets.GUI.DamageCounter.Enabled = true;
						end
					end);
				end
			end);
		end
		function CurrentWeapon()
			local ac = CombatFrameworkR.activeController;
			local ret = ac.blades[1];
			if not ret then
				return game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool").Name;
			end
			pcall(function()
				while ret.Parent ~= game.Players.LocalPlayer.Character do
					ret = ret.Parent;
				end
			end);
			if not ret then
				return game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool").Name;
			end
			return ret;
		end
		function getAllBladeHitsPlayers(Sizes)
			local Hits = {};
			local Client = game.Players.LocalPlayer;
			local Characters = game:GetService("Workspace").Characters:GetChildren();
			for i = 1, #Characters do
				local v = Characters[i];
				local Human = v:FindFirstChildOfClass("Humanoid");
				if ((v.Name ~= game.Players.LocalPlayer.Name) and Human and Human.RootPart and (Human.Health > 0) and (Client:DistanceFromCharacter(Human.RootPart.Position) < (Sizes + 5))) then
					table.insert(Hits, Human.RootPart);
				end
			end
			return Hits;
		end
		function getAllBladeHits(Sizes)
			local Hits = {};
			local Client = game.Players.LocalPlayer;
			local Enemies = game:GetService("Workspace").Enemies:GetChildren();
			for i = 1, #Enemies do
				local v = Enemies[i];
				local Human = v:FindFirstChildOfClass("Humanoid");
				if (Human and Human.RootPart and (Human.Health > 0) and (Client:DistanceFromCharacter(Human.RootPart.Position) < (Sizes + 5))) then
					table.insert(Hits, Human.RootPart);
				end
			end
			return Hits;
		end
		
		function AttackFunction()
			local ac = CombatFrameworkR.activeController
			if ac and ac.equipped then
				for indexincrement = 1, 1 do
					local bladehit = getAllBladeHits(60)
					if #bladehit > 0 then
						local AcAttack8 = debug.getupvalue(ac.attack, 5)
						local AcAttack9 = debug.getupvalue(ac.attack, 6)
						local AcAttack7 = debug.getupvalue(ac.attack, 4)
						local AcAttack10 = debug.getupvalue(ac.attack, 7)
						local NumberAc12 = (AcAttack8 * 798405 + AcAttack7 * 727595) % AcAttack9
						local NumberAc13 = AcAttack7 * 798405
						(function()
							NumberAc12 = (NumberAc12 * AcAttack9 + NumberAc13) % 1099511627776
							AcAttack8 = math.floor(NumberAc12 / AcAttack9)
							AcAttack7 = NumberAc12 - AcAttack8 * AcAttack9
						end)()
						AcAttack10 = AcAttack10 + 1 
						debug.setupvalue(ac.attack, 5, AcAttack8)
						debug.setupvalue(ac.attack, 6, AcAttack9)
						debug.setupvalue(ac.attack, 4, AcAttack7)
						debug.setupvalue(ac.attack, 7, AcAttack10)
						for k, v in pairs(ac.animator.anims.basic) do
							v:Play()
						end                 
						if game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool") and ac.blades and ac.blades[1] then 
							game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("weaponChange",tostring(CurrentWeapon()))
							game.ReplicatedStorage.Remotes.Validator:FireServer(math.floor(NumberAc12 / 1099511627776 * 16777215), AcAttack10)
							game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("hit", bladehit, indexincrement, "")
						end
					end
				end
			end
		end
		
		
		
		
		spawn(function()
			while wait(0.09) do
				if FastAttack then
					AttackFunction();
					DisabledDamage();
				end
			end
		end);

spawn(function()
	while wait() do
	if _G.WhiteScreen then
	  for i, v in pairs(game.Workspace["_WorldOrigin"]:GetChildren()) do
		  if v.Name == "CurvedRing" or v.Name == "SlashHit" or v.Name == "DamageCounter" or v.Name == "SwordSlash" or v.Name == "SlashTail" or v.Name == "Sounds" then
			  v:Destroy() 
		  end
	  end
  end
  end
end) 
if _G.WhiteScreen == true then
	game:GetService("RunService"):Set3dRenderingEnabled(false)
elseif _G.WhiteScreen == false then
	game:GetService("RunService"):Set3dRenderingEnabled(true)
end
Settings:AddToggle({
	Name = "Auto Haki",
	Value = true,
	Callback = function(t)
		_G.AUTOHAKI = t
	end
})
spawn(function()
	while wait(.1) do
		if _G.AUTOHAKI then 
			if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
				local args = {
					[1] = "Buso"
				}
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
			end
		end
	end
end)
Settings:AddToggle({
	Name = "AutoSetSpawn",
	Value = true,
	Callback = function(t)
		_G.AutoSetSpawn = t
	end
})
spawn(function()
    pcall(function()
        while wait() do
            if _G.AutoSetSpawn then
                if game:GetService("Players").LocalPlayer.Character.Humanoid.Health > 0 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                end
            end
        end
    end)
end)

Settings:AddToggle({
	Name = "BringMob",
	Value = true,
	Callback = function(t)
		_G.BringMonster = t
	end
})

spawn(function()
	while task.wait() do
		pcall(function()
			if _G.BringMonster then
				CheckQuest()
				for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
					if _G.AutoFarm and StartMagnet and v.Name == Mon and (Mon == "Factory Staff [Lv. 800]" or Mon == "Monkey [Lv. 14]" or Mon == "Dragon Crew Warrior [Lv. 1575]" or Mon == "Dragon Crew Archer [Lv. 1600]") and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 and (v.HumanoidRootPart.Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 220 then
						v.HumanoidRootPart.Size = Vector3.new(50,50,50)
						v.HumanoidRootPart.CFrame = PosMon
						v.Humanoid:ChangeState(14)
						v.HumanoidRootPart.CanCollide = false
						v.Head.CanCollide = false
						if v.Humanoid:FindFirstChild("Animator") then
							v.Humanoid.Animator:Destroy()
						end
						sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
					elseif _G.AutoFarm and StartMagnet and v.Name == Mon and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 and (v.HumanoidRootPart.Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 275 then
						v.HumanoidRootPart.Size = Vector3.new(50,50,50)
						v.HumanoidRootPart.CFrame = PosMon
						v.Humanoid:ChangeState(14)
						v.HumanoidRootPart.CanCollide = false
						v.Head.CanCollide = false
						if v.Humanoid:FindFirstChild("Animator") then
							v.Humanoid.Animator:Destroy()
						end
						sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
					end
					if _G.AutoEctoplasm and StartEctoplasmMagnet then
						if string.find(v.Name, "Ship") and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 and (v.HumanoidRootPart.Position - EctoplasmMon.Position).Magnitude <= 250 then
							v.HumanoidRootPart.Size = Vector3.new(50,50,50)
							v.HumanoidRootPart.CFrame = EctoplasmMon
							v.Humanoid:ChangeState(14)
							v.HumanoidRootPart.CanCollide = false
							v.Head.CanCollide = false
							if v.Humanoid:FindFirstChild("Animator") then
								v.Humanoid.Animator:Destroy()
							end
							sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
						end
					end
					if _G.AutoRengoku and StartRengokuMagnet then
						if (v.Name == "Snow Lurker [Lv. 1375]" or v.Name == "Arctic Warrior [Lv. 1350]") and (v.HumanoidRootPart.Position - RengokuMon.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							v.HumanoidRootPart.Size = Vector3.new(50,50,50)
							v.Humanoid:ChangeState(14)
							v.HumanoidRootPart.CanCollide = false
							v.Head.CanCollide = false
							v.HumanoidRootPart.CFrame = RengokuMon
							if v.Humanoid:FindFirstChild("Animator") then
								v.Humanoid.Animator:Destroy()
							end
							sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
						end
					end
					if _G.AutoMusketeerHat and StartMagnetMusketeerhat then
						if v.Name == "Forest Pirate [Lv. 1825]" and (v.HumanoidRootPart.Position - MusketeerHatMon.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							v.HumanoidRootPart.Size = Vector3.new(50,50,50)
							v.Humanoid:ChangeState(14)
							v.HumanoidRootPart.CanCollide = false
							v.Head.CanCollide = false
							v.HumanoidRootPart.CFrame = MusketeerHatMon
							if v.Humanoid:FindFirstChild("Animator") then
								v.Humanoid.Animator:Destroy()
							end
							sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
						end
					end
					if _G.Auto_EvoRace and StartEvoMagnet then
						if v.Name == "Zombie [Lv. 950]" and (v.HumanoidRootPart.Position - PosMonEvo.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							v.HumanoidRootPart.Size = Vector3.new(50,50,50)
							v.Humanoid:ChangeState(14)
							v.HumanoidRootPart.CanCollide = false
							v.Head.CanCollide = false
							v.HumanoidRootPart.CFrame = PosMonEvo
							if v.Humanoid:FindFirstChild("Animator") then
								v.Humanoid.Animator:Destroy()
							end
							sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
						end
					end
					if _G.AutoBartilo and AutoBartiloBring then
						if v.Name == "Swan Pirate [Lv. 775]" and (v.HumanoidRootPart.Position - PosMonBarto.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							v.HumanoidRootPart.Size = Vector3.new(50,50,50)
							v.Humanoid:ChangeState(14)
							v.HumanoidRootPart.CanCollide = false
							v.Head.CanCollide = false
							v.HumanoidRootPart.CFrame = PosMonBarto
							if v.Humanoid:FindFirstChild("Animator") then
								v.Humanoid.Animator:Destroy()
							end
							sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
						end
					end
					if _G.AutoFarmFruitMastery and StartMasteryFruitMagnet then
						if v.Name == "Monkey [Lv. 14]" then
							if (v.HumanoidRootPart.Position - PosMonMasteryFruit.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								v.Humanoid:ChangeState(14)
								v.HumanoidRootPart.CanCollide = false
								v.Head.CanCollide = false
								v.HumanoidRootPart.CFrame = PosMonMasteryFruit
								if v.Humanoid:FindFirstChild("Animator") then
									v.Humanoid.Animator:Destroy()
								end
								sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
							end
						elseif v.Name == "Factory Staff [Lv. 800]" then
							if (v.HumanoidRootPart.Position - PosMonMasteryFruit.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								v.Humanoid:ChangeState(14)
								v.HumanoidRootPart.CanCollide = false
								v.Head.CanCollide = false
								v.HumanoidRootPart.CFrame = PosMonMasteryFruit
								if v.Humanoid:FindFirstChild("Animator") then
									v.Humanoid.Animator:Destroy()
								end
								sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
							end
						elseif v.Name == Mon then
							if (v.HumanoidRootPart.Position - PosMonMasteryFruit.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								v.Humanoid:ChangeState(14)
								v.HumanoidRootPart.CanCollide = false
								v.Head.CanCollide = false
								v.HumanoidRootPart.CFrame = PosMonMasteryFruit
								if v.Humanoid:FindFirstChild("Animator") then
									v.Humanoid.Animator:Destroy()
								end
								sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
							end
						end
					end
					if _G.AutoFarmGunMastery and StartMasteryGunMagnet then
						if v.Name == "Monkey [Lv. 14]" then
							if (v.HumanoidRootPart.Position - PosMonMasteryGun.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								v.Humanoid:ChangeState(14)
								v.HumanoidRootPart.CanCollide = false
								v.Head.CanCollide = false
								v.HumanoidRootPart.CFrame = PosMonMasteryGun
								if v.Humanoid:FindFirstChild("Animator") then
									v.Humanoid.Animator:Destroy()
								end
								sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
							end
						elseif v.Name == "Factory Staff [Lv. 800]" then
							if (v.HumanoidRootPart.Position - PosMonMasteryGun.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								v.Humanoid:ChangeState(14)
								v.HumanoidRootPart.CanCollide = false
								v.Head.CanCollide = false
								v.HumanoidRootPart.CFrame = PosMonMasteryGun
								if v.Humanoid:FindFirstChild("Animator") then
									v.Humanoid.Animator:Destroy()
								end
								sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
							end
						elseif v.Name == Mon then
							if (v.HumanoidRootPart.Position - PosMonMasteryGun.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								v.Humanoid:ChangeState(14)
								v.HumanoidRootPart.CanCollide = false
								v.Head.CanCollide = false
								v.HumanoidRootPart.CFrame = PosMonMasteryGun
								if v.Humanoid:FindFirstChild("Animator") then
									v.Humanoid.Animator:Destroy()
								end
								sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
							end
						end
					end
					if _G.AutoFarmBone and StartMagnetBoneMon then
						if (v.Name == "Reborn Skeleton [Lv. 1975]" or v.Name == "Living Zombie [Lv. 2000]" or v.Name == "Demonic Soul [Lv. 2025]" or v.Name == "Posessed Mummy [Lv. 2050]") and (v.HumanoidRootPart.Position - PosMonBone.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							v.HumanoidRootPart.Size = Vector3.new(50,50,50)
							v.Humanoid:ChangeState(14)
							v.HumanoidRootPart.CanCollide = false
							v.Head.CanCollide = false
							v.HumanoidRootPart.CFrame = PosMonBone
							if v.Humanoid:FindFirstChild("Animator") then
								v.Humanoid.Animator:Destroy()
							end
							sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
						end
					end
					if _G.AutoDoughtBoss and MagnetDought then
						if (v.Name == "Cookie Crafter [Lv. 2200]" or v.Name == "Cake Guard [Lv. 2225]" or v.Name == "Baking Staff [Lv. 2250]" or v.Name == "Head Baker [Lv. 2275]") and (v.HumanoidRootPart.Position - PosMonDoughtOpenDoor.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							v.HumanoidRootPart.Size = Vector3.new(50,50,50)
							v.Humanoid:ChangeState(14)
							v.HumanoidRootPart.CanCollide = false
							v.Head.CanCollide = false
							v.HumanoidRootPart.CFrame = PosMonDoughtOpenDoor
							if v.Humanoid:FindFirstChild("Animator") then
								v.Humanoid.Animator:Destroy()
							end
							sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
						end
					end
					if _G.AutoCandy and StartMagnetCandy then
						if (v.HumanoidRootPart.Position - PosMonCandy.Position).Magnitude <= 250 and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							v.HumanoidRootPart.Size = Vector3.new(50,50,50)
							v.Humanoid:ChangeState(14)
							v.HumanoidRootPart.CanCollide = false
							v.Head.CanCollide = false
							v.HumanoidRootPart.CFrame = PosMonCandy
							if v.Humanoid:FindFirstChild("Animator") then
								v.Humanoid.Animator:Destroy()
							end
							sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
						end
					end
				end
			end
		end)
	end
end)
local Up = Page:CreateSection({
	Name = "UpStats",
	Side = "Right"
})
Up:AddToggle({
    Name = "Auto Melee",
    Value = _G.Auto_Melee,
    Callback = function(t)
        _G.Auto_Melee = t
	end
})
Up:AddToggle({
    Name = "Auto Defense",
    Value = _G.Auto_Defense,
    Callback = function(t)
        _G.Auto_Defense = t
	end
})
Up:AddToggle({   
	Name = "Auto Sword",
    Value = _G.Auto_Sword,
    Callback = function(t)
		_G.Auto_Sword = t
	end
})
Up:AddToggle({
    Name = "Auto Gun",
    Value = _G.Auto_Gun,
    Callback = function(t)
        _G.Auto_Gun = t
	end
})
Up:AddToggle({
    Name = "Auto Devil Fruits",
    Value = _G.Auto_DevilFruit,
    Callback = function(t)
		_G.Auto_DevilFruit = t
	end
})
spawn(function()
    while wait() do
        pcall(function()
            if _G.Auto_Melee then
                if game:GetService("Players")["LocalPlayer"].Data.Points.Value ~= 0 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint","Melee",_G.PointStats)
                end
            end
        end)
    end
end)

spawn(function()
    while wait() do
        pcall(function()
            if _G.Auto_Defense then
                if game:GetService("Players")["LocalPlayer"].Data.Points.Value ~= 0 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint","Defense",_G.PointStats)
                end
            end
        end)
    end
end)

spawn(function()
    while wait() do
        pcall(function()
            if _G.Auto_Sword then
                if game:GetService("Players")["LocalPlayer"].Data.Points.Value ~= 0 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint","Sword",_G.PointStats)
                end
            end
        end)
    end
end)

spawn(function()
    while wait() do
        pcall(function()
            if _G.Auto_Gun then
                if game:GetService("Players")["LocalPlayer"].Data.Points.Value ~= 0 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint","Gun",_G.PointStats)
                end
            end
        end)
    end
end)

spawn(function()
    while wait() do
        pcall(function()
            if _G.Auto_DevilFruit then
                if game:GetService("Players")["LocalPlayer"].Data.Points.Value ~= 0 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint","Demon Fruit",_G.PointStats)
                end
            end
        end)
    end
end)
Up:AddSlider({
	Name = "Select Point",
	Flag = "FarmingSection_TrickRate",
	Value = 1,
	Precise = 2,
	Min = 1,
	Max = 100,
	Callback = function(value)
		_G.PointStats = value
	end
})
local Mas = Page:CreateSection({
	Name = "Farm Mastery",
	Side = "Right"
})
Mas:AddToggle({
    Name = "Auto Farm DF Mastery",
    Value = _G.AutoFarmFruitMastery,
    Callback = function(t)
        _G.AutoFarmFruitMastery = t
        StopTween(_G.AutoFarmFruitMastery)
        if _G.AutoFarmFruitMastery == false then
            UseSkill = false 
end
        end
})

spawn(function()
    while wait() do
        if _G.AutoFarmFruitMastery then
            pcall(function()
                local QuestTitle = game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text
                if not string.find(QuestTitle, NameMon) then
                    Magnet = false
                    UseSkill = false
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                end
                if game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                    StartMasteryFruitMagnet = false
                    UseSkill = false
                    CheckQuest()
                    repeat wait()
                        TP(CFrameQuest)
                    until (CFrameQuest.Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 3 or not _G.AutoFarmFruitMastery
                    if (CFrameQuest.Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 3 then
                        wait(1.2)
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest",NameQuest,LevelQuest)
                        wait(0.5)
                    end
                elseif game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                    CheckQuest()
                    if game:GetService("Workspace").Enemies:FindFirstChild(Mon) then
                        for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                            if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
                                if v.Name == Mon then
                                    if string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, NameMon) then
                                        HealthMs = v.Humanoid.MaxHealth * _G.Kill_At/100
                                        repeat task.wait()
                                            if v.Humanoid.Health <= HealthMs then
                                                AutoHaki()
                                                EquipWeapon(game:GetService("Players").LocalPlayer.Data.DevilFruit.Value)
                                                TP(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
                                                v.HumanoidRootPart.CanCollide = false
                                                PosMonMasteryFruit = v.HumanoidRootPart.CFrame
                                                v.Humanoid.WalkSpeed = 0
                                                v.Head.CanCollide = false
                                                UseSkill = true
                                            else           
                                                UseSkill = false 
                                                AutoHaki()
                                                EquipWeapon(_G.SelectWeapon)
                                                TP(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
                                                v.HumanoidRootPart.CanCollide = false
                                                PosMonMasteryFruit = v.HumanoidRootPart.CFrame
                                                OldPos = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
                                                v.Humanoid.WalkSpeed = 0
                                                v.Head.CanCollide = false
                                            end
                                            StartMasteryFruitMagnet = true
                                            game:GetService'VirtualUser':CaptureController()
                                            game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
                                        until not _G.AutoFarmFruitMastery or v.Humanoid.Health <= 0 or not v.Parent or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
                                    else
                                        UseSkill = false
                                        StartMasteryFruitMagnet = false
                                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                    end
                                end
                            end
                        end
                    else
                        StartMasteryFruitMagnet = false   
                        UseSkill = false 
                        if game:GetService("ReplicatedStorage"):FindFirstChild(Mon) then
                            TP(game:GetService("ReplicatedStorage"):FindFirstChild(Mon).HumanoidRootPart.CFrame * CFrame.new(0,35,0))
						else
							if (CFrameQuest.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 15 then
                                if PosMon ~= nil then
								    TP(PosMon * CFrame.new(0,35,0))
                                else
                                    if OldPos ~= nil then
                                        TP(OldPos.Position)
                                    end
                                end
							end
                        end
                    end
                end
            end)
        end
    end
end)

spawn(function()
    while wait() do
        if UseSkill then
            pcall(function()
                CheckQuest()
                for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                    if game.Players.LocalPlayer.Character:FindFirstChild(game.Players.LocalPlayer.Data.DevilFruit.Value) then
                        BF = require(game:GetService("Players").LocalPlayer.Character[game.Players.LocalPlayer.Data.DevilFruit.Value].Data)
                    elseif game.Players.LocalPlayer.Backpack:FindFirstChild(game.Players.LocalPlayer.Data.DevilFruit.Value) then
                        BF = require(game:GetService("Players").LocalPlayer.Backpack[game.Players.LocalPlayer.Data.DevilFruit.Value].Data)
                    end
                    if game.Players.LocalPlayer.Character:FindFirstChild(game.Players.LocalPlayer.Data.DevilFruit.Value) then
                        MyMasteryDF = game:GetService("Players").LocalPlayer.Character[game.Players.LocalPlayer.Data.DevilFruit.Value].Level.Value
                    elseif game.Players.LocalPlayer.Backpack:FindFirstChild(game.Players.LocalPlayer.Data.DevilFruit.Value) then
                        MyMasteryDF = game:GetService("Players").LocalPlayer.Backpack[game.Players.LocalPlayer.Data.DevilFruit.Value].Level.Value
                    end
                    if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dragon-Dragon") then                      
                        if _G.SkillZ then
                            if MyMasteryDF >= BF.Lvl.Z then
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"Z",false,game)
                            end
                        end
                        if _G.SkillX then    
                            if MyMasteryDF >= BF.Lvl.X then
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"X",false,game)
                            end
                        end
                        if _G.SkillC then 
                            if MyMasteryDF >= BF.Lvl.C then
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                                wait(2)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"C",false,game)
                            end
                        end
                    elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Venom-Venom") then   
                        if _G.SkillZ then
                            if MyMasteryDF >= BF.Lvl.Z then
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"Z",false,game)
                            end
                        end
                        if _G.SkillX then    
                            if MyMasteryDF >= BF.Lvl.X then                                 
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"X",false,game)
                            end
                        end
                        if _G.SkillC then 
                            if MyMasteryDF >= BF.Lvl.C then
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"C",false,game)
                            end
                        end
                    elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Human-Human: Buddha") then
                        if _G.SkillZ and game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Size == Vector3.new(2, 2.0199999809265, 1) then    
                            if MyMasteryDF >= BF.Lvl.Z then                       
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"Z",false,game)
                            end
                        end
                        if _G.SkillX then
                            if MyMasteryDF >= BF.Lvl.X then
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"X",false,game)
                            end
                        end
                        if _G.SkillC then      
                            if MyMasteryDF >= BF.Lvl.C then                     
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"C",false,game)
                            end
                        end
                        if _G.SkillV then  
                            if MyMasteryDF >= BF.Lvl.V then                        
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"V",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"V",false,game)
                            end
                        end
                    elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild(game:GetService("Players").LocalPlayer.Data.DevilFruit.Value) then
                        if _G.SkillZ then
                            if MyMasteryDF >= BF.Lvl.Z then                   
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"Z",false,game)
                            end
                        end
                        if _G.SkillX then    
                            if MyMasteryDF >= BF.Lvl.X then
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"X",false,game)
                            end
                        end
                        if _G.SkillC then 
                            if MyMasteryDF >= BF.Lvl.C then
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"C",false,game)
                            end
                        end
                        if _G.SkillV then               
                            if MyMasteryDF >= BF.Lvl.V then           
                                game:GetService("VirtualInputManager"):SendKeyEvent(true,"V",false,game)
                                game:GetService("VirtualInputManager"):SendKeyEvent(false,"V",false,game)
                            end
                        end
                    end
                end
            end)
        end
    end
end)
Mas:AddToggle({
	Name = "Auto Farm Gun Mastery",
	Value = _G.AutoFarmGunMastery,
	Callback = function(t)
		_G.AutoFarmGunMastery = t 
		StopTween(_G.AutoFarmGunMastery)
	end
})
spawn(function()
    pcall(function()
        while wait() do
            if _G.AutoFarmGunMastery then
                local QuestTitle = game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text
                if not string.find(QuestTitle, NameMon) then
                    Magnet = false                                      
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                end
                if game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                    StartMasteryGunMagnet = false
                    CheckQuest()
                    TP(CFrameQuest)
                    if (CFrameQuest.Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 10 then
                        wait(1.2)
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
                    end
                elseif game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true then
                    CheckQuest()
                    if game:GetService("Workspace").Enemies:FindFirstChild(Mon) then
                        pcall(function()
                            for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                                if v.Name == Mon then
                                    repeat task.wait()
                                        if string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, NameMon) then
                                            HealthMin = v.Humanoid.MaxHealth * _G.Kill_At/100
                                            if v.Humanoid.Health <= HealthMin then                                                
                                                EquipWeapon(SelectWeaponGun)
                                                TP(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
                                                v.Humanoid.WalkSpeed = 0
                                                v.HumanoidRootPart.CanCollide = false
                                                v.HumanoidRootPart.Size = Vector3.new(2,2,1)
                                                v.Head.CanCollide = false                                                
                                                local args = {
                                                    [1] = v.HumanoidRootPart.Position,
                                                    [2] = v.HumanoidRootPart
                                                }
                                                game:GetService("Players").LocalPlayer.Character[SelectWeaponGun].RemoteFunctionShoot:InvokeServer(unpack(args))
                                                game:GetService'VirtualUser':CaptureController()
                                                game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
                                            else
                                                AutoHaki()
                                                EquipWeapon(_G.SelectWeapon)
                                                v.Humanoid.WalkSpeed = 0
                                                v.HumanoidRootPart.CanCollide = false
                                                v.Head.CanCollide = false               
                                                TP(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
                                                game:GetService'VirtualUser':CaptureController()
                                                game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
                                            end
                                            StartMasteryGunMagnet = true 
                                            PosMonMasteryGun = v.HumanoidRootPart.CFrame
                                        else
                                            StartMasteryGunMagnet = false
                                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AbandonQuest")
                                        end
                                    until v.Humanoid.Health <= 0 or _G.AutoFarmGunMastery == false or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
                                    StartMasteryGunMagnet = false
                                end
                            end
                        end)
                    else
                        StartMasteryGunMagnet = false
                        local Mob = game:GetService("ReplicatedStorage"):FindFirstChild(Mon) 
                        if Mob then
                            TP(Mob.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
                        end
                    end 
                end
            end
        end
    end)
end)

local SetMas = Page:CreateSection({
	Name = "Setting Mastery",
	Side = "Right"
})
SetMas:AddToggle({
	Name = "Skill Z",
	Value = true,
	Callback = function(t)
		_G.SkillZ = t
	end
})
SetMas:AddToggle({
	Name = "Skill X",
	Value = true,
	Callback = function(t)
		_G.SkillX = t
	end
})
SetMas:AddToggle({
	Name = "Skill C",
	Value = true,
	Callback = function(t)
		_G.SkillC = t
	end
})
SetMas:AddToggle({
	Name = "Skill V",
	Value = true,
	Callback = function(t)
		_G.SkillV = t
	end
})
SetMas:AddSlider({
	Name = "Kill At %",
	Flag = "FarmingSection_TrickRate",
	Value = 35,
	Precise = 2,
	Min = 1,
	Max = 100,
	Callback = function(value)
		_G.Kill_At = value
	end
})
local CakePrinceSection = Page:CreateSection({
	Name = "Cake Prince 🍰",
	Side = "Left"
})

local Mob_Kill_Cake_Prince = CakePrinceSection:AddLabel({
	Name = "Total",
	Flag = "Total"
})
spawn(function()
	while wait() do
		pcall(function()
			if string.len(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CakePrinceSpawner")) == 88 then
				Mob_Kill_Cake_Prince:Set("Kill : "..string.sub(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CakePrinceSpawner"),39,41).." : More!!!")
			elseif string.len(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CakePrinceSpawner")) == 87 then
				Mob_Kill_Cake_Prince:Set("Kill : "..string.sub(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CakePrinceSpawner"),39,40).." : More!!!")
			elseif string.len(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CakePrinceSpawner")) == 86 then
				Mob_Kill_Cake_Prince:Set("Kill : "..string.sub(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CakePrinceSpawner"),39,39).." : More!!!")
			else
				Mob_Kill_Cake_Prince:Set("Boss Is Spawned!!!")
			end
		end)
	end
end)
CakePrinceSection:AddToggle({
	Name = "Auto Spawn Cake Prince",
	Flag = "Auto_Spawn_Cake_Prince",
	Value = true,
	Callback = function(value)
		_G.Auto_Spawn_Cake_Prince = value    
	end
})

spawn(function()
	while wait() do
		if _G.Auto_Spawn_Cake_Prince then
			local args = {
				[1] = "CakePrinceSpawner",
				[2] = true
			}

			game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))                    
			local args = {
				[1] = "CakePrinceSpawner"
			}

			game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
		end
	end
end)
CakePrinceSection:AddToggle({
	Name = "Auto DoughtBoss",
	Flag = "Auto_DoughtBoss",
	Value = _G.AutoDoughtBoss,
	Callback = function(value)
		_G.AutoDoughtBoss = value  
		StopTween(_G.AutoDoughtBoss)
	end
})

spawn(function()
	while wait() do
		if _G.AutoDoughtBoss then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "Cake Prince [Lv. 2300] [Raid Boss]" then
							if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								repeat task.wait()
									AutoHaki()
									EquipWeapon(_G.SelectWeapon)
									v.HumanoidRootPart.CanCollide = false
									v.Humanoid.WalkSpeed = 0
									v.HumanoidRootPart.Size = Vector3.new(50,50,50)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(0,30,0))
									game:GetService("VirtualUser"):CaptureController()
									game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
									sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
								until not _G.AutoDoughtBoss or not v.Parent or v.Humanoid.Health <= 0
							end
						end
					end
				else
					if game:GetService("ReplicatedStorage"):FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]") then
						topos(game:GetService("ReplicatedStorage"):FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]").HumanoidRootPart.CFrame * CFrame.new(0,30,0))
					else
						if KillMob == 0 then
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CakePrinceSpawner",true)
						end                    
						if game:GetService("Workspace").Map.CakeLoaf.BigMirror.Other.Transparency == 1 then
							if game:GetService("Workspace").Enemies:FindFirstChild("Cookie Crafter [Lv. 2200]") or game:GetService("Workspace").Enemies:FindFirstChild("Cake Guard [Lv. 2225]") or game:GetService("Workspace").Enemies:FindFirstChild("Baking Staff [Lv. 2250]") or game:GetService("Workspace").Enemies:FindFirstChild("Head Baker [Lv. 2275]") then
								for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
									if v.Name == "Cookie Crafter [Lv. 2200]" or v.Name == "Cake Guard [Lv. 2225]" or v.Name == "Baking Staff [Lv. 2250]" or v.Name == "Head Baker [Lv. 2275]" then
										if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
											repeat task.wait()
												AutoHaki()
												EquipWeapon(_G.SelectWeapon)
												v.HumanoidRootPart.CanCollide = false
												v.Humanoid.WalkSpeed = 0
												v.Head.CanCollide = false 
												v.HumanoidRootPart.Size = Vector3.new(50,50,50)
												MagnetDought = true
												PosMonDoughtOpenDoor = v.HumanoidRootPart.CFrame
												topos(v.HumanoidRootPart.CFrame * CFrame.new(0,30,0))
												game:GetService("VirtualUser"):CaptureController()
												game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
											until not _G.AutoDoughtBoss or not v.Parent or v.Humanoid.Health <= 0 or game:GetService("Workspace").Map.CakeLoaf.BigMirror.Other.Transparency == 0 or game:GetService("ReplicatedStorage"):FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]") or KillMob == 0
										end
									end
								end
							else
								MagnetDought = false
								if game:GetService("ReplicatedStorage"):FindFirstChild("Cookie Crafter [Lv. 2200]") then
									topos(game:GetService("ReplicatedStorage"):FindFirstChild("Cookie Crafter [Lv. 2200]").HumanoidRootPart.CFrame * CFrame.new(0,30,0)) 
								else
									if game:GetService("ReplicatedStorage"):FindFirstChild("Cake Guard [Lv. 2225]") then
										topos(game:GetService("ReplicatedStorage"):FindFirstChild("Cake Guard [Lv. 2225]").HumanoidRootPart.CFrame * CFrame.new(0,30,0)) 
									else
										if game:GetService("ReplicatedStorage"):FindFirstChild("Baking Staff [Lv. 2250]") then
											topos(game:GetService("ReplicatedStorage"):FindFirstChild("Baking Staff [Lv. 2250]").HumanoidRootPart.CFrame * CFrame.new(0,30,0))
										else
											if game:GetService("ReplicatedStorage"):FindFirstChild("Head Baker [Lv. 2275]") then
												topos(game:GetService("ReplicatedStorage"):FindFirstChild("Head Baker [Lv. 2275]").HumanoidRootPart.CFrame * CFrame.new(0,30,0))
											end
										end
									end
								end                       
							end
						else
							if game:GetService("Workspace").Enemies:FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]") then
								topos(game:GetService("Workspace").Enemies:FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]").HumanoidRootPart.CFrame * CFrame.new(0,30,0))
							else
								if game:GetService("ReplicatedStorage"):FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]") then
									topos(game:GetService("ReplicatedStorage"):FindFirstChild("Cake Prince [Lv. 2300] [Raid Boss]").HumanoidRootPart.CFrame * CFrame.new(0,30,0))
								end
							end
						end
					end
				end
			end)
		end
	end
end)
CakePrinceSection:AddToggle({
	Name = "Auto DoughtKing",
	Flag = "Auto DoughtKing",
	Value = _G.Autodoughking,
	Callback = function(value)
		_G.Autodoughking = value  
		StopTween(_G.Autodoughking)
	end
})
CakePrinceSection:AddToggle({
	Name = "Auto DoughtKing Hop",
	Flag = "Auto DoughtKingHop",
	Value = _G.AutodoughkingHop,
	Callback = function(value)
		_G.AutodoughkingHop = value  
	end
})
spawn(function()
	while wait() do
		if  _G.Autodoughking and World3 then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Dough King [Lv. 2300] [Raid Boss]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "Dough King [Lv. 2300] [Raid Boss]" then
							if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								repeat task.wait()
									AutoHaki()
									EquipWeapon(_G.SelectWeapon)
									v.HumanoidRootPart.CanCollide = false
									v.Humanoid.WalkSpeed = 0
									v.HumanoidRootPart.Size = Vector3.new(50,50,50)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(PosX,PosY,PosZ))
									game:GetService("VirtualUser"):CaptureController()
									game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
									sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
								until not  _G.Autodoughking or not v.Parent or v.Humanoid.Health <= 0
							end
						end
					end
				else
				topos(CFrame.new(-2662.818603515625, 1062.3480224609375, -11853.6953125))
					if game:GetService("ReplicatedStorage"):FindFirstChild("Dough King [Lv. 2300] [Raid Boss]") then
						topos(game:GetService("ReplicatedStorage"):FindFirstChild("Dough King [Lv. 2300] [Raid Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
					else
						if  _G.AutodoughkingHop then
							Hop()
						end
					end
				end
			end)
		end
	end
 end)
local BoneSection = Page:CreateSection({
	Name = "Bone 🦴",
	Side = "Left"
})

BoneSection:AddToggle{
	Name = "Auto Farm Bone",
	Flag = "Auto_Farm_Bone",
	Value = _G.AutoFarmBone,
	Callback  = function(value)
		_G.AutoFarmBone = value
		StopTween(_G.AutoFarmBone)
	end
}
spawn(function()
    while wait() do 
        if _G.AutoFarmBone and Three_World then
            pcall(function()
                TP(CFrame.new(-8762.80762, 179.512558, 6170.24805, 0.980607092, 2.52412509e-08, -0.195983887, -3.00243421e-08, 1, -2.14345768e-08, 0.195983887, 2.69031872e-08, 0.980607092))
                if game:GetService("Workspace").Enemies:FindFirstChild("Reborn Skeleton [Lv. 1975]") or game:GetService("Workspace").Enemies:FindFirstChild("Living Zombie [Lv. 2000]") or game:GetService("Workspace").Enemies:FindFirstChild("Demonic Soul [Lv. 2025]") or game:GetService("Workspace").Enemies:FindFirstChild("Posessed Mummy [Lv. 2050]") then
                    for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                        if v.Name == "Reborn Skeleton [Lv. 1975]" or v.Name == "Living Zombie [Lv. 2000]" or v.Name == "Demonic Soul [Lv. 2025]" or v.Name == "Posessed Mummy [Lv. 2050]" then
                            if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
                                repeat task.wait()
                                    AutoHaki()
                                    EquipWeapon(_G.EquipWeapon)
                                    v.HumanoidRootPart.CanCollide = false
                                    v.Humanoid.WalkSpeed = 0
                                    v.Head.CanCollide = false 
                                    TP(v.HumanoidRootPart.CFrame * CFrame.new(2,20,2))
                                until not _G.AutoFarmBone or not v.Parent or v.Humanoid.Health <= 0
                            end
                        end
                    end
                else
                    for i,v in pairs(game:GetService("ReplicatedStorage"):GetChildren()) do 
                        if v.Name == "Reborn Skeleton [Lv. 1975]" then
                            TP(v.HumanoidRootPart.CFrame * CFrame.new(2,20,2))
                        elseif v.Name == "Living Zombie [Lv. 2000]" then
                            TP(v.HumanoidRootPart.CFrame * CFrame.new(2,20,2))
                        elseif v.Name == "Demonic Soul [Lv. 2025]" then
                            TP(v.HumanoidRootPart.CFrame * CFrame.new(2,20,2))
                        elseif v.Name == "Posessed Mummy [Lv. 2050]" then
                            TP(v.HumanoidRootPart.CFrame * CFrame.new(2,20,2))
                        end
                    end
                end
            end)
        end
    end
end)

BoneSection:AddToggle{
	Name = "Auto Random Surprise",
	Flag = "Auto_Random_Surprise",
	Value = _G.Auto_Random_Bone,
	Callback  = function(value)
		_G.Auto_Random_Bone = value
	end
}
spawn(function()
    pcall(function()
        while wait() do
            if _G.Auto_Random_Bone then    
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Bones","Buy",1,1)
            end
        end
    end)
end)
BoneSection:AddToggle{
	Name = "Auto Try Luck",
	Flag = "Auto_Try_Luck",
	Value = TryLuck,
	Callback  = function(value)
		TryLuck = value
	end
}
BoneSection:AddToggle{
	Name = "Auto Pray",
	Flag = "Auto_Pray",
	Value = Pray,
	Callback  = function(value)
		Pray = value
	end
}
spawn(function()
    while wait(.1) do
        if TryLuck then
            TP(CFrame.new(-8652.99707, 143.450119, 6170.50879, -0.983064115, -2.48005533e-10, 0.18326205, -1.78910387e-09, 1, -8.24392288e-09, -0.18326205, -8.43218029e-09, -0.983064115))
            wait()
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("gravestoneEvent", 1)
        end
    end
end)

spawn(function()
    while wait(.1) do
        if Pray then
            TP(CFrame.new(-8652.99707, 143.450119, 6170.50879, -0.983064115, -2.48005533e-10, 0.18326205, -1.78910387e-09, 1, -8.24392288e-09, -0.18326205, -8.43218029e-09, -0.983064115))
            wait()
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("gravestoneEvent", 2)
        end
    end
end)
local Observation = Page:CreateSection({
	Name = "Observation",
	Side = "Right"
})
local ObservationRange = Observation:AddLabel({
	Name = "Observation Range Level"
})

spawn(function()
    while wait() do
        pcall(function()
            ObservationRange:Set("Observation Range Level : "..math.floor(game:GetService("Players").LocalPlayer.VisionRadius.Value))
        end)
    end
end)
Observation:AddToggle{
	Name = "Auto Farm Observation",
	Flag = "Auto_Farm_Observation",
	Value = _G.AutoObservation,
	Callback  = function(value)
		_G.AutoObservation = value
		StopTween(_G.AutoObservation)
	end
}
spawn(function()
    while wait() do
        pcall(function()
            if _G.AutoObservation then
                repeat task.wait()
                    if not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel") then
                        game:GetService('VirtualUser'):CaptureController()
                        game:GetService('VirtualUser'):SetKeyDown('0x65')
                        wait(2)
                        game:GetService('VirtualUser'):SetKeyUp('0x65')
                    end
                until game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel") or not _G.AutoObservation
            end
        end)
    end
end)
Observation:AddToggle{
	Name = "Auto Farm Observation Hop",
	Flag = "Auto_Farm_Observation_Hop",
	Value = _G.AutoObservation_Hop,
	Callback  = function(value)
		_G.AutoObservation_Hop = value 
	end
}
spawn(function()
    pcall(function()
        while wait() do
            if _G.AutoObservation then
                if game:GetService("Players").LocalPlayer.VisionRadius.Value >= 3000 then
                    game:GetService("StarterGui"):SetCore("SendNotification", {
                        Icon = "rbxassetid://13984057027";
                        Title = "Observation", 
                        Text = "You Have Max Points"
                    })
                    wait(2)
                else
                    if World2 then
                        if game:GetService("Workspace").Enemies:FindFirstChild("Lava Pirate [Lv. 1200]") then
                            if game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel") then
                                repeat task.wait()
                                    game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").Enemies:FindFirstChild("Lava Pirate [Lv. 1200]").HumanoidRootPart.CFrame * CFrame.new(3,0,0)
                                until _G.AutoObservation == false or not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel")
                            else
                                repeat task.wait()
                                    game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").Enemies:FindFirstChild("Lava Pirate [Lv. 1200]").HumanoidRootPart.CFrame * CFrame.new(0,50,0)+
                                        wait(1)
                                    if not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel") and _G.AutoObservation_Hop == true then
                                        game:GetService("TeleportService"):Teleport(game.PlaceId,game:GetService("Players").LocalPlayer)
                                    end
                                until _G.AutoObservation == false or game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel")
                            end
                        else
                            topos(CFrame.new(-5478.39209, 15.9775667, -5246.9126))
                        end
                    elseif World1 then
                        if game:GetService("Workspace").Enemies:FindFirstChild("Galley Captain [Lv. 650]") then
                            if game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel") then
                                repeat task.wait()
                                    game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").Enemies:FindFirstChild("Galley Captain [Lv. 650]").HumanoidRootPart.CFrame * CFrame.new(3,0,0)
                                until _G.AutoObservation == false or not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel")
                            else
                                repeat task.wait()
                                    game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").Enemies:FindFirstChild("Galley Captain [Lv. 650]").HumanoidRootPart.CFrame * CFrame.new(0,50,0)
                                    wait(1)
                                    if not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel") and _G.AutoObservation_Hop == true then
                                        game:GetService("TeleportService"):Teleport(game.PlaceId,game:GetService("Players").LocalPlayer)
                                    end
                                until _G.AutoObservation == false or game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel")
                            end
                        else
                            topos(CFrame.new(5533.29785, 88.1079102, 4852.3916))
                        end
                    elseif World3 then
                        if game:GetService("Workspace").Enemies:FindFirstChild("Giant Islander [Lv. 1650]") then
                            if game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel") then
                                repeat task.wait()
                                    game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").Enemies:FindFirstChild("Giant Islander [Lv. 1650]").HumanoidRootPart.CFrame * CFrame.new(3,0,0)
                                until _G.AutoObservation == false or not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel")
                            else
                                repeat task.wait()
                                    game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").Enemies:FindFirstChild("Giant Islander [Lv. 1650]").HumanoidRootPart.CFrame * CFrame.new(0,50,0)
                                    wait(1)
                                    if not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel") and _G.AutoObservation_Hop == true then
                                        game:GetService("TeleportService"):Teleport(game.PlaceId,game:GetService("Players").LocalPlayer)
                                    end
                                until _G.AutoObservation == false or game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui:FindFirstChild("ImageLabel")
                            end
                        else
                            topos(CFrame.new(4530.3540039063, 656.75695800781, -131.60952758789))
                        end
                    end
                end
            end
        end
    end)
end)
Observation:AddToggle{
	Name = "Auto Observation Haki v2",
	Flag = "Auto_ObservationHakiv2",
	Value = _G.AutoObservationv2,
	Callback  = function(value)
		_G.AutoObservationv2 = value
		StopTween(_G.AutoObservationv2)
	end
}
spawn(function()
	while wait() do
		if _G.AutoObservationv2 then
			if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Start") == 0 then
				TP(CFrame.new(-10920.125, 624.20275878906, -10266.995117188))
				wait(1.1)
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Buy")
			else
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Fruit Bowl") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Fruit Bowl") then
					TP(CFrame.new(-10920.125, 624.20275878906, -10266.995117188))
					wait(1.1)
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("KenTalk2","Start")
				else
					if game:GetService("Workspace").AppleSpawner:FindFirstChild("Apple") then
						game:GetService("Workspace").AppleSpawner:FindFirstChild("Apple").Handle.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
					else
						if game:GetService("Workspace").BananaSpawne:FindFirstChild("Banana") then
							game:GetService("Workspace").BananaSpawne:FindFirstChild("Banana").Handle.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
						else
							if game:GetService("Workspace").PineappleSpawner:FindFirstChild("Pineapple") then
								game:GetService("Workspace").PineappleSpawner:FindFirstChild("Pineapple").Handle.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
							end
						end
					end
					if (game:GetService("Players").LocalPlayer.Character:FindFirstChild("Apple") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Apple")) and (game:GetService("Players").LocalPlayer.Character:FindFirstChild("Pineapple") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Pineapple")) and (game:GetService("Players").LocalPlayer.Character:FindFirstChild("Banana") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Banana")) then
						TP(CFrame.new(-12444.78515625, 332.40396118164, -7673.1806640625)) 
						wait(1.1)
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen")
						wait(.5)
					end
				end
			end
		end
	end
end)
local FactoryFarm = Page:CreateSection({
	Name = "FactoryFarm ☢️",
	Side = "Left"
})

FactoryFarm:AddToggle{
	Name = "Auto Factory Farm ",
	Flag = "Auto_Factory_Farm",
	Value = _G.Auto_Factory_Farm,
	Callback  = function(value)
		_G.Auto_Factory_Farm = value
		StopTween(_G.Auto_Factory_Farm)
	end
}



spawn(function()
	while wait() do
		if _G.Auto_Factory_Farm then
			pcall(function()
				if game.Workspace.Enemies:FindFirstChild("Core") then
					_G.FactoryCore = true
					_G.Auto_Farm_Level = false
					for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
						if _G.FactoryCore and v.Name == "Core" and v.Humanoid.Health > 0 then
							repeat wait()
								AutoHaki()
								EquipWeapon(_G.Select_Weapon)
								toTargetP(v.HumanoidRootPart.CFrame * CFrame.new(0,10,10))
								game:GetService'VirtualUser':CaptureController()
								game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
							until not _G.FactoryCore or v.Humanoid.Health <= 0 or _G.Auto_Factory_Farm == false
						end
					end
				elseif game.ReplicatedStorage:FindFirstChild("Core") and game.ReplicatedStorage:FindFirstChild("Core"):FindFirstChild("Humanoid") then
					_G.FactoryCore = true
					_G.Auto_Farm_Level = false
					toTargetP(CFrame.new(502.7349853515625, 143.0749053955078, -379.078125))
				elseif _G.Auto_Farm_Level then
					_G.FactoryCore = false
					_G.Auto_Farm_Level = true
				end
			end)
		end
	end
end)

FactoryFarm:AddToggle{
	Name = "Factory Hop",
	Flag = "Factory_Hop",
	Value = _G.Factory_Hop,
	Callback  = function(value)
		_G.Factory_Hop = value
	end
}

if _G.Factory_Hop then
	Hop()
end


local Ectoplasm = Page:CreateSection({
	Name = "Ectoplasm 👥",
	Side = "Right"
})


Ectoplasm:AddToggle{
	Name = "Auto Farm Ectoplasm",
	Flag = "Auto_Farm_Ectoplasm",
	Value = _G.AutoEctoplasm,
	Callback  = function(value)
		_G.AutoEctoplasm = value
		StopTween(_G.AutoEctoplasm)
	end
}


spawn(function()
	while wait() do
		if _G.AutoEctoplasm then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Ship Deckhand [Lv. 1250]") or game:GetService("Workspace").Enemies:FindFirstChild("Ship Engineer [Lv. 1275]") or game:GetService("Workspace").Enemies:FindFirstChild("Ship Steward [Lv. 1300]") or game:GetService("Workspace").Enemies:FindFirstChild("Ship Officer [Lv. 1325]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if string.find(v.Name, "Ship") then
							repeat wait()
								AutoHaki()
								EquipWeapon(_G.SelectWeapon)
								PosMonEctoplasm = v.HumanoidRootPart.CFrame
								v.HumanoidRootPart.CanCollide = false
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								toops(v.HumanoidRootPart.CFrame * CFrame.new(0,30,0))
								
								MagnetEctoplasm = true
								game:GetService'VirtualUser':CaptureController()
								game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
							until _G.AutoEctoplasm == false or not v.Parent or v.Humanoid.Health <= 0
							MagnetEctoplasm = false
						else
							MagnetEctoplasm = false
							TP(CFrame.new(904.4072265625, 181.05767822266, 33341.38671875))
						end
					end
				else
					MagnetEctoplasm = false
					local Distance = (Vector3.new(904.4072265625, 181.05767822266, 33341.38671875) - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
					if Distance > 20000 then
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(923.21252441406, 126.9760055542, 32852.83203125))
					end
					TP(CFrame.new(904.4072265625, 181.05767822266, 33341.38671875))
				end
			end)
		end
	end
end)
local Auto1 = Auto:CreateSection({
	Name = "[ East blue ]",
	Side = "Left"
})
Auto1:AddToggle{
	Name = "Auto Saber",
	Flag = "Auto Saber",
	Value =  _G.AutoSaber,
	Callback  = function(value)
		_G.AutoSaber = value
		StopTween(_G.AutoSaber)
	end
}

spawn(function()
	while wait() do
		if _G.AutoSaber then
			if game.Players.localPlayer.Data.Level.Value < 200 then
			else
				if game.Workspace.Map.Jungle.Final.Part.CanCollide == false then
					if _G.AutoSaber and game.ReplicatedStorage:FindFirstChild("Saber Expert [Lv. 200] [Boss]") or game.Workspace.Enemies:FindFirstChild("Saber Expert [Lv. 200] [Boss]") then
						if game.Workspace.Enemies:FindFirstChild("Saber Expert [Lv. 200] [Boss]") then
							for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
								if v.Name == "Saber Expert [Lv. 200] [Boss]" and v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
									repeat wait()
										if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude > 300 then
											Farmtween = topos(v.HumanoidRootPart.Position,v.HumanoidRootPart.CFrame)
										elseif (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
											if Farmtween then
												Farmtween:Stop()
											end
											AutoHaki()
											EquipWeapon(_G.SelectWeapon)
											game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0, 30, 0)
											game:GetService'VirtualUser':CaptureController()
											game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
										end
									until not _G.Auto_Saber or not v.Parent or v.Humanoid.Health <= 0
								end
							end
						else
							Questtween = topos(CFrame.new(-1405.41956, 29.8519993, 5.62435055).Position,CFrame.new(-1405.41956, 29.8519993, 5.62435055))
							if (CFrame.new(-1405.41956, 29.8519993, 5.62435055).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
								if Questtween then
									Questtween:Stop()
								end
								game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1405.41956, 29.8519993, 5.62435055, 0.885240912, 3.52892613e-08, 0.465132833, -6.60881128e-09, 1, -6.32913171e-08, -0.465132833, 5.29540891e-08, 0.885240912)
							end
						end
					else
						if _G.AutoSaberHop then
							Hop()
						end
					end
				elseif game.Players.LocalPlayer.Backpack:FindFirstChild("Relic") or game.Players.LocalPlayer.Character:FindFirstChild("Relic") and game.Players.localPlayer.Data.Level.Value >= 200 then
					EquipWeapon("Relic")
					wait(0.5)
					Questtween = topos(CFrame.new(-1405.41956, 29.8519993, 5.62435055).Position,CFrame.new(-1405.41956, 29.8519993, 5.62435055))
					if (CFrame.new(-1405.41956, 29.8519993, 5.62435055).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
						if Questtween then
							Questtween:Stop()
						end
						game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1405.41956, 29.8519993, 5.62435055, 0.885240912, 3.52892613e-08, 0.465132833, -6.60881128e-09, 1, -6.32913171e-08, -0.465132833, 5.29540891e-08, 0.885240912)
					end
				else
					if Workspace.Map.Jungle.QuestPlates.Door.CanCollide == false then
						if game.Workspace.Map.Desert.Burn.Part.CanCollide == false then
							if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","SickMan") == 0 then
								if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","RichSon") == 0 then
									if game.Workspace.Enemies:FindFirstChild("Mob Leader [Lv. 120] [Boss]") then
										for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
											if _G.AutoSaber and v:IsA("Model") and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 and v.Name == "Mob Leader [Lv. 120] [Boss]" then
												repeat
													pcall(function() wait() 
														if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude > 300 then
															Farmtween = topos(v.HumanoidRootPart.Position,v.HumanoidRootPart.CFrame)
														elseif (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
															if Farmtween then
																Farmtween:Stop()
															end
															AutoHaki()
															EquipWeapon(_G.SelectWeapon)
															if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
																local args = {
																	[1] = "Buso"
																}
																game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
															end
															game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0, 30, 0)
															game:GetService'VirtualUser':CaptureController()
															game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
														end
													end)
												until not _G.AutoSaber or not v.Parent or v.Humanoid.Health <= 0
											end
										end
									else
										Questtween = topos(CFrame.new(-2848.59399, 7.4272871, 5342.44043).Position,CFrame.new(-2848.59399, 7.4272871, 5342.44043))
										if (CFrame.new(-2848.59399, 7.4272871, 5342.44043).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
											if Questtween then
												Questtween:Stop()
											end
											game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-2848.59399, 7.4272871, 5342.44043, -0.928248107, -8.7248246e-08, 0.371961564, -7.61816636e-08, 1, 4.44474857e-08, -0.371961564, 1.29216433e-08, -0.928248107)
										end
									end
								elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","RichSon") == 1 then
									if game.Players.LocalPlayer.Backpack:FindFirstChild("Relic") or game.Players.LocalPlayer.Character:FindFirstChild("Relic") then
										EquipWeapon("Relic")
										wait(0.5)
										Questtween = topos(CFrame.new(-1405.41956, 29.8519993, 5.62435055).Position,CFrame.new(-1405.41956, 29.8519993, 5.62435055))
										if (CFrame.new(-1405.41956, 29.8519993, 5.62435055).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
											if Questtween then
												Questtween:Stop()
											end
											game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1405.41956, 29.8519993, 5.62435055)
										end
									else
										Questtween = topos(CFrame.new(-910.979736, 13.7520342, 4078.14624).Position,CFrame.new(-910.979736, 13.7520342, 4078.14624))
										if (CFrame.new(-910.979736, 13.7520342, 4078.14624).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
											if Questtween then
												Questtween:Stop()
											end
											game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-910.979736, 13.7520342, 4078.14624, 0.00685182028, -1.53155766e-09, -0.999976516, 9.15205245e-09, 1, -1.46888401e-09, 0.999976516, -9.14177267e-09, 0.00685182028)
											wait(.5)
											local args = {
												[1] = "ProQuestProgress",
												[2] = "RichSon"
											}
											game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
										end
									end
								else
									Questtween = topos(CFrame.new(-910.979736, 13.7520342, 4078.14624).Position,CFrame.new(-910.979736, 13.7520342, 4078.14624))
									if (CFrame.new(-910.979736, 13.7520342, 4078.14624).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
										if Questtween then
											Questtween:Stop()
										end
										game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-910.979736, 13.7520342, 4078.14624)
										local args = {
											[1] = "ProQuestProgress",
											[2] = "RichSon"
										}
										game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
									end
								end
							else
								if game.Players.LocalPlayer.Backpack:FindFirstChild("Cup") or game.Players.LocalPlayer.Character:FindFirstChild("Cup") then
									EquipWeapon("Cup")
									if game.Players.LocalPlayer.Character.Cup.Handle:FindFirstChild("TouchInterest") then
										Questtween = topos(CFrame.new(1397.229, 37.3480148, -1320.85217).Position,CFrame.new(1397.229, 37.3480148, -1320.85217))
										if (CFrame.new(1397.229, 37.3480148, -1320.85217).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
											if Questtween then
												Questtween:Stop()
											end
											game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1397.229, 37.3480148, -1320.85217, -0.11285457, 2.01368788e-08, 0.993611455, 1.91641178e-07, 1, 1.50028845e-09, -0.993611455, 1.90586206e-07, -0.11285457)
										end
									else
										wait(0.5)
										if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("ProQuestProgress","SickMan") ~= 0 then
											local args = {
												[1] = "ProQuestProgress",
												[2] = "SickMan"
											}
											game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
										end
									end
								else
									Questtween = topos(game.Workspace.Map.Desert.Cup.Position,game.Workspace.Map.Desert.Cup.CFrame)
									if (game.Workspace.Map.Desert.Cup.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
										if Questtween then
											Questtween:Stop()
										end
										game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Workspace.Map.Desert.Cup.CFrame
									end
									firetouchinterest(game.Workspace.Map.Desert.Cup.TouchInterest,game.Players.LocalPlayer.Character.Head, 1)
								end
							end
						else
							if game.Players.LocalPlayer.Backpack:FindFirstChild("Torch") or game.Players.LocalPlayer.Character:FindFirstChild("Torch") then
								EquipWeapon("Torch")
								Questtween = topos(CFrame.new(1114.87708, 4.9214654, 4349.8501).Position,CFrame.new(1114.87708, 4.9214654, 4349.8501))
								if (CFrame.new(1114.87708, 4.9214654, 4349.8501).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
									if Questtween then
										Questtween:Stop()
									end
									game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1114.87708, 4.9214654, 4349.8501, -0.612586915, -9.68697833e-08, 0.790403247, -1.2634203e-07, 1, 2.4638446e-08, -0.790403247, -8.47679615e-08, -0.612586915)
								end
							else
								Questtween = topos(CFrame.new(-1610.00757, 11.5049858, 164.001587).Position,CFrame.new(-1610.00757, 11.5049858, 164.001587))
								if (CFrame.new(-1610.00757, 11.5049858, 164.001587).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 300 then
									if Questtween then
										Questtween:Stop()
									end
									game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1610.00757, 11.5049858, 164.001587, 0.984807551, -0.167722285, -0.0449818149, 0.17364943, 0.951244235, 0.254912198, 3.42372805e-05, -0.258850515, 0.965917408)
								end
							end
						end
					else
						for i,v in pairs(Workspace.Map.Jungle.QuestPlates:GetChildren()) do
							if v:IsA("Model") then wait()
								if v.Button.BrickColor ~= BrickColor.new("Camo") then
									repeat wait()
										Questtween = topos(v.Button.Position,v.Button.CFrame)
										if (v.Button.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 150 then
											if Questtween then
												Questtween:Stop()
											end
											game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Button.CFrame
										end
									until not _G.AutoSaber or v.Button.BrickColor == BrickColor.new("Camo")
								end
							end
						end    
					end
				end
			end 
		end
	end
end)


Auto1:AddToggle{
Name = "Auto Waden ",
Flag = "Auto Waden ",
Value = _G.Autowaden,
Callback  = function(value)
	_G.Autowaden = value
end
} 


spawn(function()
while wait() do
	if  _G.Autowaden and World1 then
		pcall(function()
			if game:GetService("Workspace").Enemies:FindFirstChild("Chief Warden [Lv. 230] [Boss]") then
				for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
					if v.Name == "Chief Warden [Lv. 230] [Boss]" then
						if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							repeat task.wait()
								AutoHaki()
								EquipWeapon(_G.SelectWeapon)
								v.HumanoidRootPart.CanCollide = false
								v.Humanoid.WalkSpeed = 0
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
								game:GetService("VirtualUser"):CaptureController()
								game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
								sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
							until not  _G.Autowaden or not v.Parent or v.Humanoid.Health <= 0
						end
					end
				end
			else
			topos(CFrame.new(5186.14697265625, 24.86684226989746, 832.1885375976562))
				if game:GetService("ReplicatedStorage"):FindFirstChild("Chief Warden [Lv. 230] [Boss]") then
					topos(game:GetService("ReplicatedStorage"):FindFirstChild("Chief Warden [Lv. 230] [Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
				else
					if  _G.Autowadenhop then
						Hop()
					end
				end
			end
		end)
	end
end
end)


Auto1:AddToggle{
Name = "Auto Greybeard",
Flag = "Auto Greybeard",
Value = _G.Autogay,
Callback  = function(value)
	_G.Autogay = value
	StopTween()
end
} 


spawn(function()
while wait() do
	if  _G.Autogay and World1 then
		pcall(function()
			if game:GetService("Workspace").Enemies:FindFirstChild("Greybeard [Lv. 750] [Raid Boss]") then
				for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
					if v.Name == "Greybeard [Lv. 750] [Raid Boss]" then
						if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							repeat task.wait()
								AutoHaki()
								EquipWeapon(_G.SelectWeapon)
								v.HumanoidRootPart.CanCollide = false
								v.Humanoid.WalkSpeed = 0
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
								game:GetService("VirtualUser"):CaptureController()
								game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
								sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
							until not  _G.Autogay or not v.Parent or v.Humanoid.Health <= 0
						end
					end
				end
			else
			topos(CFrame.new(-5023.38330078125, 28.65203285217285, 4332.3818359375))
				if game:GetService("ReplicatedStorage"):FindFirstChild("Greybeard [Lv. 750] [Raid Boss]") then
					topos(game:GetService("ReplicatedStorage"):FindFirstChild("Greybeard [Lv. 750] [Raid Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
				else
					if  _G.Autogayhop then
						Hop()
					end
				end
			end
		end)
	end
end
end)


Auto1:AddToggle{
Name = "Auto pole",
Flag = "Auto pole",
Value = _G.Autopole,
Callback  = function(value)
	_G.Autopole = value
end
} 



spawn(function()
while wait() do
	if  _G.Autopole and World1 then
		pcall(function()
			if game:GetService("Workspace").Enemies:FindFirstChild("Thunder God [Lv. 575] [Boss]") then
				for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
					if v.Name == "Thunder God [Lv. 575] [Boss]" then
						if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							repeat task.wait()
								AutoHaki()
								EquipWeapon(_G.SelectWeapon)
								v.HumanoidRootPart.CanCollide = false
								v.Humanoid.WalkSpeed = 0
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
								game:GetService("VirtualUser"):CaptureController()
								game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
								sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
							until not  _G.Autopole or not v.Parent or v.Humanoid.Health <= 0
						end
					end
				end
			else
			topos(CFrame.new(-7748.0185546875, 5606.80615234375, -2305.898681640625))
				if game:GetService("ReplicatedStorage"):FindFirstChild("Thunder God [Lv. 575] [Boss]") then
					topos(game:GetService("ReplicatedStorage"):FindFirstChild("Thunder God [Lv. 575] [Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
				else
					if  _G.Autopolehop then
						Hop()
					end
				end
			end
		end)
	end
end
end)

    

Auto1:AddToggle{
Name = "Auto Saw",
Flag = "Auto Saw",
Value = _G.Autosaw,
Callback  = function(value)
	_G.Autosaw = value
end
} 



spawn(function()
while wait() do
	if  _G.Autosaw and World1 then
		pcall(function()
			if game:GetService("Workspace").Enemies:FindFirstChild("The Saw [Lv. 100] [Boss]") then
				for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
					if v.Name == "The Saw [Lv. 100] [Boss]" then
						if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							repeat task.wait()
								AutoHaki()
								EquipWeapon(_G.SelectWeapon)
								v.HumanoidRootPart.CanCollide = false
								v.Humanoid.WalkSpeed = 0
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
								game:GetService("VirtualUser"):CaptureController()
								game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
								sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
							until not  _G.Autosaw or not v.Parent or v.Humanoid.Health <= 0
						end
					end
				end
			else
			topos(CFrame.new(-690.33081054688, 15.09425163269, 1582.2380371094))
				if game:GetService("ReplicatedStorage"):FindFirstChild("The Saw [Lv. 100] [Boss]") then
					topos(game:GetService("ReplicatedStorage"):FindFirstChild("The Saw [Lv. 100] [Boss]").HumanoidRootPart.CFrame * CFrame.new(2,40,2))
				else
					if  _G.Autosawhop then
						Hop()
					end
				end
			end
		end)
	end
end
end)

Auto1:AddToggle{
	Name = "Auto Evo Race",
	Flag = "Auto_EvoRace",
	Value =  _G.Auto_EvoRace,
	Callback  = function(value)
		_G.Auto_EvoRace = value
		StopTween(_G.Auto_EvoRace)
	end
}
spawn(function()
	pcall(function()
		while wait() do
			if _G.Auto_EvoRace then
				if not game:GetService("Players").LocalPlayer.Data.Race:FindFirstChild("Evolved") then
					if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Alchemist","1") == 0 then
						topos(CFrame.new(-2779.83521, 72.9661407, -3574.02002, -0.730484903, 6.39014104e-08, -0.68292886, 3.59963224e-08, 1, 5.50667032e-08, 0.68292886, 1.56424669e-08, -0.730484903))
						if (Vector3.new(-2779.83521, 72.9661407, -3574.02002) - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 4 then
							wait(1.3)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Alchemist","2")
						end
					elseif game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Alchemist","1") == 1 then
						pcall(function()
							if not game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Flower 1") and not game:GetService("Players").LocalPlayer.Character:FindFirstChild("Flower 1") then
								topos(game:GetService("Workspace").Flower1.CFrame)
							elseif not game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Flower 2") and not game:GetService("Players").LocalPlayer.Character:FindFirstChild("Flower 2") then
								topos(game:GetService("Workspace").Flower2.CFrame)
							elseif not game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Flower 3") and not game:GetService("Players").LocalPlayer.Character:FindFirstChild("Flower 3") then
								if game:GetService("Workspace").Enemies:FindFirstChild("Zombie [Lv. 950]") then
									for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
										if v.Name == "Zombie [Lv. 950]" then
											repeat task.wait()
												AutoHaki()
												EquipWeapon(_G.SelectWeapon)
												topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
												v.HumanoidRootPart.CanCollide = false
												game:GetService("VirtualUser"):CaptureController()
												game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
												PosMonEvo = v.HumanoidRootPart.CFrame
												StartEvoMagnet = true
											until game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Flower 3") or not v.Parent or v.Humanoid.Health <= 0 or _G.Auto_EvoRace == false
											StartEvoMagnet = false
										end
									end
								else
									StartEvoMagnet = false
									topos(CFrame.new(-5685.9233398438, 48.480125427246, -853.23724365234))
								end
							end
						end)
					else
						if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Alchemist","1") == 2 then
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Alchemist","3")
						end
					end
				end
			end
		end
	end)
end)
Auto1:AddToggle{
	Name = "Auto Shark Saw",
	Flag = "Auto_SharkSaw",
	Value =  _G.AutoShawkSaw,
	Callback  = function(value)
		_G.AutoShawkSaw = value
		StopTween(_G.AutoSharkSaw)
	end
}
spawn(function()
	pcall(function()
		while wait() do
			if _G.AutoShawkSaw then
				if game:GetService("Workspace").Enemies:FindFirstChild("Ship Deckhand [Lv. 1250]") or game:GetService("Workspace").Enemies:FindFirstChild("Ship Engineer [Lv. 1275]") or game:GetService("Workspace").Enemies:FindFirstChild("Ship Steward [Lv. 1300]") or game:GetService("Workspace").Enemies:FindFirstChild("Ship Officer [Lv. 1325]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "The Saw [Lv. 100] [Boss]" then
							repeat task.wait()
								EquipWeapon(_G.SelectWeapon)
								AutoHaki()
								if v.Name == "The Saw [Lv. 100] [Boss]" then
									v.HumanoidRootPart.CanCollide = false
									v.Head.CanCollide = false
									topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
									SharkMon = v.HumanoidRootPart.CFrame
									StartShark = true
								end
							until _G.AutoShawkSaw == false or not v.Parent or v.Humanoid.Health <= 0
						end
					end
				end
			end
		end
	end)
end)


local Auto2 = Auto:CreateSection({
    Name = "[ Dressrosa ] ",
    Side = "Right"
})
Auto2:AddToggle{
	Name = "Auto Rengoku",
	Flag = "Auto_Rengoku",
	Value =  _G.Auto_Rainbow_Haki,
	Callback  = function(value)
		_G.AutoRengoku = value
		StopTween(_G.AutoRengoku)
	end
}
spawn(function()
	pcall(function()
		while wait() do
			if _G.AutoRengoku then
				if game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Hidden Key") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Hidden Key") then
					EquipWeapon("Hidden Key")
					topos(CFrame.new(6571.1201171875, 299.23028564453, -6967.841796875))
				else 
					if game:GetService("Workspace").Enemies:FindFirstChild("Snow Lurker [Lv. 1375]") or game:GetService("Workspace").Enemies:FindFirstChild("Arctic Warrior [Lv. 1350]") then
						for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
							if (v.Name == "Snow Lurker [Lv. 1375]" or v.Name == "Arctic Warrior [Lv. 1350]") and v.Humanoid.Health > 0 then
								repeat task.wait()
									EquipWeapon(_G.SelectWeapon)
									AutoHaki()
									v.HumanoidRootPart.CanCollide = false
									RengokuMon = v.HumanoidRootPart.CFrame
									topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
									game:GetService'VirtualUser':CaptureController()
									game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
									StartRengokuMagnet = true
								until game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Hidden Key") or _G.AutoRengoku == false or not v.Parent or v.Humanoid.Health <= 0
								StartRengokuMagnet = false
							end
						end
					else
						StartRengokuMagnet = false
						if game:GetService("ReplicatedStorage"):FindFirstChild("Snow Lurker [Lv. 1375]") then
							topos(game:GetService("ReplicatedStorage"):FindFirstChild("Snow Lurker [Lv. 1375]").HumanoidRootPart.CFrame * CFrame.new(0,35,0))
						else
							if game:GetService("ReplicatedStorage"):FindFirstChild("Arctic Warrior [Lv. 1350]") then
								topos(game:GetService("ReplicatedStorage"):FindFirstChild("Arctic Warrior [Lv. 1350]").HumanoidRootPart.CFrame * CFrame.new(0,35,0))
							end
						end
					end
				end
			end
		end
	end)
end)
Auto2:AddToggle{
	Name = "Auto Bartlio Quest",
	Flag = "Auto Bartlio Quest",
	Value =  _G.AutoBartilo,
	Callback  = function(value)
		_G.AutoBartilo = value
		StopTween(_G.AutoBartilo)
	end
}
spawn(function()
	pcall(function()
		while wait() do
			if _G.AutoBartilo then
				if game:GetService("Players").LocalPlayer.Data.Level.Value >= 800 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") == 0 then
					if string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Swan Pirates") and string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "50") and game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true then 
						if game:GetService("Workspace").Enemies:FindFirstChild("Swan Pirate [Lv. 775]") then
							Ms = "Swan Pirate [Lv. 775]"
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == Ms then
									pcall(function()
										repeat task.wait()
											sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
											EquipWeapon(_G.SelectWeapon)
											AutoHaki()
											v.HumanoidRootPart.CanCollide = false
											topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))													
											PosMonBarto =  v.HumanoidRootPart.CFrame
											AutoBartiloBring = true
										until not v.Parent or v.Humanoid.Health <= 0 or _G.AutoBartilo == false or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
										AutoBartiloBring = false
									end)
								end
							end
						else
							TP(CFrame.new(932.624451, 156.106079, 1180.27466, -0.973085582, 4.55137119e-08, -0.230443969, 2.67024713e-08, 1, 8.47491108e-08, 0.230443969, 7.63147128e-08, -0.973085582))
						end
					else
						TP(CFrame.new(-456.28952, 73.0200958, 299.895966))
						wait(1.1)
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest","BartiloQuest",1)
					end
				elseif game:GetService("Players").LocalPlayer.Data.Level.Value >= 800 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") == 1 then
					if game:GetService("Workspace").Enemies:FindFirstChild("Jeremy [Lv. 850] [Boss]") then
						Ms = "Jeremy [Lv. 850] [Boss]"
						for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
							if v.Name == Ms then
								repeat task.wait()
									sethiddenproperty(game:GetService("Players").LocalPlayer, "SimulationRadius", math.huge)
									EquipWeapon(_G.SelectWeapon)
									AutoHaki()
									v.HumanoidRootPart.CanCollide = false
									topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
									sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
								until not v.Parent or v.Humanoid.Health <= 0 or _G.AutoBartilo == false
							end
						end
					elseif game:GetService("ReplicatedStorage"):FindFirstChild("Jeremy [Lv. 850] [Boss]") then
						TP(CFrame.new(-456.28952, 73.0200958, 299.895966))
						wait(1.1)
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo")
						wait(1)
						TP(CFrame.new(2099.88159, 448.931, 648.997375))
						wait(2)
					else
						TP(CFrame.new(2099.88159, 448.931, 648.997375))
					end
				elseif game:GetService("Players").LocalPlayer.Data.Level.Value >= 800 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("BartiloQuestProgress","Bartilo") == 2 then
					TP(CFrame.new(-1850.49329, 13.1789551, 1750.89685))
					wait(1)
					TP(CFrame.new(-1858.87305, 19.3777466, 1712.01807))
					wait(1)
					TP(CFrame.new(-1803.94324, 16.5789185, 1750.89685))
					wait(1)
					TP(CFrame.new(-1858.55835, 16.8604317, 1724.79541))
					wait(1)
					TP(CFrame.new(-1869.54224, 15.987854, 1681.00659))
					wait(1)
					TP(CFrame.new(-1800.0979, 16.4978027, 1684.52368))
					wait(1)
					TP(CFrame.new(-1819.26343, 14.795166, 1717.90625))
					wait(1)
					TP(CFrame.new(-1813.51843, 14.8604736, 1724.79541))
				end
			end 
		end
	end)
end)

Auto2:AddToggle{
	Name = "Auto FarmSwanGlasses",
	Flag = "Auto FarmSwanGlasses",
	Value =  _G.AutoFarmSwanGlasses,
	Callback  = function(value)
		_G.AutoFarmSwanGlasses = value
		StopTween(_G.AutoFarmSwanGlasses)
	end
	} 
	
	spawn(function()
	pcall(function()
		while wait() do
			if _G.AutoFarmSwanGlasses then
				if game:GetService("Workspace").Enemies:FindFirstChild("Don Swan [Lv. 1000] [Boss]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "Don Swan [Lv. 1000] [Boss]" and v.Humanoid.Health > 0 and v:IsA("Model") and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") then
							repeat task.wait()
								pcall(function()
									AutoHaki()
									EquipWeapon(_G.SelectWeapon)
									v.HumanoidRootPart.CanCollide = false
									v.HumanoidRootPart.Size = Vector3.new(50,50,50)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
									game:GetService("VirtualUser"):CaptureController()
									game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 670))
									sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
								end)
							until _G.AutoFarmSwanGlasses == false or v.Humanoid.Health <= 0
						end
					end
				else 
					repeat task.wait()
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(2284.912109375, 15.537666320801, 905.48291015625)) 
					until (CFrame.new(2284.912109375, 15.537666320801, 905.48291015625).Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 4 or _G.AutoFarmSwanGlasses == false
				end
			end
		end
	end)
	end)
	

	Auto2:AddToggle{
	Name = "Auto DragonTrident",
	Flag = "Auto DragonTrident",
	Value = _G.Auto_Dragon_Trident,
	Callback  = function(value)
		_G.Auto_Dragon_Trident = value
	end
} 



spawn(function()
while wait() do
	if  _G.Auto_Dragon_Trident and World2 then
		pcall(function()
			if game:GetService("Workspace").Enemies:FindFirstChild("Tide Keeper [Lv. 1475] [Boss]") then
				for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
					if v.Name == "Tide Keeper [Lv. 1475] [Boss]" then
						if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
							repeat task.wait()
								AutoHaki()
								EquipWeapon(_G.SelectWeapon)
								v.HumanoidRootPart.CanCollide = false
								v.Humanoid.WalkSpeed = 0
								v.HumanoidRootPart.Size = Vector3.new(50,50,50)
								topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
								game:GetService("VirtualUser"):CaptureController()
								game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
								sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
							until not  _G.Auto_Dragon_Trident or not v.Parent or v.Humanoid.Health <= 0
						end
					end
				end
			else
			topos(CFrame.new(-3914.830322265625, 123.29389190673828, -11516.8642578125))
				if game:GetService("ReplicatedStorage"):FindFirstChild("Tide Keeper [Lv. 1475] [Boss]") then
					topos(game:GetService("ReplicatedStorage"):FindFirstChild("Tide Keeper [Lv. 1475] [Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
				else
					if  _G.Auto_Dragon_Trident_Hop then
						Hop()
					end
				end
			end
		end)
	end
end
end)

spawn(function()
pcall(function()
	while wait(.1) do
		if (_G.AutoFarmSwanGlasses and _G.AutoFarmSwanGlasses_Hop) and World2 and not game:GetService("ReplicatedStorage"):FindFirstChild("Don Swan [Lv. 1000] [Boss]") and not game:GetService("Workspace").Enemies:FindFirstChild("Don Swan [Lv. 1000] [Boss]") then
			Hop()
		end
	end
end)
end)



Auto2:AddToggle{
Name = "Auto BuyLegendarySword",
Flag = "Auto BuyLegendarySword",
Value = _G.AutoBuyLegendarySword,
Callback  = function(value)
	_G.AutoBuyLegendarySword = value
end
} 

spawn(function()
while wait() do
if _G.AutoBuyLegendarySword then
	pcall(function()
		local args = {
			[1] = "LegendarySwordDealer",
			[2] = "1"
		}
		game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
		local args = {
			[1] = "LegendarySwordDealer",
			[2] = "2"
		}
		game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
		local args = {
			[1] = "LegendarySwordDealer",
			[2] = "3"
		}
		game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
		if _G.AutoBuyLegendarySword_Hop and _G.AutoBuyLegendarySword and World2 then
			wait(10)
			Hop()
		end
	end)
end 
end
end)




Auto2:AddToggle{
Name = "Auto BuyEnchancementColour",
Flag = "Auto BuyEnchancementColour",
Value = _G.AutoBuyEnchancementColour,
Callback  = function(value)
_G.AutoBuyEnchancementColour = value
end
} 



spawn(function()
while wait() do
if _G.AutoBuyEnchancementColour then
	local args = {
		[1] = "ColorsDealer",
		[2] = "2"
	}
	game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
	if _G.AutoBuyEnchancementColour_Hop and _G.AutoBuyEnchancementColour and not World1 then
		wait(10)
		Hop()
	end
end 
end
end)
Auto2:AddToggle{
	Name = "Auto Law Sword",
	Flag = "Auto Law Sword",
	Value = _G.AutoOderSword,
	Callback  = function(value)
		_G.AutoOderSword = value
		StopTween(_G.AutoOderSword)
	end
} 

spawn(function()
	while wait() do
		if  _G.AutoOderSword then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Order [Lv. 1250] [Raid Boss]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "Order [Lv. 1250] [Raid Boss]" then
							if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								repeat task.wait()
									AutoHaki()
									EquipWeapon(_G.SelectWeapon)
									v.HumanoidRootPart.CanCollide = false
									v.Humanoid.WalkSpeed = 0
									v.HumanoidRootPart.Size = Vector3.new(50,50,50)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(2,20,2))
									game:GetService("VirtualUser"):CaptureController()
									game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
									sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
								until not  _G.AutoOderSword or not v.Parent or v.Humanoid.Health <= 0
							end
						end
					end
				else
					if game:GetService("ReplicatedStorage"):FindFirstChild("Order [Lv. 1250] [Raid Boss]") then
						topos(game:GetService("ReplicatedStorage"):FindFirstChild("Order [Lv. 1250] [Raid Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
					else
						if  _G.AutoOderSwordHop then
							Hop()
						end
					end
				end
			end)
		end
	end
end) 
Auto2:AddButton({
	Name = "Buy Microchip Law Boss",
	Callback = function()
		local args = {
			[1] = "BlackbeardReward",
			[2] = "Microchip",
			[3] = "2"
		 }
		 game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
	end
})
local Moon = Auto:CreateSection({
    Name = "[ Moon ]",
    Side = "Left"
})
local FM = Moon:AddLabel({
    Name = "[ Third World ]"
})
task.spawn(function()
	while task.wait() do
		pcall(function()
			if game:GetService("Lighting").Sky.MoonTextureId=="http://www.roblox.com/asset/?id=9709149431" then
				FM:Set("🌕: Full Moon 100%")
			elseif game:GetService("Lighting").Sky.MoonTextureId=="http://www.roblox.com/asset/?id=9709149052" then
				FM:Set("🌖’ : Full Moon 75%")
			elseif game:GetService("Lighting").Sky.MoonTextureId=="http://www.roblox.com/asset/?id=9709143733" then
				FM:Set("🌗“ : Full Moon 50%")
			elseif game:GetService("Lighting").Sky.MoonTextureId=="http://www.roblox.com/asset/?id=9709150401" then
				FM:Set("🌘 : Full Moon 25%")
			elseif game:GetService("Lighting").Sky.MoonTextureId=="http://www.roblox.com/asset/?id=9709149680" then
				FM:Set("🌘: Full Moon 15%")
			else
				FM:Set("Wait For Moon")
			end
		end)
	end
end)
local Auto3 = Auto:CreateSection({
    Name = "[ Zou ]",
    Side = "Left"
})
Auto3:AddToggle{
	Name = "Auto Buddy Sword",
	Flag = "Auto Buddy Sword",
	Value =  _G.AutoBudySword,
	Callback  = function(value)
		_G.AutoBudySword = value
        StopTween(_G.AutoBudySword)
	end
}

spawn(function()
	while wait() do
		if _G.AutoBudySword and World3 then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Cake Queen [Lv. 2175] [Boss]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "Cake Queen [Lv. 2175] [Boss]" then
							if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								repeat task.wait()
									AutoHaki()
									EquipWeapon(_G.SelectWeapon)
									v.HumanoidRootPart.CanCollide = false
									v.Humanoid.WalkSpeed = 0
									v.HumanoidRootPart.Size = Vector3.new(50,50,50)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
									game:GetService("VirtualUser"):CaptureController()
									game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
									sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
								until not _G.AutoBudySword or not v.Parent or v.Humanoid.Health <= 0
							end
						end
					end
				else
				topos(CFrame.new(-731.2034301757812, 381.5658874511719, -11198.4951171875))
					if game:GetService("ReplicatedStorage"):FindFirstChild("Cake Queen [Lv. 2175] [Boss]") then
						topos(game:GetService("ReplicatedStorage"):FindFirstChild("Cake Queen [Lv. 2175] [Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
					else
						if _G.AutoBudySwordHop then
							Hop()
						end
					end
				end
			end)
		end
	end
end)
Auto3:AddToggle{
	Name = "Auto Soul Guitar",
	Flag = "Auto Soul Guitar",
	Value =  _G.AutoNevaSoulGuitar,
	Callback  = function(value)
		_G.AutoNevaSoulGuitar = value    
		StopTween(_G.AutoNevaSoulGuitar)
	end
}

spawn(function()
	while wait() do
		pcall(function()
			if _G.AutoNevaSoulGuitar then
				if GetWeaponInventory("Soul Guitar") == false then
					if (CFrame.new(-9681.458984375, 6.139880657196045, 6341.3720703125).Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 5000 then
						if game:GetService("Workspace").NPCs:FindFirstChild("Skeleton Machine") then
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("soulGuitarBuy",true)
						else
							if game:GetService("Workspace").Map["Haunted Castle"].Candle1.Transparency == 0 then
								if game:GetService("Workspace").Map["Haunted Castle"].Placard1.Left.Part.Transparency == 0 then
									Quest2 = true
									repeat wait() topos(CFrame.new(-8762.69140625, 176.84783935546875, 6171.3076171875)) until (CFrame.new(-8762.69140625, 176.84783935546875, 6171.3076171875).Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 3 or not _G.AutoNevaSoulGuitar
									wait(1)
									fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"].Placard7.Left.ClickDetector)
									wait(1)
									fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"].Placard6.Left.ClickDetector)
									wait(1)
									fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"].Placard5.Left.ClickDetector)
									wait(1)
									fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"].Placard4.Right.ClickDetector)
									wait(1)
									fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"].Placard3.Left.ClickDetector)
									wait(1)
									fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"].Placard2.Right.ClickDetector)
									wait(1)
									fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"].Placard1.Right.ClickDetector)
									wait(1)
								elseif game:GetService("Workspace").Map["Haunted Castle"].Tablet.Segment1:FindFirstChild("ClickDetector") then
									if game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part1:FindFirstChild("ClickDetector") then
										Quest4 = true
										repeat wait() topos(CFrame.new(-9553.5986328125, 65.62338256835938, 6041.58837890625)) until (CFrame.new(-9553.5986328125, 65.62338256835938, 6041.58837890625).Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 3 or not _G.AutoNevaSoulGuitar
										wait(1)
										topos(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part3.CFrame)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part3.ClickDetector)
										wait(1)
										topos(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part4.CFrame)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part4.ClickDetector)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part4.ClickDetector)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part4.ClickDetector)
										wait(1)
										topos(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part6.CFrame)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part6.ClickDetector)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part6.ClickDetector)
										wait(1)
										topos(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part8.CFrame)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part8.ClickDetector)
										wait(1)
										topos(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part10.CFrame)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part10.ClickDetector)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part10.ClickDetector)
										wait(1)
										fireclickdetector(game:GetService("Workspace").Map["Haunted Castle"]["Lab Puzzle"].ColorFloor.Model.Part10.ClickDetector)
									else
										Quest3 = true
										--Not Work Yet
									end
								else
									if game:GetService("Workspace").NPCs:FindFirstChild("Ghost") then
										local args = {
											[1] = "GuitarPuzzleProgress",
											[2] = "Ghost"
										}

										game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
									end
									if game.Workspace.Enemies:FindFirstChild("Living Zombie [Lv. 2000]") then
										for i,v in pairs(game.Workspace.Enemies:GetChildren()) do
											if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
												if v.Name == "Living Zombie [Lv. 2000]" then
													EquipWeapon(_G.SelectWeapon)
													v.HumanoidRootPart.Size = Vector3.new(60,60,60)
													v.HumanoidRootPart.Transparency = 1
													v.Humanoid.JumpPower = 0
													v.Humanoid.WalkSpeed = 0
													v.HumanoidRootPart.CanCollide = false
													v.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,20,0)
													topos(CFrame.new(-10160.787109375, 138.6616973876953, 5955.03076171875))
													game:GetService'VirtualUser':CaptureController()
													game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
												end
											end
										end
									else
										topos(CFrame.new(-10160.787109375, 138.6616973876953, 5955.03076171875))
									end
								end
							else    
								if string.find(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("gravestoneEvent",2), "Error") then
									print("Go to Grave")
									topos(CFrame.new(-8653.2060546875, 140.98487854003906, 6160.033203125))
								elseif string.find(game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("gravestoneEvent",2), "Nothing") then
									print("Wait Next Night")
								else
									game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("gravestoneEvent",2,true)
								end
							end
						end
					else
						topos(CFrame.new(-9681.458984375, 6.139880657196045, 6341.3720703125))
end
else
if _G.soulGuitarhop then
hop()
end
					end
				end
		end)
	end
end)

Auto3:AddToggle{
    Name = "Auto Dual Curse Katana",
    Flag = "Auto_Cursed_Dual_Katana",
    Value =  Auto_Cursed_Dual_Katana,
    Callback  = function(value)
        Auto_Cursed_Dual_Katana = value
        StopTween(Auto_Cursed_Dual_Katana)
    end
}
spawn(function()
	while wait() do
		pcall(function()
			if Auto_Cursed_Dual_Katana then
				if GetMaterial("Alucard Fragment") == 0 then
					Auto_Quest_Yama_1 = true
					Auto_Quest_Yama_2 = false
					Auto_Quest_Yama_3 = false
					Auto_Quest_Tushita_1 = false
					Auto_Quest_Tushita_2 = false
					Auto_Quest_Tushita_3 = false
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","Progress","Evil")
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","StartTrial","Evil")
				elseif GetMaterial("Alucard Fragment") == 1 then
					Auto_Quest_Yama_1 = false
					Auto_Quest_Yama_2 = true
					Auto_Quest_Yama_3 = false
					Auto_Quest_Tushita_1 = false
					Auto_Quest_Tushita_2 = false
					Auto_Quest_Tushita_3 = false
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","Progress","Evil")
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","StartTrial","Evil")
				elseif GetMaterial("Alucard Fragment") == 2 then
					Auto_Quest_Yama_1 = false
					Auto_Quest_Yama_2 = false
					Auto_Quest_Yama_3 = true
					Auto_Quest_Tushita_1 = false
					Auto_Quest_Tushita_2 = false
					Auto_Quest_Tushita_3 = false
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","Progress","Evil")
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","StartTrial","Evil")
				elseif GetMaterial("Alucard Fragment") == 3 then
					Auto_Quest_Yama_1 = false
					Auto_Quest_Yama_2 = false
					Auto_Quest_Yama_3 = false
					Auto_Quest_Tushita_1 = true
					Auto_Quest_Tushita_2 = false
					Auto_Quest_Tushita_3 = false
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","Progress","Good")
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","StartTrial","Good")
				elseif GetMaterial("Alucard Fragment") == 4 then
					Auto_Quest_Yama_1 = false
					Auto_Quest_Yama_2 = false
					Auto_Quest_Yama_3 = false
					Auto_Quest_Tushita_1 = false
					Auto_Quest_Tushita_2 = true
					Auto_Quest_Tushita_3 = false
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","Progress","Good")
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","StartTrial","Good")
				elseif GetMaterial("Alucard Fragment") == 5 then
					Auto_Quest_Yama_1 = false
					Auto_Quest_Yama_2 = false
					Auto_Quest_Yama_3 = false
					Auto_Quest_Tushita_1 = false
					Auto_Quest_Tushita_2 = false
					Auto_Quest_Tushita_3 = true
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","Progress","Good")
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","StartTrial","Good")
				elseif GetMaterial("Alucard Fragment") == 6 then
					if game:GetService("Workspace").Enemies:FindFirstChild("Cursed Skeleton Boss [Lv. 2025] [Boss]") or game:GetService("Workspace").ReplicatedStorage:FindFirstChild("Cursed Skeleton Boss [Lv. 2025] [Boss]") then
						Auto_Quest_Yama_1 = false
						Auto_Quest_Yama_2 = false
						Auto_Quest_Yama_3 = false
						Auto_Quest_Tushita_1 = false
						Auto_Quest_Tushita_2 = false
						Auto_Quest_Tushita_3 = false
						if game:GetService("Workspace").Enemies:FindFirstChild("Cursed Skeleton Boss [Lv. 2025] [Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("Cursed Skeleton [Lv. 2200]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Cursed Skeleton Boss [Lv. 2025] [Boss]" or v.Name == "Cursed Skeleton [Lv. 2200]" then
									if v.Humanoid.Health > 0 then
										v.HumanoidRootPart.CanCollide = false
										v.HumanoidRootPart.Size = Vector3.new(60, 60, 60)
										topos(v.HumanoidRootPart.CFrame * CFrame.new(0,50,0))
										game:GetService'VirtualUser':CaptureController()
										game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
									end
								end
							end
						end
					else
						if (CFrame.new(-12361.7060546875, 603.3547973632812, -6550.5341796875).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 100 then
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","Progress","Good")
							wait(1)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","Progress","Evil")
							wait(1)
							topos(CFrame.new(-12361.7060546875, 603.3547973632812, -6550.5341796875))
							wait(1.5)
							game:GetService("VirtualInputManager"):SendKeyEvent(true, "E", false, game)
							wait(1.5)
							topos(CFrame.new(-12253.5419921875, 598.8999633789062, -6546.8388671875))
						else
							topos(CFrame.new(-12361.7060546875, 603.3547973632812, -6550.5341796875))
						end   
					end
				end
			end
		end)
	end
end)

spawn(function()
	while wait() do
		if Auto_Quest_Yama_1 then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Mythological Pirate [Lv. 1850]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "Mythological Pirate [Lv. 1850]" then
							repeat wait()
								topos(v.HumanoidRootPart.CFrame * CFrame.new(0,0,-2))
							until Auto_Cursed_Dual_Katana == false or Auto_Cursed_Dual_Katana == false or Auto_Quest_Yama_1 == false
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","StartTrial","Evil")
						end
					end
				else
					topos(CFrame.new(-13451.46484375, 543.712890625, -6961.0029296875))
				end
			end)
		end
	end
end)

spawn(function()
	while wait() do
		pcall(function()
			if Auto_Quest_Yama_2 then
				for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
					if v:FindFirstChild("HazeESP") then
						v.HazeESP.Size = UDim2.new(50,50,50,50)
						v.HazeESP.MaxDistance = "inf"
					end
				end
				for i,v in pairs(game:GetService("ReplicatedStorage"):GetChildren()) do
					if v:FindFirstChild("HazeESP") then
						v.HazeESP.Size = UDim2.new(50,50,50,50)
						v.HazeESP.MaxDistance = "inf"
					end
				end
			end
		end)
	end
end)

spawn(function()
	while wait() do
		pcall(function()
			for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
				if Auto_Quest_Yama_2 and v:FindFirstChild("HazeESP") and (v.HumanoidRootPart.Position - PosMonsEsp.Position).magnitude <= 300 then
					v.HumanoidRootPart.CFrame = PosMonsEsp
					v.HumanoidRootPart.CanCollide = false
					v.HumanoidRootPart.Size = Vector3.new(50,50,50)
					if not v.HumanoidRootPart:FindFirstChild("BodyVelocity") then
						local vc = Instance.new("BodyVelocity", v.HumanoidRootPart)
						vc.MaxForce = Vector3.new(1, 1, 1) * math.huge
						vc.Velocity = Vector3.new(0, 0, 0)
					end
				end
			end
		end)
	end
end)

spawn(function()
	while wait() do
		if Auto_Quest_Yama_2 then 
			pcall(function() 
				for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
					if v:FindFirstChild("HazeESP") then
						repeat wait()
							if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 2000 then
								topos(y.HumanoidRootPart.CFrameMon * CFrame.new(0,20,0))
							else
								StartMagnet = true
								FastAttack = true
								if Auto_Buso then
									if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
										game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
									end
								end
								if not game.Players.LocalPlayer.Character:FindFirstChild(_G.SelectWeapon) then
									wait()
									EquipWeapon(_G.SelectWeapon)
								end
								PosMonsEsp = v.HumanoidRootPart.CFrame
								if not FastAttack then
									game:GetService'VirtualUser':CaptureController()
									game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
								end
								v.HumanoidRootPart.Size = Vector3.new(60,60,60)
								if _G.Configs["Show Hitbox"] then
									v.HumanoidRootPart.Transparency = _G.Hitbox_LocalTransparency
								else
									v.HumanoidRootPart.Transparency = 1
								end
								v.Humanoid.JumpPower = 0
								v.Humanoid.WalkSpeed = 0
								v.HumanoidRootPart.CanCollide = false
								v.Humanoid:ChangeState(11)
								topos(v.HumanoidRootPart.CFrame * CFrame.new(0,20,0))								
							end      
						until Auto_Cursed_Dual_Katana == false or Auto_Quest_Yama_2 == false or not v.Parent or v.Humanoid.Health <= 0 or not v:FindFirstChild("HazeESP")
					else
						for x,y in pairs(game:GetService("ReplicatedStorage"):GetChildren()) do
							if y:FindFirstChild("HazeESP") then
								if (y.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 2000 then
									topos(y.HumanoidRootPart.CFrameMon* CFrame.new(0,20,0))
								else
									topos(y.HumanoidRootPart.CFrame * CFrame.new(0,20,0))
								end
							end
						end
					end
				end
			end)
		end
	end
end)

spawn(function()
	while wait() do
		if Auto_Quest_Yama_3 then
			pcall(function()
				if game.Players.LocalPlayer.Backpack:FindFirstChild("Hallow Essence") then         
					_G.Auto_Bone = false           
					topos(game:GetService("Workspace").Map["Haunted Castle"].Summoner.Detection.CFrame)
				elseif game:GetService("Workspace").Map:FindFirstChild("HellDimension") then
					repeat wait()
						if game:GetService("Workspace").Enemies:FindFirstChild("Cursed Skeleton [Lv. 2200]") or game:GetService("Workspace").Enemies:FindFirstChild("Cursed Skeleton [Lv. 2200] [Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("Hell's Messenger [Lv. 2200] [Boss]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Cursed Skeleton [Lv. 2200]" or v.Name == "Cursed Skeleton [Lv. 2200] [Boss]" or v.Name == "Hell's Messenger [Lv. 2200] [Boss]" then
									if v.Humanoid.Health > 0 then
										repeat wait()
											StartMagnet = true
											FastAttack = true
											if Auto_Buso then
												if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
													game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
												end
											end
											if not game.Players.LocalPlayer.Character:FindFirstChild(_G.SelectWeapon) then
												wait()
												EquipWeapon(_G.SelectWeapon)
											end
											PosMonsEsp = v.HumanoidRootPart.CFrame
											if not FastAttack then
												game:GetService'VirtualUser':CaptureController()
												game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
											end
											v.HumanoidRootPart.Size = Vector3.new(60,60,60)
											if _G.Configs["Show Hitbox"] then
												v.HumanoidRootPart.Transparency = _G.Hitbox_LocalTransparency
											else
												v.HumanoidRootPart.Transparency = 1
											end
											v.Humanoid.JumpPower = 0
											v.Humanoid.WalkSpeed = 0
											v.HumanoidRootPart.CanCollide = false
											v.Humanoid:ChangeState(11)
											topos(v.HumanoidRootPart.CFrame * CFrame.new(0,50,0))
										until v.Humanoid.Health <= 0 or not v.Parent or Auto_Quest_Yama_3 == false
									end
								end
							end
						else
							wait(5)
							topos(game:GetService("Workspace").Map.HellDimension.Torch1.CFrame)
							wait(1.5)
							game:GetService("VirtualInputManager"):SendKeyEvent(true, "E", false, game)
							wait(1.5)        
							topos(game:GetService("Workspace").Map.HellDimension.Torch2.CFrame)
							wait(1.5)
							game:GetService("VirtualInputManager"):SendKeyEvent(true, "E", false, game)
							wait(1.5)     
							topos(game:GetService("Workspace").Map.HellDimension.Torch3.CFrame)
							wait(1.5)
							game:GetService("VirtualInputManager"):SendKeyEvent(true, "E", false, game)
							wait(1.5)     
							topos(game:GetService("Workspace").Map.HellDimension.Exit.CFrame)
						end
					until Auto_Cursed_Dual_Katana == false or Auto_Quest_Yama_3 == false or GetMaterial("Alucard Fragment") == 3
				else
					if game:GetService("Workspace").Enemies:FindFirstChild("Soul Reaper [Lv. 2100] [Raid Boss]") or game.ReplicatedStorage:FindFirstChild("Soul Reaper [Lv. 2100] [Raid Boss]") then
						if game:GetService("Workspace").Enemies:FindFirstChild("Soul Reaper [Lv. 2100] [Raid Boss]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Soul Reaper [Lv. 2100] [Raid Boss]" then
									if v.Humanoid.Health > 0 then
										repeat wait()
											topos(v.HumanoidRootPart.CFrame * CFrame.new(0,0,-2))
										until Auto_Cursed_Dual_Katana == false or Auto_Quest_Yama_3 == false or game:GetService("Workspace").Map:FindFirstChild("HellDimension")
									end
								end
							end
						else
							topos(CFrame.new(-9570.033203125, 315.9346923828125, 6726.89306640625))
						end
					else
						_G.Auto_Bone = true
						game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Bones","Buy",1,1)
					end
				end
			end)
		end
	end
end)

spawn(function() 
	while wait() do
		if Auto_Quest_Tushita_1 then
			game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CDKQuest","BoatQuest",workspace.NPCs:FindFirstChild("Luxury Boat Dealer"))
		end
	end
end)

spawn(function()
	while wait() do
		if Auto_Quest_Tushita_1 then
			topos(CFrame.new(-9546.990234375, 21.139892578125, 4686.1142578125))
			wait(5)
			topos(CFrame.new(-6120.0576171875, 16.455780029296875, -2250.697265625))
			wait(5)
			topos(CFrame.new(-9533.2392578125, 7.254445552825928, -8372.69921875))    
		end
	end
end)

spawn(function()
	while wait() do
		if Auto_Quest_Tushita_2 then
			pcall(function()
				if (CFrame.new(-5539.3115234375, 313.800537109375, -2972.372314453125).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 500 then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if Auto_Quest_Tushita_2 and v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
							if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 2000 then
								repeat wait()
									v.HumanoidRootPart.CanCollide = false
									v.HumanoidRootPart.Size = Vector3.new(60, 60, 60)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(0,50,0))
									game:GetService'VirtualUser':CaptureController()
									game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
								until v.Humanoid.Health <= 0 or not v.Parent or Auto_Quest_Tushita_2 == false
							end
						end
					end
				else
					topos(CFrame.new(-5545.1240234375, 313.800537109375, -2976.616455078125))
				end
			end)
		end
	end
end)

spawn(function()
	while wait() do
		if Auto_Quest_Tushita_3 then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Cake Queen [Lv. 2175] [Boss]") or game.ReplicatedStorage:FindFirstChild("Cake Queen [Lv. 2175] [Boss]") then
					if game:GetService("Workspace").Enemies:FindFirstChild("Cake Queen [Lv. 2175] [Boss]") then
						for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
							if v.Name == "Cake Queen [Lv. 2175] [Boss]" then
								if v.Humanoid.Health > 0 then
									repeat wait()
										if Auto_Buso then
											if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
												game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
											end
										end
										if not game.Players.LocalPlayer.Character:FindFirstChild(_G.Select_Weapon) then
											wait()
											EquipWeapon(_G.SelectWeapon)
										end
										if not FastAttack then
											game:GetService'VirtualUser':CaptureController()
											game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
										end
										v.HumanoidRootPart.Size = Vector3.new(60,60,60)
										if _G.Configs["Show Hitbox"] then
											v.HumanoidRootPart.Transparency = _G.Hitbox_LocalTransparency
										else
											v.HumanoidRootPart.Transparency = 1
										end
										v.Humanoid.JumpPower = 0
										v.Humanoid.WalkSpeed = 0
										v.HumanoidRootPart.CanCollide = false
										v.Humanoid:ChangeState(11)
										topos(v.HumanoidRootPart.CFrame * CFrame.new(0,50,0))
									until Auto_Cursed_Dual_Katana == false or Auto_Quest_Tushita_3 == false or game:GetService("Workspace").Map:FindFirstChild("HeavenlyDimension")
								end
							end
						end
					else
						topos(CFrame.new(-709.3132934570312, 381.6005859375, -11011.396484375))
					end
				elseif game:GetService("Workspace").Map:FindFirstChild("HeavenlyDimension") then
					repeat wait()
						if game:GetService("Workspace").Enemies:FindFirstChild("Cursed Skeleton [Lv. 2200]") or game:GetService("Workspace").Enemies:FindFirstChild("Cursed Skeleton [Lv. 2200] [Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("Heaven's Guardian [Lv. 2200] [Boss]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Cursed Skeleton [Lv. 2200]" or v.Name == "Cursed Skeleton [Lv. 2200] [Boss]" or v.Name == "Heaven's Guardian [Lv. 2200] [Boss]" then
									if v.Humanoid.Health > 0 then
										repeat wait()
											if Auto_Buso then
												if not game.Players.LocalPlayer.Character:FindFirstChild("HasBuso") then
													game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
												end
											end
											if not game.Players.LocalPlayer.Character:FindFirstChild(_G.Select_Weapon) then
												wait()
												EquipWeapon(_G.Select_Weapon)
											end
											if not FastAttack then
												game:GetService'VirtualUser':CaptureController()
												game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
											end
											v.HumanoidRootPart.Size = Vector3.new(60,60,60)
											if _G.Configs["Show Hitbox"] then
												v.HumanoidRootPart.Transparency = _G.Hitbox_LocalTransparency
											else
												v.HumanoidRootPart.Transparency = 1
											end
											v.Humanoid.JumpPower = 0
											v.Humanoid.WalkSpeed = 0
											v.HumanoidRootPart.CanCollide = false
											v.Humanoid:ChangeState(11)
										until v.Humanoid.Health <= 0 or not v.Parent or Auto_Quest_Tushita_3 == false
									end
								end
							end
						else
							wait(5)
							topos(game:GetService("Workspace").Map.HeavenlyDimension.Torch1.CFrame)
							wait(1.5)
							game:GetService("VirtualInputManager"):SendKeyEvent(true, "E", false, game)
							wait(1.5)        
							topos(game:GetService("Workspace").Map.HeavenlyDimension.Torch2.CFrame)
							wait(1.5)
							game:GetService("VirtualInputManager"):SendKeyEvent(true, "E", false, game)
							wait(1.5)     
							topos(game:GetService("Workspace").Map.HeavenlyDimension.Torch3.CFrame)
							wait(1.5)
							game:GetService("VirtualInputManager"):SendKeyEvent(true, "E", false, game)
							wait(1.5)     
							topos(game:GetService("Workspace").Map.HeavenlyDimension.Exit.CFrame)
						end
					until Auto_Cursed_Dual_Katana == false or Auto_Quest_Tushita_3 == false or GetMaterial("Alucard Fragment") == 6
				else
					hop()
				end
			end)
		end
	end
end)
Auto3:AddToggle{
    Name = "Auto Tushita",
    Flag = "Auto Tushita",
    Value =  _G.Autotushita,
    Callback  = function(value)
        _G.Autotushita = value
        StopTween(_G.Autotushita)
    end
}

spawn(function()
	while wait() do
		if  _G.Autotushita and World3 then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Longma [Lv. 2000] [Boss]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "Longma [Lv. 2000] [Boss]" then
							if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								repeat task.wait()
									AutoHaki()
									EquipWeapon(_G.SelectWeapon)
									v.HumanoidRootPart.CanCollide = false
									v.Humanoid.WalkSpeed = 0
									v.HumanoidRootPart.Size = Vector3.new(50,50,50)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
									game:GetService("VirtualUser"):CaptureController()
									game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
									sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
								until not  _G.Autotushita or not v.Parent or v.Humanoid.Health <= 0
							end
						end
					end
				else
				topos(CFrame.new(-10238.875976563, 389.7912902832, -9549.7939453125))
					if game:GetService("ReplicatedStorage"):FindFirstChild("Longma [Lv. 2000] [Boss]") then
						topos(game:GetService("ReplicatedStorage"):FindFirstChild("Longma [Lv. 2000] [Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
					else 
						if  _G.Autotushitahop then
							Hop()
						end
					end
				end
			end)
		end
	end
end)

Auto3:AddToggle{
	Name = "Auto Cavander",
	Flag = "Auto Cavander",
	Value = _G.AutoCarvender,
	Callback  = function(value)
		_G.AutoCarvender = value
        StopTween(_G.AutoCarvender)
	end
}

spawn(function()
	while wait() do
		if  _G.AutoCarvender and World3 then
			pcall(function()
				if game:GetService("Workspace").Enemies:FindFirstChild("Beautiful Pirate [Lv. 1950] [Boss]") then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if v.Name == "Beautiful Pirate [Lv. 1950] [Boss]" then
							if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
								repeat task.wait()
									AutoHaki()
									EquipWeapon(_G.SelectWeapon)
									v.HumanoidRootPart.CanCollide = false
									v.Humanoid.WalkSpeed = 0
									v.HumanoidRootPart.Size = Vector3.new(50,50,50)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
									game:GetService("VirtualUser"):CaptureController()
									game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
									sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
								until not  _G.AutoCarvender or not v.Parent or v.Humanoid.Health <= 0
							end
						end
					end
				else
				topos(CFrame.new(5311.07421875, 426.0243835449219, 165.12762451171875))
					if game:GetService("ReplicatedStorage"):FindFirstChild("Beautiful Pirate [Lv. 1950] [Boss]") then
						topos(game:GetService("ReplicatedStorage"):FindFirstChild("Beautiful Pirate [Lv. 1950] [Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
					else
						if  _G.AutoCavanderhop then
							Hop()
						end
					end
				end
			end)
		end
	end
end)
Auto3:AddToggle{
	Name = "Auto Pirate Raid",
	Flag = "Auto Pirate Raid",
	Value = _G.AutoRaidPirate,
	Callback  = function(value)
		_G.AutoRaidPirate = value
		StopTween(_G.AutoRaidPirate)
	end
}

spawn(function()
	while wait() do
		if _G.AutoRaidPirate then
			pcall(function()
				local CFrameBoss = CFrame.new(-5496.17432, 313.768921, -2841.53027, 0.924894512, 7.37058015e-09, 0.380223751, 3.5881019e-08, 1, -1.06665446e-07, -0.380223751, 1.12297109e-07, 0.924894512)
				if (CFrame.new(-5539.3115234375, 313.800537109375, -2972.372314453125).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 500 then
					for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
						if _G.AutoRaidPirate and v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
							if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 2000 then
								repeat wait()
									AutoHaki()
									EquipWeapon(_G.SelectWeapon)
									FastA()
									Fastattack = true
									MakoriGayMag = true
									v.HumanoidRootPart.CanCollide = false
									v.HumanoidRootPart.Size = Vector3.new(60, 60, 60)
									topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
								until v.Humanoid.Health <= 0 or not v.Parent or _G.AutoRaidPirate == false
								Fastattack = false
								MakoriGayMag = false
							end
						end
					end
				else
					if ((CFrameBoss).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude <= 1500 then
						topos(CFrameBoss)
					else
						TelePBoss(CFrameBoss)
					end
				end
			end)
		end
	end
    end)
	Auto3:AddToggle{
		Name = "Auto Twin Hook",
		Flag = "Auto Twin Hook",
		Value = _G.AutoTwinHook,
		Callback  = function(value)
			_G.AutoTwinHook = value
			StopTween( _G.AutoTwinHook)
		end
	}  
 
	spawn(function()
        while wait() do
            if  _G.AutoTwinHook and World3 then
                pcall(function()
                    if game:GetService("Workspace").Enemies:FindFirstChild("Captain Elephant [Lv. 1875] [Boss]") then
                        for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                            if v.Name == "Captain Elephant [Lv. 1875] [Boss]" then
                                if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
                                    repeat task.wait()
                                        AutoHaki()
                                        EquipWeapon(_G.SelectWeapon)
                                        v.HumanoidRootPart.CanCollide = false
                                        v.Humanoid.WalkSpeed = 0
                                        v.HumanoidRootPart.Size = Vector3.new(50,50,50)
                                        topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
                                        game:GetService("VirtualUser"):CaptureController()
                                        game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
                                        sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
                                    until not  _G.AutoTwinHook or not v.Parent or v.Humanoid.Health <= 0
                                end
                            end
                        end
                    else
                    topos(CFrame.new(-13348.0654296875, 405.8904113769531, -8570.62890625))
                        if game:GetService("ReplicatedStorage"):FindFirstChild("Captain Elephant [Lv. 1875] [Boss]") then
                            topos(game:GetService("ReplicatedStorage"):FindFirstChild("Captain Elephant [Lv. 1875] [Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
                        else
                            if  _G.AutoTwinHook_Hop then
                                Hop()
                            end
                        end
                    end
                end)
            end
        end
    end)

	Auto3:AddToggle{
		Name = "Auto FarmBossHallow",
		Flag = "Auto FarmBossHallow",
		Value = _G.AutoFarmBossHallow,
		Callback  = function(value)
			_G.AutoFarmBossHallow = value
		end
	} 

    
    spawn(function()
        while wait() do
            if _G.AutoFarmBossHallow then
                pcall(function()
                    if game:GetService("Workspace").Enemies:FindFirstChild("Soul Reaper [Lv. 2100] [Raid Boss]") then
                        for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                            if string.find(v.Name , "Soul Reaper") then
                                repeat task.wait()
                                    EquipWeapon(_G.SelectWeapon)
                                    AutoHaki()
                                    v.HumanoidRootPart.Size = Vector3.new(50,50,50)
                                    topos(v.HumanoidRootPart.CFrame*CFrame.new(2,40,2))
                                    game:GetService("VirtualUser"):CaptureController()
                                    game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 670))
                                    v.HumanoidRootPart.Transparency = 1
                                    sethiddenproperty(game.Players.LocalPlayer,"SimulationRadius",math.huge)
                                until v.Humanoid.Health <= 0 or _G.AutoFarmBossHallow == false
                            end
                        end
                    elseif game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Hallow Essence") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Hallow Essence") then
                        repeat topos(CFrame.new(-8932.322265625, 146.83154296875, 6062.55078125)) wait() until (CFrame.new(-8932.322265625, 146.83154296875, 6062.55078125).Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 8                        
                        EquipWeapon("Hallow Essence")
                    else
                        if game:GetService("ReplicatedStorage"):FindFirstChild("Soul Reaper [Lv. 2100] [Raid Boss]") then
                            topos(game:GetService("ReplicatedStorage"):FindFirstChild("Soul Reaper [Lv. 2100] [Raid Boss]").HumanoidRootPart.CFrame * CFrame.new(2,20,2))
                        else
                            if _G.AutoFarmBossHallowHop then
                                Hop()
                            end
                        end
                    end
                end)
            end
        end
    end)

	Auto3:AddToggle{
		Name = "Auto DarkDagger",
		Flag = "Auto DarkDagger",
		Value =  _G.AutoDarkDagger,
		Callback  = function(value)
			_G.AutoDarkDagger = value
		end
	} 
        
    spawn(function()
        pcall(function()
            while wait() do
                if _G.AutoDarkDagger then
                    if game:GetService("Workspace").Enemies:FindFirstChild("rip_indra True Form [Lv. 5000] [Raid Boss]") or game:GetService("Workspace").Enemies:FindFirstChild("rip_indra [Lv. 5000] [Raid Boss]") then
                        for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                            if v.Name == ("rip_indra True Form [Lv. 5000] [Raid Boss]" or v.Name == "rip_indra [Lv. 5000] [Raid Boss]") and v.Humanoid.Health > 0 and v:IsA("Model") and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") then
                                repeat task.wait()
                                    pcall(function()
                                        AutoHaki()
                                        EquipWeapon(_G.SelectWeapon)
                                        v.HumanoidRootPart.CanCollide = false
                                        v.HumanoidRootPart.Size = Vector3.new(50,50,50)
                                        topos(v.HumanoidRootPart.CFrame * CFrame.new(2,40,2))
                                        game:GetService("VirtualUser"):CaptureController()
                                        game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 670),workspace.CurrentCamera.CFrame)
                                    end)
                                until _G.AutoDarkDagger == false or v.Humanoid.Health <= 0
                            end
                        end
                    else
                        topos(CFrame.new(-5344.822265625, 423.98541259766, -2725.0930175781))
                    end
                end
            end
        end)
    end)
    
    
    spawn(function()
        pcall(function()
            while wait() do
                if (_G.AutoDarkDagger_Hop and _G.AutoDarkDagger) and World3 and not game:GetService("ReplicatedStorage"):FindFirstChild("rip_indra True Form [Lv. 5000] [Raid Boss]") and not game:GetService("Workspace").Enemies:FindFirstChild("rip_indra True Form [Lv. 5000] [Raid Boss]") then
                    Hop()
                end
            end
        end)
    end)
	
	local Auto4 = Auto:CreateSection({
		Name = "[ Other ] ", 
		Side = "Right"
	})    
 
	Auto4:AddToggle{
		Name = "Auto SeaBest",
		Flag = "Auto SeaBest",
		Value = _G.AutoSeaBest,
		Callback  = function(value)
			_G.AutoSeaBest = value
		end
	} 
 
    spawn(function()
        while wait() do
            if _G.AutoSeaBest then
                   pcall(function()
                     for i,v in pairs(game:GetService("Workspace").SeaBeasts:GetChildren()) do
                         if v:FindFirstChild("HumanoidRootPart") then
                            AutoHaki()
                            EquipWeapon(_G.SelectWeapon)
                            TP(v.HumanoidRootPart.CFrame * CFrame.new(5,500,7))
                            game:GetService("VirtualUser"):CaptureController()
                            game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
                       else
                           if _G.AutoSeaBestHop then
                             Hop()
							    end
							end
                         end
                    end)
                end
            end
        end)
		Auto4:AddToggle{
			Name = "Auto Holy Torch",
			Flag = "AutoHolyTorch",
			Value =  _G.AutoHolyTorch,
			Callback  = function(value)
				_G.AutoHolyTorch = value
				StopTween(_G.AutoHolyTorch)
			end
		}
		spawn(function()
			while wait() do
				if _G.AutoHolyTorch then
					pcall(function()
						wait(1)
						TP(CFrame.new(-10752, 417, -9366))
						wait(1)
						TP(CFrame.new(-11672, 334, -9474))
						wait(1)
						TP(CFrame.new(-12132, 521, -10655))
						wait(1)
						TP(CFrame.new(-13336, 486, -6985))
						wait(1)
						TP(CFrame.new(-13489, 332, -7925))
					end)
				end
			end
		end)
			
	Auto4:AddToggle{
		Name = "Auto Yama [EliteHunter 30]",
		Flag = "Auto_Yama",
		Value =  _G.AutoYama,
		Callback  = function(value)
			_G.AutoYama = value
			StopTween(_G.AutoYama)
		end
	}
	spawn(function()
		while wait() do
			if _G.AutoYama then
				if game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("EliteHunter","Progress") >= 30 then
					repeat wait(.1)
						fireclickdetector(game:GetService("Workspace").Map.Waterfall.SealedKatana.Handle.ClickDetector)
					until game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Yama") or not _G.AutoYama
				end
			end
		end
	end)
	Auto4:AddToggle{
		Name = "Auto Musketeer Hat",
		Flag = "Auto_Musketeer_Hat",
		Value = _G.AutoMusketeerHat,
		Callback  = function(value)
			_G.AutoMusketeerHat = value
			StopTween(_G.AutoMusketeerHat)
		end
	}
	spawn(function()
		pcall(function()
			while wait() do
				if _G.AutoMusketeerHat then
					if game:GetService("Players").LocalPlayer.Data.Level.Value >= 1800 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress").KilledBandits == false then
						if string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Forest Pirate") and string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "50") and game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true then
							if game:GetService("Workspace").Enemies:FindFirstChild("Forest Pirate [Lv. 1825]") then
								for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
									if v.Name == "Forest Pirate [Lv. 1825]" then
										repeat task.wait()
											pcall(function()
												EquipWeapon(_G.SelectWeapon)
												AutoHaki()
												topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
												v.HumanoidRootPart.CanCollide = false
												game:GetService'VirtualUser':CaptureController()
												game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
												MusketeerHatMon = v.HumanoidRootPart.CFrame
												StartMagnetMusketeerhat = true
											end)
										until _G.AutoMusketeerHat == false or not v.Parent or v.Humanoid.Health <= 0 or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
										StartMagnetMusketeerhat = false
									end
								end
							else
								StartMagnetMusketeerhat = false
								topos(CFrame.new(-13206.452148438, 425.89199829102, -7964.5537109375))
							end
						else
							topos(CFrame.new(-12443.8671875, 332.40396118164, -7675.4892578125))
							if (Vector3.new(-12443.8671875, 332.40396118164, -7675.4892578125) - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 30 then
								wait(1.5)
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest","CitizenQuest",1)
							end
						end
					elseif game:GetService("Players").LocalPlayer.Data.Level.Value >= 1800 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress").KilledBoss == false then
						if game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible and string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Captain Elephant") and game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true then
							if game:GetService("Workspace").Enemies:FindFirstChild("Captain Elephant [Lv. 1875] [Boss]") then
								for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
									if v.Name == "Captain Elephant [Lv. 1875] [Boss]" then
										OldCFrameElephant = v.HumanoidRootPart.CFrame
										repeat task.wait()
											pcall(function()
												EquipWeapon(_G.SelectWeapon)
												AutoHaki()
												v.HumanoidRootPart.CanCollide = false
												topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
												v.HumanoidRootPart.CanCollide = false
												v.HumanoidRootPart.CFrame = OldCFrameElephant
												game:GetService("VirtualUser"):CaptureController()
												game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
												sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
											end)
										until _G.AutoMusketeerHat == false or v.Humanoid.Health <= 0 or not v.Parent or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
									end
								end
							else
								topos(CFrame.new(-13374.889648438, 421.27752685547, -8225.208984375))
							end
						else
							topos(CFrame.new(-12443.8671875, 332.40396118164, -7675.4892578125))
							if (CFrame.new(-12443.8671875, 332.40396118164, -7675.4892578125).Position - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 4 then
								wait(1.5)
								game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen")
							end
						end
					elseif game:GetService("Players").LocalPlayer.Data.Level.Value >= 1800 and game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("CitizenQuestProgress","Citizen") == 2 then
						topos(CFrame.new(-12512.138671875, 340.39279174805, -9872.8203125))
					end
				end
			end
		end)
	end)
	Auto4:AddToggle{
		Name = "Auto Rainbow Haki",
		Flag = "Auto_Rainbow_Haki",
		Value =  _G.Auto_Rainbow_Haki,
		Callback  = function(value)
			_G.Auto_Rainbow_Haki = value
			StopTween(_G.Auto_Rainbow_Haki)
		end
	}
	spawn(function()
		pcall(function()
			while wait() do
				if _G.Auto_Rainbow_Haki then
					if game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false then
						topos(CFrame.new(-11892.0703125, 930.57672119141, -8760.1591796875))
						if (Vector3.new(-11892.0703125, 930.57672119141, -8760.1591796875) - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 30 then
							wait(1.5)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("HornedMan","Bet")
						end
					elseif game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true and string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Stone") then
						if game:GetService("Workspace").Enemies:FindFirstChild("Stone [Lv. 1550] [Boss]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Stone [Lv. 1550] [Boss]" then
									OldCFrameRainbow = v.HumanoidRootPart.CFrame
									repeat task.wait()
										EquipWeapon(_G.SelectWeapon)
										topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
										v.HumanoidRootPart.CanCollide = false
										v.HumanoidRootPart.CFrame = OldCFrameRainbow
										game:GetService("VirtualUser"):CaptureController()
										game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
										sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
									until _G.Auto_Rainbow_Haki == false or v.Humanoid.Health <= 0 or not v.Parent or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
								end
							end
						else
							TP(CFrame.new(-1086.11621, 38.8425903, 6768.71436, 0.0231462717, -0.592676699, 0.805107772, 2.03251839e-05, 0.805323839, 0.592835128, -0.999732077, -0.0137055516, 0.0186523199))
						end
					elseif game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true and string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Island Empress") then
						if game:GetService("Workspace").Enemies:FindFirstChild("Island Empress [Lv. 1675] [Boss]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Island Empress [Lv. 1675] [Boss]" then
									OldCFrameRainbow = v.HumanoidRootPart.CFrame
									repeat task.wait()
										EquipWeapon(_G.SelectWeapon)
										topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
										v.HumanoidRootPart.CanCollide = false
										v.HumanoidRootPart.CFrame = OldCFrameRainbow
										game:GetService("VirtualUser"):CaptureController()
										game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
										sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
									until _G.Auto_Rainbow_Haki == false or v.Humanoid.Health <= 0 or not v.Parent or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
								end
							end
						else
							TP(CFrame.new(5713.98877, 601.922974, 202.751251, -0.101080291, -0, -0.994878292, -0, 1, -0, 0.994878292, 0, -0.101080291))
						end
					elseif string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Kilo Admiral") then
						if game:GetService("Workspace").Enemies:FindFirstChild("Kilo Admiral [Lv. 1750] [Boss]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Kilo Admiral [Lv. 1750] [Boss]" then
									OldCFrameRainbow = v.HumanoidRootPart.CFrame
									repeat task.wait()
										EquipWeapon(_G.SelectWeapon)
										topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
										v.HumanoidRootPart.CanCollide = false
										v.HumanoidRootPart.CFrame = OldCFrameRainbow
										game:GetService("VirtualUser"):CaptureController()
										game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
										sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
									until _G.Auto_Rainbow_Haki == false or v.Humanoid.Health <= 0 or not v.Parent or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
								end
							end
						else
							TP(CFrame.new(2877.61743, 423.558685, -7207.31006, -0.989591599, -0, -0.143904909, -0, 1.00000012, -0, 0.143904924, 0, -0.989591479))
						end
					elseif string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Captain Elephant") then
						if game:GetService("Workspace").Enemies:FindFirstChild("Captain Elephant [Lv. 1875] [Boss]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Captain Elephant [Lv. 1875] [Boss]" then
									OldCFrameRainbow = v.HumanoidRootPart.CFrame
									repeat task.wait()
										EquipWeapon(_G.SelectWeapon)
										topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
										v.HumanoidRootPart.CanCollide = false
										v.HumanoidRootPart.CFrame = OldCFrameRainbow
										game:GetService("VirtualUser"):CaptureController()
										game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
										sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
									until _G.Auto_Rainbow_Haki == false or v.Humanoid.Health <= 0 or not v.Parent or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
								end
							end
						else
							TP(CFrame.new(-13485.0283, 331.709259, -8012.4873, 0.714521289, 7.98849911e-08, 0.69961375, -1.02065748e-07, 1, -9.94383065e-09, -0.69961375, -6.43015241e-08, 0.714521289))
						end
					elseif string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Beautiful Pirate") then
						if game:GetService("Workspace").Enemies:FindFirstChild("Beautiful Pirate [Lv. 1950] [Boss]") then
							for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
								if v.Name == "Beautiful Pirate [Lv. 1950] [Boss]" then
									OldCFrameRainbow = v.HumanoidRootPart.CFrame
									repeat task.wait()
										EquipWeapon(_G.SelectWeapon)
										topos(v.HumanoidRootPart.CFrame * CFrame.new(0,35,0))
										v.HumanoidRootPart.CanCollide = false
										v.HumanoidRootPart.CFrame = OldCFrameRainbow
										game:GetService("VirtualUser"):CaptureController()
										game:GetService("VirtualUser"):Button1Down(Vector2.new(1280, 672))
										sethiddenproperty(game:GetService("Players").LocalPlayer,"SimulationRadius",math.huge)
									until _G.Auto_Rainbow_Haki == false or v.Humanoid.Health <= 0 or not v.Parent or game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false
								end
							end
						else
							TP(CFrame.new(5312.3598632813, 20.141201019287, -10.158538818359))
						end
					else
						TP(CFrame.new(-11892.0703125, 930.57672119141, -8760.1591796875))
						if (Vector3.new(-11892.0703125, 930.57672119141, -8760.1591796875) - game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 30 then
							wait(1.5)
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("HornedMan","Bet")
						end
					end
				end
			end
		end)
	end)

		local HOP = Auto:CreateSection({
			Name = "[ Setting ]",
			Side = "Left"
		})
		HOP:AddToggle{
			Name = "Enabled Hop Server",
			Flag = "Allow Hop Server",
			Value =  _G.Hop,
			Callback  = function(value)
				_G.Hop = value
			end
		}
		spawn(function()
			while wait() do
				if _G.Hop then
					 Hop(5)
				 end
			 end
		 end)

		local Player = PVP:CreateSection({
			Name = "[ Player ]",
			Side = "Left"
		})

		local plyserv = Player:AddLabel({
			Name = "Players"
		})

    

    spawn(function()
        while wait() do
            pcall(function()
                for i,v in pairs(game:GetService("Players"):GetPlayers()) do
                    if i == 12 then
                        plyserv:Set("Players :".." "..i.." ".."/".." ".."12".." ".."(Max)")
                    elseif i == 1 then
                        plyserv:Set("Player :".." "..i.." ".."/".." ".."12")
                    else
                        plyserv:Set("Players :".." "..i.." ".."/".." ".."12")
                    end
                end
            end)
        end
    end)
    
    Playerslist = {}
    
    for i,v in pairs(game:GetService("Players"):GetChildren()) do
        table.insert(Playerslist,v.Name)
    end
	Player:AddDropdown{
		Name = "Select Player",
		Value = "",
		List = Playerslist,
		Callback = function(value)
			_G.SelectPly = value
		end
	}
	Player:AddButton{
		Name = "Refresh Player",
		Callback = function()
			Playerslist = {}
        SelectedPly:Clear()
        for i,v in pairs(game:GetService("Players"):GetChildren()) do  
            SelectedPly:Add(v.Name)
		end
	end
	}
	Player:AddToggle({
		Name = "Spectate Player",
		Flag = "Spectate Player",
		Value =  SpectatePlys,
		Callback  = function(value)
			SpectatePlys = value
			local plr1 = game:GetService("Players").LocalPlayer.Character.Humanoid
			local plr2 = game:GetService("Players"):FindFirstChild(_G.SelectPly)
			repeat wait(.1)
				game:GetService("Workspace").Camera.CameraSubject = game:GetService("Players"):FindFirstChild(_G.SelectPly).Character.Humanoid
			until SpectatePlys == false 
			game:GetService("Workspace").Camera.CameraSubject = game:GetService("Players").LocalPlayer.Character.Humanoid
		end
	})		

	Player:AddToggle({
		Name = "Teleport To Player",
		Flag = "Teleport To Player",
		Value =  _G.TeleportPly,
		Callback  = function(value)
			_G.TeleportPly = value
			pcall(function()
				if _G.TeleportPly then
					repeat TP(game:GetService("Players")[_G.SelectPly].Character.HumanoidRootPart.CFrame) wait() until _G.TeleportPly == false
				end
				StopTween(_G.TeleportPly)
			end)
		end
	})
	Player:AddToggle({
		Name = "Auto Farm Player",
		Flag = "Auto Farm Player",
		Value =  _G.Auto_Kill_Ply,
		Callback  = function(value)
			_G.Auto_Kill_Ply = value
			StopTween(_G.Auto_Kill_Ply)
		end
	})
			spawn(function()
				while wait() do
					if _G.Auto_Kill_Ply then
						pcall(function()
							if _G.SelectPly ~= nil then 
								if game.Players:FindFirstChild(_G.SelectPly) then
									if game.Players:FindFirstChild(_G.SelectPly).Character.Humanoid.Health > 0 then
										repeat task.wait()
											EquipWeapon(_G.SelectWeapon)
											AutoHaki()
											game.Players:FindFirstChild(_G.SelectPly).Character.HumanoidRootPart.CanCollide = false
											TP(game.Players:FindFirstChild(_G.SelectPly).Character.HumanoidRootPart.CFrame * CFrame.new(0,5,0))
											spawn(function()
												pcall(function()
													if _G.SelectWeapon == SelectWeaponGun then
														local args = {
															[1] = game.Players:FindFirstChild(_G.SelectPly).Character.HumanoidRootPart.Position,
															[2] = game.Players:FindFirstChild(_G.SelectPly).Character.HumanoidRootPart
														}
														game:GetService("Players").LocalPlayer.Character[SelectWeaponGun].RemoteFunctionShoot:InvokeServer(unpack(args))
													else
														game:GetService("VirtualUser"):CaptureController()
														game:GetService("VirtualUser"):Button1Down(Vector2.new(1280,672))
													end
												end)
											end)
										until game.Players:FindFirstChild(_G.SelectPly).Character.Humanoid.Health <= 0 or not game.Players:FindFirstChild(_G.SelectPly) or not _G.Auto_Kill_Ply
									end
								end
							end
						end)
					end
				end
			end)
		
	Player:AddButton({
		Name = "Get Quest Elite Players",
		Callback = function()
			game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("PlayerHunter")
		end
	})
	local RUOK = PVP:CreateSection({
		Name = "[ Aimbot ]",
		Side = "Left"
	})
	spawn(function()
        while wait() do
            pcall(function()
                local MaxDistance = math.huge
                for i,v in pairs(game:GetService("Players"):GetPlayers()) do
                    if v.Name ~= game:GetService("Players").LocalPlayer.Name then
                        local Distance = v:DistanceFromCharacter(game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position)
                        if Distance < MaxDistance then
                            MaxDistance = Distance
                            PlayerSelectAimbot = v.Name
                        end
                    end
                end
            end)
        end
    end)

	RUOK:AddToggle({
		Name = "Enabled Fov",
		Flag = "Teleport To Player",
		Value =  ShowFov,
		Callback  = function(value)
			ShowFov = value
			end
		})
		RUOK:AddSlider({
			Name = "FOVSize",
			Value = 200, 
			Min = 1,
			Max = 700,
			Format = "FOVSize : %s%%",
			Callback = function(value)
				_G.FOVSize = value
			end
		})
		local FOVCircle = Drawing.new("Circle")
		FOVCircle.Thickness = 5
		FOVCircle.NumSides = 460
		FOVCircle.Filled = false
		FOVCircle.Transparency = 0.5
		FOVCircle.Radius = 200
		FOVCircle.Color = Color3.fromRGB(255, 255, 255)
		
		game:GetService("RunService").Stepped:Connect(function()
			FOVCircle.Radius = _G.FOVSize
			FOVCircle.Thickness = 1
			FOVCircle.NumSides = 11
			FOVCircle.Position = game:GetService('UserInputService'):GetMouseLocation()
			if ShowFov then
				FOVCircle.Visible = true
			else
				FOVCircle.Visible = false
			end
		end)
		
		local lp = game:GetService('Players').LocalPlayer
		local mouse = lp:GetMouse()
		spawn(function()
			while wait() do
				if AimBotFullFunction then
					pcall(function()
						local MaxDist, Closest = math.huge
						for i,v in pairs(game:GetService("Players"):GetChildren()) do 
							local Head = v.Character:FindFirstChild("HumanoidRootPart")
							local Pos, Vis = game.Workspace.CurrentCamera.WorldToScreenPoint(game.Workspace.CurrentCamera, Head.Position)
							local MousePos, TheirPos = Vector2.new(mouse.X, mouse.Y), Vector2.new(Pos.X, Pos.Y)
							local Dist = (TheirPos - MousePos).Magnitude
							if Dist < MaxDist and Dist <= _G.FOVSize and v.Name ~= game.Players.LocalPlayer.Name then
								MaxDist = Dist
								_G.CharacterAimBot = v
							end
						end
					end)
				end
			end
		end)
		
		spawn(function()
			local gg = getrawmetatable(game)
			local old = gg.__namecall
			setreadonly(gg,false)
			gg.__namecall = newcclosure(function(...)
				local method = getnamecallmethod()
				local args = {...}
				if tostring(method) == "FireServer" then
					if tostring(args[1]) == "RemoteEvent" then
						if tostring(args[2]) ~= "true" and tostring(args[2]) ~= "false" then
							if AimBotFullFunction then
								args[2] = _G.CharacterAimBot.Character.HumanoidRootPart.Position
								return old(unpack(args))
							end
						end
					end
				end
				return old(...)
			end)
		end)
		spawn(function()
			while wait() do
				for i,v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do  
					if v:IsA("Tool") then
						if v:FindFirstChild("RemoteFunctionShoot") then 
							SelectToolWeaponGun = v.Name
						end
					end
				end
				for i,v in pairs(game.Players.LocalPlayer.Character:GetChildren()) do  
					if v:IsA("Tool") then
						if v:FindFirstChild("RemoteFunctionShoot") then 
							SelectToolWeaponGun = v.Name
						end
					end
				end
			end
		end)
		
		spawn(function()
			mouse.Button1Down:Connect(function()
				if SelectToolWeaponGun ~= nil then
					if AimBotFullFunction and game.Players.LocalPlayer.Character:FindFirstChild(SelectToolWeaponGun) and game:GetService("Players"):FindFirstChild(_G.CharacterAimBot.Name) then
						tool = game:GetService("Players").LocalPlayer.Character[SelectToolWeaponGun]
						v17 = workspace:FindPartOnRayWithIgnoreList(Ray.new(tool.Handle.CFrame.p, (game:GetService("Players"):FindFirstChild(_G.CharacterAimBot.Name).Character.HumanoidRootPart.Position - tool.Handle.CFrame.p).unit * 100), { game.Players.LocalPlayer.Character, workspace._WorldOrigin });
						game:GetService("Players").LocalPlayer.Character[SelectToolWeaponGun].RemoteFunctionShoot:InvokeServer(game:GetService("Players"):FindFirstChild(_G.CharacterAimBot.Name).Character.HumanoidRootPart.Position, (require(game.ReplicatedStorage.Util).Other.hrpFromPart(v17)));
					end 
				end
			end)
		end)

		spawn(function()
			while wait() do
				pcall(function()
					local MaxDistance = math.huge
					for i,v in pairs(game:GetService("Players"):GetPlayers()) do
						if v.Name ~= game:GetService("Players").LocalPlayer.Name then
							local Distance = v:DistanceFromCharacter(game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position)
							if Distance < MaxDistance then
								MaxDistance = Distance
								PlayerSelectAimbot = v.Name
							end
						end
					end
				end)
			end
		end)
		RUOK:AddToggle({
			Name = "Enabled Aimbot [Gun]",
			Flag = "Teleport To Player",
			Value =  _G.Aimbot_Gun,
			Callback  = function(value)
				_G.Aimbot_Gun = value
				end
			})
    spawn(function()
        while task.wait() do
            if _G.Aimbot_Gun and game:GetService("Players").LocalPlayer.Character:FindFirstChild(SelectWeaponGun) then
                pcall(function()
                    game:GetService("Players").LocalPlayer.Character[SelectWeaponGun].Cooldown.Value = 0
                    local args = {
                        [1] = game:GetService("Players"):FindFirstChild(PlayerSelectAimbot).Character.HumanoidRootPart.Position,
                        [2] = game:GetService("Players"):FindFirstChild(PlayerSelectAimbot).Character.HumanoidRootPart
                    }
                    game:GetService("Players").LocalPlayer.Character[SelectWeaponGun].RemoteFunctionShoot:InvokeServer(unpack(args))
                    game:GetService'VirtualUser':CaptureController()
                    game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
                end)
            end
        end
    end)
			RUOK:AddToggle({
				Name = "Enabled Aimbot [Skill]",
				Flag = "Teleport To Player",
				Value =  _G.Aimbot_Skill,
				Callback  = function(value)
					_G.Aimbot_Skill = value
					end
				})
				spawn(function()
					pcall(function()
						while task.wait() do
							if _G.Aimbot_Skill and PlayerSelectAimbot ~= nil and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool") and game.Players.LocalPlayer.Character[game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool").Name]:FindFirstChild("MousePos") then
								local args = {
									[1] = game:GetService("Players"):FindFirstChild(PlayerSelectAimbot).Character.HumanoidRootPart.Position
								}
								
								game:GetService("Players").LocalPlayer.Character:FindFirstChild(game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool").Name).RemoteEvent:FireServer(unpack(args))
							end
						end
					end)
				end)

				RUOK:AddToggle({
					Name = "Enabled PvP",
					Flag = "Teleport To Player",
					Value =  _G.EnabledPvP,
					Callback  = function(value)
						_G.EnabledPvP = value
						end
					})
					spawn(function()
						pcall(function()
							while wait(.1) do
								if _G.EnabledPvP then
									if game:GetService("Players").LocalPlayer.PlayerGui.Main.PvpDisabled.Visible == true then
										game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("EnablePvp")
									end
								end
							end
						end)
					end)
					RUOK:AddToggle({
						Name = "Safe Mode",
						Flag = "Teleport To Player",
						Value =  _G.Safe_Mode,
						Callback  = function(value)
							_G.Safe_Mode = value
							StopTween(_G.Safe_Mode)
							end
						})	
					spawn(function()
						pcall(function()
							while wait() do
								if _G.Safe_Mode then
									game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame
								end
							end
						end)
					end)
					RUOK:AddButton({
						Name = "Respawn",
						Callback = function()
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetTeam","Pirates") 
							wait()	
						end
					})


					
local Current = RUOK:AddLabel({
    Name = "Current Bounties :"
})
					local Bounty = tostring(game:GetService("Players").LocalPlayer.leaderstats["Bounty/Honor"].Value)
					local sub = string.sub 
					local len = string.len
					spawn(function()
						while wait() do
							pcall(function()
								if len(Bounty) == 4 then
									Bounty1 = sub(Bounty,1,1).."."..sub(Bounty,2,3).."K"
								elseif len(Bounty) == 5 then
									Bounty1 = sub(Bounty,1,2).."."..sub(Bounty,3,4).."K"
								elseif len(Bounty) == 6 then
									Bounty1 = sub(Bounty,1,3).."."..sub(Bounty,4,5).."K"
								elseif len(Bounty) == 7 then
									Bounty1 = sub(Bounty,1,1).."."..sub(Bounty,2,3).."M"
								elseif len(Bounty) == 8 then
									Bounty1 = sub(Bounty,1,2).."."..sub(Bounty,3,4).."M"
								elseif len(Bounty) <= 3 then
									Bounty1 = Bounty
								end
								if tonumber(Bounty) == 25000000 then
									Current:Set("Current Bounties : "..Bounty1.." [ Max ]")
								elseif tonumber(Bounty) < 25000000 then
									Current:Set("Current Bounties : "..Bounty1)
								end
							end)
						end
					end)
					local Current = RUOK:AddLabel({
						Name = "Earned"
					})
					local OldBounty = game:GetService("Players").LocalPlayer.leaderstats["Bounty/Honor"].Value
    local Bounty = tostring(game:GetService("Players").LocalPlayer.leaderstats["Bounty/Honor"].Value)
    local Earned = tostring(game:GetService("Players").LocalPlayer.leaderstats["Bounty/Honor"].Value - OldBounty)
    local sub = string.sub 
    local len = string.len
    spawn(function()
        while wait() do
            pcall(function()
                if len(Earned) == 4 then
                    Earned1 = sub(Earned,1,1).."."..sub(Earned,2,3).."K"
                elseif len(Earned) == 5 then
                    Earned1 = sub(Earned,1,2).."."..sub(Earned,3,4).."K"
                elseif len(Earned) == 6 then
                    Earned1 = sub(Earned,1,3).."."..sub(Earned,4,5).."K"
                elseif len(Earned) == 7 then
                    Earned1 = sub(Earned,1,1).."."..sub(Earned,2,3).."M"
                elseif len(Earned) == 8 then
                    Earned1 = sub(Earned,1,2).."."..sub(Earned,3,4).."M"
                elseif len(Earned) <= 3 then
                    Earned1 = Earned
                end
                Earn:Set("Earned : "..tonumber(Earned1))
            end)
        end
    end)
	RUOK:AddToggle({
		Name = "Auto Farm Bounty [NotWork]",
		Flag = "Teleport To Player",
		Value =  _G.AutoFarmBounty,
		Callback  = function(value)
			_G.AutoFarmBounty = value
			StopTween(_G.AutoFarmBounty)
			end
		})
		RUOK:AddToggle({
			Name = "Auto Farm Bounty Hop [NotWork]",
			Flag = "Teleport To Player",
			Value =  _G.AutoFarmBounty_Hop,
			Callback  = function(value)
				_G.AutoFarmBounty_Hop = value
				end
			})
	spawn(function()
        while wait() do
            pcall(function()
                if _G.AutoFarmBounty then
                    for i,v in pairs(game:GetService("Players").LocalPlayer.Character:GetChildren()) do
                        if v:IsA("Shirt") then
                            v:Destroy()
                        end
                        if v:IsA("Pants") then
                            v:Destroy()
                        end
                        if v:IsA("Accessory") then
                            v:Destroy()
                        end
                    end
                end
            end)
        end
    end)
    
    spawn(function()
        pcall(function()
            if _G.AutoFarmBounty then
                while wait() do
                    if game.Players.LocalPlayer.Character.Humanoid.Health > 0 then
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                    end
                end
            end
        end)
    end)
    
    spawn(function()
        while wait() do
            pcall(function()
                if _G.AutoFarmBounty then
                    if not game:GetService("Players").LocalPlayer.Character:FindFirstChild("HasBuso") then
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Buso")
                    end
                end
            end)
        end
    end)
    
    spawn(function()
        while task.wait() do
            pcall(function()
                if _G.AutoFarmBounty then
                    game:GetService("Players").LocalPlayer.Character[SelectWeaponGun].Cooldown.Value = 0
                    spawn(function()
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Beli.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.HP.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Energy.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.StatsButton.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.ShopButton.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Skills.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Level.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.MenuButton.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Code.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Settings.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Mute.Visible = false
                        game:GetService("Players")["LocalPlayer"].PlayerGui.Main.CrewButton.Visible = false
                        game.Players.LocalPlayer.Character.Animate.Disabled = true
                    end)
                end
            end)
        end
    end)
    CastlePostoMansion = CFrame.new(-5084.8447265625, 316.48101806641, -3145.3752441406)
    MansiontoCastlePos = CFrame.new(-12464.596679688, 376.30590820312, -7567.2626953125)
    Castletophydra = CFrame.new(-5095.33984375, 316.48101806641, -3168.3134765625)
    HydratoCastle = CFrame.new(5741.869140625, 611.94750976562, -282.61154174805)
    spawn(function()
        while wait() do
            pcall(function()
                if _G.AutoFarmBounty then
                    for i,v in pairs(game:GetService("Workspace").Characters:GetChildren()) do
                        if v.Name ~= game.Players.LocalPlayer.Name then
                            if v:WaitForChild("Humanoid").Health > 0 and (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - v.HumanoidRootPart.Position).Magnitude <= 17000 then
                                plyselecthunthelpold = v.Humanoid.Health
                                repeat task.wait()
                                    EquipWeapon(SelectWeaponGun)
                                    NameTarget = v.Name
                                    if tostring(game.Players.LocalPlayer.Team) == "Pirates" then
                                        topos(v.HumanoidRootPart.CFrame * CFrame.new(0,60,-20))
                                    elseif tostring(game.Players.LocalPlayer.Team) == "Marines" then
                                        if game.Players[NameTarget].Team ~= game.Players.LocalPlayer.Team then
                                            topos(v.HumanoidRootPart.CFrame * CFrame.new(0,60,-20))
                                        end
                                    end
                                    spawn(function()
                                        if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 150 then
                                            StartCheckTarget = true
                                        end
                                    end)
                                    v.HumanoidRootPart.CanCollide = false
                                    spawn(function()
                                        pcall(function()
                                            local args = {
                                                [1] = v.HumanoidRootPart.Position,
                                                [2] = v.HumanoidRootPart
                                            }
                                            game:GetService("Players").LocalPlayer.Character[SelectWeaponGun].RemoteFunctionShoot:InvokeServer(unpack(args))
                                        end)
                                    end)
                                    TargetSelectHunt = v.Humanoid
                                until _G.AutoFarmBounty == false or v.Humanoid.Health == 0 or not v:FindFirstChild("HumanoidRootPart") or not v:FindFirstChild("Humanoid") or not v.Parent or NextplySelect == true
                                NextplySelect = false
                                StartCheckTarget = false
                            end
                        end
                    end
                end
            end)
        end
    end)
    
    spawn(function()
        pcall(function()
            while task.wait() do
                if _G.AutoFarmBounty then
                    game:GetService("Players").LocalPlayer.PlayerGui.Main.InCombat.Visible = false
                    game:GetService("Players").LocalPlayer.PlayerGui.Main.SafeZone.Visible = false
                end
            end
        end)
    end)
    
    spawn(function()
        pcall(function()
            while wait() do
                if _G.AutoFarmBounty then
                    if TargetSelectHunt ~= nil then
                        if StartCheckTarget then
                            wait(6.5)
                            if TargetSelectHunt.Health == TargetSelectHunt.MaxHealth or TargetSelectHunt.Health >= plyselecthunthelpold then
                                NextplySelect = true
                                TargetSelectHunt = nil
                            end
                        end
                    end
                end
            end
        end)
    end)
    
    spawn(function()
        pcall(function()
            while wait(.1) do
                if _G.AutoFarmBounty then
                    if game:GetService("Players").LocalPlayer.PlayerGui.Main.PvpDisabled.Visible == true then
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("EnablePvp")
                    end
                end
            end
        end)
    end)
	spawn(function()
        while wait() do
            if _G.AutoFarmBounty then
                if _G.AutoFarmBounty_Hop then
                    pcall(function()
                        wait(120)
                        Hop()
                    end)
                end
            end
        end
    end)
	local LabelByNino = RUOK:AddLabel({
		Name = "「 Misc Bounty 」"
	})
	RUOK:AddButton({
		Name = "Next Player",
		Callback = function()
			NextplySelect = true
			wait(.1)
			NextplySelect = false
		end
	})
	RUOK:AddSlider({
		Name = "Lock Bounty",
		Value = 750000,
		Min = 1, 
		Max = 25000000, 
		Format = "Lock Bounty : %s%%",
		Callback = function(value)
			_G.BountyLock = value
		end
	})
	RUOK:AddToggle({
		Name = "Start Bounty Lock",
		Flag = "Teleport To Player",
		Value =  _G.StartBountyLock,
		Callback  = function(value)
			_G.StartBountyLock = value
			end
		})
	spawn(function()
        while wait() do
            if _G.StartBountyLock then
                pcall(function()
                    if game:GetService("Players").LocalPlayer.leaderstats["Bounty/Honor"].Value >= _G.BountyLock then
                        game:GetService("Players").LocalPlayer:Kick("Successfully! Bounty Farm")
                    end
                end)
            end
        end
    end)
	local World = PVP:CreateSection({
		Name = "[ World ]",
		Side = "Right"
	})
	World:AddButton({
		Name = "Teleport To Old World",
		Callback = function()
			game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelMain")
		end
	})
	World:AddButton({
		Name = "Teleport To Second Sea",
		Callback = function()
			game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelDressrosa")
		end
	})
	World:AddButton({
		Name = "Teleport To Third Sea",
		Callback = function()
			game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("TravelZou")
		end
	})

	local TP = PVP:CreateSection({
		Name = "[ Teleport ]",
		Side = "Right"
	})
	local Island = {
		"WindMill [World 1]",
		"Marine [World 1]",
		"Middle Town [World 1]",
		"Jungle [World 1]",
		"Pirate Village [World 1]",
		"Desert [World 1]",
		"Snow Island [World 1]",
		"MarineFord [World 1]",
		"Colosseum [World 1]",
		"Sky Island 1 [World 1]",
		"Sky Island 2 [World 1]",
		"Sky Island 3 [World 1]",
		"Prison [World 1]",
		"Magma Village [World 1]",
		"Under Water Island [World 1]",
		"Fountain City [World 1]",
		"Shank Room [World 1]",
		"Mob Island [World 1]",
		"The Cafe [World 2]",
		"Frist Spot [World 2]",
		"Dark Area [World 2]",
		"Flamingo Mansion [World 2]",
		"Flamingo Room [World 2]",
		"Green Zone [World 2]",
		"Factory [World 2]",
		"Colossuim [World 2]",
		"Zombie Island [World 2]",
		"Two Snow Mountain [World 2]",
		"Punk Hazard [World 2]",
		"Cursed Ship [World 2]",
		"Ice Castle [World 2]",
		"Forgotten Island [World 2]",
		"Ussop Island [World 2]",
		"Mini Sky Island [World 2]",
		"Mansion [World 3]",
		"Port Town [World 3]",
		"Great Tree [World 3]",
		"Castle On The Sea [World 3]",
		"MiniSky [World 3]", 
		"Hydra Island [World 3]",
		"Floating Turtle [World 3]",
		"Haunted Castle [World 3]",
		"Ice Cream Island [World 3]",
		"Peanut Island [World 3]",
		"Cake Island [World 3]",
		"Cocoa Island [World 3]",
		"Candy Island [World 3]"
		}


	TP:AddDropdown({
		Name = "Select Island",
		Value = "",
		List = Island,
		Callback = function(value)
			_G.SelectIsland = value
		end
	})
TP:AddToggle({
		Name = "Teleport",
		Flag = "Teleport To Player",
		Value =  _G.TeleportIsland,
		Callback  = function(value)
			_G.TeleportIsland = value
		if _G.TeleportIsland == true then
            repeat wait()
                if _G.SelectIsland == "WindMill" then
                    TP(CFrame.new(979.79895019531, 16.516613006592, 1429.0466308594))
                elseif _G.SelectIsland == "Marine" then
                    TP(CFrame.new(-2566.4296875, 6.8556680679321, 2045.2561035156))
                elseif _G.SelectIsland == "Middle Town" then
                    TP(CFrame.new(-690.33081054688, 15.09425163269, 1582.2380371094))
                elseif _G.SelectIsland == "Jungle" then
                    TP(CFrame.new(-1612.7957763672, 36.852081298828, 149.12843322754))
                elseif _G.SelectIsland == "Pirate Village" then
                    TP(CFrame.new(-1181.3093261719, 4.7514905929565, 3803.5456542969))
                elseif _G.SelectIsland == "Desert" then
                    TP(CFrame.new(944.15789794922, 20.919729232788, 4373.3002929688))
                elseif _G.SelectIsland == "Snow Island" then
                    TP(CFrame.new(1347.8067626953, 104.66806030273, -1319.7370605469))
                elseif _G.SelectIsland == "MarineFord" then
                    TP(CFrame.new(-4914.8212890625, 50.963626861572, 4281.0278320313))
                elseif _G.SelectIsland == "Colosseum" then
                    TP( CFrame.new(-1427.6203613281, 7.2881078720093, -2792.7722167969))
                elseif _G.SelectIsland == "Sky Island 1" then
                    TP(CFrame.new(-4869.1025390625, 733.46051025391, -2667.0180664063))
                elseif _G.SelectIsland == "Sky Island 2" then  
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-4607.82275, 872.54248, -1667.55688))
                elseif _G.SelectIsland == "Sky Island 3" then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-7894.6176757813, 5547.1416015625, -380.29119873047))
                elseif _G.SelectIsland == "Prison" then
                    TP( CFrame.new(4875.330078125, 5.6519818305969, 734.85021972656))
                elseif _G.SelectIsland == "Magma Village" then
                    TP(CFrame.new(-5247.7163085938, 12.883934020996, 8504.96875))
                elseif _G.SelectIsland == "Under Water Island" then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(61163.8515625, 11.6796875, 1819.7841796875))
                elseif _G.SelectIsland == "Fountain City" then
                    TP(CFrame.new(5127.1284179688, 59.501365661621, 4105.4458007813))
                elseif _G.SelectIsland == "Shank Room" then
                    TP(CFrame.new(-1442.16553, 29.8788261, -28.3547478))
                elseif _G.SelectIsland == "Mob Island" then
					TP(CFrame.new(-2850.20068, 7.39224768, 5354.99268))
                elseif _G.SelectIsland == "The Cafe" then
                    TP(CFrame.new(-380.47927856445, 77.220390319824, 255.82550048828))
                elseif _G.SelectIsland == "Frist Spot" then
                    TP(CFrame.new(-11.311455726624, 29.276733398438, 2771.5224609375))
                elseif _G.SelectIsland == "Dark Area" then
                    TP(CFrame.new(3780.0302734375, 22.652164459229, -3498.5859375))
                elseif _G.SelectIsland == "Flamingo Mansion" then
                    TP(CFrame.new(-483.73370361328, 332.0383605957, 595.32708740234))
                elseif _G.SelectIsland == "Flamingo Room" then
                    TP(CFrame.new(2284.4140625, 15.152037620544, 875.72534179688))
                elseif _G.SelectIsland == "Green Zone" then
                    TP( CFrame.new(-2448.5300292969, 73.016105651855, -3210.6306152344))
                elseif _G.SelectIsland == "Factory" then
                    TP(CFrame.new(424.12698364258, 211.16171264648, -427.54049682617))
                elseif _G.SelectIsland == "Colossuim" then
                    TP( CFrame.new(-1503.6224365234, 219.7956237793, 1369.3101806641))
                elseif _G.SelectIsland == "Zombie Island" then
                    TP(CFrame.new(-5622.033203125, 492.19604492188, -781.78552246094))
                elseif _G.SelectIsland == "Two Snow Mountain" then
                    TP(CFrame.new(753.14288330078, 408.23559570313, -5274.6147460938))
                elseif _G.SelectIsland == "Punk Hazard" then
                    TP(CFrame.new(-6127.654296875, 15.951762199402, -5040.2861328125))
                elseif _G.SelectIsland == "Cursed Ship" then
                    TP(CFrame.new(923.40197753906, 125.05712890625, 32885.875))
                elseif _G.SelectIsland == "Ice Castle" then
                    TP(CFrame.new(6148.4116210938, 294.38687133789, -6741.1166992188))
                elseif _G.SelectIsland == "Forgotten Island" then
                    TP(CFrame.new(-3032.7641601563, 317.89672851563, -10075.373046875))
                elseif _G.SelectIsland == "Ussop Island" then
                    TP(CFrame.new(4816.8618164063, 8.4599885940552, 2863.8195800781))
                elseif _G.SelectIsland == "Mini Sky Island" then
                    TP(CFrame.new(-288.74060058594, 49326.31640625, -35248.59375))
                elseif _G.SelectIsland == "Great Tree" then
                    TP(CFrame.new(2681.2736816406, 1682.8092041016, -7190.9853515625))
                elseif _G.SelectIsland == "Castle On The Sea" then
                    TP(CFrame.new(-5074.45556640625, 314.5155334472656, -2991.054443359375))
                elseif _G.SelectIsland == "MiniSky" then
                    TP(CFrame.new(-260.65557861328, 49325.8046875, -35253.5703125))
                elseif _G.SelectIsland == "Port Town" then
                    TP(CFrame.new(-290.7376708984375, 6.729952812194824, 5343.5537109375))
                elseif _G.SelectIsland == "Hydra Island" then
                    TP(CFrame.new(5228.8842773438, 604.23400878906, 345.0400390625))
                elseif _G.SelectIsland == "Floating Turtle" then
                    TP(CFrame.new(-13274.528320313, 531.82073974609, -7579.22265625))
                elseif _G.SelectIsland == "Mansion" then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance",Vector3.new(-12471.169921875, 374.94024658203, -7551.677734375))
                elseif _G.SelectIsland == "Haunted Castle" then
                    TP(CFrame.new(-9515.3720703125, 164.00624084473, 5786.0610351562))
                elseif _G.SelectIsland == "Ice Cream Island" then
                    TP(CFrame.new(-902.56817626953, 79.93204498291, -10988.84765625))
                elseif _G.SelectIsland == "Peanut Island" then
                    TP(CFrame.new(-2062.7475585938, 50.473892211914, -10232.568359375))
                elseif _G.SelectIsland == "Cake Island" then
                    TP(CFrame.new(-1884.7747802734375, 19.327526092529297, -11666.8974609375))
                elseif _G.SelectIsland == "Cocoa Island" then
                    TP(CFrame.new(87.94276428222656, 73.55451202392578, -12319.46484375))
                elseif _G.SelectIsland == "Candy Island New⛄" then
                    TP(CFrame.new(-1014.4241943359375, 149.11068725585938, -14555.962890625))
                end
            until not _G.TeleportIsland
        end
        StopTween(_G.TeleportIsland)
    end
})
    local NPC = {
		"Random Devil Fruit [World 1]",
		"Blox Fruits Dealer [World 1]",
		"Remove Devil Fruit [World 1]",
		"Ability Teacher [World 1]",
		"Dark Step [World 1]", 
		"Electro [World 1]",
		"Fishman Karate [World 1]",
			"Dargon Berath [World 2]",
			"Mtsterious Man [World 2]",
			"Mysterious Scientist [World 2]",
			"Awakening Expert [World 2]",
			"Nerd [World 2]", 
			"Bar Manager [World 2]",
			"Blox Fruits Dealer [World 2]",
			"Trevor [World 2]",
			"Enhancement Editor [World 2]",
			"Pirate Recruiter [World 2]",
			"Marines Recruiter [World 2]",
			"Chemist [World 2]",
			"Cyborg [World 2]",
			"Ghoul Mark [World 2]",
			"Guashiem [World 2]",
			"El Admin [World 2]",
			"El Rodolfo [World 2]",
			"Arowe [World 2]",
			"Blox Fruits Dealer [World 3]",
			"Remove Devil Fruit [World 3]",
			"Horned Man [World 3]",
			"Hungey Man [World 3]",
			"Previous Hero [World 3]", 
			"Butler [World 3]",
			"Lunoven [World 3]",
			"Trevor [World 3]",
			"Elite Hunter [World 3]",
			"Player Hunter [World 3]",
			"Uzoth [World 3]"
			}


		TP:AddDropdown({
			Name = "Select NPC",
			Value = "",
			List = NPC,
			Callback = function(value)
				_G.SelectNPC = value
			end
		})
		TP:AddToggle({
			Name = "Teleport",
			Flag = "Teleport To NPC",
			Value =  _G.TeleportIsland,
			Callback  = function(value)
				_G.TeleportIsland = value
			if _G.TeleportNPC == true then
				repeat wait()
					if _G.SelectNPC == "Dargon Berath" then
						TP(CFrame.new(703.372986, 186.985519, 654.522034, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Mtsterious Man" then
						TP(CFrame.new(-2574.43335, 1627.92371, -3739.35767, 0.378697902, -9.06400288e-09, 0.92552036, -8.95582009e-09, 1, 1.34578926e-08, -0.92552036, -1.33852689e-08, 0.378697902))
					elseif _G.SelectNPC == "Mysterious Scientist" then
						TP(CFrame.new(-6437.87793, 250.645355, -4498.92773, 0.502376854, -1.01223634e-08, -0.864648759, 2.34106086e-08, 1, 1.89508653e-09, 0.864648759, -2.11940012e-08, 0.502376854))
					elseif _G.SelectNPC == "Awakening Expert" then
						TP(CFrame.new(-408.098846, 16.0459061, 247.432846, 0.028394036, 6.17599138e-10, 0.999596894, -5.57905944e-09, 1, -4.59372484e-10, -0.999596894, -5.56376767e-09, 0.028394036))
					elseif _G.SelectNPC == "Nerd" then
						TP(CFrame.new(-401.783722, 73.0859299, 262.306702, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Bar Manager" then
						TP(CFrame.new(-385.84726, 73.0458984, 316.088806, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Blox Fruits Dealer" then
						TP(CFrame.new(-450.725464, 73.0458984, 355.636902, -0.780352175, -2.7266168e-08, 0.625340283, 9.78516468e-09, 1, 5.58128797e-08, -0.625340283, 4.96727601e-08, -0.780352175))
					elseif _G.SelectNPC == "Trevor" then
						TP(CFrame.new(-341.498322, 331.886444, 643.024963, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Plokster" then
						TP( CFrame.new(-1885.16016, 88.3838196, -1912.28723, -0.513468027, 0, 0.858108759, 0, 1, 0, -0.858108759, 0, -0.513468027))
					elseif _G.SelectNPC == "Enhancement Editor" then
						TP(CFrame.new(-346.820221, 72.9856339, 1194.36218, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Pirate Recruiter" then  
						TP(CFrame.new(-428.072998, 72.9495239, 1445.32422, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Marines Recruiter" then
						TP(CFrame.new(-1349.77295, 72.9853363, -1045.12964, 0.866493046, 0, -0.499189168, 0, 1, 0, 0.499189168, 0, 0.866493046))
					elseif _G.SelectNPC == "Chemist" then
						TP( CFrame.new(-2777.45288, 72.9919434, -3572.25732, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Ghoul Mark" then
						TP(CFrame.new(635.172546, 125.976357, 33219.832, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Cyborg" then
						TP(CFrame.new(629.146851, 312.307373, -531.624146, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Guashiem" then
						TP(CFrame.new(937.953003, 181.083359, 33277.9297, 1, -8.60126406e-08, 3.81773896e-17, 8.60126406e-08, 1, -1.89969598e-16, -3.8177373e-17, 1.89969598e-16, 1))
					elseif _G.SelectNPC == "El Admin" then
						TP(CFrame.new(1322.80835, 126.345039, 33135.8789, 0.988783717, -8.69797603e-08, -0.149354503, 8.62223786e-08, 1, -1.15461916e-08, 0.149354503, -1.46101409e-09, 0.988783717))
					elseif _G.SelectNPC == "El Rodolfo" then
						TP(CFrame.new(941.228699, 40.4686775, 32778.9922, -0.818029106, -1.19524382e-08, 0.575176775, -1.28741648e-08, 1, 2.47053866e-09, -0.575176775, -5.38394795e-09, -0.818029106))
					elseif _G.SelectNPC == "Arowe" then
						TP(CFrame.new(-1994.51038, 125.519142, -72.2622986, -0.16715166, -6.55417338e-08, -0.985931218, -7.13315558e-08, 1, -5.43836585e-08, 0.985931218, 6.12376851e-08, -0.16715166))
					elseif _G.SelectNPC == "Random Devil Fruit" then
						TP(CFrame.new(-1436.19727, 61.8777695, 4.75247526, -0.557794094, 2.74216543e-08, 0.829979479, 5.83273234e-08, 1, 6.16037932e-09, -0.829979479, 5.18467118e-08, -0.557794094))
					elseif _G.SelectNPC == "Blox Fruits Dealer" then
						TP(CFrame.new(-923.255066, 7.67800522, 1608.61011, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Remove Devil Fruit" then
						TP(CFrame.new(5664.80469, 64.677681, 867.85907, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Ability Teacher" then
						TP(CFrame.new(-1057.67822, 9.65220833, 1799.49146, -0.865874112, -9.26330159e-08, 0.500262439, -7.33759435e-08, 1, 5.816689e-08, -0.500262439, 1.36579752e-08, -0.865874112))
					elseif _G.SelectNPC == "Dark Step" then
						TP( CFrame.new(-987.873047, 13.7778397, 3989.4978, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Electro" then
						TP(CFrame.new(-5389.49561, 13.283, -2149.80151, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Fishman Karate" then
						TP( CFrame.new(61581.8047, 18.8965912, 987.832703, 1, 0, 0, 0, 1, 0, 0, 0, 1))
					elseif _G.SelectNPC == "Random Devil Fruit" then
						TP(CFrame.new(-12491, 337, -7449))
					elseif _G.SelectNPC == "Blox Fruits Dealer" then
						TP(CFrame.new(-12511, 337, -7448))
					elseif _G.SelectNPC == "Remove Devil Fruit" then
						TP(CFrame.new(-5571, 1089, -2661))
					elseif _G.SelectNPC == "Horned Man" then
						TP(CFrame.new(-11890, 931, -8760))
					elseif _G.SelectNPC == "Hungey Man" then
						TP(CFrame.new(-10919, 624, -10268))
					elseif _G.SelectNPC == "Previous Hero" then
						TP(CFrame.new(-10368, 332, -10128))
					elseif _G.SelectNPC == "Butler" then
						TP(CFrame.new(-5125, 316, -3130)) 
					elseif _G.SelectNPC == "Lunoven" then
						TP(CFrame.new(-5117, 316, -3093))
					elseif _G.SelectNPC == "Elite Hunter" then
						TP(CFrame.new(-5420, 314, -2828))
					elseif _G.SelectNPC == "Player Hunter" then
						TP(CFrame.new(-5559, 314, -2840))
					elseif _G.SelectNPC == "Uzoth" then
						TP(CFrame.new(-9785, 852, 6667))
					end
				until not _G.TeleportNPC
			end
			StopTween(_G.TeleportNPC)
		end
	})
		
	local Raid1 = Raid:CreateSection({
		Name = "[ Raid ]",
		Side = "Left"
	})
	local TimeRaid = Raid1:AddLabel({
		Name = "Wait For Dungeon"
	})
	spawn(function()
        pcall(function()
            while wait() do
                if game:GetService("Players").LocalPlayer.PlayerGui.Main.Timer.Visible == true then
                    TimeRaid:Set(game:GetService("Players").LocalPlayer.PlayerGui.Main.Timer.Text)
                else
                    TimeRaid:Set("Wait For Dungeon")
                end
            end
        end)
    end)
	Raid1:AddToggle({
		Name = "Auto Next Island",
		Flag = "Teleport To Player",
		Value =  _G.Auto_Dungeon,
		Callback  = function(value)
			_G.Auto_Dungeon = value
			StopTween(_G.Auto_Dungeon)
			end
		})
		spawn(function()
			pcall(function()
				while wait() do
					if _G.Auto_Dungeon then
						if game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == true then
							if game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 5") then
								TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 5").CFrame*CFrame.new(0,25,0))
							elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 4") then
								TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 4").CFrame*CFrame.new(0,25,0))
							elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 3") then
								TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 3").CFrame*CFrame.new(0,25,0))
							elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 2") then
								TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 2").CFrame*CFrame.new(0,25,0))
							elseif game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1") then
								TP(game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1").CFrame*CFrame.new(0,25,0))
							end
						end
					end
				end
			end)
		end)
		Raid1:AddToggle({
			Name = "Auto Awakener",
			Flag = "Teleport To Player",
			Value = _G.Auto_Awakener,
			Callback  = function(value)
				_G.Auto_Awakener = value
				end
			})
			spawn(function()
				pcall(function()
					while wait(.1) do
						if _G.Auto_Awakener then
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Awakener","Check")
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Awakener","Awaken")
						end
					end
				end)
			end)
			Raid1:AddToggle({
				Name = "Kill Aura",
				Flag = "Teleport To Player",
				Value = _G.Kill_Aura,
				Callback  = function(value)
					_G.Kill_Aura = value
					end
				})
				spawn(function()
					while wait() do
						if _G.Kill_Aura then
							for i,v in pairs(game.Workspace.Enemies:GetDescendants()) do
								if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
									pcall(function()
										repeat wait(.1)
											v.Humanoid.Health = 0
											v.HumanoidRootPart.CanCollide = false
											sethiddenproperty(game.Players.LocalPlayer, "SimulationRadius", math.huge)
										until not _G.Kill_Aura  or not v.Parent or v.Humanoid.Health <= 0
									end)
								end
							end
						end
					end
					end)
					local Chips = {"Flame","Ice","Quake","Light","Dark","String","Rumble","Magma","Human: Buddha","Sand","Bird: Phoenix","Dough"}
					Raid1:AddDropdown({
						Name = "Select Chip",
						Value = "",
						List = Chips,
						Callback = function(value)
							_G.SelectChip = value
						end
					})
				Raid1:AddToggle({
					Name = "Auto Select Dungeon",
					Flag = "Teleport To Player",
					Value = _G.AutoSelectDungeon,
					Callback  = function(value)
						_G.AutoSelectDungeon = value
						end
					})
					spawn(function()
						while wait() do
							if _G.AutoSelectDungeon then
								pcall(function()
									if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Flame-Flame") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Flame-Flame") then
										_G.SelectChip = "Flame"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Ice-Ice") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Ice-Ice") then
										_G.SelectChip = "Ice"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Quake-Quake") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Quake-Quake") then
										_G.SelectChip = "Quake"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Light-Light") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Light-Light") then
										_G.SelectChip = "Light"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dark-Dark") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dark-Dark") then
										_G.SelectChip = "Dark"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("String-String") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("String-String") then
										_G.SelectChip = "String"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Rumble-Rumble") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Rumble-Rumble") then
										_G.SelectChip = "Rumble"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Magma-Magma") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Magma-Magma") then
										_G.SelectChip = "Magma"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Human-Human: Buddha Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Human-Human: Buddha Fruit") then
										_G.SelectChip = "Human: Buddha"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Sand-Sand") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Sand-Sand") then
										_G.SelectChip = "Sand"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Bird-Bird: Phoenix") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Bird-Bird: Phoenix") then
										_G.SelectChip = "Bird: Phoenix"
									elseif game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dough") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dough") then
										_G.SelectChip = "Dough"
									else
										_G.SelectChip = "Flame"
									end
								end)
							end
						end
					end)
					Raid1:AddToggle({
						Name = "Auto Buy Chip",
						Flag = "Teleport To Player",
						Value = _G.AutoBuyChip,
						Callback  = function(value)
							_G.AutoBuyChip = value
							end
						})

					spawn(function()
						pcall(function()
							while wait() do
								if _G.AutoBuyChip then
									if not game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Special Microchip") or not game:GetService("Players").LocalPlayer.Character:FindFirstChild("Special Microchip") then
										if not game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1") then
											game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc", "Select", _G.SelectChip)
										end
									end
								end
							end
						end)
					end)
					Raid1:AddButton({
						Name = "Buy Chip Select",
						Callback = function()
							game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("RaidsNpc","Select",_G.SelectChip)
						end
					})
					
					Raid1:AddToggle({
						Name = "Auto Start Go To Dungeon",
						Flag = "Teleport To Player",
						Value = _G.Auto_StartRaid,
						Callback  = function(value)
							_G.Auto_StartRaid = value
							end
						})
						spawn(function()
							while wait(.1) do
								pcall(function()
									if _G.Auto_StartRaid then
										if game:GetService("Players")["LocalPlayer"].PlayerGui.Main.Timer.Visible == false then
											if not game:GetService("Workspace")["_WorldOrigin"].Locations:FindFirstChild("Island 1") and game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Special Microchip") or game:GetService("Players").LocalPlayer.Character:FindFirstChild("Special Microchip") then
												if World2 then
													fireclickdetector(game:GetService("Workspace").Map.CircleIsland.RaidSummon2.Button.Main.ClickDetector)
												elseif World3 then
													fireclickdetector(game:GetService("Workspace").Map["Boat Castle"].RaidSummon2.Button.Main.ClickDetector)
												end
											end
										end
									end
								end)
							end
						end)


				
						Raid1:AddButton({
							Name = "Teleport to Lab [ Second Sea ]",
							Callback = function()
								TP(CFrame.new(-6438.73535, 250.645355, -4501.50684))
							end
						})
						Raid1:AddButton({
							Name = "Teleport to Lab [ Third Sea ]",
							Callback = function()
								TP(CFrame.new(-5017.40869, 314.844055, -2823.0127, -0.925743818, 4.48217499e-08, -0.378151238, 4.55503146e-09, 1, 1.07377559e-07, 0.378151238, 9.7681621e-08, -0.925743818))
							end
						})
						Raid1:AddButton({
							Name = "Awakening Room [ Second Sea ]",
							Callback = function()
								TP(CFrame.new(266.227783, 1.39509034, 1857.00732))
							end
						})
						Raid1:AddButton({
							Name = "Awakening Room [ Third Sea ]",
							Callback = function()
								TP(CFrame.new(-11571.440429688, 49.172668457031, -7574.7368164062))
							end
						})
						local BF = Raid:CreateSection({
							Name = "Devil Fruit",
							Side = "Right"
						})
						FruitList = {
							"Bomb-Bomb",
							"Spike-Spike",
							"Chop-Chop",
							"Spring-Spring",
							"Kilo-Kilo",
							"Spin-Spin",
							"Bird: Falcon",
							"Smoke-Smoke",
							"Flame-Flame",
							"Ice-Ice",
							"Sand-Sand",
							"Dark-Dark",
							"Revive-Revive",
							"Diamond-Diamond",
							"Light-Light",
							"Love-Love",
							"Rubber-Rubber",
							"Barrier-Barrier",
							"Magma-Magma",
							"Door-Door",
							"Quake-Quake",
							"Human-Human: Buddha",
							"String-String",
							"Bird-Bird: Phoenix",
							"Rumble-Rumble",
							"Paw-Paw",
							"Gravity-Gravity",
							"Dough-Dough",
							"Venom-Venom",
							"Shadow-Shadow",
							"Control-Control",
							"Soul-Soul",
							"Dragon-Dragon",
							"Leopard-Leopard"
						}
						
						_G.SelectFruit = ""
						
BF:AddDropdown({
	Name = "Select Fruits Sniper",
	Value = "",
	List = FruitList,
	MultiSelect = true,
	Callback = function(value)
		_G.SelectFruit = value
	end
})
BF:AddToggle({
    Name = "Auto Buy Fruit Sniper",
	Value = _G.AutoBuyFruitSniper,
    Callback = function(value)
        _G.AutoBuyFruitSniper = value
    end
})
BF:AddDropdown({
	Name = "Select Fruits Eat",
	Value = "",
	List = FruitList,
	MultiSelect = true,
	Callback = function(value)
		_G.SelectFruitEat = value
	end
})
BF:AddToggle({
    Name = "Auto Eat Fruit",
	Value = _G.AutoEatFruit,
    Callback = function(value)
        _G.AutoEatFruit = value
    end
})
spawn(function()
	pcall(function()
		while wait(.1) do
			if _G.AutoEatFruit then
				game:GetService("Players").LocalPlayer.Character:FindFirstChild(_G.SelectFruitEat).EatRemote:InvokeServer()
			end
		end
	end)
end)
BF:AddToggle({
    Name = "Auto Eat Fruit Hop",
	Value = _G.AutoEatFruitHop,
    Callback = function(value)
        _G.AutoEatFruitHop = value
    end
})
spawn(function()
	pcall(function()
		while wait(.1) do wait(10)
			if _G.AutoEatFruitHop and _G.SelectFruitEat ~= nil then
				if not game:GetService("Players").LocalPlayer.Character:FindFirstChild(_G.SelectFruitEat) or not game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(_G.SelectFruitEat) then
					Hop()
				else
					game:GetService("Players").LocalPlayer.Character:FindFirstChild(_G.SelectFruitEat).EatRemote:InvokeServer()
				end
			end
		end
	end)
end)

spawn(function()
	pcall(function()
		while wait(.1) do
			if _G.AutoBuyFruitSniper then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("GetFruits")
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("PurchaseRawFruit",_G.SelectFruit)
			end 
		end
	end)
end)
BF:AddToggle({
    Name = "Auto Random Fruit",
	Value = _G.Random_Auto,
    Callback = function(value)
        _G.Random_Auto = value
    end
})
spawn(function()
	pcall(function()
		while wait(.1) do
			if _G.Random_Auto then
				game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin","Buy")
			end 
		end
	end)
end)

BF:AddButton({
    Name = "Random Fruit",
    Callback = function()
		game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("Cousin","Buy")
    end
})

BF:AddButton({
    Name = "Open Devil Shop",
    Callback = function()
		game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("GetFruits")
        game:GetService("Players").LocalPlayer.PlayerGui.Main.FruitShop.Visible = true
    end
})
BF:AddToggle({
    Name = "Auto Drop Fruit",
	Value = _G.DropFruit,
    Callback = function(value)
        _G.DropFruit = value
    end
})
spawn(function()
	while wait() do
		if _G.DropFruit then
			pcall(function()
				for i,v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
					if string.find(v.Name, "Fruit") then
						EquipWeapon(v.Name)
						wait(.1)
						if game:GetService("Players").LocalPlayer.PlayerGui.Main.Dialogue.Visible == true then
							game:GetService("Players").LocalPlayer.PlayerGui.Main.Dialogue.Visible = false
						end
						EquipWeapon(v.Name)
						game:GetService("Players").LocalPlayer.Character:FindFirstChild(SelectFruit).EatRemote:InvokeServer("Drop")
					end
				end
			for i,v in pairs(game:GetService("Players").LocalPlayer.Character:GetChildren()) do
					if string.find(v.Name, "Fruit") then
						EquipWeapon(v.Name)
						wait(.1)
						if game:GetService("Players").LocalPlayer.PlayerGui.Main.Dialogue.Visible == true then
							game:GetService("Players").LocalPlayer.PlayerGui.Main.Dialogue.Visible = false
						end
						EquipWeapon(v.Name)
						game:GetService("Players").LocalPlayer.Character:FindFirstChild(SelectFruit).EatRemote:InvokeServer("Drop")
					end
				end
			end)
		end
	end
end)
BF:AddToggle({
    Name = "Auto Store Fruit",
	Value = _G.AutoStoreFruit,
    Callback = function(value)
        _G.AutoStoreFruit = value
    end
})
function DropFruit()
	pcall(function()
		game.Players.LocalPlayer.PlayerGui.Main.FruitInventory.Position = UDim2.new(10.100, 0, 0.100, 0) -- HideUi
		game.Players.LocalPlayer.PlayerGui.Main.FruitInventory.Visible = true -- เปิดไว้ถึงจะเช็คได้
		local invenfruit = game.Players.LocalPlayer.PlayerGui.Main.FruitInventory.Container.Stored.ScrollingFrame.Frame
		wait(.3)
		for i,v in pairs(invenfruit:GetChildren()) do
			if string.find(v.Name,"-") then
				for _,Backpack in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
					FruitStoreF = string.split(Backpack.Name, " ")[1]
					FruitStoreR = FruitStoreF.."-"..FruitStoreF
					if v.Name == FruitStoreR then
						game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(Backpack.Name):Destroy()
					end												
				end
			end
		end
		for i,v in pairs(invenfruit:GetChildren()) do
			if string.find(v.Name,"-") then
				for _,Character in pairs(game:GetService("Players").LocalPlayer.Character:GetChildren()) do
					FruitStoreF = string.split(Character.Name, " ")[1]
					FruitStoreR = FruitStoreF.."-"..FruitStoreF
					if v.Name == FruitStoreR then
						game:GetService("Players").LocalPlayer.Character:FindFirstChild(Character.Name):Destroy()
					end												
				end
			end
		end
	end)
end

spawn(function()
	while wait() do
		if _G.AutoStoreFruit then
			pcall(function()
				DropFruit()
				wait()
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Bomb Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Bomb Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Bomb-Bomb",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Bomb Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Bomb Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Spike Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Spike Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Spike-Spike",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Spike Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Spike Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Chop Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Chop Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Chop-Chop",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Chop Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Chop Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Spring Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Spring Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Spring-Spring",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Spring Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Spring Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Kilo Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Kilo Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Kilo-Kilo",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Kilo Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Kilo Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Smoke Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Smoke Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Smoke-Smoke",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Smoke Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Smoke Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Spin Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Spin Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Spin-Spin",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Spin Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Spin Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Flame Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Flame Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Flame-Flame",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Flame Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Flame Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Bird: Falcon Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Bird: Falcon Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Bird-Bird: Falcon",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Bird: Falcon Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Bird: Falcon Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Ice Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Ice Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Ice-Ice",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Ice Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Ice Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Sand Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Sand Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Sand-Sand",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Sand Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Sand Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dark Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dark Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Dark-Dark",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dark Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dark Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Revive Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Revive Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Revive-Revive",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Revive Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Revive Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Diamond Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Diamond Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Diamond-Diamond",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Diamond Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Diamond Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Light Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Light Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Light-Light",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Light Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Light Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Love Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Love Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Love-Love",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Love Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Love Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Rubber Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Rubber Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Rubber-Rubber",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Rubber Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Rubber Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Barrier Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Barrier Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Barrier-Barrier",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Barrier Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Barrier Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Magma Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Magma Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Magma-Magma",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Magma Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Magma Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Door Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Door Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Door-Door",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Door Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Door Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Quake Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Quake Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Quake-Quake",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Quake Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Quake Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Human-Human: Buddha Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Human-Human: Buddha Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Human-Human: Buddha",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Human-Human: Buddha Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Human-Human: Buddha Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("String Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("String Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","String-String",game:GetService("Players").LocalPlayer.Character:FindFirstChild("String Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("String Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Bird: Phoenix Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Bird: Phoenix Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Bird-Bird: Phoenix",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Bird: Phoenix Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Bird: Phoenix Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Rumble Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Rumble Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Rumble-Rumble",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Rumble Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Rumble Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Paw Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Paw Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Paw-Paw",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Paw Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Paw Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Gravity Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Gravity Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Gravity-Gravity",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Gravity Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Gravity Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dough Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dough Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Dough-Dough",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dough Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dough Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Shadow Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Shadow Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Shadow-Shadow",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Shadow Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Shadow Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Venom Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Venom Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Venom-Venom",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Venom Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Venom Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Control Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Control Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Control-Control",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Control Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Control Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dragon Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dragon Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Dragon-Dragon",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Dragon Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Dragon Fruit"))
				end
				if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Leopard Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Leopard Fruit") then
					game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit","Leopard-Leopard",game:GetService("Players").LocalPlayer.Character:FindFirstChild("Leopard Fruit") or game:GetService("Players").LocalPlayer.Backpack:FindFirstChild("Leopard Fruit"))
				end
			end)
		 end
	   end
    end)
	BF:AddToggle({
		Name = "Bring to Fruit",
		Value = _G.Grabfruit,
		Callback = function(value)
			_G.Grabfruit = value
		end
	})
	spawn(function()
		while wait(.1) do
			if _G.Grabfruit then
				for i,v in pairs(game.Workspace:GetChildren()) do
					if string.find(v.Name, "Fruit") then
						game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Handle.CFrame
					end
				end
			end
end
end)
BF:AddToggle({
	Name = "Tween to Fruit",
	Value = _G.Tweenfruit,
	Callback = function(value)
		_G.Tweenfruit = value
		StopTween(_G.Tweenfruit)
	end
})
spawn(function()
	while wait(.1) do
		if _G.Tweenfruit then
			for i,v in pairs(game.Workspace:GetChildren()) do
				if string.find(v.Name, "Fruit") then
					topos(v.Handle.CFrame)
				end
			end
		end
end
end)
BF:AddToggle({
	Name = "Bring All Fruit 75% Kick System",
	Value = _G.BringFruitBF,
	Callback = function(value)
		_G.BringFruitBF = value
	end
})
spawn(function()
	while wait() do
		if _G.BringFruitBF then
			pcall(function()
				for i,v in pairs(game.Workspace:GetChildren()) do
					if v:IsA("Tool") then
						v.Handle.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
					end
				end	
			end)
		end
	end
end)

return library, library_flags, library.subs
	
