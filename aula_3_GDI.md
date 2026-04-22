# Aula: Modelo Relacional — Gerenciamento de Dados e Informação
**Instituição:** CIn / UFPE  
**Professores:** Robson Fidalgo, Valeria Times  
**Disciplina:** Gerenciamento de Dados e Informação  

---

## Sumário

1. [Definição e Aceitação do Modelo Relacional](#1-definição-e-aceitação-do-modelo-relacional)
2. [Conceitos Básicos](#2-conceitos-básicos)
3. [Domínio](#3-domínio)
4. [Tupla](#4-tupla)
5. [Atributo e Chave Primária](#5-atributo-e-chave-primária)
6. [Chave Candidata e Chave Estrangeira](#6-chave-candidata-e-chave-estrangeira)
7. [Esquema Relacional](#7-esquema-relacional)
8. [Restrições de Integridade](#8-restrições-de-integridade)
9. [Álgebra Relacional — Visão Geral](#9-álgebra-relacional--visão-geral)
10. [Seleção (σ)](#10-seleção-σ)
11. [Projeção (π)](#11-projeção-π)
12. [Junção (⋈)](#12-junção-)
13. [Divisão (÷)](#13-divisão-)
14. [União (∪)](#14-união-)
15. [Interseção (∩)](#15-interseção-)
16. [Diferença (−)](#16-diferença-)
17. [Quadro Geral do Modelo Relacional](#17-quadro-geral-do-modelo-relacional)
18. [Teoria das Dependências](#18-teoria-das-dependências)
19. [Normalização — Introdução](#19-normalização--introdução)
20. [1NF — Primeira Forma Normal](#20-1nf--primeira-forma-normal)
21. [2NF — Segunda Forma Normal](#21-2nf--segunda-forma-normal)
22. [3NF — Terceira Forma Normal](#22-3nf--terceira-forma-normal)
23. [BCNF — Forma Normal de Boyce-Codd](#23-bcnf--forma-normal-de-boyce-codd)
24. [4NF — Quarta Forma Normal (Dependência Multivalorada)](#24-4nf--quarta-forma-normal-dependência-multivalorada)
25. [5NF — Quinta Forma Normal (Dependência de Junção)](#25-5nf--quinta-forma-normal-dependência-de-junção)
26. [Resumo da Normalização](#26-resumo-da-normalização)
27. [Mapeamento E/R → Relacional](#27-mapeamento-er--relacional)
28. [Exemplo Completo: Esquema Relacional Normalizado](#28-exemplo-completo-esquema-relacional-normalizado)

---

## 1. Definição e Aceitação do Modelo Relacional

- Definido por **E. F. Codd em 1970**
- Grande aceitação comercial a partir de **meados da década de 1980**

**Razões da grande aceitação:**
- Simplicidade dos conceitos básicos
- Poder dos operadores de manipulação

---

## 2. Conceitos Básicos

**Definição formal de Relação:**

> Dada uma coleção de conjuntos D₁, D₂, ..., Dₙ (não necessariamente disjuntos), **R é uma Relação** sobre estes n conjuntos se ela é um conjunto de **n-uplas ordenadas** `<d₁, d₂, ..., dₙ>` tal que d₁ ∈ D₁, d₂ ∈ D₂, ..., dₙ ∈ Dₙ.

- D₁, D₂, ..., Dₙ são **Domínios**
- **n** é o **grau** de R

**Exemplo concreto:**

Sejam os domínios:
- D₁ = D-PESSOA: {José, Maria, João, Thaís, Branca}
- D₂ = D-ENDEREÇO: {R.A/30, R.B/45, R.C/17, R.D/67, R.E/55}

Seja a relação `<Esposo, Esposa, Logradouro>` em D₁ × D₁ × D₂:

| Esposo | Esposa | Logradouro |
|--------|--------|------------|
| José   | Maria  | R. A, 30   |
| João   | Thaís  | R. D, 67   |

---

## 3. Domínio

O **Domínio** representa o conjunto de **valores atômicos admissíveis** de um componente de uma relação. Funciona como um **conector semântico inter-relação a 2 níveis**:

| Nível | Regra |
|-------|-------|
| **Definição** | Todo valor vᵢ de uma n-upla é tal que vᵢ ∈ Dᵢ |
| **Manipulação** | 2 valores só podem ser comparados se definidos sobre o **mesmo domínio D** |

**Exemplo:**

```
D-IDADE = {15, 25, 30}
D-PESSOA = {José, Maria, João, Thaís, Bianca}

Relação Aluno(Nome, Idade):
  <José, 25>, <João, 30>, <Thaís, 25>

Relação Professora(Nome, Idade):
  <Maria, 25>, <Bianca, 15>
```

> É **válido** comparar a idade da professora com a dos alunos porque ambas são definidas sobre o **mesmo domínio** D-IDADE.

---

## 4. Tupla

Uma **tupla** é uma n-upla `<a₁, a₂, ..., aₙ>` de uma relação R(D₁, ..., Dₙ) tal que aᵢ ∈ Dᵢ (1 ≤ i ≤ n).

| Grau | Nome | Exemplo |
|------|------|---------|
| 2 | dupla | `<d₁, d₂>` |
| 3 | tripla | `<d₁, d₂, d₃>` |
| n | n-upla | `<d₁, d₂, ..., dₙ>` |

**Exemplo:** `<José, 25>` é uma tupla onde:
- José ∈ D-PESSOA
- 25 ∈ D-IDADE

---

## 5. Atributo e Chave Primária

### Atributo

O **atributo** explicita o **papel de um domínio em uma relação**.

> **Importante:** Um mesmo domínio pode aparecer em vários papéis/atributos diferentes na mesma relação.

**Exemplo:**
```
Fone-res: D-FONE    ← dois atributos distintos
Fone-com: D-FONE    ← sobre o mesmo domínio D-FONE
```

- Os atributos de uma mesma relação devem ter **nomes diferentes**
- Um (ou vários) atributos **identificam** uma relação: a **Chave Primária**

### Chave Primária

Propriedades obrigatórias de uma chave primária:

| Propriedade | Descrição |
|-------------|-----------|
| **Unicidade** | Não existem duas tuplas com o mesmo valor de chave primária |
| **Minimalidade** | Nenhum subconjunto próprio da chave também possui a propriedade de unicidade |

**Notação pseudo-relacional:**

```
Piloto(Num-cad, Nome, CPF, Ende_res)
```

> Atributos sublinhados = Chave Primária.

**Exemplo de tabela:**

| **Num_cad** | Nome  | CPF    | Ende_res  |
|-------------|-------|--------|-----------|
| 0101        | João  | 123456 | Recife    |
| 0035        | José  | 234567 | São Paulo |
| 0987        | Pedro | 567890 | Recife    |

- `Num_cad` = Chave Primária
- `CPF` = Chave Candidata (outro identificador único)

---

## 6. Chave Candidata e Chave Estrangeira

### Chave Candidata

> Uma relação pode ter **mais de um atributo** como identificador único. Um deles é escolhido como **chave primária** e os outros são **chaves candidatas**.

### Chave Estrangeira

> Um atributo que corresponde a uma **chave primária em outra relação**.

**Notação pseudo-relacional com FK:**

```
Voo(Num_voo, ..., Num_pil, ...)
    Num_pil referencia Piloto(Num_cad)
```

**Exemplo:**

| Num_voo | ... | Num_pil | ... |
|---------|-----|---------|-----|
| 330     | ... | 0101    | ... |

> `Num_pil = 0101` referencia a tupla do piloto João em `Piloto(Num_cad)`. Indica quem pilota o Voo 330.

---

## 7. Esquema Relacional

> **Esquema Relacional** = conjunto de relações **semanticamente ligadas** por seus domínios de definição.

O conceito de **relação** permite representar:
- Uma **entidade** (ex: Empregado, Departamento)
- Uma **relação semântica** — relacionamento M:N entre entidades (ex: Cliente —[Movimenta]— Conta)

---

## 8. Restrições de Integridade

O modelo relacional define três tipos fundamentais de restrições:

### 8.1 Integridade de Domínio

> Diz respeito ao **controle sintático e semântico** de um dado e faz referência ao tipo de definição do domínio.

**Regra:** vᵢ ∈ Dᵢ, ∀ vᵢ, Dᵢ

Ou seja: todo valor de um atributo deve pertencer ao domínio definido para aquele atributo.

### 8.2 Integridade de Entidade

> Diz respeito aos **valores de chave primária** que devem ser:
> - **Únicos** — não existem duas tuplas com o mesmo valor de PK
> - **Não nulos** — nenhum componente da PK pode ser NULL

### 8.3 Integridade Referencial

> Diz respeito aos valores de **chave estrangeira** e aos valores da **chave primária correspondente**.

**Regra:** Para cada valor de chave estrangeira que aparece numa relação B, deve existir um valor **igual** de chave primária na relação A referenciada.

| Situação | Permitido? |
|----------|-----------|
| FK aponta para PK existente | ✔ Sim |
| FK com valor NULL (quando permitido pela semântica) | ✔ Sim |
| FK aponta para valor inexistente na PK | ✗ Não — viola integridade referencial |

---

## 9. Álgebra Relacional — Visão Geral

A **Linguagem Algébrica / Álgebra Relacional** compreende dois tipos de operadores:

```
Álgebra Relacional
├── Operadores CLÁSSICOS sobre conjuntos
│   ├── União (∪)
│   ├── Interseção (∩)
│   └── Diferença (−)
│       (somente entre relações UNIÃO COMPATÍVEIS)
└── Operadores RELACIONAIS
    ├── Unários de restrição
    │   ├── Seleção (σ)
    │   └── Projeção (π)
    └── Binários de extensão
        ├── Junção (⋈)
        └── Divisão (÷)
```

---

## 10. Seleção (σ)

> **Seleção:** seleciona todas as tuplas que **satisfazem à condição de seleção** em uma relação R.

**Notação:**
```
σ <condição de seleção> (R)
```

**Tipos de condição:**
- **Simples:** `=`, `<>`, `<`, `<=`, `>`, `>=`
- **Booleana:** conexão de condições simples por `AND`, `OR`, `NOT`

**Exemplo:** Quais os pilotos que residem em São Paulo?

```
σ Endereço = 'São Paulo' (Piloto)
```

| Num_cad | Nome | CPF    | Ende_res  |
|---------|------|--------|-----------|
| 0035    | José | 234567 | São Paulo |

> Em SQL: `SELECT * FROM Piloto WHERE Endereço = 'São Paulo';`

---

## 11. Projeção (π)

> **Projeção:** produz uma nova relação com **alguns dos atributos** de uma relação R (elimina colunas).

**Notação:**
```
π <lista de atributos> (R)
```

**Exemplo:** Quais os nomes e CPF dos pilotos da companhia?

```
π nome, CPF (Piloto)
```

| Nome  | CPF    |
|-------|--------|
| João  | 123456 |
| José  | 234567 |
| Pedro | 567890 |

> Em SQL: `SELECT Nome, CPF FROM Piloto;`

**Composição:** Seleção + Projeção podem ser combinadas:
```
π nome, sexo (σ Num_Dep = 1 AND salario > 4000 (Empregado))
```

---

## 12. Junção (⋈)

> **Junção:** produz **todas as combinações de tuplas** das relações R1 e R2 que satisfazem à condição de junção.

**Notação:**
```
R1 ⋈ <condição de junção> R2
```

**Exemplo:** Quais os dados do piloto e do voo 330?

```
Piloto ⋈ num_cad = num_pil AND num_voo = 330 Voo
```

| Num_cad | Nome | CPF   | Endereço | Num_voo | Num_pil |
|---------|------|-------|----------|---------|---------|
| 0101    | João | 12345 | Recife   | 330     | 0101    |

> Em SQL: `SELECT * FROM Piloto P, Voo V WHERE P.num_cad = V.num_pil AND V.num_voo = 330;`

---

## 13. Divisão (÷)

> **Divisão:** produz a relação R(X) incluindo todas as tuplas de R1(A) que aparecem em R1, **combinadas com cada tupla de R2(B)**, onde B ⊆ A e X = A − B.

**Notação:**
```
R1 ÷ R2
```

**Semântica:** Responde à pergunta **"quem fez TODOS?"**

**Exemplo:** Quais os pilotos que conduzem **todos** os aviões?

Relação V (Piloto, Aviao):

| Piloto | Aviao |
|--------|-------|
| 0020   | 101   |
| 0020   | 105   |
| 0010   | 101   |
| 0010   | 104   |
| 0010   | 105   |
| 0010   | 103   |
| 0015   | 103   |
| 0015   | 104   |

Divisor (Aviao):

| Aviao |
|-------|
| 101   |
| 104   |
| 105   |
| 103   |

```
V ÷ Divisor
```

Resultado R (Piloto):

| Piloto |
|--------|
| 0010   |

> Apenas o piloto 0010 conduz **todos** os quatro aviões.

> Em SQL: implementado com dupla negação (`NOT EXISTS`) — ver aula de SQL.

---

## 14. União (∪)

> **União:** produz uma relação que inclui **todas as tuplas** de R1 **ou** de R2 (sem duplicatas).

**Notação:**
```
R1 ∪ R2
```

**Pré-requisito:** R1 e R2 devem ser **união compatíveis**.

**Definição de União Compatível:**
> Duas relações R(a₁, a₂, ..., aₙ) e S(b₁, b₂, ..., bₙ) são união compatíveis se:
> 1. Têm o **mesmo grau** n
> 2. Dom(aᵢ) = Dom(bᵢ) para 1 ≤ i ≤ n

**Exemplo:** Listar as cidades que são residência de pilotos **ou** que são locais de aviões:

```
π Cidade (Piloto) ∪ π Local (Aviao)
```

| Cidade (Piloto) | | Local (Aviao) |
|---------|---|-------|
| Recife  | | Belém |
| Caruaru | | Natal |
| Olinda  | | Recife|

Resultado (Lugar):

| Lugar    |
|----------|
| Recife   |
| Caruaru  |
| Olinda   |
| Belém    |
| Natal    |

> **Nota:** Para que a união seja possível, as projeções devem ser transformadas em **união compatíveis** (renomear atributo para o mesmo nome, ex: `Lugar`).

---

## 15. Interseção (∩)

> **Interseção:** produz uma relação que inclui as **tuplas comuns** de R1 e R2. R1 e R2 devem ser união compatíveis.

**Notação:**
```
R1 ∩ R2
```

**Exemplo:** Listar as cidades que são residência de pilotos **e** locais de aviões:

```
π Cidade (Piloto) ∩ π Local (Aviao)
```

| Cidade (Piloto) | | Local (Aviao) |
|---------|---|-------|
| Recife  | | Belém |
| Caruaru | | Natal |
| Olinda  | | Recife|

Resultado:

| Lugar  |
|--------|
| Recife |

---

## 16. Diferença (−)

> **Diferença:** produz uma relação que inclui todas as tuplas de R1 que **não estão** em R2. R1 e R2 devem ser união compatíveis.

**Notação:**
```
R1 − R2
```

**Exemplo:** Listar as cidades que são residência de pilotos e **não** são locais de aviões:

```
π Cidade (Piloto) − π Local (Aviao)
```

| Cidade (Piloto) | | Local (Aviao) |
|---------|---|-------|
| Recife  | | Belém |
| Caruaru | | Natal |
| Olinda  | | Recife|

Resultado:

| Cidade   |
|----------|
| Caruaru  |
| Olinda   |

---

## 17. Quadro Geral do Modelo Relacional

```
┌─────────────────────────────────────────────────────────┐
│                   MODELO RELACIONAL                     │
├──────────────────┬──────────────────┬───────────────────┤
│   ESTRUTURAS     │   OPERADORES     │   RESTRIÇÕES      │
├──────────────────┼──────────────────┼───────────────────┤
│ Relação          │ União            │ Integridade de:   │
│ Atributo         │ Interseção       │  • Domínio        │
│ Domínio          │ Diferença        │  • Entidade       │
│ Chave Primária   │ Seleção          │  • Referencial    │
│ Chave Estrangeira│ Projeção         │                   │
│ Chave Candidata  │ Junção           │                   │
│                  │ Divisão          │                   │
└──────────────────┴──────────────────┴───────────────────┘
```

---

## 18. Teoria das Dependências

### 18.1 Dependência Funcional (DF)

> Sejam R(A₁, A₂, ..., Aₙ) e X, Y contidos em {A₁, A₂, ..., Aₙ}. Diz-se que existe uma **Dependência Funcional (DF) de X para Y** (X → Y) se e somente se, em R, **a um valor de X corresponde um e um só valor de Y**.

**Notação:** `X → Y` (X determina Y / Y depende funcionalmente de X)

**Exemplo:** `Num_cad → Nome` (dado um Num_cad, existe um único Nome associado)

### 18.2 DF Total (Bicondicional)

> Se X → Y **e** Y → X, então `X ↔ Y`

**Exemplo:** `Num_cad ↔ CPF` (cada número de cadastro corresponde a um único CPF e vice-versa)

### 18.3 DF Plena

> Ocorre quando um atributo é funcionalmente dependente de **dois ou mais atributos** combinados (chave composta).

**Exemplo:**
```
Num_pil ──┐
           ├──→ Trajeto
Num_av  ──┘
```
Ou seja: `(Num_pil, Num_av) → Trajeto`

O trajeto só pode ser determinado conhecendo-se **ambos** o piloto e o avião.

### 18.4 Dependência Transitiva

> Ocorre quando Y depende de X e Z depende de Y. Logo, Z também depende (transitivamente) de X.

**Cadeia:** `X → Y → Z`, portanto `X → Z` (transitivamente)

**Exemplo concreto:**
```
No_aviao → Tipo → Capacidade
```
- `No_aviao → Tipo` (cada avião tem um tipo)
- `Tipo → Capacidade` (cada tipo tem uma capacidade fixa)
- Logo: `No_aviao → Capacidade` (transitivamente, mas via `Tipo`)

### 18.5 Dependência Multivalorada (DMV)

> Dada uma relação R com atributos A, B, C, existe uma **dependência multivalorada** de A em B (`A →→ B`) se um valor de A é associado a uma **coleção específica de valores de B**, independentemente de quaisquer valores de C.

**Regras:**
- A DMV só existe se R tem **no mínimo 3 atributos**
- Dada R(A, B, C), a DMV `A →→ B` existe se `A →→ C` também existir
- Notação completa: `A →→ B|C`

**Exemplo:** Relação Voo(Piloto, Aviao, Trajeto):
- Um piloto voa em vários aviões (`Piloto →→ Aviao`)
- Um piloto faz vários trajetos (`Piloto →→ Trajeto`)
- Estas duas multidependências são independentes entre si

### 18.6 Dependência de Junção (DJ)

> Uma relação R satisfaz à **Dependência de Junção** `*(X, Y, ..., Z)` se e somente se R é igual à junção de suas projeções em X, Y, ..., Z (onde X, Y, ..., Z são subconjuntos dos atributos de R).

**Propriedade:** A 5NF é sempre realizável — qualquer relação pode ser decomposta sem perdas em relações 5NF.

### 18.7 Chave Primária via Dependências Funcionais

> Um atributo A (ou uma coleção) é **chave primária** de R se:
> 1. **Todos** os atributos de R são funcionalmente dependentes de A
> 2. **Nenhum subconjunto** de A também tem a propriedade 1 (minimalidade)

---

## 19. Normalização — Introdução

### Por que normalizar?

No projeto de um banco de dados, é necessário:
- Identificar dados
- Fazer com que estes dados representem eficientemente o mundo real

O processo envolve: **Decomposição → Modelo relacional → Normalização**

### O que é Normalização?

> Método que permite identificar a existência de **problemas potenciais (anomalias de atualização)** no projeto de um BD relacional. Converte progressivamente uma tabela em tabelas de grau e cardinalidade **menores** até que pouca ou nenhuma redundância de dados exista.

### Resultados de uma normalização bem-sucedida

- O espaço de armazenamento dos dados **diminui**
- A tabela pode ser atualizada com **maior eficiência**
- A descrição do BD será **imediata**

---

## 20. 1NF — Primeira Forma Normal

### Definição

> Uma relação está na **Primeira Forma Normal (1NF)** se todos os atributos que a compõem são **atômicos** (indivisíveis).

### Violação: atributo não atômico (composto)

**Exemplo de relação NÃO em 1NF:**

| **Num_cad** | Nome   | CPF    | Telefone     |
|-------------|--------|--------|--------------|
| 0010        | José   | 123456 | 81 324569    |
| 0015        | João   | 234567 | 83 456785    |
| 0020        | Manuel | 345678 | 45 768439    |
| 0028        | Josué  | 987654 | 21 347654    |

> O campo `Telefone` contém dois componentes (DDD + número): **não é atômico**.

### Correção

Separar os componentes do atributo composto em **atributos distintos**:

| **Num_cad** | Nome   | CPF    | COD | TELEFONE |
|-------------|--------|--------|-----|----------|
| 0010        | José   | 123456 | 81  | 324569   |
| 0015        | João   | 234567 | 83  | 456785   |
| 0020        | Manuel | 345678 | 45  | 768439   |
| 0028        | Josué  | 987654 | 21  | 347654   |

### Consequências da normalização para 1NF

Quando a 1NF é aplicada e gera uma chave primária composta, surgem novos problemas:

1. **Extensão da chave primária** (passa a ser composta)
2. **Dependência funcional de parte da chave primária** (dependências parciais)
3. **Anomalias de atualização:**

| Tipo de Anomalia | Descrição |
|------------------|-----------|
| **Atualização** | É necessário atualizar **todas as tuplas** com mesmo valor de atributo |
| **Inconsistência** | Se a atualização não for feita em todos os níveis |
| **Inclusão** | De um item que não tem correspondente para os outros campos da chave primária |
| **Remoção** | De um item da chave provoca a remoção de informações adicionais |

**Exemplo de relação com anomalias após 1NF** (chave composta: Num_cad + Diploma):

| **NumCad** | Nome   | CPF    | Salario  | **Diploma** | Descricao      |
|------------|--------|--------|----------|-------------|----------------|
| 0010       | José   | 123456 | 5.000,00 | D1          | Helicópteros   |
| 0010       | José   | 123456 | 5.000,00 | D2          | Aviões a jato  |
| 0015       | João   | 234567 | 3.000,00 | D3          | Bi-motor       |
| 0020       | Manuel | 345678 | 8.000,00 | D1          | Helicópteros   |
| 0020       | Manuel | 345678 | 8.000,00 | D2          | Aviões a jato  |
| 0020       | Manuel | 345678 | 8.000,00 | D4          | Concorde       |
| 0018       | Josué  | 987654 | 4.000,00 | D2          | Aviões a jato  |

> Problema: Nome, CPF e Salario dependem **apenas** de Num_cad (parte da PK), não da PK composta inteira.

---

## 21. 2NF — Segunda Forma Normal

### Definição

> Uma relação está na **Segunda Forma Normal (2NF)** se ela está na **1NF** e todo atributo não-chave é **plenamente dependente** da chave primária (sem dependências parciais).

### Dependências Parciais — o problema

Na relação do exemplo acima:

```
Chave Primária: (Num_cad, Diploma)

Num_cad  →  Nome, CPF, Salario     ← dependência PARCIAL (só parte da PK)
Diploma  →  Descricao              ← dependência PARCIAL (só parte da PK)
(Num_cad, Diploma) → (tudo)        ← dependência PLENA (PK completa)
```

### Correção

> Para cada **subconjunto de atributos** que compõe a chave primária, criar uma relação com este subconjunto como chave primária. Colocar cada atributo com o subconjunto **mínimo** do qual ele depende.

**Relações resultantes:**

```
(Num_cad, Nome, CPF, Salario)   →  Piloto
(Diploma, Descricao)            →  Diplomas
(Num_cad, Diploma)              →  Formacao
```

**Tabelas após 2NF:**

**Piloto:**

| **Num_cad** | Nome   | CPF   | Salario  |
|-------------|--------|-------|----------|
| 0010        | José   | 12345 | 5.000,00 |
| 0015        | João   | 23456 | 3.000,00 |
| 0020        | Manuel | 34567 | 8.000,00 |
| 0018        | José   | 98765 | 4.000,00 |

**Diplomas:**

| **Diploma** | Descricao     |
|-------------|---------------|
| D1          | Helicópteros  |
| D2          | Aviões a jato |
| D3          | Bi-motor      |
| D4          | Concorde      |

**Formacao:**

| **Num_cad** | **Diploma** |
|-------------|-------------|
| 0010        | D1          |
| 0010        | D2          |
| 0015        | D3          |
| 0020        | D1          |
| 0020        | D2          |
| 0020        | D4          |
| 0018        | D2          |

> **Observação:** As anomalias foram eliminadas — não houve perda de informação.

---

## 22. 3NF — Terceira Forma Normal

### Definição

> Uma relação está na **Terceira Forma Normal (3NF)** se ela está na **2NF** e nenhum atributo não-chave é **transitivamente dependente** da chave primária.

### O problema: Dependência Transitiva

**Exemplo:** Relação Aviao(No_av, Tipo, Capacidade, Local):

```
No_av → Tipo → Capacidade
         └─────────────────→ dependência TRANSITIVA de No_av para Capacidade
```

| **No_av** | Tipo | Capacidade | Local   |
|-----------|------|------------|---------|
| 101       | A320 | 320        | Rio     |
| 104       | B727 | 250        | S.Paulo |
| 105       | DC10 | 350        | Rio     |
| 103       | B727 | 250        | Recife  |
| 110       | B727 | 250        | Rio     |

> Problema: se a capacidade do B727 mudar, é preciso atualizar **todas** as tuplas onde `Tipo = B727`.

### Correção

> Para cada determinante que **não é chave candidata**, remover da relação os atributos que dependem dele e criar uma nova relação na qual o determinante será chave primária.

**Tabelas após 3NF:**

**Aviao:**

| **No_av** | Tipo | Local   |
|-----------|------|---------|
| 101       | A320 | Rio     |
| 104       | B727 | S.Paulo |
| 105       | DC10 | Rio     |
| 103       | B727 | Recife  |
| 110       | B727 | Rio     |

**Tipo_av:**

| **Tipo** | Capacidade |
|----------|------------|
| A320     | 320        |
| B727     | 250        |
| DC10     | 350        |

---

## 23. BCNF — Forma Normal de Boyce-Codd

### Definição

> Uma relação está na **Forma Normal de Boyce/Codd (BCNF)** se **todo determinante é uma chave candidata**.

### Comparação 3NF × BCNF

A BCNF é **mais restritiva** que a 3NF. Uma relação pode estar em 3NF mas não em BCNF.

### Exemplo clássico — relação com múltiplas chaves candidatas

Relação **ADP(Aluno, Disciplina, Professor)** com as regras:
- Para cada disciplina, cada estudante tem **um único professor**
- Cada professor ensina **uma única disciplina**
- Cada disciplina é ensinada por **vários professores**

| **Aluno** | **Disciplina** | Professor |
|-----------|----------------|-----------|
| Maria     | BD             | Fernando  |
| Maria     | ES             | Paulo     |
| José      | BD             | Fernando  |
| José      | ES             | André     |

**Análise:**
- Está na **3NF** (não há dependência transitiva com não-chave)
- **Não está na BCNF** porque `Professor → Disciplina` (Professor é determinante mas não é chave candidata da relação)

### Correção

Decompor em duas relações:

**AP (Aluno, Professor):**

| Aluno | Professor |
|-------|-----------|
| Maria | Fernando  |
| Maria | Paulo     |
| José  | Fernando  |
| José  | André     |

**PD (Professor, Disciplina):**

| Professor | Disciplina |
|-----------|------------|
| Fernando  | BD         |
| Paulo     | ES         |
| André     | ES         |

---

## 24. 4NF — Quarta Forma Normal (Dependência Multivalorada)

### Definição

> Uma relação está na **Quarta Forma Normal (4NF)** se ela está na **3NF (BCNF)** e **não existem dependências multivaloradas**.

### O problema: DMV na prática

Relação **Voo(Piloto, Aviao, Trajeto)**:

Um piloto voa em vários aviões (`Piloto →→ Aviao`) e faz vários trajetos (`Piloto →→ Trajeto`), **independentemente entre si**. Isso gera uma explosão combinatória de tuplas para representar corretamente todas as combinações:

| Piloto | Aviao | Trajeto  |
|--------|-------|----------|
| 0020   | 101   | Rec-Rio  |
| 0020   | 101   | Rio-Spa  |
| 0020   | 101   | Spa-Rec  |
| 0020   | 105   | Rec-Rio  |
| 0020   | 105   | Rio-Spa  |
| 0020   | 105   | Spa-Rec  |
| 0010   | 101   | Rec-For  |
| 0015   | 103   | Rio-Spa  |

### Correção

> Separar a relação em relações, cada uma contendo o atributo A que multidetermina os outros (B, C): R1(A, B) e R2(A, C).

**Voo1 (Piloto, Aviao):**

| Piloto | Aviao |
|--------|-------|
| 0020   | 101   |
| 0020   | 105   |
| 0010   | 101   |
| 0010   | 104   |
| 0015   | 103   |

**Voo2 (Piloto, Trajeto):**

| Piloto | Trajeto  |
|--------|----------|
| 0020   | Rec-Rio  |
| 0020   | Rio-Spa  |
| 0020   | Spa-Rec  |
| 0010   | Rec-For  |
| 0015   | Rio-Spa  |

### Observação prática para evitar relações não 4NF

Quando existir mais de um atributo multivalorado, durante a aplicação da **1NF**:
1. Criar uma relação para cada **Atributo Multivalorado (AMV)** (e os que ele determina)
2. Incluir a **Chave Primária da Relação Original (CPO)**
3. A nova chave primária será: **CPO + AMV**

---

## 25. 5NF — Quinta Forma Normal (Dependência de Junção)

### Definição

> Uma relação R está na **Quinta Forma Normal (5NF)** (também chamada **PJNF — Forma Normal Projeção/Junção**) se e somente se cada **dependência de junção** em R for uma consequência das **chaves candidatas** de R.

### Propriedades

- Qualquer relação 5NF está na 4NF
- Qualquer relação pode ser decomposta sem perdas em relações 5NF (a 5NF é **sempre realizável**)
- Para verificar se R está na 5NF, é necessário conhecer suas **chaves candidatas** e **todas as DJ** em R

### Notas sobre dificuldades práticas

- É fácil identificar DF e DMV
- A DJ é **mais difícil** de identificar porque seu significado intuitivo não é óbvio

### Exemplo: n-decomposição (n > 2)

Relação **Voo(Piloto, Aviao, Trajeto)** — não pode ser decomposta sem perdas em **duas** relações, mas pode ser decomposta em **três**:

Projetando em **três relações** sem perda de informação:

**V1 (Piloto, Aviao):**

| Piloto | Aviao |
|--------|-------|
| 0020   | 101   |
| 0020   | 105   |
| 0010   | 101   |

**V2 (Aviao, Trajeto):**

| Aviao | Trajeto  |
|-------|----------|
| 101   | Rec-Rio  |
| 105   | Rec-Rio  |
| 101   | Rio-Spa  |

**V3 (Trajeto, Piloto):**

| Trajeto  | Piloto |
|----------|--------|
| Rec-Rio  | 0020   |
| Rio-Spa  | 0020   |
| Rec-Rio  | 0010   |

A junção de V1, V2 e V3 reproduz exatamente a relação Voo original.

> A relação Voo satisfaz à DJ: `*({Piloto, Aviao}, {Aviao, Trajeto}, {Trajeto, Piloto})`

---

## 26. Resumo da Normalização

```
┌─────────────────────────────────────────────────────────────────┐
│                    PROCESSO DE NORMALIZAÇÃO                     │
├──────────┬──────────────────────────────────────────────────────┤
│  Forma   │ O que elimina                                        │
├──────────┼──────────────────────────────────────────────────────┤
│  1NF     │ Atributos NÃO ATÔMICOS                               │
│  2NF     │ Dependências Funcionais NÃO PLENAS (parciais)        │
│  3NF     │ Dependências TRANSITIVAS                             │
│  BCNF    │ DF cujo determinante NÃO é chave candidata           │
│  4NF     │ Dependências MULTIVALORADAS (DMV)                    │
│  5NF     │ Dependências de JUNÇÃO (DJ) — se encontradas         │
└──────────┴──────────────────────────────────────────────────────┘
```

> **Dica prática para 4NF:** Para evitar relações não-4NF, ao aplicar a **1NF**, remover para outras relações os **atributos multivalorados**.

### Checklist de uma relação em 5NF

Para cada relação R, verificar:
1. ✔ Atributos atômicos → **1NF**
2. ✔ Chave não composta (ou DF plena se composta) → **2NF**
3. ✔ Não há dependência transitiva → **3NF**
4. ✔ Só há DF cujo determinante é chave candidata → **BCNF**
5. ✔ Não há dependência multivalorada → **4NF**
6. ✔ Não há dependência de junção → **5NF**

---

## 27. Mapeamento E/R → Relacional

Um **esquema relacional** pode ser facilmente derivado de um **esquema conceitual** desenvolvido usando o modelo E-R (notação R. Elmasri & S. Navathe — EERCASE).

O mapeamento é feito em **7 passos**:

---

### Passo 1 — Entidades Regulares

> Para cada **entidade regular** E no esquema E-R, criar uma relação R que inclui todos os atributos de E, **exceto multivalorados**.

**Exemplo:**

```
Entidade Empregado: Cad, Nome, Sexo, Salario
```

```
Empregado (Cad, Nome, Sexo, Salario)
```

---

### Passo 2 — Entidades Fracas

> Para cada **entidade fraca** F com entidade proprietária P:
> 1. Criar uma relação R com todos os atributos de F
> 2. Incluir o(s) atributo(s) chave primária de P como FK
> 3. A chave primária de R = PK de P + **chave parcial (atributo discriminador)** de F

**Exemplo:**

```
Entidade Fraca: Dependentes (proprietária: Empregado)
Atributo discriminador: Nome + Data_nasc
```

```
Dependente (Cad_emp, Nome, Data_nasc, Grau_P)
    Cad_emp referencia Empregado(Cad)
```

---

### Passo 3 — Relacionamentos 1:1

> Para cada relacionamento **1:1**:
> 1. Identificar as relações das entidades participantes
> 2. Escolher **uma** das relações e incluir como FK a PK da outra
> 3. Incluir todos os atributos do relacionamento na relação escolhida

**Exemplo:** Empregado — [Gerencia] — Departamento (1:1):

```
Departamento (Numero, Nome, Cad_Ger, Data_Ini)
    Cad_Ger referencia Empregado(Cad)
```

> `Cad_Ger` e `Data_Ini` são atributos do relacionamento Gerencia que migram para Departamento.

**Mapeamento alternativo para 1:1:**
> É possível juntar as duas entidades em uma **única relação**. Isto é apropriado quando as entidades **não participam de outros relacionamentos**.

---

### Passo 4 — Relacionamentos 1:N

> Para cada relacionamento **1:N** regular (não fraco):
> 1. Identificar a relação S que representa a entidade do **lado N**
> 2. Incluir como FK a PK da relação do **lado 1**
> 3. Incluir os atributos do relacionamento em S

**Exemplo:** Departamento —[Trabalha_para]— Empregado (1:N) e Empregado —[Supervisão]— Empregado (1:N auto-referência):

```
Empregado (Cad, Nome, Sexo, Salario, Num_Dep, Cad_Supv)
    Num_Dep  referencia Departamento(Numero)
    Cad_Supv referencia Empregado(Cad)
```

> `Num_Dep` migra do lado 1 (Departamento) para o lado N (Empregado).
> `Cad_Supv` é auto-referência (supervisor é também um Empregado).

---

### Passo 5 — Relacionamentos M:N

> Para cada relacionamento **M:N**:
> 1. Criar uma **nova relação** para representar o relacionamento
> 2. Incluir como FKs as PKs das relações participantes — estas **chaves combinadas** formam a PK da nova relação
> 3. Incluir eventuais atributos do relacionamento

**Exemplo:** Empregado —[Trabalha_em]— Projeto (M:N) com atributo `Horas`:

```
Trabalha_em (Cad_Emp, Num_Pro, Horas)
    Cad_Emp referencia Empregado(Cad)
    Num_Pro referencia Projeto(Numero)
```

---

### Passo 6 — Atributos Multivalorados

> Para cada **atributo multivalorado** A:
> 1. Criar uma nova relação R com o atributo A mais a PK (K) da relação original
> 2. A PK de R = A + K

**Exemplo:** Departamento com atributo multivalorado `Locais`:

```
Locais (Num_dep, Nome_Loc)
    Num_Dep referencia Departamento(Numero)
```

---

### Passo 7 — Relacionamentos n-ários (n > 2)

> Para cada relacionamento **n-ário** (n > 2):
> 1. Criar uma nova relação S
> 2. Incluir como FKs as PKs de todas as entidades participantes
> 3. Incluir eventuais atributos do relacionamento
> 4. A PK de S é normalmente a **combinação de todas as FKs**

---

### Esquema Relacional Final (exemplo completo)

```
Empregado (Cad, Nome, Sexo, Salario, Num_Dep, Cad_Supv)
    Num_Dep  referencia Departamento(Numero)
    Cad_Supv referencia Empregado(Cad)

Departamento (Numero, Nome, Cad_Ger, Data_Ini)
    Cad_Ger referencia Empregado(Cad)

Locais (Num_dep, Nome_Loc)
    Num_Dep referencia Departamento(Numero)

Projeto (Numero, Nome, Num_Dep)
    Num_Dep referencia Departamento(Numero)

Trabalha_em (Cad_Emp, Num_Pro, Horas)
    Cad_Emp referencia Empregado(Cad)
    Num_Pro referencia Projeto(Numero)

Dependente (Cad_emp, Nome, Data_nasc, Grau_P)
    Cad_emp referencia Empregado(Cad)
```

---

## 28. Exemplo Completo: Esquema Relacional Normalizado

Este exemplo demonstra o processo de normalização aplicado a um **exercício completo** de modelagem EER de uma empresa.

### Cenário (resumo)

- Empregados têm CPF, nome, sexo, salário, data de nascimento, telefones e endereço (CEP + descrição)
- Empregados podem supervisionar outros (auto-referência)
- Empregados trabalham em departamentos (histórico, M:N com data)
- Empregados podem ter gratificações ao trabalhar em departamentos
- Empregados participam de projetos realizando atividades (ternário)
- Empregados são graduados ou técnicos (especialização)
- Graduados têm titulações (com data, grau e IES outorgante)

---

### Análise relação a relação

#### Projeto (Cod, descricao, valor)
✔ **5NF**
- Atributos atômicos (1NF)
- Chave não composta (2NF)
- Não há dependência transitiva (3NF)
- Só há DF para chave (BCNF)
- Não há DMV (4NF)
- Não há DJ (5NF)

---

#### Atividade (Cod, descricao)
✔ **5NF** — mesma análise acima.

---

#### Empregado — análise com problemas

**Versão inicial com problemas:**
```
Empregado (CPF, nome, sexo, salario, dtNasc, CEP, descricao, Fones, CPF_Spv)
```

| Problema | Forma Normal violada |
|----------|---------------------|
| `Fones` é multivalorado | **1NF** |
| `CPF → CEP → Descricao` (transitiva) | **3NF** |

**Decomposição correta:**

```
Empregado (CPF, nome, sexo, salario, dtNasc, CPF_Spv)
    CPF_Supv referencia Empregado(CPF)      ✔ 5NF

Endereco (CEP, descricao, CPF)
    CPF referencia Empregado(CPF)           ✔ 5NF

Fones (CPF, fone)                           ✔ 5NF
```

---

#### Departamento (Cod, descricao, CPF_chefe)
```
CPF_chefe referencia Empregado(CPF)
```
✔ **5NF** — chave simples, sem transitiva, sem DMV.

---

#### Gratificacao (Cod, descricao)
✔ **5NF** — análise idêntica a Projeto.

---

#### Trabalha (CPF, Cod_Depto, data, Cod_Gratif)
```
CPF       referencia Empregado(CPF)
Cod_Depto referencia Departamento(Cod)
Cod_Gratif referencia Gratificação(Cod)
```
✔ **5NF**
- Atributos atômicos (1NF)
- Dependência Funcional Plena da Chave Primária (2NF)
- Sem transitiva (3NF)
- Só DF para chave (BCNF)
- Sem DMV (4NF)
- Sem DJ (5NF)

---

#### Participa (Cod_Proj, Cod_Ativ, CPF)
```
Cod_Proj referencia Projeto(Cod)
Cod_Ativ referencia Atividade(Cod)
CPF      referencia Empregado(CPF)
```
✔ **5NF** — relacionamento ternário; PK composta; sem parciais, transitivas ou multivaloradas.

---

#### Tecnico (CPF, ultimaSerie)
```
CPF referencia Empregado(CPF)   [implícito via especialização]
```
✔ **5NF** — tabela de especialização, chave simples.

---

#### Graduado (CPF)
✔ **5NF** — tabela de especialização sem atributos próprios além da FK.

---

#### Grau (Cod, tipo)
✔ **5NF** — chave simples, sem dependências problemáticas.

---

#### IES (Cod, nome, sigla)
✔ **5NF**
- Nota: `nome ↔ sigla` (possível DF total entre não-chaves), mas sem dependência transitiva envolvendo atributo não-chave como intermediário.

---

#### TitulacaoEmpregado (CPF, data, Cod_Grau, Cod_IES)
```
CPF      referencia Empregado(CPF)
Cod_grau referencia Grau(Cod)
Cod_IES  referencia IES(Cod)
```
✔ **5NF**
- Atributos atômicos (1NF)
- Dependência Funcional Plena da Chave Primária (2NF)
- Sem transitiva (3NF)
- Só DF para chave (BCNF)
- Sem DMV (4NF)
- Sem DJ (5NF)

---

### Esquema Relacional Normalizado Final (exercício)

```
Projeto         (Cod, descricao, valor)

Atividade       (Cod, descricao)

Empregado       (CPF, nome, sexo, salario, dtNasc, CPF_Spv)
                    CPF_Spv referencia Empregado(CPF)

Endereco        (CEP, descricao, CPF)
                    CPF referencia Empregado(CPF)

Fones           (CPF, fone)
                    CPF referencia Empregado(CPF)

Departamento    (Cod, descricao, CPF_chefe)
                    CPF_chefe referencia Empregado(CPF)

Gratificacao    (Cod, descricao)

Trabalha        (CPF, Cod_Depto, data, Cod_Gratif)
                    CPF       referencia Empregado(CPF)
                    Cod_Depto referencia Departamento(Cod)
                    Cod_Gratif referencia Gratificacao(Cod)

Participa       (Cod_Proj, Cod_Ativ, CPF)
                    Cod_Proj referencia Projeto(Cod)
                    Cod_Ativ referencia Atividade(Cod)
                    CPF      referencia Empregado(CPF)

Tecnico         (CPF, ultimaSerie)
                    CPF referencia Empregado(CPF)

Graduado        (CPF)
                    CPF referencia Empregado(CPF)

Grau            (Cod, tipo)

IES             (Cod, nome, sigla)

TitulacaoEmpregado (CPF, data, Cod_Grau, Cod_IES)
                    CPF      referencia Empregado(CPF)
                    Cod_Grau referencia Grau(Cod)
                    Cod_IES  referencia IES(Cod)
```

---

## Apêndice — Referência Rápida

### Operadores da Álgebra Relacional

| Operador | Símbolo | SQL equivalente | Tipo |
|----------|---------|-----------------|------|
| Seleção | σ | `WHERE` | Unário |
| Projeção | π | `SELECT` | Unário |
| Junção | ⋈ | `JOIN` | Binário |
| Divisão | ÷ | `NOT EXISTS` aninhado | Binário |
| União | ∪ | `UNION` | Conjunto |
| Interseção | ∩ | `INTERSECT` | Conjunto |
| Diferença | − | `MINUS` / `EXCEPT` | Conjunto |

### Formas Normais — Quando aplicar

| Forma | Pré-requisito | Elimina |
|-------|--------------|---------|
| 1NF | — | Atributos não atômicos |
| 2NF | 1NF | DF parciais (em PK composta) |
| 3NF | 2NF | Dependências transitivas |
| BCNF | 3NF | Todo determinante não-candidato |
| 4NF | BCNF | Dependências multivaloradas |
| 5NF | 4NF | Dependências de junção |

### Tipos de Dependência

| Tipo | Notação | Significado |
|------|---------|-------------|
| Funcional | X → Y | A um valor de X corresponde um único Y |
| Total (bicondicional) | X ↔ Y | X → Y e Y → X |
| Plena | (X,Y) → Z | Z depende da combinação de X e Y |
| Transitiva | X → Y → Z | Z depende de X via Y |
| Multivalorada | A →→ B | Um valor de A associa-se a um conjunto de B, independente de C |
| Junção | *(X,Y,...,Z) | R = junção de suas projeções nos subconjuntos |

### Passos do Mapeamento E/R → Relacional

| Passo | Elemento E-R | Ação |
|-------|-------------|------|
| 1 | Entidade regular | Criar relação com seus atributos (exceto multivalorados) |
| 2 | Entidade fraca | Criar relação com atributos + PK da proprietária; PK = PK_prop + atrib_discriminador |
| 3 | Relacionamento 1:1 | Inserir PK de uma entidade como FK na outra |
| 4 | Relacionamento 1:N | Inserir PK do lado 1 como FK no lado N |
| 5 | Relacionamento M:N | Criar nova relação com as PKs das entidades como PK composta |
| 6 | Atributo multivalorado | Criar nova relação com o atributo + PK da relação original |
| 7 | Relacionamento n-ário | Criar nova relação com PKs de todas as entidades participantes |

---

*Documento gerado para alimentar IA de ensino. Fonte: Slides da disciplina Gerenciamento de Dados e Informação — CIn/UFPE. Professores: Robson Fidalgo e Valeria Times.*
