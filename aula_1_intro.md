# Introdução — Gerenciamento de Dados e Informação
**Autores:** Robson Fidalgo & Valeria Times  
**Instituição:** CIn — UFPE  

---

## 1. Sistema de Informação (SI)

### 1.1 Definição
Coleção de atividades que regulam o **compartilhamento** e a **distribuição** de **informações** e o **armazenamento** de **dados** relevantes ao gerenciamento de uma organização.

- É a **instrumentação** de uma organização.

### 1.2 Tipos de SI

| Tipo | Descrição |
|---|---|
| **Manuais** | Baseados em processos humanos sem automação computacional |
| **Baseados em Computador** | Automatizados, com suporte tecnológico |

### 1.3 Papel dos Sistemas de Informação

Sistemas de Informação são **essenciais** para:
- Criação de **firmas competitivas**
- Gerenciamento de **corporações globais**
- Prover **produtos e serviços** úteis para os clientes

**Fatores que aumentam a importância dos SI:**
- Crescimento da **Internet**
- **Globalização** do Comércio
- Crescimento da **Economia da Informação**

---

## 2. Principais Tipos de Sistemas de Informação

### 2.1 Tabela Completa — Tipos × Níveis Organizacionais

| Nível da Organização | Tipo de Sistema |
|---|---|
| **Estratégico** | Sistemas de Suporte a Executivos (SSE) |
| **Gerencial** | Sistemas de Informações Gerenciais (SIG) |
| **Gerencial** | Sistemas de Suporte à Decisão (SSD) |
| **De Conhecimento** | Sistemas de Automação de Escritórios (SAE) |
| **Operacional** | Sistema de Processamento de Transações (SPT) |
| *(Todos os níveis)* | Bibliotecas Digitais (BDIG) |

### 2.2 Descrição Detalhada de Cada Tipo

#### SPT — Sistemas de Processamento de Transações
- Dão suporte a **operações do dia a dia**.
- Mantêm **registros detalhados**.
- Nível: **Operacional**.

#### SIG — Sistemas de Informação Gerenciais
- Facilitam o **gerenciamento**.
- Produzem **relatórios**.
- Nível: **Gerencial**.

#### SSD — Sistemas de Suporte à Decisão
- Ajudam à **tomada de decisão** em situações **menos estruturadas**.
- Nível: **Gerencial**.

#### SE — Sistemas Especialistas
- Proveem **orientação e assistência** em determinada área do conhecimento.
- Nível: **De Conhecimento**.

#### SAE — Sistemas de Automação de Escritórios
- **Criam, armazenam, modificam e processam** comunicação interpessoal.
- Nível: **De Conhecimento**.

#### SSE — Sistemas de Suporte a Executivos
- Dão suporte às necessidades de informação dos **principais executivos**.
- **Sumarizam e apresentam** dados em níveis mais altos de agregação.
- Nível: **Estratégico**.

#### SSC — Sistemas de Suporte à Colaboração
- Objetivo principal: facilitar a **eficácia de grupos de trabalho**.

#### BDIG — Bibliotecas Digitais
- Armazenamento do acervo na **forma digital**.
- **Comunicação direta** do usuário para obtenção de material.
- Usuário consulta **cópia de uma versão mestre**.

---

## 3. Dados × Informação × Conhecimento

### 3.1 Os Três Elementos

Os três conceitos formam uma hierarquia progressiva de abstração e significado:

```
Desorganização
      ↓  [Estruturação por percepção + Influência externa]
   DADOS
      ↓  [Forte conteúdo semântico]
 INFORMAÇÃO
      ↓  [Processo de apropriação por uma pessoa ou corporação]
CONHECIMENTO
```

### 3.2 Dados

**Definição:** Fatos **registrados**, e que têm um **significado implícito**, sobre fenômenos do mundo real.

- Gravação em **código adequado** de uma observação, de um objeto, de um fenômeno.
- Utilizados para **transmitir, armazenar e deduzir** informações.

> Os dados têm significado implícito — ainda não interpretado ou contextualizado.

### 3.3 Informação

**Definição:** O que pode ser **inferido** dos dados.

