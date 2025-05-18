# ConvaiText-Image
# 🔮 Unreal Bot Context Visualizer

Projeto criado em **Unreal Engine 5.3** totalmente em **Blueprints**, que conecta um sistema de diálogo com lógica de contexto e visualização multimídia em um ambiente 3D interativo.

---

## 🎯 Objetivo

Detectar falas de um bot, analisar seu conteúdo e exibir dinamicamente **imagens, vídeos ou modelos 3D** com base em:

- **Contexto geral da fala**
- **Palavras-chave específicas** (inclusive compostas)

---

## 🛠️ Tecnologias e Plugins

- **Unreal Engine 5.3**
- **Blueprint Only** (sem código C++)
- Plugins utilizados:
  - [**Convai Plugin**](https://www.convai.com/) – sistema de diálogo com bots inteligentes
  - [**HttpBlueprint**](https://www.unrealengine.com/marketplace/en-US/product/http-request-blueprints) – requisições HTTP assíncronas para APIs externas

---

## 🌐 Integrações com API

O projeto faz uso das seguintes APIs externas:

### 🤖 Convai API
- Utilizada para controlar a lógica de conversação com o bot
- Evento de fala (`OnBotSpoke`) dispara o processamento de contexto ou palavra-chave

### 📊 Hugging Face Inference API
- **Modelo:** `facebook/bart-large-mnli`
- Endpoint: `https://api-inference.huggingface.co/models/facebook/bart-large-mnli`
- Utilizado para **classificação de intenção/contexto** com base em `candidate_labels` extraídos da Data Table
- A resposta define qual tipo de conteúdo deve ser exibido no mundo

---

## 📚 Estrutura do Projeto

### 🎤 `BotSpeechAnalyzer` (Actor)

- Recebe eventos de fala do bot (via Convai)
- Envia o texto para a API Hugging Face para análise de contexto
- Trata a resposta e decide o tipo de asset a exibir com base em Data Tables

### 🧠 DataTables

#### `DT_ContextAssets`

- Mapeia contextos como `"cidade"`, `"floresta"` etc.
- Define o tipo de asset (imagem, vídeo, modelo 3D)

#### `DT_KeywordAssets`

- Mapeia palavras-chave como `"física quântica"`, `"robótica"` etc.
- Associadas diretamente a um asset (sem depender de contexto geral)

---

## 🖼️ Exibição 3D: `BP_ContextDisplay`

- Usa um `WidgetComponent` no mundo 3D para exibir imagens ou vídeos
- Renderiza:
  - `WBP_DisplayPanel` para UI multimídia
  - Vídeo com `MediaPlayer` e `MediaTexture`
  - Modelos 3D com `StaticMeshComponent` animado com `Timeline`

---

## ⚙️ Lógica Visual

- Detecção de keywords via `Contains (String)` com normalização (sem acentos)
- Exibição animada com `Timeline` para crescimento do modelo 3D
- Evita delay visual com inicialização forçada de visibilidade/escala

---

## 🔄 Fluxo de Execução

1. O bot fala (evento via Convai)
2. O texto é enviado à API Hugging Face com os `candidate_labels` derivados da `DT_ContextAssets`
3. Se uma **keyword** da `DT_KeywordAssets` for detectada:
   - O asset correspondente é exibido
4. Senão, o contexto retornado pela API é utilizado para exibir o asset via `DT_ContextAssets`

---

## 📦 Como Expandir

- Adicionar suporte a múltiplas keywords por entrada
- Usar IA para gerar assets em tempo real (ex: via imagem/vídeo de LLM)
- Integrar sistema de busca ou histórico do que foi mostrado

---

## 🧪 Requisitos

- Unreal Engine 5.3
- Plugins:
  - Convai (ativado no projeto)
  - HttpBlueprint (para requisições HTTP)

---

## 📂 Organização de Pastas (Recomendada)

/Blueprints/               → Lógica principal em Blueprints
/Characters/               → Personagens e suas Blueprint Classes
/ContextObjects/           → Ator e lógica de exibição baseada em contexto e palavras-chave
/Assets/                   → Texturas, malhas e vídeos utilizados
/Developers/               → Dados temporários ou pessoais de devs
/FirstPerson/, /FPWeapon/  → Conteúdo do template First Person
/LevelPrototyping/         → Níveis de teste ou layout
/MetaHumans/               → Personagens MetaHuman

## 📧 Contato

Linkedin: https://www.linkedin.com/in/jesus-abrah%C3%A3o-adad-filho-b68b20141/
