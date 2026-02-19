# Script.lua
-- [[ SUPREME SCRIPT SEARCHER V70 - DETAILED STATUS & RESTORATION ]]
local player = game.Players.LocalPlayer
local httpService = game:GetService("HttpService")
local coreGui = game:GetService("CoreGui")
local userInput = game:GetService("UserInputService")
local marketService = game:GetService("MarketplaceService")

-- [ESTADO E TRADUÇÕES]
local currentLang = "PT"
local translations = {
    PT = { congrats = "Parabéns", search = "Buscar", refresh = "Recarregar", clear = "Limpar", execute = "Executar", key = "Key:", universal = "Universal:" },
    EN = { congrats = "Congratulations", search = "Search", refresh = "Refresh", clear = "Clear", execute = "Execute", key = "Key:", universal = "Universal:" },
    ES = { congrats = "Felicitaciones", search = "Buscar", refresh = "Refrescar", clear = "Limpiar", execute = "Ejecutar", key = "Key:", universal = "Universal:" }
}

local success, productInfo = pcall(function() return marketService:GetProductInfo(game.PlaceId) end)
local cleanGameName = success and productInfo.Name:gsub("[^\32-\126]", ""):gsub("[%p%c]", " "):gsub("%s+", " "):match("^%s*(.-)%s*$") or "Universal"

local sg = Instance.new("ScreenGui", coreGui); sg.Name = "SupremeHub_V70"

-- [BARRA MINIMIZADA MÓVEL]
local miniBar = Instance.new("Frame", sg)
miniBar.Size = UDim2.new(0, 250, 0, 45); miniBar.Position = UDim2.new(0.5, -125, 0.05, 0)
miniBar.BackgroundColor3 = Color3.fromRGB(25, 27, 45); miniBar.Visible = false; miniBar.Active = true
Instance.new("UICorner", miniBar)
local miniStroke = Instance.new("UIStroke", miniBar); miniStroke.Thickness = 2

local miniTitle = Instance.new("TextLabel", miniBar)
miniTitle.Size = UDim2.new(1, -50, 1, 0); miniTitle.Position = UDim2.new(0, 15, 0, 0); miniTitle.BackgroundTransparency = 1
miniTitle.Text = "Script Searcher 2.0"; miniTitle.TextColor3 = Color3.new(1,1,1); miniTitle.Font = "GothamBold"; miniTitle.TextSize = 16; miniTitle.TextXAlignment = "Left"

local maximizeBtn = Instance.new("TextButton", miniBar)
maximizeBtn.Size = UDim2.new(0, 40, 0, 40); maximizeBtn.Position = UDim2.new(1, -45, 0.5, -20); maximizeBtn.BackgroundTransparency = 1
maximizeBtn.Text = "□"; maximizeBtn.TextColor3 = Color3.new(1,1,1); maximizeBtn.Font = "GothamBold"; maximizeBtn.TextSize = 25

-- [JANELA PRINCIPAL]
local main = Instance.new("Frame", sg)
main.Size = UDim2.new(0, 750, 0, 450); main.Position = UDim2.new(0.5, -375, 0.4, -225); main.BackgroundColor3 = Color3.fromRGB(25, 27, 45); main.Visible = false; main.Active = true
Instance.new("UICorner", main); local ledStroke = Instance.new("UIStroke", main); ledStroke.Thickness = 3

-- [BOTÃO MINIMIZAR GRANDE]
local minimizeBtn = Instance.new("TextButton", main)
minimizeBtn.Size = UDim2.new(0, 45, 0, 45); minimizeBtn.Position = UDim2.new(1, -50, 0, 5); minimizeBtn.BackgroundTransparency = 1
minimizeBtn.Text = "−"; minimizeBtn.TextColor3 = Color3.new(1,1,1); minimizeBtn.Font = "GothamBold"; minimizeBtn.TextSize = 40; minimizeBtn.ZIndex = 10

local container = Instance.new("Frame", main); container.Size = UDim2.new(1, -75, 1, 0); container.Position = UDim2.new(0, 70, 0, 0); container.BackgroundTransparency = 1
local searchPage = Instance.new("Frame", container); searchPage.Size = UDim2.new(1, 0, 1, 0); searchPage.BackgroundTransparency = 1
local benchPage = Instance.new("Frame", container); benchPage.Size = UDim2.new(1, 0, 1, 0); benchPage.BackgroundTransparency = 1; benchPage.Visible = false