- Significado **associado ou deduzido** de um conjunto de dados e de associações entre eles.
- Dado com um **significado visível**, ou seja, que tenha uma estrutura ou que possa ser expresso por meio de uma linguagem.

> A informação é o dado com contexto e estrutura — seu significado se torna explícito.

### 3.4 Conhecimento

**Definição:** Informação **adicional** extraída dos dados ou do **especialista do domínio** da aplicação.

- Informação que é **integrada e entendida** por alguém.

**Papéis do conhecimento:**
1. **Transformar** dados em informações.
2. **Derivar** novas informações de informações existentes.
3. **Adquirir** novos conhecimentos.

> O conhecimento pressupõe a apropriação ativa da informação por uma pessoa ou corporação.

### 3.5 Quadro Comparativo

| Conceito | Característica Central | Processo de Formação |
|---|---|---|
| **Dados** | Significado implícito; fatos brutos | Observação e registro |
| **Informação** | Significado visível; dado contextualizado | Estruturação e associação |
| **Conhecimento** | Compreensão integrada | Apropriação e raciocínio |

---

## 4. Sistemas de Arquivos

### 4.1 Característica Principal
Principal característica é a **replicação** e **isolamento** de dados — criando **"ilhas de informações"**.

### 4.2 Problemas dos Sistemas de Arquivos

| Problema | Descrição |
|---|---|
| **Redundância descontrolada** | Para cada nova aplicação criava-se um novo arquivo |
| **Isolamento** | Aplicações eram escritas para um determinado arquivo — sem integração |
| **Formatos diferentes** | Arquivos possuíam formatos heterogêneos |
| **Inconsistência** | Mesmo dado representado de formas distintas |

**Exemplos de inconsistência de formato:**
- `Sexo = M ou F` e `Sexo = 0 ou 1` (mesmo dado, representações diferentes)
- `Nome CHAR(50)` e `Nome CHAR(40)` (campos com tamanhos diferentes)

**Tecnologia usada na época:** Linguagens de programação como **COBOL** e **PL/I**.

### 4.3 Sistemas de Arquivos vs. SGBD (diagrama conceitual)

```
SISTEMAS DE ARQUIVOS                    SGBD
────────────────────────────────────────────────────────────
Sis. Produção → Arq. Produção           Sis. Produção ─┐
                 [Produto ...]                           │
Sis. Vendas   → Arq. Vendas             Sis. Vendas   ──┼→ [Banco de Dados]
                 [Produto ...]                           │    [Produto ...]
Sis. Compras  → Arq. Compras            Sis. Compras ──┘         ↑
                 [Produto ...]                                   SGBD

Produto aparece TRIPLICADO              Produto aparece UMA VEZ (compartilhado)
```

---

## 5. Sistemas de Gerenciamento de Banco de Dados (SGBD)

### 5.1 Definição
Consistem em uma **coleção de dados inter-relacionados** e de um **conjunto de programas** para acessá-los.

- A coleção de dados contém informações sobre um **empreendimento particular**.
- Essa coleção é chamada de **Banco de Dados**.

### 5.2 Banco de Dados (BD)

**Definição:** Conjunto de dados **inter-relacionados**, **estruturados**, que são **confiáveis**, **coerentes** e **compartilhados** por usuários que têm necessidades de informações diferentes.

> ⚠️ **Atenção:** Banco de Dados ≠ Bando de Dados!

#### Instância × Esquema

| Conceito | Definição |
|---|---|
| **Instância (Extensão do BD)** | O conteúdo atual do BD — os dados armazenados em um dado momento |
| **Esquema (Intenção)** | O projeto geral do BD ou descrição dos dados do BD |

#### Níveis de Esquema

| Nível | Dependência | Descrição |
|---|---|---|
| **Conceitual** | Independe de SGBD | Visão abstrata dos dados (ex.: Modelo E/R) |
| **Lógico** | Depende de SGBD | Estrutura em termos do modelo do SGBD (ex.: tabelas relacionais) |
| **Físico** | Descreve estrutura física | Como os dados são armazenados em disco |

