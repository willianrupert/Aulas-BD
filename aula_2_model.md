# Modelagem Conceitual — Gerenciamento de Dados e Informação
**Autores:** Robson Fidalgo & Valeria Times  
**Instituição:** CIn — UFPE  

---

## 1. Ciclo de Desenvolvimento de Banco de Dados

O desenvolvimento de um banco de dados segue um ciclo iterativo com as seguintes etapas:

1. **Investigação dos Dados** — Levantamento e análise dos dados existentes na organização.
2. **Modelagem dos Dados** ← *etapa central desta aula*
3. **Projeto do Banco de Dados**
4. **Implementação do Banco de Dados**
5. **Monitoramento e Manutenção do BD**

> As etapas permitem retroalimentação: resultados de etapas posteriores podem exigir revisão das anteriores.

---

## 2. Modelagem

**Definição:** Transformar aspectos do mundo real em elementos de um modelo de dados formal.

### Níveis de Modelagem

| Nível | Tipo | Característica |
|---|---|---|
| **Modelagem Conceitual** | Modelo de Dados Genérico | Independente de SGBD |
| **Modelagem Lógica** | Modelo de Dados Específico de SGBD | Dependente do sistema escolhido |

---

## 3. Modelo de Dados

Todo modelo de dados é composto por **três componentes fundamentais**:

### 3.1 Estruturas
- Coleção de tipos de objetos — blocos básicos de construção de modelos.
- Representam valores que são **interpretações de objetos do mundo real** e suas propriedades.
- **Exemplo (Modelo Relacional):** Relações e Domínios.
- Domínios de exemplo: `Naturais`, `Pessoa`; atributos como `Idade`, `Quantidade de Veículos`, `Anos de Trabalho`.

### 3.2 Operadores
- Meio de **manipular e atualizar** os tipos de objetos.
- **Exemplo (Modelo Relacional):** Álgebra Relacional.
- **Operações:**
  - Especificam uma ação — *"O que é para ser feito"*
  - Transformam um **estado de BD em outro estado**
  - Preservam propriedades do esquema do BD e do modelo de dados
- Operações típicas: **Acessar, Inserir, Remover, Atualizar**
- Exemplo prático: consulta via Seleção → *"Qual é a Idade de Maria?"* acessa dados e os armazena em memória principal.

### 3.3 Regras de Integridade (Restrições)
- **Restringem o conjunto de estados válidos** dos tipos de objetos (conteúdo).

#### 3.3.1 Classificação das Restrições

**Por razão:**

| Tipo | Propósito | Exemplo |
|---|---|---|
| **Semânticas (Regras de Negócio)** | Esquemas reflitam situações do mundo real | Um empregado do depto. "Finanças" não pode ter categoria funcional "Engenheiro" |
| **De Integridade** | SGBD restringe estados possíveis do BD | O CPF de todo empregado deve ser único e não pode ser desconhecido |

**Por tipo básico:**

| Tipo | Descrição | Exemplo |
|---|---|---|
| **Implícitas** | Inerentes ao modelo; parte integral das estruturas | Ausência de duplicidade e ordem em Conjuntos e Relações (Modelo Relacional) |
| **Explícitas Estáticas** | Expressam regras para determinar **estados válidos** do BD | O salário de um funcionário deve ser igual ou superior ao salário mínimo nacional |
| **Explícitas Dinâmicas** | Especificam **transições de estados** permitidas (dirigidas a operações) | O salário de um funcionário nunca poderá diminuir, só aumentar |

---

## 4. Modelagem Conceitual — Análise de Dados

### Objetivos da Análise de Dados

1. Determinar os **recursos de dados fundamentais** de uma organização por meio da catalogação dos dados existentes em termos de **entidades e relacionamentos**.
2. Permitir um projeto de uma estrutura de arquivos capaz de dar **apoio a diversas aplicações** relacionadas.
3. Auxiliar o **desenvolvimento ou conversão** de aplicações.
4. Formar uma base para o **controle de dados, segurança e auditoria**.
5. Estabelecer as bases de todos os **fatos relevantes** à organização de dados.
6. Facilitar a **integração das diversas divisões** de uma organização pela indicação dos dados que lhes são comuns.
7. Determinar uma base para **avaliação de SGBD**.

---

## 5. Modelo Entidade-Relacionamento (E/R)

### 5.1 Histórico e Características

