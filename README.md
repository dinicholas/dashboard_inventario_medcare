# MedCare · IT Asset Intelligence 💻🏥

> **Painel Executivo de Inventário e Governança de TI**  
> *Plataforma interativa para monitoramento do parque computacional, triagem de exceções operacionais e gestão de prontidão de ativos.*

---

## 📋 Sumário

- [Visão Geral](#-visão-geral)
- [Funcionalidades Principais](#-funcionalidades-principais)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Estrutura dos Dados (JSON Schema)](#-estrutura-dos-dados-json-schema)
- [Arquitetura & Design](#-arquitetura--design)
- [Seções do Dashboard](#-seções-do-dashboard)
- [Como Executar / Implantar](#-como-executar--implantar)
- [Licença](#-licença)

---

## 📌 Visão Geral

O **MedCare · IT Asset Intelligence** é um dashboard executivo autocontido (*single-file web app*) construído para simplificar e otimizar a gestão de ativos de Tecnologia da Informação. 

O sistema consolida a análise da **frota de computadores atual** (46 equipamentos cadastrados) e faz a gestão segregada do **pipeline de novas aquisições** (18 equipamentos Lenovo de última geração), permitindo que gestores e diretores identifiquem gargalos de hardware, riscos de segurança, desconformidades de licenciamento e necessidades de upgrade sem distorcer as métricas operacionais vigentes.

---

## 🔥 Funcionalidades Principais

- **📊 Dashboard Interativo sem Dependências Externas:** Construído em HTML5, CSS3 puro e JavaScript vanilla (SVG nativo para gráficos). Não necessita de React, Vue, D3.js ou bibliotecas pesadas.
- **🔍 Filtragem Reativa Dinâmica:** Filtre o parque por **Setor**, **Fabricante** ou **Sistema Operacional**. Todos os KPIs, gráficos SVG e tabelas são recalculados instantaneamente via JS.
- **🛡️ Sistema de Scoring de Risco (Fila de Atenção):** Algoritmo interno de pontuação que identifica máquinas vulneráveis combinando fatores como:
  - Sistema operacional desatualizado (*Windows 10* vs *Windows 11*)
  - Memória RAM subdimensionada ($\le 6\text{ GB}$)
  - Armazenamento obsoleto (*HDD* em vez de *SSD*)
  - Ausência de Antivírus (*Trend Micro*)
  - Status de licenciamento do Office não ativado/duvidoso
  - Defeitos físicos relatados (ex: *Tela com defeito*)
- **📦 Gestão Separada de Pipeline (Máquinas Novas):** Segregação estrita dos 18 novos computadores (*Lenovo V15 G4 AMN* e *Lenovo V15 G5 IRL*) mantendo a integridade dos KPIs da frota atual enquanto monitora a prontidão do novo lote.
- **🔍 Modal de Detalhamento por Ativo:** Clique em qualquer equipamento na lista de risco para abrir o modal com especificações detalhadas de hardware e software.
- **🖥️ Modo Apresentação & Impressão/PDF:** Suporte nativo para tela cheia (*Fullscreen API*) e CSS `@media print` estilizado para relatórios executivos em papel ou PDF sem componentes de UI desnecessários (esconde filtros, abas e botões).

---

## 🛠️ Tecnologias Utilizadas

| Camada | Tecnologia | Descrição |
| :--- | :--- | :--- |
| **Front-end UI** | **HTML5 Semântico** | Estrutura leve e acessível. |
| **Estilização** | **CSS3 (Custom Properties)** | Layout responsivo (Grid/Flexbox), Glassmorphism, UI modo escuro (Dark Theme) e estilos dedicados para impressão. |
| **Gráficos** | **SVG Nativos** | Gráficos de barra e donut renderizados via JS vanilla manipulando nós DOM / SVG sem dependências externas. |
| **Lógica / Engine** | **JavaScript (ES6+)** | Manipulação do estado global de dados (`DATA.inventory` e `DATA.new_machines`), cálculo de KPIs, filtragem em tempo real e sistema de modal. |

---

## 📁 Estrutura do Projeto

O projeto adota uma arquitetura **Single-File Application (SFA)**, onde o arquivo `index.html` contém toda a marcação, os estilos e a lógica necessária para o seu funcionamento offline ou via servidor HTTP estático.

```
medcare-it-asset-intelligence/
├── base de dados/
│   └── inventario_computadores_medcare_2026.xlsx  # Base de dados original (Excel)
├── index.html                                    # Arquivo único (HTML + CSS inline + JS + JSON de dados)
└── README.md                                     # Documentação completa do repositório
```

---

## 📊 Estrutura dos Dados (JSON Schema)

Os dados estão embutidos diretamente no script JS no objeto constante `DATA`.

### 1. Frota Atual (`DATA.inventory`)
Array contendo os equipamentos em uso na operação:

```json
{
  "Registro": 17,
  "Usuário": "user",
  "Setor": "OPME",
  "Fabricante": "Dell",
  "Fabricante_norm": "Dell",
  "Marca_Modelo": "DELL Vostro 3401",
  "Modelo_norm": "Vostro 3401",
  "Modelo": "Vostro 3401",
  "Processador": "Intel(R) Core(TM) i3-1005G1 CPU @ 1.20GHz",
  "RAM": 4,
  "Memória_Qtd": "4 GB",
  "Memória_Tipo": "DDR4",
  "HD_Tipo": "SSD 128 GB",
  "SO": "Microsoft Windows 10 Professional (x64) Build 19045.6332 (22H2)",
  "SO_version": "Windows 10",
  "SO_Short": "Windows 10",
  "Office_Versão": "2016 Home&Business",
  "Office_Status": "Ativado",
  "Office_status": "Ativado",
  "Trend_Antivirus": "Não",
  "Antivirus": "Não",
  "obs": "* Tela com defeito",
  "Score": 11,
  "Flags": ["RAM ≤6 GB", "Windows 10", "Antivírus ausente", "Tela com defeito"]
}
```

### 2. Pipeline de Máquinas Novas (`DATA.new_machines`)
Array contendo as máquinas recebidas ou em fase de alocação/preparação:

```json
{
  "Setor": "-",
  "Fabricante": "Lenovo",
  "Marca_Modelo": "LENOVO Lenovo V15 G4 AMN",
  "Processador": "AMD Ryzen 5 7520U with Radeon Graphics",
  "RAM": 8,
  "Memória_Qtd": "8 GB",
  "Memória_Tipo": "DDR5",
  "HD_Tipo": "SSD 256 GB",
  "SO": "Microsoft Windows 11 Professional (x64) Build 26200.7922 (25H2)",
  "Office_Versão": "2016 Home&Business",
  "Office_Status": "Ativado",
  "Trend_Antivirus": "Sim"
}
```

---

## 📐 Arquitetura & Design

- **Dark Theme Executivo:** Desenvolvido com uma paleta de cores escura inspirada em painéis de segurança e operações de TI (`--bg: #07131f`, `--teal: #55dfcd`, `--cyan: #76b8ff`, `--amber: #f0c86a`, `--rose: #f18b97`).
- **Design Responsivo:** Adaptável a telas ultrawide (até `1500px`), notebooks, tablets e smartphones com breakpoints CSS em `1200px`, `860px` e `560px`.
- **Renderização SVG Dinâmica:** Função `drawBar()` e `drawDonut()` que convertem o estado de dados agrupados diretamente em vetores matemáticos `rect`, `circle`, `path` e `text`.

---

## 🖥️ Seções do Dashboard

1. **Visão Executiva (Overview):**
   - **Indicadores de KPI:** Total da Frota, % no Windows 11, % de Licenciamento do Office, Cobertura de Antivírus, % Máquinas com RAM $\le 6\text{ GB}$, e % uso de SSD.
   - **Gráfico de Concentração por Setor:** Barras SVG horizontais filtráveis via clique.
   - **Gráfico de Sistema Operacional:** Donut chart com proporção entre Windows 10 e Windows 11.
   - **Perfil de Memória & Ranking de Modelos:** Gráfico e lista ranqueada dos SKUs mais populares no parque (ex: *Lenovo V14 G4 AMN*, *Dell Vostro 3401*).
   - **Sinais para Decisão:** Callouts com alertas de atenção rápida (ex: vulnerabilidades e conformidade).

2. **Exceções & Riscos:**
   - **Fila de Atenção (Risk Table):** Tabela ordenada pelo algoritmo de *Score de Risco*. Destaca equipamentos críticos que precisam de retrofit, substituição ou correção de software.
   - **Controles de Conformidade:** Mini-métricas consolidadas do estado de proteção do parque.

3. **Máquinas Novas (Pipeline):**
   - Visão isolada das 18 máquinas recém-adquiridas (*Lenovo V15 G4 AMN* e *Lenovo V15 G5 IRL*), permitindo avaliar a padronização do novo lote (100% DDR5, SSD 256GB, Windows 11) antes da distribuição para as áreas de negócio.

---

## 🚀 Como Executar / Implantar

### Opção 1: Execução Local Direct-to-Browser
Como o projeto é 100% autocontido em um arquivo `.html`, você não precisa de Node.js, compiladores ou servidores complexos:
1. Acesse via navegador (Chrome, Edge, Firefox, Safari) pelo endereço:
   ```bash
   https://dinicholas.github.io/dashboard_inventario_medcare/
   ```
2. Ou baixe o arquivo `index.html` e abra diretamente em qualquer navegador moderno (Chrome, Edge, Firefox, Safari).

---

## 📝 Licença

Este projeto é disponibilizado para fins de monitoramento e governança interna de TI. Sinta-se à vontade para adaptar os scripts e a interface para a realidade da sua infraestrutura!