-- [EDITOR / PÁGINA DE TRABALHO]
local editor = Instance.new("TextBox", benchPage)
editor.Size = UDim2.new(0, 650, 0, 300); editor.Position = UDim2.new(0, 10, 0, 15); editor.BackgroundColor3 = Color3.fromRGB(15, 16, 28); editor.TextColor3 = Color3.new(1,1,1); editor.Font = "Code"; editor.TextSize = 14; editor.MultiLine = true; editor.TextXAlignment = "Left"; editor.TextYAlignment = "Top"; editor.Text = "-- Supreme Editor V2.0"; Instance.new("UICorner", editor)
local execBtn = Instance.new("TextButton", benchPage); execBtn.Size = UDim2.new(0, 150, 0, 40); execBtn.Position = UDim2.new(0, 10, 0, 325); execBtn.BackgroundColor3 = Color3.fromRGB(85, 95, 210); execBtn.TextColor3 = Color3.new(1,1,1); execBtn.Font = "GothamBold"; Instance.new("UICorner", execBtn)
local clearBtn = Instance.new("TextButton", benchPage); clearBtn.Size = UDim2.new(0, 150, 0, 40); clearBtn.Position = UDim2.new(0, 170, 0, 325); clearBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50); clearBtn.TextColor3 = Color3.new(1,1,1); clearBtn.Font = "GothamBold"; Instance.new("UICorner", clearBtn)

-- [LISTAGEM DE SCRIPTS]
local scroll = Instance.new("ScrollingFrame", searchPage); scroll.Size = UDim2.new(0.98, 0, 0, 340); scroll.Position = UDim2.new(0, 0, 0, 75); scroll.BackgroundTransparency = 1; scroll.ScrollBarThickness = 3; scroll.AutomaticCanvasSize = "Y"
local grid = Instance.new("UIGridLayout", scroll); grid.CellSize = UDim2.new(0, 210, 0, 240); grid.CellPadding = UDim2.new(0, 12, 0, 15)

local searchBarFrame = Instance.new("Frame", searchPage); searchBarFrame.Size = UDim2.new(0.9, 0, 0, 45); searchBarFrame.Position = UDim2.new(0, 0, 0, 15); searchBarFrame.BackgroundColor3 = Color3.fromRGB(15, 16, 28); Instance.new("UICorner", searchBarFrame)
local searchInput = Instance.new("TextBox", searchBarFrame); searchInput.Size = UDim2.new(0.5, 0, 1, 0); searchInput.Position = UDim2.new(0.02, 0, 0, 0); searchInput.BackgroundTransparency = 1; searchInput.Text = cleanGameName; searchInput.TextColor3 = Color3.new(1,1,1); searchInput.Font = "Gotham"; searchInput.TextSize = 14; searchInput.TextXAlignment = "Left"; searchInput.ClearTextOnFocus = false
local searchBtn = Instance.new("TextButton", searchBarFrame); searchBtn.Size = UDim2.new(0, 80, 0, 35); searchBtn.Position = UDim2.new(1, -170, 0.5, -17); searchBtn.BackgroundColor3 = Color3.fromRGB(85, 95, 210); searchBtn.TextColor3 = Color3.new(1,1,1); searchBtn.Font = "GothamBold"; Instance.new("UICorner", searchBtn)
local refreshBtn = Instance.new("TextButton", searchBarFrame); refreshBtn.Size = UDim2.new(0, 80, 0, 35); refreshBtn.Position = UDim2.new(1, -85, 0.5, -17); refreshBtn.BackgroundColor3 = Color3.fromRGB(50, 150, 100); refreshBtn.TextColor3 = Color3.new(1,1,1); refreshBtn.Font = "GothamBold"; Instance.new("UICorner", refreshBtn)