- Criado por **Peter Chen em 1976**; posteriormente recebeu extensões (modelo EER — Extended ER).
- O mais **difundido e utilizado** pela comunidade de BD.
- É um **padrão para modelagem conceitual**.
- Modelo **simples** (poucos conceitos).
- Possui **representação gráfica** (fácil compreensão).
- Utilizado para construir modelos conceituais de BD.

### 5.2 Componentes Básicos

| Componente | Correspondência no Minimundo | Símbolo Gráfico |
|---|---|---|
| **Entidades** | Substantivos | Retângulo nomeado |
| **Relacionamentos** | Verbos | Losango nomeado |
| **Atributos** | Complementos | Elipse conectada |

---

## 6. Entidades

### 6.1 Definição
Representação abstrata de **objetos do mundo real** — algo sobre o qual dados são armazenados no BD.

- **Objetos concretos:** cliente, livro, produto...
- **Objetos abstratos:** conta, empréstimo, contrato...

**Exemplo:** Um banco tem contas e clientes → Entidades `Clientes` e `Contas`.

### 6.2 Conjunto de Entidades
- **Definição:** Grupos de entidades com características similares.
- Correspondem aos **substantivos** na descrição informal (minimundo) da análise de dados.
- Cada membro do conjunto é chamado de **instância**.
- **Representação gráfica:** Retângulo nomeado (pode variar em ferramentas CASE distintas).

```
┌──────────┐        ┌──────────┐
│ Clientes │        │  Contas  │
└──────────┘        └──────────┘
```

---

## 7. Relacionamentos

### 7.1 Definição
Representação abstrata de **associação entre objetos do mundo real** para armazenar no BD.

**Exemplo:** No banco, um cliente `c1` é titular da conta `ct3`.

### 7.2 Conjunto de Relacionamentos
- Grupo de relacionamentos **do mesmo tipo**, sobre os quais deseja-se manter informações no BD.
- Corresponde aos **verbos** na descrição informal (minimundo) da análise de dados.
- **Representação gráfica:** Losango nomeado.

```
┌──────────┐    ◇──────────◇    ┌──────────┐
│ Clientes │────│ Movimenta │────│  Contas  │
└──────────┘    ◇──────────◇    └──────────┘
```

### 7.3 Cardinalidade

#### 7.3.1 Conceito Geral
Determina a quantidade (**mínima e máxima**) de ocorrências de relacionamentos que uma instância de entidade pode ter com outras instâncias de entidades.

#### 7.3.2 Cardinalidade Mínima (Participação)
- Determina a **quantidade mínima** de ocorrências de relacionamentos.
- Indica se a participação é **obrigatória ou opcional** (tipo do relacionamento).

| Valor | Significado |
|---|---|
| `Min = 0` | Relacionamento **opcional** ou **parcial** |
| `Min > 0` (na prática, `Min = 1`) | Relacionamento **obrigatório** ou **total** |

> **Regra:** Min ≤ Max. Para efeitos práticos, são relevantes `Min = 0` ou `Min = 1`.

#### 7.3.3 Cardinalidade Máxima
- Determina a **quantidade máxima** de ocorrências de relacionamentos.

| Valor | Significado |
|---|---|
| `Max > 0` | Válido |
| `Max = 1` | Uma instância se associa a **no máximo uma** |
| `Max = n` | Uma instância se associa a **várias** |

> **Regra:** Max ≥ Min. Para efeitos práticos, são relevantes `1` e `n`.

#### 7.3.4 Como Ler a Cardinalidade — "Look Across" vs. "Look Here"

| Leitura | Aplica-se a | Estratégia |
|---|---|---|
| **Cardinalidade Máxima** | Quantidade máxima de associações | **Look Across** (olha para o outro lado) |
| **Cardinalidade Mínima / Participação** | Obrigatoriedade | **Look Here** ou **Look Across** |

- **Total/Obrigatório** → linha dupla (ou traço marcado) no diagrama.
- **Parcial/Opcional** → linha simples.

#### 7.3.5 Tipos de Relacionamentos (pela cardinalidade máxima)

| Tipo | Notação | Descrição |
|---|---|---|
| Um para Um | **1:1** | Uma instância de A se relaciona com no máximo uma de B |
| Um para Vários | **1:N** | Uma instância de A se relaciona com várias de B |
| Vários para Vários | **M:N** | Várias instâncias de A se relacionam com várias de B |

