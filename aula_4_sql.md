# Aula: SQL — Gerenciamento de Dados e Informação
**Instituição:** CIn / UFPE  
**Professores:** Robson Fidalgo, Valeria Times  
**Disciplina:** Gerenciamento de Dados e Informação  

---

## Sumário

1. [Introdução ao SQL](#1-introdução-ao-sql)
2. [Origem e Histórico](#2-origem-e-histórico)
3. [Enfoques de SQL](#3-enfoques-de-sql)
4. [Usos de SQL (DDL / DML)](#4-usos-de-sql-ddl--dml)
5. [Vantagens e Desvantagens](#5-vantagens-e-desvantagens)
6. [Comandos SQL — Padrão ANSI](#6-comandos-sql--padrão-ansi)
7. [Esquema Relacional dos Exemplos](#7-esquema-relacional-dos-exemplos)
8. [DDL — Criação de Tabelas (CREATE TABLE)](#8-ddl--criação-de-tabelas-create-table)
9. [DDL — Alteração de Tabelas (ALTER TABLE)](#9-ddl--alteração-de-tabelas-alter-table)
10. [DDL — Remoção de Tabelas (DROP TABLE)](#10-ddl--remoção-de-tabelas-drop-table)
11. [DML — Consulta Básica (SELECT)](#11-dml--consulta-básica-select)
12. [Cláusula WHERE e Operadores](#12-cláusula-where-e-operadores)
13. [Operadores Especiais](#13-operadores-especiais)
14. [Ordenação — ORDER BY](#14-ordenação--order-by)
15. [Cálculos em Consultas](#15-cálculos-em-consultas)
16. [Funções Agregadas](#16-funções-agregadas)
17. [DISTINCT](#17-distinct)
18. [GROUP BY](#18-group-by)
19. [Cláusula WITH (CTE)](#19-cláusula-with-cte)
20. [Cláusula HAVING](#20-cláusula-having)
21. [Alias de Tabelas](#21-alias-de-tabelas)
22. [Junção de Tabelas (JOIN)](#22-junção-de-tabelas-join)
23. [Consultas Aninhadas (Subqueries)](#23-consultas-aninhadas-subqueries)
24. [ANY / SOME e ALL](#24-anysome-e-all)
25. [EXISTS e NOT EXISTS](#25-exists-e-not-exists)
26. [Regras Genéricas de Subconsultas](#26-regras-genéricas-de-subconsultas)
27. [Operações de Conjunto](#27-operações-de-conjunto)
28. [Operação de Divisão Relacional](#28-operação-de-divisão-relacional)
29. [DML — Inserção (INSERT)](#29-dml--inserção-insert)
30. [DML — Atualização (UPDATE)](#30-dml--atualização-update)
31. [DML — Remoção de Tuplas (DELETE)](#31-dml--remoção-de-tuplas-delete)
32. [Visões (VIEWS)](#32-visões-views)
33. [Controle de Acesso (GRANT / REVOKE)](#33-controle-de-acesso-grant--revoke)

---

## 1. Introdução ao SQL

**SQL** — *Structured Query Language* (Linguagem de Consulta Estruturada).

Apesar do termo "Query" no nome, o SQL **não é apenas uma linguagem de consulta**: ele permite:
- **DDL** — Definição de dados (criar, alterar, remover estruturas)
- **DML** — Manipulação de dados (consultar, inserir, atualizar, remover)

O SQL é **fundamentado no modelo relacional** (álgebra relacional).

> **Atenção para IA de ensino:** Cada implementação de SQL pode possuir adaptações específicas para o SGBD alvo (Oracle, PostgreSQL, MySQL etc.). Os exemplos desta aula usam sintaxe **Oracle**.

---

## 2. Origem e Histórico

| Ano | Evento |
|-----|--------|
| 1974 | Primeira versão: **SEQUEL**, definida por Chamberlain na IBM |
| 1975 | Implementação do primeiro protótipo |
| 1976–1977 | Revisão e ampliação; nome alterado para **SQL** por razões jurídicas |
| 1982 | **ANSI** torna SQL padrão oficial de linguagem em ambiente relacional |

O SQL pode ser utilizado tanto de forma **interativa** (ad-hoc) como **embutida em linguagens hospedeiras** (C, Java, Python etc.).

---

## 3. Enfoques de SQL

O SQL pode ser utilizado em seis contextos principais:

| Enfoque | Descrição |
|---------|-----------|
| **Linguagem interativa de consulta (ad-hoc)** | Usuários definem consultas independentemente de programas |
| **Linguagem de programação para BD** | Comandos SQL embutidos em aplicações |
| **Linguagem de administração de dados** | O DBA usa SQL para realizar suas tarefas |
| **Linguagem cliente/servidor** | Programas clientes usam SQL para comunicar e compartilhar dados com o servidor |
| **Linguagem para BD distribuídos** | Auxilia na distribuição de dados por vários nós e na transferência entre sistemas |
| **Caminho de acesso a outros BDs** | Auxilia na conversão entre diferentes produtos em diferentes máquinas |

---

## 4. Usos de SQL (DDL / DML)

```
                         SQL
               ┌──────────────────────┐
          DDL                      DML
  ┌───────────────┐       ┌──────────────────┐
  Criar (CREATE)          Consultar (SELECT)
  Remover (DROP)          Inserir (INSERT)
  Modificar (ALTER)       Remover (DELETE)
                          Atualizar (UPDATE)
```

Além de DDL e DML, o SQL abrange: **Segurança**, **Controle**, **Administração**, **Implementação** e **Ambiente**.

---

## 5. Vantagens e Desvantagens

### Vantagens

- Independência de fabricante
- Portabilidade entre sistemas
- Redução de custos com treinamento
- Comandos em Inglês (legibilidade)
- Consulta interativa
- Múltiplas visões de dados
- Manipulação dinâmica dos dados

### Desvantagens

- A padronização inibe a criatividade
- Está longe de ser uma linguagem relacional ideal
- Críticas específicas:
  - Falta de ortogonalidade nas expressões
  - Discordância com as linguagens hospedeiras
  - Não dá suporte a alguns aspectos do modelo relacional

---

## 6. Comandos SQL — Padrão ANSI

Os comandos SQL padrão ANSI cobrem:

1. Criação, alteração e remoção de tabelas
2. Inserção, modificação e remoção de dados
3. Extração de dados de uma tabela (Consultas)
4. Definição de visões
5. Definição de privilégios de acesso

---

## 7. Esquema Relacional dos Exemplos

Todos os exemplos da aula usam o seguinte esquema:

```
Empregado (Cad, Nome, Sexo, Salario, Num_Dep, Cad_Spv)
    Num_Dep  → referencia Departamento(Numero)
    Cad_Spv  → referencia Empregado(Cad)         [auto-referência]

Departamento (Numero, Nome, Cad_Ger, Data_Ini)
    Cad_Ger  → referencia Empregado(Cad)

Locais (Num_dep, Nome_Loc)
    Num_Dep  → referencia Departamento(Numero)

Projeto (Numero, Nome, Num_Dep, Local)
    Num_Dep  → referencia Departamento(Numero)

Trabalha_em (Cad_Emp, Num_Pro, Horas)
    Cad_Emp  → referencia Empregado(Cad)
    Num_Pro  → referencia Projeto(Numero)

Dependente (Cad_emp, Nome, Data_nasc, Grau_P)
    Cad_emp  → referencia Empregado(Cad)
```

### Dados de exemplo usados na aula

**Empregado**

| Cad | Nome  | Sexo | Salario  | Num_Dep | Cad_Spv |
|-----|-------|------|----------|---------|---------|
| 1   | José  | M    | 5000.00  | 1       | 2       |
| 2   | Maria | F    | 800.00   | 2       | NULL    |
| 3   | João  | M    | 3000.00  | 1       | 2       |

**Departamento**

| Numero | Nome          | Cad_Ger | Data_Ini   |
|--------|---------------|---------|------------|
| 1      | RH            | 3       | 12/03/2010 |
| 2      | Contabilidade | 2       | 15/01/2009 |

**Projeto**

| Numero | Nome           | Num_Dep | Local  |
|--------|----------------|---------|--------|
| 1      | Desenvolvimento| 1       | Recife |
| 2      | Análise        | 1       | Olinda |
| 3      | Testes         | 2       | NULL   |

**Trabalha_em**

| Cad_Emp | Num_Proj | Horas |
|---------|----------|-------|
| 1       | 1        | 20    |
| 1       | 2        | 24    |
| 2       | 2        | 44    |
| 3       | 3        | 15    |
| 3       | 2        | 15    |
| 3       | 1        | 14    |

**Dependente**

| Cad_emp | Nome  | Data_nasc  | Grau_P |
|---------|-------|------------|--------|
| 1       | Bruno | 01/02/2000 | P      |
| 2       | Gina  | 05/10/2002 | M      |
| 1       | Telma | 04/03/2010 | A      |

---

## 8. DDL — Criação de Tabelas (CREATE TABLE)

### Sintaxe geral

```sql
CREATE TABLE <nome da tabela> (
    <descrição dos atributos>
    <descrição das chaves>
    <descrição das restrições>
);
```

### Tipos de dados (Oracle)

| Tipo | Descrição |
|------|-----------|
| `VARCHAR2(n)` | String de tamanho variável, máx. n caracteres |
| `CHAR(n)` | String de tamanho fixo |
| `NUMBER` | Número genérico |
| `NUMBER(p)` | Número inteiro com p dígitos |
| `NUMBER(p,e)` | Número com p dígitos totais e e decimais |
| `INTEGER` | Número inteiro |
| `BINARY_INTEGER` | Inteiro binário |
| `BINARY_FLOAT` | Ponto flutuante simples precisão |
| `BINARY_DOUBLE` | Ponto flutuante dupla precisão |
| `DATE` | Data |
| `TIMESTAMP` | Data + hora |
| `BLOB` | Dados binários grandes |
| `CLOB` | Dados de texto grandes |
| `NCLOB` | Dados de texto grandes (Unicode) |

### Chave Primária

```sql
CONSTRAINT nometabela_pkey PRIMARY KEY (<atributos>)
```

### Chave Primária por Auto-numeração (Oracle — Sequence)

No Oracle, chaves auto-incrementadas são implementadas com `SEQUENCE`:

```sql
CREATE SEQUENCE <nome_seq>
    INCREMENT BY 1 START WITH 1;
```

> O tipo do atributo que será a chave primária deve ser `INTEGER`.

Para usar ao inserir:

```sql
INSERT INTO Cliente (Id, Nome) VALUES (seq.NEXTVAL, 'Ana Sousa');
```

### Chave Estrangeira

```sql
CONSTRAINT nometabela_fkey
    FOREIGN KEY (<atributo>)
    REFERENCES <outra_tabela> (<chave primária>)
```

### Restrições (CHECK e UNIQUE)

```sql
-- Valor mínimo (ex: salário mínimo)
CONSTRAINT nometabela_check
    CHECK (salario >= 1400.00)

-- Unicidade de valor (não é PK, mas deve ser único)
CONSTRAINT nometabela_const
    UNIQUE (nome)
```

---

### Exemplos completos de CREATE TABLE

#### Exemplo 1 — Tabela Departamento (sem FK para Empregado ainda)

```sql
CREATE SEQUENCE Num
    INCREMENT BY 1 START WITH 1;

CREATE TABLE Departamento (
    Numero   INTEGER,
    Nome     VARCHAR2(15),
    Cad_Ger  INTEGER,
    Data_Ini DATE,
    CONSTRAINT Departamento_pkey PRIMARY KEY (Numero)
);
```

> A FK para Empregado (`Cad_Ger`) é adicionada depois (ver `ALTER TABLE`), pois Empregado ainda não existe no momento da criação.

#### Exemplo 2 — Tabela Empregado (com CHECK em Sexo e Salario)

```sql
CREATE TABLE Empregado (
    Cad      INTEGER,
    Nome     VARCHAR2(20),
    Sexo     CHAR,
    Salario  NUMBER(10,2),
    Num_Dep  NUMBER(1),
    Cad_Spv  NUMBER,
    CONSTRAINT empregado_pkey    PRIMARY KEY (Cad),
    CONSTRAINT empregado_fkey1   FOREIGN KEY (Num_Dep)  REFERENCES Departamento(Numero),
    CONSTRAINT empregado_fkey2   FOREIGN KEY (Cad_Spv)  REFERENCES Empregado(Cad),
    CONSTRAINT Empregado_checkSal CHECK (salario >= 1400.00),
    CONSTRAINT Empregado_checkSex CHECK (sexo = 'M' OR sexo = 'F')
);
```

#### Exemplo 3 — Tabela Trabalha_em (chave primária composta)

```sql
CREATE TABLE Trabalha_em (
    Cad_Emp   INTEGER,
    Num_Proj  INTEGER,
    Horas     NUMBER(3,1),
    CONSTRAINT trabalha_em_pkey  PRIMARY KEY (Cad_Emp, Num_Proj),
    CONSTRAINT trabalha_em_fkey1 FOREIGN KEY (Cad_Emp)  REFERENCES Empregado(Cad),
    CONSTRAINT trabalha_em_fkey2 FOREIGN KEY (Num_Proj) REFERENCES Projeto(Numero)
);
```

### Criação de Índices

```sql
CREATE [UNIQUE] INDEX <nome> ON <tabela> (<atributo1>[, <atributo2>...]);
```

**Exemplo 3 (índice):** Criar índice sobre `salario` de Empregado:

```sql
CREATE INDEX indice_sal ON Empregado (salario);
```

> O índice é usado **automaticamente** pelo sistema nas consultas filtradas por `salario`.

---

## 9. DDL — Alteração de Tabelas (ALTER TABLE)

```sql
ALTER TABLE <ação>;
```

### Adicionar atributo

**Exemplo 4:** Acrescentar o atributo `Diploma` na tabela Empregado:

```sql
ALTER TABLE EMPREGADO ADD (Diploma VARCHAR2(20));
```

### Remover atributo

**Exemplo 5:** Remover o atributo `Diploma`:

```sql
ALTER TABLE EMPREGADO DROP (Diploma);
```

### Adicionar chave estrangeira

**Exemplo 6:** Acrescentar FK de `Empregado` como gerente na tabela `Departamento`:

```sql
ALTER TABLE DEPARTAMENTO
    ADD (
        CONSTRAINT Departamento_fkey
            FOREIGN KEY (Cad_Ger) REFERENCES Empregado(Cad)
    );
```

> **Regra importante:** Quando duas relações têm chave estrangeira uma da outra (dependência circular), uma das FKs deve ser adicionada via `ALTER TABLE` depois que ambas as tabelas já existem.

---

## 10. DDL — Remoção de Tabelas (DROP TABLE)

```sql
DROP TABLE <tabela>;
```

**Exemplo 7:** Remover a tabela `Trabalha_em`:

```sql
DROP TABLE Trabalha_em;
```

> **Observação:** `DROP TABLE` elimina **dados e estrutura**.

Para eliminar apenas os dados (preservando a estrutura), usar no Oracle:

```sql
TRUNCATE TABLE Trabalha_em;
```

---

## 11. DML — Consulta Básica (SELECT)

### Sintaxe geral

```sql
SELECT <lista de atributos>
FROM <tabela>
[WHERE <condição>];
```

**Álgebra Relacional equivalente:** `π <lista> (Nome-da-relação)`

### Selecionando atributos específicos (Projeção)

**Exemplo 8:** Listar nome e salário de todos os empregados:

```sql
SELECT Nome, Salario FROM Empregado;
```

**Resultado:**

| Nome  | Salario  |
|-------|----------|
| José  | 5000.00  |
| Maria | 800.00   |
| João  | 3000.00  |

### Selecionando todos os atributos

**Exemplo 9:**

```sql
SELECT * FROM Empregado;
```

> **Atenção:** `SELECT *` deve ser usado com cautela — pode gerar custo desnecessário ao trafegar colunas não utilizadas.

---

## 12. Cláusula WHERE e Operadores

### Filtrando tuplas (Seleção)

```sql
SELECT <lista de atributos>
FROM <tabela>
WHERE <condição>;
```

A `<condição>` tem a forma: `<nome atributo> <operador> <valor>`

### Operadores disponíveis

| Categoria | Operador | Significado |
|-----------|----------|-------------|
| **Relacionais** | `=` | Igual a |
| | `<>` ou `!=` | Diferente de |
| | `>` | Maior que |
| | `>=` | Maior ou igual a |
| | `<` | Menor que |
| | `<=` | Menor ou igual a |
| **Lógicos** | `AND` | E |
| | `OR` | Ou |
| | `NOT` | Não |

O `<valor>` pode ser: uma **constante**, uma **variável** ou uma **consulta aninhada**.

### Exemplos

**Exemplo 10:** Listar todos os dados dos empregados do departamento 1:

```sql
SELECT * FROM Empregado WHERE Num_Dep = 1;
```

| Cad | Nome | Sexo | Salario | Num_Dep | Cad_Spv |
|-----|------|------|---------|---------|---------|
| 1   | José | M    | 5000.00 | 1       | 2       |
| 3   | João | M    | 3000.00 | 1       | 2       |

**Exemplo 11:** Nome e sexo dos empregados do departamento 1 com salário > R$ 4.000,00:

```sql
SELECT Nome, Sexo
FROM Empregado
WHERE Num_Dep = 1 AND Salario > 4000.00;
```

| Nome | Sexo |
|------|------|
| José | M    |

### Parâmetro em tempo de execução (Oracle)

Para o usuário fornecer o valor durante a execução (apenas no SQL*Plus do Oracle):

```sql
SELECT <lista de atributos>
FROM <tabela>
WHERE <atributo> = &<variável>;
```

**Exemplo 12:** Listar nome e salário por departamento informado pelo usuário:

```sql
SELECT Nome, Salario
FROM Empregado
WHERE Num_Dep = &cod_dep;
```

> O sistema exibirá: `Informe o valor para cod_dep:`. Digitando `1`, retorna José e João.

---

## 13. Operadores Especiais

### BETWEEN / NOT BETWEEN

Substitui o uso combinado de `>=` e `<=`:

```sql
... WHERE <atributo> BETWEEN <valor1> AND <valor2>;
```

**Exemplo 13:** Listar nomes dos empregados com salário entre R$ 4.000,00 e R$ 10.000,00:

```sql
SELECT Nome FROM Empregado
WHERE Salario BETWEEN 4000 AND 10000;
```

| Nome |
|------|
| José |

---

### LIKE / NOT LIKE

- Aplica-se **apenas a atributos do tipo `CHAR`/`VARCHAR`**
- `LIKE` opera como `=` (com suporte a curingas)
- `NOT LIKE` opera como `<>`
- `%` → substitui **qualquer cadeia** de caracteres (0 ou mais)
- `_` → substitui **exatamente um** caractere

```sql
... WHERE <atributo> LIKE <padrão>;
```

**Exemplo 14:** Listar nomes dos empregados que iniciam com "Jo":

```sql
SELECT Nome FROM Empregado
WHERE Nome LIKE 'Jo%';
```

| Nome |
|------|
| José |
| João |

---

### IN / NOT IN

Verifica se o valor está (ou não) dentro de um conjunto:

```sql
... WHERE <atributo> IN (<valor1>, <valor2>, ...);
```

**Exemplo 15:** Listar nome e data de nascimento dos dependentes com grau de parentesco 'M' ou 'P':

```sql
SELECT Nome, Data_Nasc
FROM Dependente
WHERE Grau_P IN ('M', 'P');
```

| Nome  | Data_nasc  |
|-------|------------|
| Bruno | 01/02/2000 |
| Gina  | 05/10/2002 |

---

### IS NULL / IS NOT NULL

Verifica se um atributo é nulo (não informado):

```sql
... WHERE <atributo> IS NULL;
... WHERE <atributo> IS NOT NULL;
```

**Exemplo 16:** Listar número e nome dos projetos sem local definido:

```sql
SELECT Numero, Nome FROM Projeto
WHERE Local IS NULL;
```

| Numero | Nome   |
|--------|--------|
| 3      | Testes |

---

## 14. Ordenação — ORDER BY

```sql
SELECT <lista atributos>
FROM <tabela>
[WHERE <condição>]
ORDER BY <Nome atributo> {ASC | DESC};
```

- `ASC` — crescente (padrão, pode ser omitido)
- `DESC` — decrescente

### Exemplos

**Exemplo 17:** Todos os empregados ordenados **ascendentemente** por nome:

```sql
SELECT * FROM Empregado ORDER BY Nome;
```

| Cad | Nome  | Sexo | Salario | Num_Dep | Cad_Spv |
|-----|-------|------|---------|---------|---------|
| 3   | João  | M    | 3000.00 | 1       | 2       |
| 1   | José  | M    | 5000.00 | 1       | 2       |
| 2   | Maria | F    | 800.00  | 2       | NULL    |

**Exemplo 18:** Todos os empregados ordenados **descendentemente** por salário:

```sql
SELECT * FROM Empregado ORDER BY Salario DESC;
```

| Cad | Nome  | Sexo | Salario | Num_Dep | Cad_Spv |
|-----|-------|------|---------|---------|---------|
| 1   | José  | M    | 5000.00 | 1       | 2       |
| 3   | João  | M    | 3000.00 | 1       | 2       |
| 2   | Maria | F    | 800.00  | 2       | NULL    |

> **Múltiplos critérios:** `ORDER BY E.Salario DESC, E.Nome` — ordena primeiro por salário decrescente, depois por nome crescente em caso de empate.

---

## 15. Cálculos em Consultas

É possível criar colunas calculadas no resultado usando **operadores aritméticos**:

| Operador | Operação |
|----------|----------|
| `+` | Adição |
| `-` | Subtração |
| `*` | Multiplicação |
| `/` | Divisão |

Para renomear a coluna calculada, usar `AS <alias>`.

**Exemplo 19:** Nome e novo salário (reajuste de 60%) para empregados que ganham abaixo de R$ 4.000,00:

```sql
SELECT Nome, (Salario * 1.60) AS Novo_salario
FROM Empregado
WHERE Salario < 4000.00;
```

| Nome  | Novo_Salario |
|-------|-------------|
| Maria | 1280.00     |
| João  | 4800.00     |

---

## 16. Funções Agregadas

Operam sobre **conjuntos de valores** e são chamadas a partir do `SELECT`:

| Função | Descrição |
|--------|-----------|
| `AVG` | Média |
| `MIN` | Mínimo |
| `MAX` | Máximo |
| `COUNT` | Contar |
| `SUM` | Somar |

### Exemplos

**Exemplo 20:** Nome e salário do empregado que recebe o **maior salário** (usando subconsulta):

```sql
SELECT Nome, Salario
FROM Empregado
WHERE Salario IN (
    SELECT MAX(Salario) FROM EMPREGADO
);
```

| Nome | Salario |
|------|---------|
| José | 5000.00 |

**Exemplo 21:** Salário médio dos empregados:

```sql
SELECT AVG(Salario) FROM Empregado;
```

| AVG(Salario) |
|-------------|
| 2933.33     |

**Exemplo 22:** Quantos empregados ganham mais de R$ 4.000,00:

```sql
SELECT COUNT(*) FROM Empregado
WHERE Salario > 4000.00;
```

| Count(*) |
|----------|
| 1        |

---

## 17. DISTINCT

Elimina **tuplas duplicadas** do resultado de uma consulta:

```sql
SELECT DISTINCT <atributo> FROM <tabela>;
```

**Exemplo 23:** Quais os diferentes códigos dos supervisores dos empregados?

```sql
SELECT DISTINCT Cad_spv FROM Empregado;
```

| Cad_Spv |
|---------|
| NULL    |
| 2       |

---

## 18. GROUP BY

Organiza a seleção de dados em **grupos**:

```sql
SELECT <atributos>, <função agregada>
FROM <tabela>
GROUP BY <atributos>;
```

> **Regra fundamental:** Todos os atributos presentes no `SELECT` **devem aparecer no `GROUP BY`**, com exceção das funções agregadas.

### Exemplos de erros comuns

```sql
-- ERRO: Sexo está no SELECT mas não no GROUP BY
SELECT Sexo, Count(*) FROM Empregado;

-- CORRETO: Sexo no GROUP BY
SELECT Sexo, Count(*) FROM Empregado GROUP BY Sexo;

-- OK: sem atributo não-agregado no SELECT
SELECT Count(*) FROM Empregado GROUP BY Sexo;

-- OK: apenas agregação sem GROUP BY
SELECT Count(*) FROM Empregado;
```

**Exemplo 24:** Quantitativos de empregados de cada sexo:

```sql
SELECT Sexo, Count(*)
FROM Empregado
GROUP BY Sexo;
```

| Sexo | Count(*) |
|------|----------|
| M    | 2        |
| F    | 1        |

---

## 19. Cláusula WITH (CTE)

*Common Table Expression* — funciona como uma **subconsulta temporária** que pode ser referenciada várias vezes na instrução:

```sql
WITH <alias_consulta> AS (
    SELECT coluna1, ..., colunaN
    FROM <tabela>
    WHERE <condição1>
)
SELECT *
FROM <alias_consulta>
WHERE <condição2>;
```

**Exemplo 25:** Total de empregados por departamento usando `WITH`:

```sql
WITH total AS (
    SELECT Num_dep, COUNT(*)
    FROM Empregado
    GROUP BY Num_dep
)
SELECT * FROM total;
```

---

## 20. Cláusula HAVING

Filtra **grupos** após o agrupamento. É equivalente ao `WHERE`, mas aplicado a grupos:

- Vem **depois** do `GROUP BY`
- Vem **antes** do `ORDER BY`

```sql
SELECT <atributos>, <função agregada>
FROM <tabela>
[WHERE <condição sobre tuplas>]
GROUP BY <atributos>
HAVING <condição sobre grupos>
[ORDER BY <atributos>];
```

**Exemplo 26:** Total de empregados com salário > R$ 1.000,00 em departamentos com mais de 1 empregado:

```sql
SELECT Num_Dep, COUNT(*)
FROM Empregado
WHERE Salario > 1000
GROUP BY Num_Dep
HAVING COUNT(*) > 1;
```

| Num_Dep | Count(*) |
|---------|----------|
| 1       | 2        |

---

## 21. Alias de Tabelas

Permitem substituir nomes de tabelas em comandos SQL. São definidos na cláusula `FROM`:

```sql
SELECT A.nome FROM Departamento A WHERE A.Numero = 15;
```

> Quando se juntam tabelas com colunas de mesmo nome (ex: `Empregado.Nome` e `Departamento.Nome`), o uso de alias é **obrigatório** para evitar ambiguidades.

---

## 22. Junção de Tabelas (JOIN)

### Inner Join (Junção Simples)

Retorna **apenas as tuplas que satisfazem a condição de junção**. Equivalente à junção natural.

**Técnica clássica (vírgula no FROM + WHERE):**

```sql
SELECT <atributos>
FROM <tabela_A> A, <tabela_B> B
WHERE A.<chave> = B.<chave_estrangeira>;
```

---

**Exemplo 27:** Nome de cada empregado e nome do seu departamento:

```sql
SELECT E.Nome, D.Nome
FROM Empregado E, Departamento D
WHERE E.Num_Dep = D.Numero;
```

| E.NOME | D.NOME        |
|--------|---------------|
| José   | RH            |
| João   | RH            |
| Maria  | Contabilidade |

---

**Exemplo 28:** Nomes dos departamentos que têm projetos:

```sql
SELECT D.Nome
FROM Departamento D, Projeto P
WHERE P.Num_Dep = D.Numero;
```

| Nome |
|------|
| RH   |

---

**Exemplo 29:** Departamentos com projetos de número < 3, localizados em Olinda ou Recife, ordenados por nome:

```sql
SELECT D.Nome
FROM Departamento D, Projeto P
WHERE P.Local IN ('Olinda', 'Recife')
  AND P.Numero < 3
  AND P.Num_Dep = D.Numero
ORDER BY D.Nome;
```

| Nome |
|------|
| RH   |

---

**Exemplo 30:** Nome do departamento, número, nome e salário de seus empregados; ordenado por salário decrescente e nome crescente:

```sql
SELECT D.Nome, E.Cad, E.Nome, E.Salario
FROM Departamento D, Empregado E
WHERE D.Numero = E.Num_Dep
ORDER BY E.Salario DESC, E.Nome;
```

| D.NOME        | E.CAD | E.NOME | E.SALARIO |
|---------------|-------|--------|-----------|
| RH            | 1     | José   | 5000.00   |
| RH            | 3     | João   | 3000.00   |
| Contabilidade | 2     | Maria  | 800.00    |

---

**Exemplo 31:** Total de projetos de cada empregado por departamento:

```sql
SELECT E.Num_Dep, E.Cad, COUNT(*) AS Total
FROM Trabalha_em T, Empregado E
WHERE E.Cad = T.Cad_Emp
GROUP BY E.Num_Dep, E.Cad
ORDER BY E.Num_Dep, E.Cad;
```

| E.NUM_DEP | E.CAD | TOTAL |
|-----------|-------|-------|
| 1         | 1     | 2     |
| 1         | 3     | 3     |
| 2         | 2     | 1     |

---

**Exemplo 32:** Junção com mais de duas tabelas — empregados e departamentos onde trabalham > 20 horas em algum projeto:

```sql
SELECT E.Nome, D.Nome
FROM Empregado E, Departamento D, Trabalha_em T
WHERE T.Horas > 20
  AND T.Cad_Emp = E.Cad
  AND E.Num_Dep = D.Numero;
```

| E.NOME | D.NOME        |
|--------|---------------|
| José   | RH            |
| Maria  | Contabilidade |

---

### Outer Join

Retorna **todas as tuplas de uma das tabelas** mais as tuplas correspondentes da outra. Onde não há correspondência, os campos da outra tabela recebem `NULL`.

#### LEFT OUTER JOIN

Retorna **todas as tuplas de A** + tuplas de B onde a condição é satisfeita:

```sql
SELECT <atributos>
FROM <tabela A> LEFT [OUTER] JOIN <tabela B>
ON <condição de junção>;
```

**Exemplo 33:** Todos os departamentos com os nomes e locais de seus projetos:

```sql
SELECT D.Nome, P.Nome, P.Local
FROM Departamento D LEFT OUTER JOIN Projeto P
ON D.Numero = P.Num_Dep;
```

| D.NOME        | P.NOME          | P.LOCAL |
|---------------|-----------------|---------|
| RH            | Desenvolvimento | Recife  |
| RH            | Análise         | Olinda  |
| RH            | Testes          | (NULL)  |
| Contabilidade | (NULL)          | (NULL)  |

> "Contabilidade" aparece mesmo sem projetos, com NULLs nos campos de Projeto.

---

#### RIGHT OUTER JOIN

Retorna **todas as tuplas de B** + tuplas de A onde a condição é satisfeita:

```sql
SELECT <atributos>
FROM <tabela A> RIGHT [OUTER] JOIN <tabela B>
ON <condição de junção>;
```

**Exemplo 34:** Departamentos com projetos + projetos sem departamento:

*(Projeto 4 = "Otimização" sem Num_Dep definido, apenas para este exemplo)*

```sql
SELECT D.Nome, P.Nome, P.Local
FROM Departamento D RIGHT OUTER JOIN Projeto P
ON D.Numero = P.Num_Dep;
```

| D.NOME | P.NOME          | P.LOCAL |
|--------|-----------------|---------|
| RH     | Desenvolvimento | Recife  |
| RH     | Análise         | Olinda  |
| RH     | Testes          | (NULL)  |
| (NULL) | Otimização      | Caruaru |

---

#### FULL OUTER JOIN

Retorna **todas as tuplas de A e B**, com NULLs onde não há correspondência:

```sql
SELECT <atributos>
FROM <tabela A> FULL [OUTER] JOIN <tabela B>
ON <condição de junção>;
```

**Exemplo 35:**

```sql
SELECT D.Nome, P.Nome, P.Local
FROM Departamento D FULL OUTER JOIN Projeto P
ON D.Numero = P.Num_Dep;
```

| D.NOME        | P.NOME          | P.LOCAL |
|---------------|-----------------|---------|
| RH            | Desenvolvimento | Recife  |
| RH            | Análise         | Olinda  |
| RH            | Testes          | (NULL)  |
| Contabilidade | (NULL)          | (NULL)  |
| (NULL)        | Otimização      | Caruaru |

---

#### LEFT JOIN para encontrar "tuplas fantasmas" (sem correspondência)

```sql
SELECT A.nome, A.aluno_id, M.matricula_id, M.nota_final
FROM Aluno A LEFT JOIN Matricula M
ON A.aluno_id = M.aluno_id;
```

> Alunos sem matrícula aparecerão com `NULL` nas colunas de Matrícula. Útil para encontrar registros sem correspondência.

---

## 23. Consultas Aninhadas (Subqueries)

O resultado de uma consulta é utilizado por outra consulta no **mesmo comando SQL**:

- O `SELECT` interno (**subselect** ou **subconsulta**) pode ser usado nas cláusulas `WHERE`, `HAVING` e `FROM`
- Subconsultas devem ser escritas entre `(` e `)`

### Tipos de subconsultas

| Tipo | Retorna |
|------|---------|
| **ESCALAR** | Um único valor (1 linha × 1 coluna) |
| **ÚNICA LINHA** | Várias colunas, mas apenas 1 linha |
| **TABELA** | Uma ou mais colunas e múltiplas linhas |

---

**Exemplo 36:** Cadastro, nome e salário dos empregados do departamento de Contabilidade:

```sql
SELECT Cad, Nome, Salario
FROM Empregado
WHERE Num_Dep = (
    SELECT Numero
    FROM Departamento
    WHERE Nome = 'Contabilidade'
);
```

| E.CAD | E.NOME | E.SALARIO |
|-------|--------|-----------|
| 2     | Maria  | 800.00    |

> Esta subconsulta é **escalar** (retorna um único valor).

---

**Exemplo 37:** Empregados com salário acima da média, mostrando a diferença:

```sql
SELECT Cad, Nome,
       Salario - (SELECT AVG(Salario) FROM Empregado) AS DifSal
FROM Empregado
WHERE Salario > (SELECT AVG(Salario) FROM Empregado);
```

| Cad | Nome | DifSal  |
|-----|------|---------|
| 1   | José | 2400.00 |

---

**Exemplo 38:** Dependentes dos funcionários do departamento RH (dois níveis de aninhamento):

```sql
SELECT Nome, Data_nasc, Grau_P
FROM Dependente
WHERE Cad_Emp IN (
    SELECT Cad FROM Empregado
    WHERE Num_Dep = (
        SELECT Numero
        FROM Departamento
        WHERE Nome = 'RH'
    )
);
```

| Nome  | Data_Nasc  | Grau_P |
|-------|------------|--------|
| Jonas | 12/05/2000 | Pai    |
| Clara | 13/02/2001 | Pai    |

---

## 24. ANY/SOME e ALL

### ANY / SOME

Usados com subconsultas que produzem uma **única coluna de números**.

- `> SOME(subconsulta)` → maior que **pelo menos um** valor da subconsulta

**Exemplo 39:** Empregados com salário maior que o de pelo menos um funcionário do departamento 1:

```sql
SELECT Cad, Nome, Sexo, Salario
FROM Empregado
WHERE Salario > SOME (
    SELECT Salario FROM Empregado WHERE Num_Dep = 1
);
```

| CAD | NOME | SEXO | SALARIO |
|-----|------|------|---------|
| 1   | José | M    | 5000.00 |

---

### ALL

- `< ALL(subconsulta)` → menor que **todos** os valores da subconsulta

**Exemplo 40:** Empregados com salário menor que o de **cada** funcionário do departamento 1:

```sql
SELECT Cad, Nome, Sexo, Salario
FROM Empregado
WHERE Salario < ALL (
    SELECT Salario FROM Empregado WHERE Num_Dep = 1
);
```

| CAD | NOME  | SEXO | SALARIO |
|-----|-------|------|---------|
| 2   | Maria | F    | 800.00  |

---

## 25. EXISTS e NOT EXISTS

Projetadas para uso **exclusivamente com subconsultas**:

- `EXISTS` → retorna `TRUE` se a subconsulta retornar **pelo menos uma linha**
- `NOT EXISTS` → retorna `TRUE` se a subconsulta retornar **nenhuma linha**

> Deve ser usado quando for necessário certificar-se que haverá resposta a uma subconsulta.

**Exemplo 41:** Todos os empregados do departamento RH:

```sql
SELECT Cad, Nome, Sexo, Salario
FROM Empregado E
WHERE EXISTS (
    SELECT D.Numero FROM Departamento D
    WHERE E.Num_Dep = D.Numero
      AND D.Nome = 'RH'
);
```

| CAD | NOME | SEXO | SALARIO |
|-----|------|------|---------|
| 1   | José | M    | 5000.00 |
| 3   | João | M    | 3000.00 |

---

## 26. Regras Genéricas de Subconsultas

1. A cláusula `ORDER BY` **não pode** ser usada dentro de uma subconsulta
2. A lista de atributos no `SELECT` de uma subconsulta deve conter **um único elemento** (exceto para `EXISTS`)
3. Nomes de atributos na subconsulta estão associados às tabelas listadas no `FROM` da **mesma subconsulta**
4. É possível referenciar uma tabela do `FROM` externo usando **qualificadores de atributos** (correlated subquery)
5. Quando a subconsulta é um dos operandos em uma comparação, ela deve aparecer no **lado direito** da comparação

---

## 27. Operações de Conjunto

### UNION

Remove linhas duplicadas. Combina resultados de dois `SELECT`.

**Exemplo 42:** Lista de todos os locais com departamento ou projeto:

```sql
(SELECT Local FROM Projeto WHERE Local IS NOT NULL)
UNION
(SELECT Nome_Loc FROM Locais);
```

| Local      |
|------------|
| Recife     |
| Olinda     |
| Camaragibe |
| Jaboatão   |

---

### INTERSECT

Retorna apenas os valores **presentes em ambos** os resultados.

**Exemplo 43:** Locais onde existem departamento **e** projeto:

```sql
(SELECT Local FROM Projeto)
INTERSECT
(SELECT Nome_Loc FROM Locais);
```

| Local  |
|--------|
| Olinda |

---

### MINUS (ou EXCEPT em outros SGBDs)

Retorna os valores do primeiro `SELECT` que **não existem** no segundo.

**Exemplo 44:** Locais com departamento mas **sem** projeto:

```sql
(SELECT Nome_Loc FROM Locais)
MINUS
(SELECT Nome_Loc FROM Projeto);
```

| Nome_Loc   |
|------------|
| Camaragibe |
| Jaboatão   |

---

### Correspondência entre Álgebra Relacional e SQL

| Álgebra Relacional | SQL |
|-------------------|-----|
| σ (Seleção) | `WHERE` |
| π (Projeção) | `SELECT` |
| × (Produto Cartesiano) | `CROSS JOIN` |
| ⋈ (Junção) | `INNER JOIN` / `LEFT JOIN` |
| ÷ (Divisão) | Lógica de `NOT EXISTS` |

---

## 28. Operação de Divisão Relacional

O SQL padrão **não possui** um operador `DIVIDE BY`. A divisão relacional responde à pergunta: **"Quem fez TODOS os itens de um conjunto?"**

> Exemplo motivador: "Quais alunos cursaram TODAS as disciplinas obrigatórias do curso de Informática?"

### Abordagem 1 — Dupla Negação com NOT EXISTS Aninhado

Lógica: *"Não existe disciplina obrigatória para a qual o aluno NÃO tenha sido aprovado"*

```sql
SELECT A.nome
FROM Aluno A
WHERE NOT EXISTS (
    SELECT D.disc_id
    FROM Disciplina D
    WHERE D.curso = 'Informatica'
      AND NOT EXISTS (
          SELECT M.aluno_id
          FROM Matricula M
          WHERE M.aluno_id = A.aluno_id
            AND M.disc_id = D.disc_id
            AND M.situacao = 'aprovado'
      )
);
```

**Leitura:** Para cada aluno A, retorna-o se **não existe** disciplina de Informática para a qual **não existe** uma matrícula aprovada do aluno.

---

### Abordagem 2 — Agrupamento e Contagem

Lógica: *"Quantidade de disciplinas cursadas pelo aluno = quantidade total de disciplinas do curso"*

```sql
SELECT A.nome
FROM Aluno A, Matricula M, Disciplina D
WHERE D.curso = 'Informatica'
  AND M.aluno_id = A.aluno_id
  AND M.disc_id = D.disc_id
  AND M.situacao = 'aprovado'
GROUP BY A.aluno_id, A.nome
HAVING COUNT(DISTINCT M.disc_id) = (
    SELECT COUNT(*) FROM Disciplina D
    WHERE D.curso = 'Informatica'
);
```

---

## 29. DML — Inserção (INSERT)

### Inserção de uma única linha

```sql
INSERT INTO <tabela> (<lista de atributos>)
VALUES (<valores>);
```

Se forem fornecidos valores para **todos os atributos na ordem** em que foram definidos, a lista de atributos é opcional:

```sql
INSERT INTO <tabela> VALUES (<valores>);
```

**Exemplo 45:** Inserir um novo empregado:

```sql
INSERT INTO Empregado
VALUES (4, 'Clara', 'F', 7000.00, 1, 2);
```

**Tabela resultante:**

| Cad | Nome  | Sexo | Salario | Num_Dep | Cad_Spv |
|-----|-------|------|---------|---------|---------|
| 1   | José  | M    | 5000.00 | 1       | 2       |
| 2   | Maria | F    | 800.00  | 2       | NULL    |
| 3   | João  | M    | 3000.00 | 1       | 2       |
| 4   | Clara | F    | 7000.00 | 1       | 2       |

---

### Inserção de várias linhas (via SELECT)

```sql
INSERT INTO <tabela> (<lista de atributos>)
SELECT <lista de atributos>
FROM <tabela_origem>
WHERE <condição>;
```

> As duas listas de atributos devem ser **união compatíveis** (mesma quantidade e tipos compatíveis).

**Exemplo 46:** Armazenar em `Depto_Info` (Nome_Depto, Num_Emp, Total_Sal) os departamentos com mais de 1 empregado:

```sql
INSERT INTO Depto_info (nome_depto, num_emp, total_sal)
SELECT D.nome, COUNT(*), SUM(E.salario)
FROM Departamento D, Empregado E
WHERE D.numero = E.Num_Dep
GROUP BY D.nome
HAVING COUNT(*) > 1;
```

**Resultado em Depto_Info:**

| Nome_Depto | Num_Emp | Total_Sal |
|------------|---------|-----------|
| RH         | 3       | 15000.00  |

---

## 30. DML — Atualização (UPDATE)

```sql
UPDATE <nome tabela>
SET <nome atributo> = <valor>
WHERE <condição>;
```

**Exemplo 47:** Atualizar salário do empregado 2 para R$ 9.500,00:

```sql
UPDATE Empregado SET Salario = 9500.00
WHERE Cad = 2;
```

**Tabela resultante:**

| Cad | Nome  | Sexo | Salario | Num_Dep | Cad_Spv |
|-----|-------|------|---------|---------|---------|
| 1   | José  | M    | 5000.00 | 1       | 2       |
| 2   | Maria | F    | **9500.00** | 2   | NULL    |
| 3   | João  | M    | 3000.00 | 1       | 2       |
| 4   | Clara | F    | 7000.00 | 1       | 2       |

---

## 31. DML — Remoção de Tuplas (DELETE)

```sql
DELETE FROM <tabela> WHERE <condição>;
```

**Exemplo 48:** Remover todos os empregados com salário inferior a R$ 5.000,00:

```sql
DELETE FROM Empregado
WHERE Salario < 5000.00;
```

**Tabela resultante** (João foi removido por ter salário de R$ 3.000,00):

| Cad | Nome  | Sexo | Salario | Num_Dep | Cad_Spv |
|-----|-------|------|---------|---------|---------|
| 1   | José  | M    | 5000.00 | 1       | 2       |
| 2   | Maria | F    | 9500.00 | 2       | NULL    |
| 4   | Clara | F    | 7000.00 | 1       | 2       |

> **Atenção:** `DELETE FROM <tabela>` sem `WHERE` remove **todas** as tuplas.

---

## 32. Visões (VIEWS)

São **tabelas virtuais** que não ocupam espaço físico no banco de dados para os dados. Armazenam apenas a definição da consulta.

### Criação

```sql
CREATE VIEW <nome da view> AS
SELECT ...;
```

### Operações possíveis

- Criação e utilização (consulta via `SELECT`)
- Inserção e modificação (semântica depende da natureza da visão)

**Exemplo 49:** Criar uma visão dos empregados do departamento 1 com mais de 20 horas em algum projeto:

```sql
CREATE VIEW Dep_1 AS
    SELECT E.Nome, T.Num_Proj
    FROM Empregado E, Trabalha_em T
    WHERE T.Horas > 20
      AND T.Cad_Emp = E.Cad
      AND E.Num_Dep = 1;
```

**Testando a VIEW:**

```sql
SELECT * FROM Dep_1;
```

| Nome  | Num_Proj |
|-------|----------|
| José  | 2        |
| Clara | 1        |

---

## 33. Controle de Acesso (GRANT / REVOKE)

### Concedendo privilégios — GRANT

```sql
GRANT <privilégios> ON <nome tabela/view>
TO <usuário>;
```

| Parâmetro | Valores possíveis |
|-----------|------------------|
| `<privilégios>` | `SELECT`, `INSERT`, `DELETE`, `UPDATE`, `ALL PRIVILEGES` |
| `<usuário>` | usuário cadastrado ou `PUBLIC` (todos os usuários) |

**Exemplo 50:** Conceder permissão de consulta sobre `EMPREGADO` ao usuário `user`:

```sql
GRANT SELECT ON Empregado TO user;
```

---

### Removendo privilégios — REVOKE

```sql
REVOKE <privilégios> ON <nome tabela/view>
FROM <usuário>;
```

**Exemplo 51:** Remover a permissão de consulta sobre `Projeto` de todos os usuários:

```sql
REVOKE SELECT ON Projeto FROM PUBLIC;
```

---

## Apêndice — Resumo dos Principais Comandos

### DDL

| Comando | Finalidade |
|---------|-----------|
| `CREATE TABLE` | Criar nova tabela |
| `ALTER TABLE ADD` | Adicionar atributo ou restrição |
| `ALTER TABLE DROP` | Remover atributo |
| `DROP TABLE` | Remover tabela (estrutura + dados) |
| `TRUNCATE TABLE` | Remover dados (preserva estrutura) |
| `CREATE INDEX` | Criar índice para otimização |
| `CREATE SEQUENCE` | Criar sequência auto-incremento (Oracle) |
| `CREATE VIEW` | Criar visão (tabela virtual) |

### DML

| Comando | Finalidade |
|---------|-----------|
| `SELECT` | Consultar dados |
| `INSERT INTO` | Inserir dados |
| `UPDATE SET` | Atualizar dados |
| `DELETE FROM` | Remover dados |

### Cláusulas de SELECT

| Cláusula | Finalidade |
|----------|-----------|
| `FROM` | Especifica tabela(s) |
| `WHERE` | Filtra tuplas |
| `GROUP BY` | Agrupa resultado |
| `HAVING` | Filtra grupos |
| `ORDER BY` | Ordena resultado |
| `DISTINCT` | Remove duplicatas |
| `WITH ... AS` | Define CTE (consulta temporária) |

### Operadores Especiais

| Operador | Uso |
|----------|-----|
| `BETWEEN v1 AND v2` | Intervalo de valores |
| `LIKE 'padrão'` | Casamento de padrão em strings |
| `IN (lista)` | Pertence ao conjunto |
| `IS NULL` | Valor nulo |
| `EXISTS (subq)` | Subconsulta retorna alguma linha |
| `> SOME (subq)` | Maior que pelo menos um valor |
| `< ALL (subq)` | Menor que todos os valores |

### Operações de Conjunto

| Operação | SQL | Álgebra |
|----------|-----|---------|
| União | `UNION` | ∪ |
| Intersecção | `INTERSECT` | ∩ |
| Diferença | `MINUS` / `EXCEPT` | − |

### Controle de Acesso

| Comando | Finalidade |
|---------|-----------|
| `GRANT` | Conceder privilégios |
| `REVOKE` | Revogar privilégios |

---

*Documento gerado para alimentar IA de ensino. Fonte: Slides da disciplina Gerenciamento de Dados e Informação — CIn/UFPE. Professores: Robson Fidalgo e Valeria Times.*