### 5.3 Conjunto de Programas do SGBD

O conjunto de programas do SGBD é responsável por:

- **Descrever** — definir a estrutura dos dados
- **Armazenar** — persistir os dados no BD
- **Manipular** — inserir, atualizar, remover dados
- **Consultar** — recuperar dados
- **Tratar** — gerenciar transações, segurança, integridade

### 5.4 Objetivo dos SGBD

Prover um ambiente **conveniente e eficiente** para recuperar e armazenar informações de Bancos de Dados.

**Problemas a eliminar ou reduzir:**
1. **Redundância e inconsistência** de dados
2. **Dificuldade no acesso** aos dados
3. **Isolamento** dos dados
4. **Anomalias de acesso concorrente**
5. **Problemas de segurança**

### 5.5 Abstração de Dados

Simplifica a **interação do usuário** com o sistema.

**Modelo de Dados:** Uma coleção de ferramentas conceituais para **descrição de dados**, **relacionamentos** entre eles, a **semântica dos dados** e **restrições de consistência**.

| Tipo de Modelo | Exemplos |
|---|---|
| **Conceituais** | ER, UML, ... |
| **Lógicos** | Relacional, OO, ... |
| **Físicos** | Estruturas de Memória |

### 5.6 Independência de Dados

**Definição:** Habilidade de **modificar a definição de um esquema em um nível** sem afetar a definição do esquema em um nível mais alto.

| Tipo | Descrição |
|---|---|
| **Independência física de dados** | Modificar o esquema físico sem afetar o esquema lógico |
| **Independência lógica de dados** | Modificar o esquema lógico sem afetar as aplicações externas |

### 5.7 Linguagens do SGBD

| Linguagem | Sigla | Função |
|---|---|---|
| **Linguagem de Definição de Dados** | DDL | Especifica o **esquema** do BD |
| **Linguagem de Manipulação de Dados** | DML | Manipula os dados conforme o modelo de dados |
| **Linguagem de Consulta** | Query Language | Porção da DML voltada para **recuperação** de dados |
| **Linguagem de 4ª Geração** | — | Combina estruturas de controle de linguagens de programação com estruturas para manipulação de BD |

> **Nota:** SGBD relacionais utilizam **SQL**, que integra DDL + DML + Query em uma única linguagem.

### 5.8 Fluxo de Trabalho com SGBD (Ciclo Completo)

```
Realidade Nebulosa
      ↓ [Observação + Organiza Ideias]
   Minimundo
      ↓ [ABD — Administrador de BD — Define]
Modelo Conceitual  ←→  [Descreve Estado do BD]
      ↓ [Descreve]
 Modelo Lógico
      ↓ [Cria]
 Modelo Físico  →→→  BD
                      ↑
                [Atualiza Valores]
```

- **ABD (Administrador de BD):** responsável por definir o Modelo Conceitual a partir da realidade observada.
- O Modelo Físico **cria** o BD; o BD **descreve seu estado** e tem seus **valores atualizados**.

---

## 6. Evolução dos SGBD

### 6.1 Linha do Tempo

```
Sistemas de Arquivos
        ↓
SGBD Hierárquicos  →  SGBD em Rede
        ↓
SGBD Relacionais
        ↓
SGBD Orientados a Objetos
        ↓
SGBD Objeto-Relacional  →  ...  (NoSQL, NewSQL, etc.)
```

### 6.2 Classificação por Gerações

#### Primeira Geração (Fim dos anos 60) — BD Convencionais
- **Hierárquico**
- **Rede**

#### Segunda Geração (Fim dos anos 70) — BD Convencionais
- **Relacional**

#### Terceira Geração (A partir de meados dos anos 80) — BD Não Convencionais
- Modelos semânticos
- Extensões do modelo relacional
- Orientação a objetos
- Objeto-relacionais
- (Pós-relacionais, NoSQL)

---

## 7. Sistemas Baseados no Modelo Hierárquico

### 7.1 Características
- Projetado para **representar hierarquias**.
- Dados organizados em estrutura de **árvore** (pai → filho).
- Exemplos de sistemas: **IMS, UNIVAC 1100, CDC 6000, CYBER 70 e 170**.