> **Boas práticas:** Nas primeiras iterações do projeto conceitual, pode-se usar apenas as cardinalidades máximas. Contudo, recomenda-se definir as cardinalidades mínimas o quanto antes.

#### 7.3.6 Exemplos de Leitura de Participação (Cardinalidade Mínima)

| Diagrama (forma textual) | Interpretação |
|---|---|
| Cliente ─(N)── Movimenta ──(N)── Conta, com traço em Cliente | Um cliente **precisa** movimentar uma conta para ser cadastrado. Uma conta **não precisa** ser movimentada por um cliente para ser cadastrada. |
| Cliente ─(N)── Movimenta ──(N)── Conta, com traço em Conta | Um cliente **não precisa** movimentar uma conta para ser cadastrado. Uma conta **precisa** ser movimentada por um cliente para ser cadastrada. |
| Traços em ambos | Participação total obrigatória nos dois lados. |
| Sem traços | Participação opcional nos dois lados. |

#### 7.3.7 Exemplos de Leitura de Cardinalidade Máxima

| Diagrama | Interpretação (look across) |
|---|---|
| Cliente ─(1)── Movimenta ──(N)── Conta | Um cliente pode movimentar **várias contas**. Uma conta pode ser movimentada por **no máximo um cliente**. |
| Cliente ─(N)── Movimenta ──(1)── Conta | Um cliente pode movimentar **no máximo uma conta**. Uma conta pode ser movimentada por **vários clientes**. |
| Cliente ─(1)── Movimenta ──(1)── Conta | Máximo de um em ambos os lados. |
| Cliente ─(N)── Movimenta ──(N)── Conta | Vários em ambos os lados. |

### 7.4 Múltiplos Relacionamentos entre as Mesmas Entidades
- Pode existir **mais de um relacionamento** entre as mesmas entidades.
- **Exemplo:** Professor–Disciplina pode ter os relacionamentos `Ministra` e `Coordena` simultaneamente.

### 7.5 Papel de Entidade no Relacionamento
- **Definição:** Função que uma ocorrência de uma entidade cumpre em uma ocorrência de um relacionamento.
- Relevante especialmente em **auto-relacionamentos**.

---

## 8. Atributos

### 8.1 Definição
Toda **propriedade descritiva** de uma entidade ou relacionamento.

- Uma entidade sempre é representada por um **conjunto de atributos**.
- Correspondem aos **complementos** na descrição informal (minimundo) da análise conceitual.

**Exemplos:**
- Atributos de `Cliente`: `Cert_reservista`, `nome`, `telefone`, `CPF`, `logradouro`, `CEP`
- Atributos de `Conta`: `numero`, `data_abertura`, `saldo`

**Representação gráfica:** Elipse (oval) conectada à entidade ou relacionamento.

### 8.2 Atributos em Relacionamentos
- Relacionamentos também **podem ter atributos**.
- **Exemplo:** No relacionamento N:N entre `Cliente` e `Conta`: `data_hora_saque`, `valor_saque`.

#### 8.2.1 Cardinalidade e Localização do Atributo

| Cardinalidade do Relacionamento | Onde colocar o atributo |
|---|---|
| **1:1, 1:N ou N:1** | Pode ser inserido nas **entidades** (lado N, geralmente) |
| **N:M** | Deve ser inserido no **relacionamento** |

**Exemplo prático:**
- `Cliente–Conta (1:N)` — atributo `ultimo_acesso` fica em **Conta** (último acesso do cliente em cada conta).
- `Cliente–Conta (N:M)` — atributo `ultimo_acesso` fica no **relacionamento** (último acesso de cada cliente em cada conta).

### 8.3 Tipos de Atributos

| Tipo | Descrição | Exemplo |
|---|---|---|
| **Simples** | Não pode ser dividido | `CPF`, `Saldo` |
| **Composto** | Pode ser dividido em outros atributos | `Nome` → `prenome`, `nome_intermediário`, `sobrenome` |
| **Monovalorado** | Só possui um valor para uma instância | `CPF` (uma pessoa tem um CPF) |
| **Multivalorado** | Pode possuir vários valores para uma instância | `telefone` (uma pessoa pode ter vários) |
| **Obrigatório** | Não pode ter valor nulo ou vazio | `CPF` em aplicações bancárias |
| **Opcional** | Pode ter valor nulo ou não informado | `cert_reservista` de um cliente |
| **Identificador (Chave)** | Identifica unicamente uma instância de entidade | `CPF` para Cliente |
| **Discriminador (Chave Parcial)** | Identifica instâncias dentro de uma entidade fraca | `número` de dependente (junto com o CPF do titular) |
| **Derivado** | Valor calculado a partir de outros atributos; **não armazenado** | `Idade` (calculada a partir da data de nascimento), `quantidade de contas` |

