# Modelando Tabelas relacionais para modelo transacional - estrela

Olá! Este README detalha a jornada de um projeto de análise de dados com Power Bi. O objetivo desse projeto era criar tabelas com modelo estrela (modelo dimensional) a partir de uma tabela relacional (entidade relacionamento).

Será que deu certo? Que deu trabalho isso é certo. Vamos lá!

---

## 🚀 O Ponto de Partida: A Coleta e o Primeiro Olhar

Toda grande análise começa com os dados. Nossa jornada iniciou com a coleta de uma amostra crucial: o dataset **"Financials"**. 

### 1. Coleta dos Dados de Amostra (Financials)
![Imagem do Processo 1](https://github.com/DaDosValle/Imagens/blob/main/1%20-%20Coletando%20so%20dados.png)

Aqui, a matéria-prima foi adquirida. O primeiro contato com a tabela "Financials" nos deu a visão inicial do que tínhamos em mãos.

---

## 🏗️ O Alicerce: Construindo Nossas Fundações de Dados

Com os dados em mãos, o próximo passo foi fundamental: dar forma a eles, organizando-os de maneira lógica e eficiente. Decidimos construir um **modelo dimensional estrela**, separando as informações em tabelas de dimensão (o "quem", "o quê", "quando") e uma tabela de fato.

### 2. Criação das Tabelas Dimensão e Fato
![Imagem do Processo 2](https://github.com/DaDosValle/Imagens/blob/main/2%20-%20Cria%C3%A7%C3%A3o%20das%20tabelas%20dimens%C3%A3o%20e%20fato.png)

A partir da nossa tabela de origem `Financials`, iniciamos a duplicação e renomeação estratégica para dar vida às futuras tabelas de dimensão e à nossa tabela de fato. Esse foi o primeiro passo para organizar o nosso futuro banco de dados analítico.

 ![Imagem do Processo 2.1](https://github.com/DaDosValle/Imagens/blob/main/2.1%20-%20Cria%C3%A7%C3%A3o%20das%20tabelas%20dimens%C3%A3o%20e%20fato.png)

---

## 🗺️ O Mapeamento: Definindo o Papel de Cada Atributo

Com as tabelas criadas, o desafio agora era refinar cada uma delas, garantindo que contivessem apenas os atributos essenciais para o nosso modelo estrela. Cada coluna precisava ter um propósito claro.

### 3. Definição dos Atributos de Cada Tabela

Inicialmente, todas as tabelas partilhavam a mesma configuração de colunas:

* **Product** (texto)
* **Discount Band** (texto)
* **Units Sold** (decimal)
* **Sale Price** (inteiro)
* **Manufacturing Price** (inteiro)
* **COGS** (decimal)
* **Gross Sales** (decimal)
* **Discounts** (decimal)
* **Sale** (decimal)
* **Profit** (decimal)
* **Segment** (texto)
* **Country** (texto)
* **Date** (Data)
* **Month Number** (inteiro)
* **Month Name** (texto)
* **Year** (inteiro)

A partir daqui, cada tabela passou por um processo de lapidação:

#### 3.1 Tabela `F_Vendas`: O Coração das Métricas

Nesta tabela, que representa o centro do nosso modelo dimensional, removemos os atributos que pertenceriam a outras dimensões:
* `gross sale` (texto)
* `Year` (inteiro)
* `Month Name` (Texto)
* `Month Number` (inteiro)

Esses atributos de tempo seriam, futuramente, adicionados à nossa `D_Calendario`.

##### 3.1.1 Adição de Chaves (IDs)

Para conectar nossa tabela de fatos às tabelas de dimensão, adicionamos chaves de identificação essenciais:
* `Id_Produto`
* `SK_Id` (ambos criados via coluna condicional no Power BI)

---

#### 3.2 Tabela `D_Produtos`: O Que Vendemos?

Focamos em manter apenas os dados que descrevem nossos produtos. Removemos métricas e dados de outras dimensões:
* `COGS` (decimal)
* `DATA` (Data)
* `Gross Sales` (decimal)
* `Profit` (decimal)
* `Segment` (texto)
* `Country` (texto)
* `Date` (Data)
* `Month Number` (inteiro)
* `Month Name` (texto)
* `Year` (inteiro)

##### 3.2.1 Adição de Chave para Detalhamento
![Imagem do Processo 3.2.1](https://github.com/DaDosValle/Imagens/blob/main/3.2.1%20-%20Adicionando%20Id%20por%20Coluna%20condicional.png)

Para futuras conexões, incluímos:
* `Id_Produto` (via coluna condicional no Power BI)

---

#### 3.3 Tabela `D_Descontos`: Entendendo as Estratégias

Esta dimensão foi criada para detalhar a banda de desconto e suas características. Removemos os atributos não relacionados:
* `Product` (texto)
* `Units Sold` (decimal)
* `Sale Price` (inteiro)
* `Manufacturing Price` (inteiro)
* `COGS` (decimal)
* `Gross Sales` (decimal)
* `Sale` (decimal)
* `Profit` (decimal)
* `Segment` (texto)
* `Country` (texto)
* `Date` (Data)
* `Month Number` (inteiro)
* `Month Name` (texto)
* `Year` (inteiro)

##### 3.3.1 Agrupamento de Atributos de Desconto

Realizamos um agrupamento para criar atributos sumarizados relevantes para a análise de descontos:
* `Valor Mínimo de Desconto`
* `Valor Máximo de Desconto`
* `Valor Médio de Desconto`
* `Valor Mediano de Desconto`
* `Valor Total de Desconto`
(Todos gerados via coluna condicional no Power BI).

---

#### 3.4 Tabela `D_Paises`: Visão Granular do Negócio

Focada nos detalhes de segmentação, removemos as colunas de contexto geral:
* `Segment` (texto)
* `Country` (texto)

##### 3.4.1 Agrupamento Detalhado
![Imagem do Processo 3.4.1](https://github.com/DaDosValle/Imagens/blob/main/3.4.1%20-Detalhes%20foram%20adicionados%20(VER%20QUAL%20DOS%20DOIS%20COLCOAR).png)

Realizado um agrupamento dos dados para consolidação e simplificação.

---

#### 3.5 Tabela `D_Paises`: Onde Nossas Vendas Acontecem?

Esta dimensão isola as informações geográficas. Removemos:
* `Segment` (texto)
* `Country` (texto)

##### 3.5.1 Adição de Chave para Conexão

Incluído:
* `Id_Produto` (via coluna condicional no Power BI)

---

#### 3.6 Tabela `D_Calendario` (Provisória no Power Query)

Criamos uma versão inicial da tabela de calendário diretamente no Power Query. No entanto, essa foi uma medida provisória para organização visual. A versão final e robusta da tabela `D_Calendario` seria criada usando **DAX**, uma linguagem de fórmulas mais apropriada para esse fim no Power BI.

---

## 🌐 A Linguagem Universal: Padronizando Nomes

Com a estrutura definida, era hora de garantir que todos "falassem a mesma língua".

### 4. Padronização entre Idiomas

Traduzimos os nomes dos atributos, que estavam em inglês conforme a base de dados original, para o português. Isso facilita a compreensão e a colaboração em nosso projeto.

---

## 🗄️ Organização é a Chave: Reorganizando as Colunas

A clareza visual é tão importante quanto a lógica dos dados.

### 5. Reorganização das Colunas

As colunas de cada tabela foram reorganizadas em uma ordem mais intuitiva e que fazia mais sentido para a navegação e análise, otimizando a experiência do usuário.

---

## 📅 O Tempo é Essencial: Criando a Dimensão Calendário

Nenhum modelo estrela estaria completo sem uma dimensão de tempo rica e flexível.

### 6. Criando a Tabela Calendário
![Imagem do Processo 6](https://github.com/DaDosValle/Imagens/blob/main/6.1%20-%20Criando%20a%20tabela%20Calend%C3%A1rio.png)

Esta foi uma etapa fundamental. Criamos a tabela dimensional de calendário (`D_Calendario`) que é a espinha dorsal para análises temporais, permitindo-nos fatiar e analisar os dados por ano, mês, dia, etc.

A tabela foi criada a partir de uma função DAX específica, definindo o intervalo de datas:
`Data_Detalhada = CALENDAR(DATE(2013,09,01), DATE(2014,12,01))`

---

## 🔗 O Poder das Conexões: Estabelecendo Relacionamentos

A verdadeira mágica de um modelo dimensional acontece quando as tabelas se conectam.

### 7. Estabelecendo Relacionamentos (Schema Estrela)
![Imagem do Processo 7](https://github.com/DaDosValle/Imagens/blob/main/7%20-%20Estabelecendo%20Relacionamentos.png)

Nesta etapa crucial, estabelecemos os relacionamentos entre a nossa tabela de fatos (`F_Vendas`) e todas as nossas tabelas de dimensão (`D_Produtos`, `D_Descontos`, etc.). Todos os relacionamentos foram definidos com **cardinalidade de um para N (um para muitos)**, formando o nosso poderoso **Schema Estrela**.

Essa estrutura nos permite fazer análises complexas, filtrar dados de forma eficiente e criar dashboards interativos que respondem a perguntas de negócio com agilidade.


Olá, gostou do readme ou apenas achou "bonitinho"? Chama no [LinkdIn](edin.com/in/fernando-m-do-valle-b653a7349/).