### 7.2 Exemplo de Estrutura

```
Cliente (raiz)
├── Mário  |  Av. S.Carlos  |  S.P.
│   ├── Conta: 1234  |  R$ 55,00
│   └── Conta: 7556  |  R$ 3.000,00
├── Rui    |  Rua XV       |  S.Carlos
│   └── Conta: 1333  |  R$ 600,00
└── Silvia |  Av. D.Pedro  |  Itu
    ├── Conta: 5512  |  R$ 350,00
    └── Conta: 7556  |  R$ 3.000,00
```

> **Problema:** A conta 7556 aparece **duplicada** (para Mário e Sílvia) — redundância inevitável no modelo hierárquico.

---

## 8. Sistemas Baseados no Modelo em Redes

### 8.1 Características
- Reconhece a natureza geral de dados como **não-hierárquica**.
- **Construídos** a partir de um modelo definido (padrão CODASYL/DBTG).
- Permite que um nó tenha **múltiplos pais** — eliminando parte da redundância do modelo hierárquico.
- Exemplos: **DBMS10, IDS II, DMS II, IMAGE**.

### 8.2 Exemplo de Estrutura

```
Cliente ──────────────── Conta Bancária
Mário  | Av. S.Carlos | S.P. ─────→ 1234 | R$ 55,00
Rui    | Rua XV       | S.Carlos ─→ 1333 | R$ 600,00
                               └──→ 7556 | R$ 3.000,00
Silvia | Av. D.Pedro  | Itu ──────→ 5512 | R$ 350,00
```

> **Melhoria:** A conta 7556 agora pode ser compartilhada por múltiplos clientes sem duplicação.

---

## 9. Sistemas Relacionais

### 9.1 Características
- Dados representados segundo **tabelas**.
- Modelo formal apoiado na **teoria dos conjuntos** (álgebra relacional).
- **Tecnologia relacional** consolidada como padrão de mercado.
- Exemplos: **DB/2, ORACLE, MySQL, MS SQL Server**.

### 9.2 Exemplo de Estrutura

**Tabela Cliente:**

| nome | rua | cidade |
|---|---|---|
| Mário | Av. S. Carlos | S.P. |
| Rui | Rua XV | S. Carlos |
| Silvia | Av. D. Pedro | Itu |

**Tabela Conta:**

| nro_conta | saldo |
|---|---|
| 1234 | 55,00 |
| 1333 | 600,00 |
| 5512 | 350,00 |
| 7556 | 3.000,00 |

**Tabela Cli_Contas (associação):**

| nome | nro_conta |
|---|---|
| Mário | 1234 |
| Rui | 1333 |
| Rui | 7556 |
| Silvia | 5512 |
| Silvia | 7556 |

> **Vantagem:** Os dados são separados em tabelas distintas sem redundância, e as associações são representadas explicitamente.

---

## 10. Sistemas Orientados a Objetos

### 10.1 Vantagens sobre o Modelo Relacional
- Conceito mais especializado de detalhamento da realidade (**Herança**).
- Conceito de **reutilização**, permitindo maior produtividade.
- Aumentam a **consistência** do resultado da análise.
- Melhor **ligação analista × usuário**.
- Dão suporte mais **flexível** a alterações na realidade.
- Podem enfrentar de forma mais **completa** domínios mais complexos da realidade.
- Possuem maior **continuidade** em todas as fases do ciclo de vida do projeto.

### 10.2 Características Básicas dos Sistemas OO

| Característica | Descrição |
|---|---|
| **Abstração** | De dados e procedimentos — esconde detalhes de implementação |
| **Encapsulamento** | Dados e métodos agrupados em um objeto |
| **Herança** | Subclasses herdam propriedades e comportamentos de superclasses |
| **Comunicação por mensagens** | Objetos interagem enviando mensagens entre si |
| **Polimorfismo** | Mesmo método se comporta de forma diferente em objetos distintos |

### 10.3 SGBD Orientados a Objetos

**Exemplos:** O2, OBJECTSTORE, IRIS, JASMINE.