> ⚠️ **Atenção:** Nulo ≠ Zero! Um valor nulo significa "não informado/desconhecido", enquanto zero é um valor numérico válido.

### 8.4 Atributo Identificador (Chave)

- Usado para **identificar unicamente** uma instância de uma entidade.
- **Não pode haver atributo identificador em relacionamentos.**
- Um identificador deve ser **mínimo** (formado pelo menor número possível de atributos).

| Tipo | Descrição | Exemplo |
|---|---|---|
| **Simples** | Um único atributo identifica a entidade | `CPF` identifica `Cliente` |
| **Composto** | Mais de um atributo forma o identificador | (`agência`, `número`) identifica `Conta` |

**Exemplo de identificador composto para Conta:**
- (Ag-1, CC1) ✓
- (Ag-1, CC2) ✓
- (Ag-2, CC1) ✓
- (Ag-2, CC2) ✓
- (Ag-2, CC2) ✗ ← duplicado, inválido

### 8.5 Atributo Discriminador (Chave Parcial)

- Aparece em **entidades fracas** ou **relacionamentos** (normalmente como chave temporal).
- Não é suficiente por si só para identificar a entidade — precisa ser combinado com o identificador da entidade forte.

### 8.6 Representação no EERCASE

| Tipo de Atributo | Representação no EERCASE |
|---|---|
| Simples | Elipse simples |
| Composto | Elipse com sub-elipses |
| Monovalorado | Elipse simples |
| Multivalorado | Elipse dupla |
| Obrigatório | Definir na aba de Propriedades |
| Opcional | Definir na aba de Propriedades |
| Identificador | Elipse com nome sublinhado |
| Discriminador | Elipse com nome sublinhado (tracejado) |
| Derivado | Elipse tracejada |

---

## 9. Conceitos Avançados do Modelo E/R

### 9.1 Entidade Fraca e Relacionamento Identificador

#### 9.1.1 Motivação — Cenário 1
- Clientes podem ter vários **dependentes**.
- Dependentes precisam do identificador de `Cliente` para formar o seu.

#### 9.1.2 Definição
- A entidade **fraca** não tem atributos suficientes para formar seu identificador.
- Só existe se estiver **relacionada a outra entidade (forte)**.
- Usa o **identificador da entidade forte** para compor o seu.
- O relacionamento que liga a entidade fraca à forte é chamado de **Relacionamento Identificador**.

#### 9.1.3 Representação Gráfica

```
┌══════════┐    ◈══════════════◈    ┌──────────┐
║Dependente║════║ Relacionamento ════│  Cliente │
║ (Fraca)  ║    ║  Identificador║    │  (Forte) │
└══════════┘    ◈══════════════◈    └──────────┘
```

- Entidade fraca: **retângulo duplo**.
- Relacionamento identificador: **losango duplo**.
- O EERCASE usa atributo **discriminador (chave parcial)** para estes casos.

#### 9.1.4 Recursão em Entidade Fraca
- Uma entidade pode ser fraca em um relacionamento e forte em outro.
- **Exemplo:** `Grupo → Banco` (Banco é Fraca em relação a Grupo) e `Banco (Forte) → Agência` (Agência é Fraca em relação a Banco).

> ⚠️ O termo "Entidade Fraca" deve ser usado com cautela — uma entidade fraca em um relacionamento não é necessariamente fraca em outro.

---

### 9.2 Auto-Relacionamento

#### 9.2.1 Motivação — Cenário 2
- Clientes novos devem ser patrocinados ("indicados") por um cliente antigo.
- Um cliente antigo pode patrocinar vários clientes novos.

#### 9.2.2 Definição
- Representa uma **associação entre ocorrências de um mesmo conjunto de entidades**.
- **Exige a identificação de papéis** (roles).

#### 9.2.3 Exemplo

