# Conguitos UI Library

Uma biblioteca de interface de usuário elegante e leve para Roblox Lua.

## Índice

- [Instalação](#instalação)
- [Introdução Rápida](#introdução-rápida)
- [Exemplos](#exemplos)
  - [Exemplo Básico](#exemplo-básico)
  - [Interface Completa](#interface-completa)
- [API de Referência](#api-de-referência)
  - [Inicialização](#inicialização)
  - [Tabs](#tabs)
  - [Seções](#seções)
  - [Elementos](#elementos)
    - [Botões](#botões)
    - [Toggles](#toggles)
    - [Sliders](#sliders)
    - [Dropdowns](#dropdowns)
    - [Caixas de Texto](#caixas-de-texto)
  - [Notificações](#notificações)
- [Personalização](#personalização)
- [Atalhos de Teclado](#atalhos-de-teclado)
- [Compatibilidade](#compatibilidade)
- [Contribuição](#contribuição)
- [Créditos](#créditos)

## Instalação

### Método 1: Usar o módulo diretamente

1. Insira o script `conguitosui.lua` no seu jogo na pasta `ReplicatedStorage` ou outro local acessível
2. Require o módulo em seu script:

```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local ConguitosUI = require(ReplicatedStorage:WaitForChild("conguitosui"))
```

### Método 2: Usar GitHub

```lua
local ConguitosUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/TheRealGuaxi/Conguitos-UI-Library/refs/heads/main/conguitosuiofuscado.lua"))()
```

## Introdução Rápida

Crie uma interface básica com apenas algumas linhas de código:

```lua
-- Inicializa a UI
local UI = ConguitosUI.new("Meu Script")

-- Cria uma aba
local mainTab = UI:CreateTab("Principal")

-- Adiciona elementos à aba
UI:CreateButton(mainTab, "Clique em Mim", function()
    print("Botão clicado!")
end)

UI:CreateToggle(mainTab, "Ativar Recurso", false, function(state)
    print("Toggle alterado para:", state)
end)
```

## Exemplos

### Exemplo Básico

```lua
local ConguitosUI = require(game:GetService("ReplicatedStorage"):WaitForChild("conguitosui"))

-- Criar uma nova interface
local UI = ConguitosUI.new("Meu Primeiro Script")

-- Criar uma aba
local mainTab = UI:CreateTab("Principal")

-- Criar uma seção na aba
local settingsSection = UI:CreateSection(mainTab, "Configurações")

-- Adicionar um botão
UI:CreateButton(settingsSection, "Dizer Olá", function()
    print("Olá, mundo!")
end)

-- Adicionar um toggle
local autoFarmToggle = UI:CreateToggle(settingsSection, "Auto Farm", false, function(state)
    if state then
        print("Auto Farm ativado")
    else
        print("Auto Farm desativado")
    end
end)

-- Adicionar um slider
local speedSlider = UI:CreateSlider(settingsSection, "Velocidade", 1, 100, 50, function(value)
    print("Velocidade definida para:", value)
end)
```

### Interface Completa

Aqui está um exemplo mais completo que demonstra todos os recursos da biblioteca Conguitos UI:

```lua
local ConguitosUI = require(game:GetService("ReplicatedStorage"):WaitForChild("conguitosui"))

-- Configurar tema personalizado
local customTheme = {
    Primary = Color3.fromRGB(30, 30, 40),
    Secondary = Color3.fromRGB(40, 40, 50),
    Accent = Color3.fromRGB(80, 120, 240),
    Text = Color3.fromRGB(240, 240, 250),
    TextDim = Color3.fromRGB(180, 180, 190)
}

-- Criar uma nova interface com o tema personalizado
local UI = ConguitosUI.new("Conguitos Demo", customTheme)

-- Criar múltiplas abas
local mainTab = UI:CreateTab("Principal")
local combatTab = UI:CreateTab("Combate", "rbxassetid://id") 
local farmTab = UI:CreateTab("Farm", "rbxassetid://id")
local settingsTab = UI:CreateTab("Configurações", "rbxassetid://id")

-- Aba Principal
local mainSection1 = UI:CreateSection(mainTab, "Informações")
local mainSection2 = UI:CreateSection(mainTab, "Ações Rápidas")

UI:CreateButton(mainSection1, "Mostrar Jogadores", function()
    local playersText = "Jogadores no servidor:"
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        playersText = playersText .. "\n- " .. player.Name
    end
    UI:CreateNotification("Jogadores", playersText, 5)
end)

-- Criar alguns toggles
UI:CreateToggle(mainSection2, "Modo Invisível", false, function(state)
    print("Invisibilidade:", state)
end)

UI:CreateToggle(mainSection2, "ESP de Jogadores", false, function(state)
    print("ESP de Jogadores:", state)
end)

-- Aba Combate
local combatSection1 = UI:CreateSection(combatTab, "Aimbot")
local combatSection2 = UI:CreateSection(combatTab, "Outros")

UI:CreateToggle(combatSection1, "Ativar Aimbot", false, function(state)
    print("Aimbot:", state)
end)

UI:CreateDropdown(combatSection1, "Alvo", {"Cabeça", "Torso", "Pernas"}, "Cabeça", function(value)
    print("Alvo do aimbot:", value)
end)

UI:CreateSlider(combatSection1, "Distância", 10, 500, 100, function(value)
    print("Distância do aimbot:", value)
end)

UI:CreateToggle(combatSection2, "Kill Aura", false, function(state)
    print("Kill Aura:", state)
end)

UI:CreateSlider(combatSection2, "Raio da Aura", 5, 50, 15, function(value)
    print("Raio da Kill Aura:", value)
end)

-- Aba Farm
local farmSection = UI:CreateSection(farmTab, "Configurações de Farm")

local autoFarm = UI:CreateToggle(farmSection, "Auto Farm", false, function(state)
    print("Auto Farm:", state)
end)

UI:CreateDropdown(farmSection, "Tipo de Farm", {"Moedas", "XP", "Recursos", "Inimigos"}, "Moedas", function(value)
    print("Tipo de farm selecionado:", value)
end)

UI:CreateTextbox(farmSection, "Delay (ms)", "200", function(text)
    local number = tonumber(text)
    if number then
        print("Delay de farm definido para:", number, "ms")
    else
        UI:CreateNotification("Erro", "Insira um número válido para o delay", 3)
    end
end)

-- Aba Configurações
local uiSection = UI:CreateSection(settingsTab, "Interface")
local miscSection = UI:CreateSection(settingsTab, "Diversos")

UI:CreateToggle(uiSection, "Notificações", true, function(state)
    print("Notificações:", state)
end)

local keybindInput = UI:CreateTextbox(uiSection, "Tecla", "RightShift", function(text)
    print("Nova tecla de atalho:", text)
end)

UI:CreateButton(miscSection, "Resetar Configurações", function()
    autoFarm:SetState(false)
    keybindInput:SetText("RightShift")
    UI:CreateNotification("Sucesso", "Configurações resetadas!", 3)
end)

UI:CreateButton(miscSection, "Reportar Bug", function()
    UI:CreateNotification("Info", "Envie bugs para nosso Discord!", 3)
end)

-- Botão para mostrar como criar uma notificação personalizada
UI:CreateButton(miscSection, "Testar Notificação", function()
    UI:CreateNotification("Conguitos UI", "Notificação de teste criada com sucesso!", 3)
end)
```

## API de Referência

### Inicialização

#### `ConguitosUI.new(title, theme)`

Cria uma nova instância de interface Conguitos UI.

- `title` (string): O título exibido na barra de título da interface
- `theme` (table, opcional): Uma tabela com cores personalizadas para a UI
  - `Primary`: Cor de fundo principal
  - `Secondary`: Cor de fundo secundária
  - `Accent`: Cor de destaque
  - `Text`: Cor de texto principal
  - `TextDim`: Cor de texto secundária

Retorna a instância da interface.

```lua
local UI = ConguitosUI.new("Meu Script")

-- Com tema personalizado
local UI = ConguitosUI.new("Meu Script", {
    Primary = Color3.fromRGB(30, 30, 40),
    Secondary = Color3.fromRGB(40, 40, 50),
    Accent = Color3.fromRGB(65, 105, 225),
    Text = Color3.fromRGB(240, 240, 250),
    TextDim = Color3.fromRGB(180, 180, 190)
})
```

### Tabs

#### `UI:CreateTab(name, icon)`

Cria uma nova aba na interface.

- `name` (string): Nome da aba
- `icon` (string, opcional): ID do ícone da aba (rbxassetid)

Retorna uma referência à aba criada.

```lua
local mainTab = UI:CreateTab("Principal")
local settingsTab = UI:CreateTab("Configurações", "rbxassetid://id")
```

#### `UI:SelectTab(name)`

Seleciona uma aba específica para ser exibida.

- `name` (string): Nome da aba a ser selecionada

```lua
UI:SelectTab("Configurações")
```

### Seções

#### `UI:CreateSection(tab, title)`

Cria uma nova seção em uma aba.

- `tab` (referência ou string): A aba ou nome da aba onde a seção será criada
- `title` (string): Título da seção

Retorna uma referência à seção criada.

```lua
local mainSection = UI:CreateSection(mainTab, "Configurações Gerais")
```

### Elementos

#### Botões

#### `UI:CreateButton(parent, text, callback)`

Cria um botão clicável.

- `parent` (referência): A seção ou aba onde o botão será criado
- `text` (string): Texto exibido no botão
- `callback` (função): Função a ser executada quando o botão for clicado

Retorna uma referência ao botão.

```lua
UI:CreateButton(mainSection, "Clique em Mim", function()
    print("Botão foi clicado!")
end)
```

#### Toggles

#### `UI:CreateToggle(parent, text, default, callback)`

Cria uma opção de alternância (liga/desliga).

- `parent` (referência): A seção ou aba onde o toggle será criado
- `text` (string): Texto descritivo do toggle
- `default` (boolean): Estado inicial (true = ligado, false = desligado)
- `callback` (função): Função chamada quando o estado mudar, recebe o novo estado

Retorna um objeto com métodos para controlar o toggle.

```lua
local myToggle = UI:CreateToggle(mainSection, "Ativar Recurso", false, function(state)
    if state then
        print("Recurso ativado")
    else
        print("Recurso desativado")
    end
end)

-- Controlar programaticamente
myToggle:SetState(true)  -- Liga o toggle
local isOn = myToggle:GetState()  -- Obtém o estado atual
```

#### Sliders

#### `UI:CreateSlider(parent, text, min, max, default, callback)`

Cria um controle deslizante para selecionar valores numéricos.

- `parent` (referência): A seção ou aba onde o slider será criado
- `text` (string): Texto descritivo do slider
- `min` (número): Valor mínimo
- `max` (número): Valor máximo
- `default` (número): Valor inicial
- `callback` (função): Função chamada quando o valor mudar, recebe o novo valor

Retorna um objeto com métodos para controlar o slider.

```lua
local speedSlider = UI:CreateSlider(mainSection, "Velocidade", 0, 100, 50, function(value)
    print("Velocidade ajustada para:", value)
end)

-- Controlar programaticamente
speedSlider:SetValue(75)  -- Define o valor para 75
local currentSpeed = speedSlider:GetValue()  -- Obtém o valor atual
```

#### Dropdowns

#### `UI:CreateDropdown(parent, text, options, default, callback)`

Cria um menu suspenso para seleção entre várias opções.

- `parent` (referência): A seção ou aba onde o dropdown será criado
- `text` (string): Texto descritivo do dropdown
- `options` (tabela): Array com as opções disponíveis
- `default` (string): Opção selecionada inicialmente
- `callback` (função): Função chamada quando uma opção for selecionada, recebe a opção selecionada

Retorna um objeto com métodos para controlar o dropdown.

```lua
local targetDropdown = UI:CreateDropdown(mainSection, "Alvo", {
    "Jogadores", "NPCs", "Recursos", "Todos"
}, "Jogadores", function(option)
    print("Alvo selecionado:", option)
end)

-- Controlar programaticamente
targetDropdown:SetValue("NPCs")  -- Seleciona a opção "NPCs"
local currentTarget = targetDropdown:GetValue()  -- Obtém a opção selecionada

-- Atualizar as opções disponíveis
targetDropdown:UpdateOptions({
    "Jogadores Próximos", "Jogadores Distantes", "Todos os Jogadores"
})
```

#### Caixas de Texto

#### `UI:CreateTextbox(parent, text, placeholder, callback)`

Cria uma caixa de texto para entrada de dados.

- `parent` (referência): A seção ou aba onde a caixa de texto será criada
- `text` (string): Texto descritivo da caixa de texto
- `placeholder` (string): Texto de marcação exibido quando vazio
- `callback` (função): Função chamada quando o texto for alterado, recebe o texto e um booleano indicando se Enter foi pressionado

Retorna um objeto com métodos para controlar a caixa de texto.

```lua
local nameInput = UI:CreateTextbox(mainSection, "Nome", "Digite seu nome...", function(text, enterPressed)
    if enterPressed then
        print("Nome confirmado:", text)
    else
        print("Nome sendo digitado:", text)
    end
end)

-- Controlar programaticamente
nameInput:SetText("Jogador123")  -- Define o texto
local currentName = nameInput:GetText()  -- Obtém o texto atual
```

### Notificações

#### `UI:CreateNotification(title, message, duration)`

Cria uma notificação temporária.

- `title` (string): Título da notificação
- `message` (string): Mensagem da notificação
- `duration` (número, opcional): Duração em segundos (padrão: 3)

Retorna uma referência à notificação criada.

```lua
UI:CreateNotification("Sucesso", "Operação concluída com sucesso!", 5)
```

## Personalização

### Temas

Você pode personalizar a aparência da interface definindo um tema personalizado:

```lua
local UI = ConguitosUI.new("Meu Script", {
    Primary = Color3.fromRGB(35, 35, 45),      -- Cor de fundo principal
    Secondary = Color3.fromRGB(45, 45, 55),    -- Cor de fundo secundária
    Accent = Color3.fromRGB(65, 105, 225),     -- Cor de destaque (azul royal)
    Text = Color3.fromRGB(240, 240, 250),      -- Cor de texto principal
    TextDim = Color3.fromRGB(180, 180, 190)    -- Cor de texto secundária
})
```

### Temas de Exemplo

#### Tema Escuro (padrão)
```lua
local darkTheme = {
    Primary = Color3.fromRGB(40, 40, 50),
    Secondary = Color3.fromRGB(50, 50, 60),
    Accent = Color3.fromRGB(65, 105, 225),
    Text = Color3.fromRGB(240, 240, 250),
    TextDim = Color3.fromRGB(180, 180, 190)
}
```

#### Tema Claro
```lua
local lightTheme = {
    Primary = Color3.fromRGB(230, 230, 235),
    Secondary = Color3.fromRGB(220, 220, 225),
    Accent = Color3.fromRGB(50, 80, 220),
    Text = Color3.fromRGB(40, 40, 45),
    TextDim = Color3.fromRGB(100, 100, 110)
}
```

#### Tema Vermelho
```lua
local redTheme = {
    Primary = Color3.fromRGB(40, 30, 30),
    Secondary = Color3.fromRGB(50, 40, 40),
    Accent = Color3.fromRGB(220, 60, 60),
    Text = Color3.fromRGB(240, 230, 230),
    TextDim = Color3.fromRGB(190, 170, 170)
}
```

#### Tema Verde
```lua
local greenTheme = {
    Primary = Color3.fromRGB(30, 40, 30),
    Secondary = Color3.fromRGB(40, 50, 40),
    Accent = Color3.fromRGB(60, 220, 80),
    Text = Color3.fromRGB(230, 240, 230),
    TextDim = Color3.fromRGB(170, 190, 170)
}
```

## Atalhos de Teclado

- **RightShift** (padrão): Alterna a visibilidade da interface

Para alterar a tecla de atalho, modifique a variável `ToggleKey` no início do script:

```lua
-- No código fonte (conguitosui.lua):
local ToggleKey = Enum.KeyCode.RightShift
```

## Compatibilidade

Conguitos UI foi testado e é compatível com:

- Roblox Studio
- Scripts de lado do cliente
- Scripts locais
- Exploits que suportam interfaces de usuário personalizadas

## Contribuição

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou enviar pull requests.

1. Faça um fork do repositório
2. Crie um branch para sua feature (`git checkout -b feature/nova-funcionalidade`)
3. Faça commit das suas alterações (`git commit -m 'Adiciona nova funcionalidade'`)
4. Faça push para o branch (`git push origin feature/nova-funcionalidade`)
5. Abra um Pull Request

## Créditos

- Desenvolvido por TheRealGuaxi | @guaxi.js
- Inspirado por várias LIBs de UI para Roblox
- Ícones usados nas demos por shibathe9