**Exemplo de estrutura:**

```
Objeto Cliente                    Objeto Conta
┌──────────────────────────┐      ┌──────────────────────────┐
│ A1, A2, ... An           │      │ A1, A2, ... An           │
│ M1, M2, ... Mn           │◄────►│ M1, M2, ... Mn           │
│                          │Troca │                          │
└──────────────────────────┘de msg└──────────────────────────┘

Instâncias:
Mário,  Av. S.Carlos, SP,  [1234]
Rui,    Rua XV, S.Carlos,  [1333, 7556]
Silvia, Av. D. Pedro, Itu, [5512, 7556]

1234, 55.00
1333, 600.00
5512, 350.00
7556, 3000.00
```

> Cada cliente já carrega a lista de suas contas como atributo — diferente do relacional que usa tabela de associação.

---

## 11. Sistemas Objeto-Relacionais

### 11.1 Características
Extensão do modelo relacional com conceitos de orientação a objetos **em contexto SQL**:

- **Extensão de tipo básico** — novos tipos de dados além dos primitivos
- **Objetos complexos** — atributos que são objetos
- **Herança** — em contexto SQL
- **Suporte para regras de produção**

### 11.2 Estrutura
- Usa conceitos de OO **sobre estruturas relacionais** — tabelas com capacidade de objetos.
- Um **SGBD Objeto-Relacional** combina o melhor dos dois mundos: poder expressivo do OO + maturidade do relacional.

### 11.3 Aplicações Típicas

| Aplicação | Descrição |
|---|---|
| **Entretenimento** | Gerenciamento de acervos gráficos e de vídeo |
| **Mercado financeiro** | Problemas de análise de séries de tempo |
| **Ciências** | Bancos de dados científicos |
| **Geografia** | Sistemas de informações geográficas (GIS) |
| **Web** | Dados multimídia frequentemente acessados pela WWW |

**Exemplos de SGBD Objeto-Relacional:** DB2/6000 C/S, **PostgreSQL**, ORACLE 8i / 9i / 10g / 11g / 12c.

---

## 12. Classificação dos SGBD por Geração

### 12.1 Primeira e Segunda Gerações — BD Convencionais

**Sistemas:** Hierárquico, Rede (1ª geração) e Relacional (2ª geração).

**Características:**

| Característica | Descrição |
|---|---|
| **Dados** | Bem estruturados |
| **Tipos de dados** | Simples: Inteiros, Reais, Caracteres, ... |
| **Transações** | Simples e curtas |
| **Acesso** | Por meio de chaves |

**Exemplos de aplicações:**
- Folha de pagamento
- Controle de estoque
- Contas a pagar

### 12.2 Terceira Geração — BD Não Convencionais

**Sistemas:** Modelos semânticos, Extensões relacionais, Orientação a objetos, Objeto-relacionais, Pós-relacionais, NoSQL.

**Características:**

| Característica | Descrição |
|---|---|
| **Dados** | Grande volume de dados estruturados |
| **Tipos de dados** | Complexos: Textos, Gráficos, Imagens, Sons |
| **Transações** | Longas |
| **Acesso** | Caminhos de acesso não triviais |
| **Versionamento** | Controle de versões |

**Exemplos de aplicações:**
- Automação de escritórios
- Projeto assistido por computador (**CAD**)
- Engenharia de software (**CASE**)
- Cartografia

---

## 13. Tecnologia de Banco de Dados

**Definição geral:** Conceitos, Métodos, Ferramentas e Sistemas para o **Gerenciamento** e **Uso** de Bancos de Dados.

### 13.1 Propriedades dos Bancos de Dados

#### Para o Gerenciamento:

| Propriedade | Significado |
|---|---|
| **Durável** | Vida dos dados > vida dos processos (dados persistem além das aplicações) |
| **Confiável** | Integridade, consistência, prevenção de perdas |
| **Independente** | Independência mútua aplicação–BD (mudança em um não afeta o outro) |

#### Para o Uso:

| Propriedade | Significado |
|---|---|
| **Confortável** | Interfaces de alto nível (não exige conhecimento técnico profundo) |
| **Flexível** | Acesso ad-hoc (consultas não previstas antecipadamente) |