```
         1 Patrocinador
Cliente ──────────────────┐
  │      ◇ Patrocínio ◇   │
  └──────────────────────→┘
         N Patrocinado
```

- Um cliente pode ser **patrocinador** de vários clientes (1 Patrocinador → N Patrocinado).
- Um cliente só pode ser **patrocinado** por um cliente (N Patrocinado ← 1 Patrocinador).

---

### 9.3 Relacionamento Ternário

#### 9.3.1 Motivação — Cenário 3
- Um cliente pode ter N contas em um banco (N≥1).
- Uma conta pode ser de M clientes (M≥1).
- Uma conta pode ter X produtos bancários (X≥1).
- Uma conta pode ter **produtos bancários diferentes para clientes diferentes**.

#### 9.3.2 Grau de Relacionamento

| Grau | Definição | Descrição |
|---|---|---|
| **Binário** | Grau 2 | Envolve instâncias de **dois** conjuntos de entidades simultaneamente |
| **Ternário** | Grau 3 | Envolve instâncias de **três** conjuntos de entidades simultaneamente |
| **N-ário** | Grau N | Analogamente |

> Um conjunto de entidades pode ser tratado como um relacionamento de **grau zero** para efeito de comparação.

#### 9.3.3 Representação Gráfica — Relacionamento Ternário

```
           Produto
              │
          ◇ Possui ◇
         /           \
    Cliente          Conta
```

> ⚠️ **Atenção:** Cada ocorrência de "Possui" relaciona **3 ocorrências de entidade**: Cliente, Produto e Conta.

#### 9.3.4 Cardinalidade em Relacionamentos Ternários

- Definida considerando cada agrupamento de **N-1 entidades**, até cobrir todos os agrupamentos.
- A cardinalidade **refere-se ao par das demais entidades**.
- Pode ser definida usando **"look across"** ou **"look here"**.

**Exemplos de cardinalidade:**

| Situação | Cardinalidade de Produto (no relacionamento Cliente-Conta-Produto) |
|---|---|
| Uma conta contratada por um cliente pode ter **vários** produtos bancários | N (look across em Produto) |
| Um produto vinculado a uma conta pode ser contratado por **vários** clientes | N (look across em Cliente) |
| Um cliente pode contratar o **mesmo** produto para **várias** contas | N (look across em Conta) |

#### 9.3.5 Participação em Relacionamentos Ternários

- A abordagem **"look here"** é a única que consegue capturar corretamente a **semântica de participação** em relacionamentos N-ários.

**Exemplos:**

| Cenário | Participação |
|---|---|
| Pode-se cadastrar clientes, produtos e contas **isoladamente** | Todos com participação **opcional** (min = 0) |
| Pode-se cadastrar clientes e produtos isoladamente, mas **uma conta só pode ser cadastrada se ocorrer o relacionamento** | Conta com participação **obrigatória** (min = 1), demais com min = 0 |

#### 9.3.6 Relacionamento Ternário ≠ 3 Relacionamentos Binários

> ⚠️ **Crítico:** `(cli, prod, cont)` **≠** `(cli, prod)`, `(cli, cont)` e `(prod, cont)` combinados!
> Um relacionamento ternário captura uma semântica que **não pode ser decomposta** em três binários sem perda de informação.

#### 9.3.7 Cuidado com Cardinalidade Máxima 1 em Ternários

- A cardinalidade máxima `1` deve ser usada com atenção.
- **Exemplo:** Se `Cliente = 1` no relacionamento `(Cliente, Produto, Conta)`, isso significa que para cada par `(prod, cont)` existe apenas **um único cliente** — o relacionamento é identificado unicamente pelo par `(prod, cont)`.

---

### 9.4 Entidade Associativa (Agregação)

#### 9.4.1 Motivação — Cenário 4
- Como modelar uma associação com um relacionamento que **já é um relacionamento**?
- Ex.: `Promoção` se associa a `Cliente–Conta` (que já é um relacionamento `Possui`).

#### 9.4.2 Definição
- **Substitui a associação entre relacionamentos**, a qual não é prevista pelo modelo ER simples.
- É um **relacionamento que passa a ser tratado como entidade**.
- Permite o uso de **relacionamento opcional**.

#### 9.4.3 Estrutura

