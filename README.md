# 📘 EB QueryBuilder — Manual do Usuário

**Versão 2.0**  
**Desenvolvedor:** [Eliemar Bueno](https://br.linkedin.com/in/eliemar-bueno)  
**Tecnologia:** Python + PySide6 (Qt)  
**Licença:** Gratuito para uso pessoal e profissional

---

## Sumário

1. [Apresentação](#1-apresentação)
2. [O que é o EB QueryBuilder](#2-o-que-é-o-eb-querybuilder)
3. [Principais Funcionalidades](#3-principais-funcionalidades)
4. [Pré-requisitos](#4-pré-requisitos)
5. [Instalação](#5-instalação)
6. [Configuração](#6-configuração)
7. [Como Usar — Passo a Passo](#7-como-usar--passo-a-passo)
   - [7.1 Carregar um Modelo PDM](#71-carregar-um-modelo-pdm)
   - [7.2 Aba 1 — JOINs (Tabelas e Relacionamentos)](#72-aba-1--joins-tabelas-e-relacionamentos)
   - [7.3 Aba 2 — Campos / SELECT](#73-aba-2--campos--select)
   - [7.4 Aba 3 — Critérios / WHERE](#74-aba-3--critérios--where)
   - [7.5 Importar SQL Existente](#75-importar-sql-existente)
   - [7.6 Dialetos SQL Suportados](#76-dialetos-sql-suportados)
8. [Funcionalidades Detalhadas](#8-funcionalidades-detalhadas)
9. [Contribuição e Apoio](#12-contribuição-e-apoio)

---

## 1. Apresentação

O **EB QueryBuilder** é uma ferramenta visual e **100% offline** para geração de consultas SQL a partir de modelos físicos do **SAP Power Designer** (arquivos `.pdm`).

Se você trabalha com bancos de dados relacionais e utiliza o Power Designer para modelagem de dados, esta ferramenta vai economizar horas do seu dia — eliminando a digitação manual de SQL e evitando erros de sintaxe.

> *"Economize tempo e proteja a segurança dos seus dados com um gerador SQL confiável que roda totalmente offline."*

---

## 2. O que é o EB QueryBuilder

O EB QueryBuilder (anteriormente PD SQL Builder) é um **construtor visual de consultas SQL** que:

- ✅ **Lê arquivos `.pdm`** do SAP Power Designer (formato XML)
- ✅ **Extrai automaticamente** todas as tabelas, colunas, chaves primárias e estrangeiras
- ✅ **Monta consultas `SELECT ... FROM ... JOIN ... WHERE`** visualmente, sem digitar SQL
- ✅ **Detecta automaticamente as condições `ON`** dos JOINs através das chaves estrangeiras do modelo
- ✅ **Suporta múltiplos dialetos SQL**: Oracle, PostgreSQL, SQL Server e MySQL
- ✅ **Gera SQL em tempo real** — cada alteração reflete imediatamente no preview
- ✅ **Permite importar SQL existente** para continuar a edição visualmente
- ✅ **Funciona 100% offline** — seus dados nunca saem da sua máquina

---

## 3. Principais Funcionalidades

| Funcionalidade | Descrição |
|---|---|
| **Carregamento de modelos PDM** | Abre arquivos `.pdm` (XML) do SAP Power Designer |
| **Conversão de PDM binário** | Detecta e converte arquivos binários para XML (Windows c/ Power Designer instalado) |
| **Seleção visual de tabelas** | Navegue por todas as tabelas do modelo com busca por nome/código |
| **Auto-JOIN por chave estrangeira** | As condições `ON` são geradas automaticamente a partir das FKs do modelo |
| **Navegação de relacionamentos** | Mostra tabelas pais (⬆) e filhas (⬇) sugeridas para cada tabela selecionada |
| **Múltiplos dialetos SQL** | Oracle, PostgreSQL, SQL Server e MySQL |
| **Preview SQL em tempo real** | O SQL gerado é atualizado automaticamente a cada alteração |
| **Alias de colunas** | Nome conceitual, físico ou alias personalizado com prefixo/sufixo |
| **Batch prefixo/sufixo** | Aplica prefixo/sufixo em várias colunas de uma vez |
| **Reordenação de colunas** | Move colunas para cima/baixo no SELECT |
| **Colunas duplicadas** | Mesma coluna pode aparecer múltiplas vezes com sufixos `_2`, `_3` |
| **Construtor WHERE** | Condições com operadores `=`, `IN`, `LIKE` e lógica `AND`/`OR` |
| **Importar SQL** | Cole um SQL existente para popular as abas automaticamente |
| **Copiar SQL** | Um clique para copiar o SQL gerado para a área de transferência |
| **Configuração de exibição** | Mostrar/ocultar nome conceitual, código físico e alias nas listas |
| **Tela de abertura (Splash)** | Apresentação inicial com informações do aplicativo |
| **Feedback e Apoio** | Diálogo com QR Code PIX para contribuições voluntárias |

---

## 4. Pré-requisitos

### Para executar o código-fonte

| Requisito | Versão mínima |
|---|---|
| Python | 3.13+ |
| PySide6 | 6.8.0+ |
| lxml | 5.3.0+ |

### Para executar o binário compilado

- **Windows:** Nenhum — o executável é autossuficiente
- **Linux:** Bibliotecas Qt do sistema (`libxcb-xinerama0`, `libgl1-mesa-glx`)

---

## 5. Instalação

### Opção 1: Executável compilado (recomendado)

Baixe o executável da versão mais recente e execute diretamente — sem necessidade de instalar Python ou dependências.
[Versão para Linux](https://github.com/eliemarbueno/releases/releases/download/Latest/EbQueryBuilderFull_Linux.zip)
[Versão para Windows](https://github.com/eliemarbueno/releases/releases/download/Latest/EbQueryBuilderFull_Linux.zip)
---

## 6. Configuração

O arquivo `config.ini` (na raiz do projeto ou ao lado do executável) permite personalizar:

```ini
[app]
default_model   = Nome do Modelo
default_dialect = oracle          ; oracle | postgres | sqlserver | mysql

[powerdesigner]
install_dir = C:\caminho\PowerDesigner16   ; necessário para converter .pdm binário

[models]
; Formato: Nome = caminho\do\arquivo.pdm | dialeto (opcional)
Meu Modelo      = C:\Modelos\meu_modelo.pdm | oracle
Sistema Legado  = C:\Modelos\legado.pdm     | sqlserver
```

> 💡 **Dica:** Você pode definir um dialeto diferente para cada modelo! Se omitido, usa o `default_dialect`.
>              Se você colocar os `arquivos.pdm` e apagar o arquivo `config.ini` dentro da pasta onde o executável está, a aplicação vai pesquisar por todos os subdiretórios para preparar identificar os possíveis odelos que você tem acesso e trabalha e se configurar.

---

## 7. Como Usar — Passo a Passo

### 7.1 Carregar um Modelo PDM

1. Selecione o modelo desejado no combo superior
2. Clique em **"Carregar modelo"**
3. O aplicativo lê o arquivo `.pdm` e extrai todas as tabelas, colunas e relacionamentos
4. As três abas (JOINs, Campos, WHERE) são populadas automaticamente

> ⚠️ Se o arquivo estiver no formato **binário** do Power Designer, o botão **"Converter binário → XML"** aparecerá. Clique nele para converter (requer Power Designer instalado no Windows).
> Importante lembrar que nem sempre a conversão garante que o aplicativo vai conseguir ler o arquivo .pdm.

### 7.2 Aba 1 — JOINs (Tabelas e Relacionamentos)

**Objetivo:** Definir quais tabelas farão parte da consulta e como elas se relacionam.

1. **Painel esquerdo** — "Tabelas disponíveis": lista todas as tabelas do modelo. Use o campo de busca para filtrar.
2. Selecione uma tabela e clique **"→ Adicionar"** (ou dê duplo-clique)
3. Um diálogo será exibido para configurar:
   - **Alias** da tabela (sugerido automaticamente com 3 letras)
   - **Tipo de JOIN** (INNER, LEFT, RIGHT)
   - **Condições ON** (detectadas automaticamente via FK, mas editáveis)
4. **Painel direito (superior)** — "Tabelas na consulta": mostra a tabela principal `[FROM]` e as tabelas adicionadas com JOIN
5. **Painel direito (inferior)** — "Tabelas sugeridas": mostra pais (⬆) e filhas (⬇) da tabela selecionada, facilitando a navegação pelos relacionamentos
6. Use **"✎ Editar alias"** para renomear o alias de uma tabela já adicionada

### 7.3 Aba 2 — Campos / SELECT

**Objetivo:** Selecionar as colunas que aparecerão no `SELECT`.

1. Selecione a tabela no combo (com busca)
2. Escolha as colunas desejadas na lista
3. Clique **"→ Adicionar"** ou dê duplo-clique para adicionar
4. Use **"⇨ Adicionar todos"** para incluir todas as colunas de uma vez
5. Colunas com `🔑` são chaves primárias
6. **Reordene** as colunas com **"↑ Subir"** e **"↓ Descer"**
7. **Configure** cada coluna com **"✎ Configurar coluna"**:
   - **Nome Conceitual** — ex: `Employee ID` → vira `Employee_ID`
   - **Nome Físico** — ex: `EMPLOYEE_ID` (sem alias)
   - **Alias personalizado** — você define o nome
   - **Prefixo/Sufixo** — aplicado em lote ou individualmente

> 💡 A mesma coluna pode ser adicionada múltiplas vezes (sufixos `_2`, `_3`...).

### 7.4 Aba 3 — Critérios / WHERE

**Objetivo:** Adicionar filtros à consulta.

1. Selecione a **tabela** (com busca)
2. Selecione a **coluna** (com busca)
3. Escolha o **operador**: `=`, `IN` ou `LIKE`
4. Digite o **valor**
5. Escolha o **conectivo lógico**: `AND` ou `OR` (para o próximo critério)
6. Clique **"Adicionar critério"**

Os critérios aparecem na lista abaixo e podem ser removidos individualmente.

### 7.5 Importar SQL Existente

Se você já tem um SQL pronto e quer editá-lo visualmente:

1. Clique em **"Importar SQL"**
2. Cole o SQL no diálogo
3. Clique em **OK**
4. O aplicativo analisa o SQL e preenche automaticamente:
   - **Aba JOINs** → tabelas e joins
   - **Aba Campos** → colunas do SELECT
   - **Aba WHERE** → condições de filtro
5. Se já existir uma consulta em andamento, você pode **Substituir** ou **Mesclar**

> Itens importados são marcados com `📥` para fácil identificação.

### 7.6 Dialetos SQL Suportados

| Dialeto | Quoting de identificadores | Exemplo |
|---|---|---|
| **Oracle** | `"aspas duplas"` | `SELECT t1."EMPLOYEE_ID"` |
| **PostgreSQL** | `"aspas duplas"` | `SELECT t1."EMPLOYEE_ID"` |
| **SQL Server** | `[colchetes]` | `SELECT t1.[EMPLOYEE_ID]` |
| **MySQL** | `` `crases` `` | `` SELECT t1.`EMPLOYEE_ID` `` |

Basta selecionar o dialeto desejado no combo superior — o SQL é regenerado automaticamente.

---

## 8. Funcionalidades Detalhadas

### 🔍 Navegação Inteligente por Relacionamentos

Ao selecionar uma tabela na aba JOINs, o painel "Tabelas sugeridas" mostra automaticamente:
- **⬆ Tabelas pais** (tabelas referenciadas por chaves estrangeiras da tabela atual)
- **⬇ Tabelas filhas** (tabelas que referenciam a tabela atual)

Isso permite construir consultas complexas com múltiplos JOINs de forma intuitiva.

### 🎨 Display Customizável

Você pode controlar como tabelas e colunas são exibidas nas listas:
- **Nome conceitual** — o nome lógico do Power Designer (ex: "Employees")
- **Código físico** — o nome real no banco de dados (ex: "EMPLOYEES")
- **Alias** — o alias definido na consulta (ex: "emp")

### 🔄 Conversão de PDM Binário

O Power Designer pode salvar modelos em formato binário. O EB QueryBuilder:
1. Detecta automaticamente arquivos binários pela assinatura `PDM_DATA-MODEL_BIN`
2. Oferece conversão via Power Designer instalado (Windows)
3. Usa múltiplas estratégias de conversão (COM automation, VBScript)

### 📋 SQL Import Inteligente

O importador de SQL:
- Reconhece `SELECT`, `FROM`, `JOIN` (INNER/LEFT/RIGHT), `WHERE`
- Resolve tabelas e colunas contra o modelo PDM carregado (case-insensitive)
- Detecta subconsultas e avisa o usuário
- Valida se tabelas/colunas existem no modelo
- Oferece substituir ou mesclar com consulta existente

---

## 9. Contribuição e Apoio

### 💡 Contribua com Ideias

O EB QueryBuilder é um projeto em constante evolução. Se você tem ideias para novas funcionalidades, encontrou algum bug ou quer sugerir melhorias, sua contribuição é muito bem-vinda!

Entre em contato pelo LinkedIn:
- **🔗 [Eliemar Bueno](https://br.linkedin.com/in/eliemar-bueno)**

### ☕ Apoie o Desenvolvimento com PIX

Este software foi desenvolvido de forma independente, **100% offline** e **sem anúncios**. Se ele ajudou você no seu trabalho, considere fazer uma contribuição voluntária para manter o projeto ativo e em evolução.

**Chave PIX (aleatória):** `604f7cf3-dfae-4a17-afda-ff63cf2b7e5f`

Você pode escanear o QR Code diretamente no aplicativo através do botão **"Feedback e Apoio"** na janela principal.

> 🙏 **Obrigado por usar o EB QueryBuilder!**  
> Sua contribuição, seja financeira ou com ideias, ajuda a manter esta ferramenta gratuita e cada vez melhor.

---

*Documentação gerada em julho de 2026 • EB QueryBuilder v2.0*