#### Sobre os Bancos de Dados:

| Propriedade | Significado |
|---|---|
| **Grandes** | Tamanho dos dados > tamanho da memória (dados em disco) |
| **Integrados** | De/para múltiplas aplicações, com redundância controlada |
| **Multi-usuários** | Acessos paralelos (concorrência gerenciada) |

---

## 14. Resumo Comparativo dos Modelos de SGBD

| Modelo | Geração | Estrutura de Dados | Exemplos |
|---|---|---|---|
| **Arquivos** | Pré-SGBD | Arquivos isolados por aplicação | COBOL, PL/I |
| **Hierárquico** | 1ª | Árvore (pai-filho) | IMS, UNIVAC 1100 |
| **Rede** | 1ª | Grafo (nós com múltiplos pais) | DBMS10, IDS II |
| **Relacional** | 2ª | Tabelas + álgebra relacional | Oracle, MySQL, PostgreSQL |
| **Orientado a Objetos** | 3ª | Objetos com encapsulamento/herança | O2, OBJECTSTORE |
| **Objeto-Relacional** | 3ª | Tabelas + tipos complexos/OO | PostgreSQL, Oracle 8i+ |
| **Pós-Relacional / NoSQL** | 3ª+ | Variado (documento, grafo, chave-valor...) | MongoDB, Cassandra, Neo4j |

---

## 15. Conceitos-Chave para Revisão

### Perguntas de Revisão

1. Qual a definição de **Sistema de Informação** e quais são os dois grandes tipos?
2. Cite e descreva **5 tipos de Sistemas de Informação** e os níveis organizacionais correspondentes.
3. Qual a diferença entre **dados**, **informação** e **conhecimento**? Dê exemplos.
4. Quais os papéis do **conhecimento** no ciclo dados → informação → conhecimento?
5. Por que os **Sistemas de Arquivos** eram problemáticos? Cite pelo menos 3 problemas.
6. O que é um **SGBD**? Qual o seu objetivo principal?
7. Qual a diferença entre **instância** e **esquema** de um banco de dados?
8. Quais os três **níveis de esquema** de um banco de dados? O que cada um representa?
9. O que é **independência de dados**? Qual a diferença entre independência física e lógica?
10. Quais as **linguagens de um SGBD**? O que é DDL, DML e Query Language?
11. Descreva a **evolução dos SGBD** desde os sistemas de arquivos até os modelos atuais.
12. Quais as **características** dos sistemas de 1ª/2ª geração vs. 3ª geração?
13. Quais as **5 características básicas** dos Sistemas Orientados a Objetos?
14. O que diferencia um **SGBD Objeto-Relacional** de um **SGBD Orientado a Objetos**?
15. Liste as **propriedades da Tecnologia de Banco de Dados** para gerenciamento e uso.

### Tabela de Correspondência Rápida

| Sigla | Nome Completo | Função Principal |
|---|---|---|
| SI | Sistema de Informação | Gerenciar informações de uma organização |
| SPT | Sistema de Processamento de Transações | Operações do dia a dia |
| SIG | Sistema de Informação Gerencial | Relatórios gerenciais |
| SSD | Sistema de Suporte à Decisão | Apoio a decisões menos estruturadas |
| SE | Sistema Especialista | Orientação em área do conhecimento |
| SAE | Sistema de Automação de Escritórios | Comunicação interpessoal |
| SSE | Sistema de Suporte a Executivos | Informação estratégica para executivos |
| SSC | Sistema de Suporte à Colaboração | Eficácia de grupos de trabalho |
| BDIG | Biblioteca Digital | Acervo digital acessível ao usuário |
| SGBD | Sistema de Gerenciamento de Banco de Dados | Gerenciar coleções de dados inter-relacionados |
| DDL | Data Definition Language | Definir esquemas do BD |
| DML | Data Manipulation Language | Manipular dados do BD |
| SQL | Structured Query Language | DDL + DML + Query (SGBD relacionais) |
| ABD | Administrador de Banco de Dados | Modelar e administrar o BD |