```
              Promoção
                  │
                  N
              ◇ Participa ◇
                  │
                  N
          ┌──────────────┐
          │  Cliente-Conta│  (Entidade Associativa = Relacionamento Possui elevado a entidade)
          └──────────────┘
          /               \
      Cliente             Conta
   N ◇ Possui ◇ N
```

- Os **relacionamentos identificadores são do lado N**.

#### 9.4.4 Alternativa sem Entidade Associativa
Caso não se deseje usar o conceito de entidade associativa, deve-se transformar o relacionamento em **entidade fraca**, a qual pode ser relacionada com outra entidade.

#### 9.4.5 Entidade Associativa como Alternativa ao Relacionamento Ternário
O exemplo com `Cliente-Conta` como entidade associativa relacionada a `Promoção` corresponde a uma **implementação alternativa para um relacionamento ternário**.

---

### 9.5 Generalização / Especialização

#### 9.5.1 Motivação — Cenário 5
- Um cliente pode ser **pessoa física ou jurídica**.

#### 9.5.2 Definição
- Permite atribuir **atributos e/ou relacionamentos particulares** a um subconjunto de entidades especializadas.
- Permite a **herança de propriedades (atributos) e relacionamentos**.
- A entidade especializada agrega ao seu conjunto de propriedades as propriedades e relacionamentos da **entidade genérica** (superclasse).

#### 9.5.3 Tipos de Generalização/Especialização

| Dimensão | Tipos | Descrição |
|---|---|---|
| **Completude** | **Total** | Toda instância da entidade genérica pertence a alguma especialização |
| **Completude** | **Parcial** | Pode existir instância da genérica que não pertence a nenhuma especialização |
| **Sobreposição** | **Disjunta (d)** | Uma instância pertence a **no máximo uma** especialização |
| **Sobreposição** | **Sobreposta (o)** | Uma instância pode pertencer a **mais de uma** especialização simultaneamente |

**Combinações e exemplos:**

| Combinação | Exemplo |
|---|---|
| **Disjunta-Total (d)** | `Cliente` → `P.Física` ou `P.Jurídica` (todo cliente é um ou outro, nunca ambos) |
| **Sobreposta-Total (o)** | `Veículo` → `Terrestre`, `Aéreo`, `Aquático` (pode ser anfíbio = Terrestre + Aquático) |
| **Disjunta-Parcial (d)** | `Funcionário` → `Chefe`, `Diretor` (pode ser só Funcionário sem especialização) |
| **Sobreposta-Parcial (o)** | `Pessoa` → `Aluno`, `Professor`, `Funcionário` |

---

## 10. Determinação de Existência de Relacionamentos

### 10.1 Como Identificar Relacionamentos Relevantes
1. Identificar tipos **diferentes de entidades**.
2. Determinar se alguma **questão significativa** pode ser feita ligando os dois.
   - Ex.: `Cliente` e `Conta` → `Movimenta`.
3. Determinar se o relacionamento é **relevante** (não redundante).

### 10.2 Relacionamento Relevante vs. Redundante
- Necessita **compreensão detalhada do ambiente**.
- Um relacionamento é redundante se pode ser **inferido** por meio de outros relacionamentos no modelo.

### 10.3 Quando Transformar um Atributo em Entidade
Considerar transformar um atributo em novo tipo de entidade quando:

| Critério | Descrição |
|---|---|
| O próprio atributo tem **atributos relevantes adicionais** | Ex.: `ENDEREÇO` tem `Rua`, `Num`, `Bairro`, `Cidade` |
| O atributo **descreve de fato** um novo tipo de entidade | A semântica aponta para algo mais complexo |
| O novo tipo de entidade é **por si só relevante** | Tem vida própria no domínio |
| **Outras entidades** podem se relacionar com entidades do novo tipo | Ex.: vários clientes no mesmo endereço |

---

## 11. Exemplo Completo — Sistema Único de Saúde Ideal

### 11.1 Minimundo (Narrativa)

**Parte 1:**
- Hospitais são **necessariamente formados** por um ou mais Ambulatórios; cada Ambulatório está **obrigatoriamente em um único** Hospital. (1:N, total em Ambulatório)
- Médicos devem clinicar em um **único Hospital**; cada Hospital **necessariamente** agrega vários Médicos. (1:N, total em ambos)
- Hospitais podem solicitar exames clínicos em **vários Laboratórios**; cada Laboratório pode receber solicitações de **vários Hospitais**. (N:M)
- Pacientes podem consultar **vários Médicos**; esses são consultados por **vários Pacientes**. (N:M)