-- [FUNÇÃO BUSCA COM INFORMAÇÕES DETALHADAS]
local function fetch(query)
    if query == "" then return end
    for _,v in pairs(scroll:GetChildren()) do if v:IsA("Frame") then v:Destroy() end end
    local s, res = pcall(function() return game:HttpGet("https://scriptblox.com/api/script/search?q="..httpService:UrlEncode(query).."&max=30") end)
    if s then
        local d = httpService:JSONDecode(res)
        if d.result and d.result.scripts then
            for _, sc in pairs(d.result.scripts) do 
                local card = Instance.new("Frame", scroll); card.BackgroundColor3 = Color3.fromRGB(35, 38, 60); Instance.new("UICorner", card)
                
                local gameImg = Instance.new("ImageLabel", card)
                gameImg.Size = UDim2.new(1, -20, 0, 90); gameImg.Position = UDim2.new(0, 10, 0, 10); gameImg.BackgroundColor3 = Color3.fromRGB(15, 15, 20); Instance.new("UICorner", gameImg)
                gameImg.Image = "https://www.roblox.com/asset-thumbnail/image?assetId="..(sc.game and sc.game.gameId or 0).."&width=420&height=420&format=png"

                local title = Instance.new("TextLabel", card); title.Size = UDim2.new(1, -20, 0, 30); title.Position = UDim2.new(0, 10, 0, 105); title.Text = sc.title; title.TextColor3 = Color3.new(1,1,1); title.Font = "GothamBold"; title.TextSize = 11; title.TextWrapped = true; title.BackgroundTransparency = 1
                
                -- STATUS DO SCRIPT (KEY / UNIVERSAL)
                local infoTxt = Instance.new("TextLabel", card)
                infoTxt.Size = UDim2.new(1, -20, 0, 40); infoTxt.Position = UDim2.new(0, 10, 0, 135); infoTxt.BackgroundTransparency = 1; infoTxt.Font = "Gotham"; infoTxt.TextSize = 10; infoTxt.TextWrapped = true
                local hasKey = sc.key and "Sim/Yes" or "Não/No"
                local isUni = sc.isUniversal and "Sim/Yes" or "Não/No"
                infoTxt.Text = "🔑 Key: " .. hasKey .. "\n🌎 Universal: " .. isUni .. "\n⚡ Status: " .. (sc.isPatched and "Patched" or "Working")
                infoTxt.TextColor3 = sc.isPatched and Color3.new(1,0.4,0.4) or Color3.new(0.6,1,0.6)

                local bExec = Instance.new("TextButton", card); bExec.Size = UDim2.new(0.9, 0, 0, 25); bExec.Position = UDim2.new(0.05, 0, 1, -30); bExec.BackgroundColor3 = Color3.fromRGB(85, 95, 210); bExec.Text = "Execute"; bExec.TextColor3 = Color3.new(1,1,1); bExec.Font = "GothamBold"; Instance.new("UICorner", bExec)
                bExec.MouseButton1Click:Connect(function() pcall(loadstring(sc.script)) end)
            end
        end
    end
end

-- [ARRASTE]
local function drag(gui)
    local d, ds, sp; gui.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then d = true; ds = i.Position; sp = gui.Position end end)
    userInput.InputChanged:Connect(function(i) if d and i.UserInputType == Enum.UserInputType.MouseMovement then local dl = i.Position - ds; gui.Position = UDim2.new(sp.X.Scale, sp.X.Offset + dl.X, sp.Y.Scale, sp.Y.Offset + dl.Y) end end)
    gui.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 then d = false end end)
end
drag(main); drag(miniBar)

-- [BOTÕES]
minimizeBtn.MouseButton1Click:Connect(function() main.Visible = false; miniBar.Visible = true end)
maximizeBtn.MouseButton1Click:Connect(function() miniBar.Visible = false; main.Visible = true end)
searchBtn.MouseButton1Click:Connect(function() fetch(searchInput.Text) end)
refreshBtn.MouseButton1Click:Connect(function() fetch(searchInput.Text) end)
execBtn.MouseButton1Click:Connect(function() pcall(loadstring(editor.Text)) end)
clearBtn.MouseButton1Click:Connect(function() editor.Text = "" end)