**Parte 2:**
- Ambulatórios devem atender **vários Pacientes**; Pacientes só devem ser atendidos por um **único Ambulatório**. (1:N, total em Paciente)
- Pessoal de apoio deve estar alocado a um **único Ambulatório**; cada Ambulatório deve contar com **vários integrantes** do Pessoal de apoio. (1:N, total em ambos)
- Pacientes podem realizar **vários Exames**; cada Exame é realizado necessariamente por um **único Paciente**. (1:N, total em Exame)
- Laboratórios podem realizar **vários Exames**; cada Exame é necessariamente feito em um **único Laboratório**. (1:N, total em Exame)

**Parte 3:**
- Cada Paciente pode receber **vários Diagnósticos**; cada Diagnóstico é necessariamente associado a um **único Paciente**. (1:N, total em Diagnóstico)

### 11.2 Entidades e Relacionamentos Identificados

| Entidade | Relacionamento | Entidade |
|---|---|---|
| Hospital | Formado (1:N) | Ambulatório |
| Hospital | Clínica (1:N) | Médico |
| Hospital | Solicita (N:M) | Laboratório |
| Médico | Consulta (N:M) | Paciente |
| Ambulatório | Atende (1:N) | Paciente |
| Ambulatório | Aloca (1:N) | Pessoal |
| Paciente | Realiza (1:N) | Exame |
| Laboratório | Faz (1:N) | Exame |
| Paciente | Recebe (1:N) | Diagnóstico |

---

## 12. Exercício — Esquema EER de uma Empresa

### 12.1 Narrativa do Exercício

**Regras de modelagem:**
- Definir os atributos identificadores, discriminadores e/ou temporais (quando necessário).
- Não é permitido o uso de atributos opcionais.
- Não é permitido o uso de atributo derivado ou de relacionamento redundante.
- Fazer apenas o que é solicitado.

**Parte 1 — Empregados e Departamentos:**
- Um empregado tem: `CPF`, `nome`, `sexo`, `salário`, `data de nascimento`, `telefones` (multivalorado), `endereço` (`descrição` e `CEP`).
- Um empregado pode supervisionar outros empregados; cada empregado pode ser supervisionado por no máximo um empregado. (Auto-relacionamento 1:N)
- Um empregado pode trabalhar em vários departamentos ao longo da vida na empresa, e vários empregados podem trabalhar em um departamento. Deve-se guardar o **histórico** (relacionamento com atributo temporal) + `código` e `descrição` do departamento.

**Parte 2 — Chefias e Gratificações:**
- Um empregado pode chefiar um departamento; um departamento deve ser chefiado por um único empregado. (1:1 ou 1:N)
- Um empregado que trabalha em um departamento pode ter uma gratificação, a qual pode ser usufruída por outros empregados. Toda gratificação tem `código` e `descrição`.

**Parte 3 — Projetos e Atividades:**
- Projetos têm: `código`, `descrição`, `valor`.
- Atividades têm: `código`, `descrição`.
- Empregados podem participar de vários projetos; projetos podem envolver vários empregados. Todo empregado que participa em um projeto deve realizar atividades, as quais podem ser realizadas por outros empregados no mesmo projeto. (Relacionamento ternário Empregado–Projeto–Atividade)

**Parte 4 — Especialização:**
- Empregados podem ser **Graduados** ou **Técnicos** (generalização/especialização disjunta-parcial).
- Técnicos: guardar `última série de estudo`.
- Graduados: guardar todas as `titulações` (incluindo `data` de obtenção). As titulações de um empregado são identificadas a partir do seu `CPF` → **entidade fraca** `TitulaçãoEmpregado`.

**Parte 5 — IES e Graus:**
- Toda titulação de um empregado pode ser outorgada por uma **IES** (Instituição de Ensino Superior), a qual pode outorgar várias titulações. IES tem: `código`, `nome`, `sigla`.
- Toda titulação de um empregado pode ter um **Grau**, o qual tem `código`, `tipo` (Graduação, Especialização, Mestrado ou Doutorado) e pode ser de várias titulações.

### 12.2 Entidades e Relacionamentos do Exercício

| Entidade | Tipo | Identificador |
|---|---|---|
| Empregado | Forte | CPF |
| Departamento | Forte | COD |
| Projeto | Forte | COD |
| Atividade | Forte | COD |
| Gratificação | Forte | COD |
| Graduado | Especialização de Empregado | herda CPF |
| Técnico | Especialização de Empregado | herda CPF |
| TitulaçãoEmpregado | Fraca (discriminador: data + grau?) | CPF + discriminador |
| IES | Forte | COD |
| Grau | Forte | COD |

| Relacionamento | Entidades | Cardinalidade | Observação |
|---|---|---|---|
| Supervisiona / Supervisionado | Empregado ↔ Empregado | 1:N | Auto-relacionamento |
| Chefia | Empregado ↔ Departamento | 1:N | |
| Trabalha | Empregado ↔ Departamento | N:M | Com atributo temporal (histórico) |
| PodeTer | Empregado/Departamento ↔ Gratificação | N:M | Via entidade associativa |
| Participa (ternário) | Empregado ↔ Projeto ↔ Atividade | N:N:N | Ternário |
| Possui | Graduado ↔ TitulaçãoEmpregado | 1:N | Identificador |
| Lei (outorga) | TitulaçãoEmpregado ↔ IES | N:1 | |
| Outorgada | TitulaçãoEmpregado ↔ Grau | N:1 | |

---

## 13. Resumo dos Símbolos Gráficos do Modelo E/R

| Elemento | Símbolo |
|---|---|
| Conjunto de Entidades (forte) | Retângulo simples |
| Conjunto de Entidades (fraca) | Retângulo duplo |
| Conjunto de Relacionamentos (regular) | Losango simples |
| Conjunto de Relacionamentos (identificador) | Losango duplo |
| Atributo simples / monovalorado | Elipse simples |
| Atributo composto | Elipse com sub-elipses |
| Atributo multivalorado | Elipse dupla |
| Atributo derivado | Elipse tracejada |
| Atributo identificador | Elipse com nome sublinhado (traço contínuo) |
| Atributo discriminador | Elipse com nome sublinhado (traço tracejado) |
| Participação total (obrigatória) | Linha dupla |
| Participação parcial (opcional) | Linha simples |
| Cardinalidade | Notação "1" ou "N"/"M" próxima à linha |
| Generalização/Especialização | Triângulo / círculo com "d" (disjunta) ou "o" (sobreposta) |
| Entidade Associativa | Relacionamento dentro de retângulo |

---

## 14. Conceitos-Chave para Revisão

### Perguntas de Revisão

1. Qual a diferença entre **modelagem conceitual** e **modelagem lógica**?
2. Quais são os **três componentes** de um Modelo de Dados? Descreva cada um.
3. Qual a diferença entre restrição **implícita** e **explícita**? E entre **estática** e **dinâmica**?
4. No modelo E/R, o que corresponde graficamente a **entidades**, **relacionamentos** e **atributos**?
5. Explique a diferença entre **cardinalidade mínima** e **cardinalidade máxima**.
6. O que significam os valores `Min = 0` e `Min = 1` para a participação?
7. Qual a diferença entre as abordagens **"look across"** e **"look here"** para leitura de cardinalidade?
8. O que é uma **entidade fraca**? Quando ela ocorre?
9. O que é um **auto-relacionamento**? Qual elemento é obrigatório nele?
10. Por que um **relacionamento ternário** não é equivalente a três relacionamentos binários?
11. Qual a diferença entre **cardinalidade em binário** e **cardinalidade em ternário**?
12. O que é uma **entidade associativa** e quando ela é usada?
13. Explique os quatro tipos de **generalização/especialização** (disjunta-total, disjunta-parcial, sobreposta-total, sobreposta-parcial).
14. Quando se deve transformar um **atributo** em **entidade**?
15. Qual a diferença entre **atributo identificador** e **atributo discriminador**?

### Tabela de Correspondência Minimundo → Diagrama E/R

| No texto (minimundo) | No diagrama E/R |
|---|---|
| Substantivos | Conjuntos de Entidades |
| Verbos | Conjuntos de Relacionamentos |
| Complementos / adjetivos descritivos | Atributos |
| "deve", "precisa", "obrigatoriamente" | Participação total (min = 1) |
| "pode", "opcionalmente" | Participação parcial (min = 0) |
| "vários", "muitos", "N" | Cardinalidade máxima N |
| "um único", "apenas um", "somente um" | Cardinalidade máxima 1 |