-- [SIDEBAR]
local sidebar = Instance.new("Frame", main); sidebar.Size = UDim2.new(0, 60, 1, 0); sidebar.BackgroundColor3 = Color3.fromRGB(15, 16, 28); Instance.new("UICorner", sidebar)
local function addSBtn(ic, f)
    local b = Instance.new("TextButton", sidebar); b.Size = UDim2.new(0, 45, 0, 45); b.Text = ic; b.BackgroundColor3 = Color3.fromRGB(25, 27, 45); b.TextColor3 = Color3.new(1,1,1); b.Font = "GothamBold"; b.TextSize = 22; Instance.new("UICorner", b); b.MouseButton1Click:Connect(f)
end
Instance.new("UIListLayout", sidebar).HorizontalAlignment = "Center"; sidebar.UIListLayout.Padding = UDim.new(0, 20)
addSBtn("🔍", function() benchPage.Visible = false; searchPage.Visible = true end)
addSBtn("🛠️", function() searchPage.Visible = false; benchPage.Visible = true end)

-- [LOADING]
local loader = Instance.new("Frame", sg); loader.Size = UDim2.new(0, 400, 0, 280); loader.Position = UDim2.new(0.5, -200, 0.5, -140); loader.BackgroundColor3 = Color3.fromRGB(20, 22, 35); Instance.new("UICorner", loader)
local playerImg = Instance.new("ImageLabel", loader); playerImg.Size = UDim2.new(0, 80, 0, 80); playerImg.Position = UDim2.new(0.5, -40, 0.1, 0); playerImg.Image = "https://www.roblox.com/headshot-thumbnail/image?userId="..player.UserId.."&width=420&height=420&format=png"; Instance.new("UICorner", playerImg).CornerRadius = UDim.new(1, 0)
local congratsTxt = Instance.new("TextLabel", loader); congratsTxt.Size = UDim2.new(1, 0, 0, 30); congratsTxt.Position = UDim2.new(0, 0, 0.42, 0); congratsTxt.BackgroundTransparency = 1; congratsTxt.TextColor3 = Color3.new(1,1,1); congratsTxt.Font = "GothamBold"; congratsTxt.TextSize = 16; congratsTxt.Text = "Welcome, " .. player.DisplayName
local barBg = Instance.new("Frame", loader); barBg.Size = UDim2.new(0.8, 0, 0, 10); barBg.Position = UDim2.new(0.1, 0, 0.55, 0); barBg.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
local barFill = Instance.new("Frame", barBg); barFill.Size = UDim2.new(0,0,1,0); barFill.BackgroundColor3 = Color3.fromRGB(85, 95, 210)
local langFrame = Instance.new("Frame", loader); langFrame.Size = UDim2.new(1, 0, 0, 100); langFrame.Position = UDim2.new(0, 0, 0.65, 0); langFrame.BackgroundTransparency = 1; langFrame.Visible = false
Instance.new("UIListLayout", langFrame).FillDirection = "Horizontal"; langFrame.UIListLayout.HorizontalAlignment = "Center"; langFrame.UIListLayout.Padding = UDim.new(0, 10)

local function startHub(lang)
    currentLang = lang
    local t = translations[lang]
    searchBtn.Text = t.search; refreshBtn.Text = t.refresh; execBtn.Text = t.execute; clearBtn.Text = t.clear
    congratsTxt.Text = t.congrats .. ", " .. player.DisplayName
    task.wait(0.5); loader:Destroy(); main.Visible = true
    task.spawn(function() while task.wait() do 
        ledStroke.Color = Color3.fromHSV(tick() % 5 / 5, 1, 1) 
        miniStroke.Color = Color3.fromHSV(tick() % 5 / 5, 1, 1)
    end end)
    fetch(cleanGameName)
end

local function addLang(flag, langCode)
    local b = Instance.new("TextButton", langFrame); b.Size = UDim2.new(0, 60, 0, 40); b.BackgroundColor3 = Color3.fromRGB(35, 38, 60); b.Text = flag; b.TextSize = 25; Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function() startHub(langCode) end)
end
addLang("🇧🇷", "PT"); addLang("🇺🇸", "EN"); addLang("🇪🇸", "ES")

task.spawn(function()
    for i = 0, 1, 0.05 do barFill.Size = UDim2.new(i, 0, 1, 0); task.wait(0.05) end
    barBg.Visible = false; langFrame.Visible = true
end)
