# PL/SQL — Gerenciamento de Dados e Informação
**Instituição:** CIn — UFPE  
**Autores:** Robson Fidalgo, Valeria Times

---

## 1. O que é PL/SQL?

**PL/SQL** = *Procedural Language / SQL*

- Linguagem de programação sofisticada utilizada para ter acesso a uma base de dados Oracle a partir de vários ambientes.
- **Integrada** no servidor da base de dados.
- Também disponível em ferramentas cliente Oracle.
- Pode ser usada a partir de aplicações em outras linguagens (ex.: Java via JDBC).
- O modelo para sua criação é a linguagem **ADA**.

### Por que usar PL/SQL?

- Combina o **poder e a flexibilidade de SQL** com as **estruturas de código procedural** encontradas nas linguagens de 3ª geração.
- Estruturas de procedimento disponíveis:
  - Variáveis e tipos (pré-definidos ou não)
  - Estruturas de controle: `IF-THEN-ELSE` e laços
  - Procedimentos e funções
  - Tipos de objeto e métodos (a partir da versão 8)

---

## 2. Elementos Básicos de PL/SQL

### 2.1 Variáveis

#### Conceito
- Utilizadas para transmitir informação entre o programa PL/SQL e a base de dados.
- Localização de memória que pode ser lida ou ter valor armazenado a partir do programa.
- **Não inicializadas recebem o valor `NULL` por default.**

#### Identificadores (Nomes de Variáveis)
- Sequência de **até 30 caracteres**.
- Deve **iniciar por letra**.
- Os demais caracteres podem ser: letras, dígitos, sublinhado (`_`) e cifrões (`$`).
- **Não são case sensitive** (maiúsculas e minúsculas equivalentes).
- **Não deve ser** uma palavra reservada.
- Evitar usar nomes de colunas da base de dados.

#### Tipos de Variáveis
Os mesmos usados pelo Oracle:

| Categoria   | Tipo                   | Descrição |
|-------------|------------------------|-----------|
| Numérico    | `BINARY_INTEGER`       | Inteiro de −2²³¹−1 a 2²³¹−1 |
| Numérico    | `NATURAL`              | Inteiro de 0 a 2³¹ |
| Numérico    | `POSITIVE`             | Inteiro de 1 a 2³¹ |
| Numérico    | `NUMBER(p, e)`         | `p` = precisão; `e` = escala (casas decimais) |
| Caractere   | `CHAR(N)`              | Tamanho **fixo** N |
| Caractere   | `VARCHAR2(N)`          | Tamanho **máximo** N |
| Booleano    | `BOOLEAN`              | Valores: `TRUE` ou `FALSE` |
| Data-Tempo  | `DATE`                 | Usar apóstrofo `'`. Para operações, usar funções Oracle para Date-Time. |

---

### 2.2 Registros

#### Declaração
```sql
CREATE TYPE nome IS RECORD (
    <identificador1> <tipo1>,
    ...
    <identificadorn> <tipon>
);
```

#### Uso
```sql
<variável> nome;
```

#### Declarar registro com os mesmos tipos de uma tabela do BD:
```sql
<variável> cliente%ROWTYPE;
-- "cliente" é o nome da tabela
```

#### Exemplo 1 — Funcionário definido por nome, CPF e salário
```sql
CREATE TYPE Funcionario IS RECORD (
    nome    VARCHAR2(30),
    CPF     VARCHAR2(13),
    Salario NUMBER(8,2)
);

-- Declaração de variável do tipo Funcionario:
V_func Funcionario;
```

---

### 2.3 Tabelas (Estrutura Virtual)

- Estrutura virtual que **só ocorre em memória principal**, análoga às Visões Relacionais.
- Só existe durante a execução do programa PL.
- Acesso **apenas por indexação** dos elementos.
- Índice deve ser do tipo `BINARY_INTEGER`.
- **Não** se pode usar `SELECT`, `INSERT`, etc., para acessar esta estrutura.

#### Declaração
```sql
TYPE <tipo-tabela> IS TABLE OF <tipo-de-dado>
INDEX BY BINARY_INTEGER;
```

- Define-se o **tipo** da tabela e depois uma **variável** desse tipo.
- Funcionam analogamente a matrizes em C.
- `<tipo-de-dado>` pode ser referência a tipo escalar usando `%TYPE`.

#### Exemplo 2 — Tabela virtual com tipo do atributo `modelo` da tabela `carro`
```sql
TYPE Tabela IS TABLE OF carro.modelo%TYPE
INDEX BY BINARY_INTEGER;

v_modelo Tabela;   -- Declaração da variável
...
v_modelo(i)...     -- Acesso ao elemento (i deve ser BINARY_INTEGER)
```

---

### 2.4 Operadores

| Operador       | Símbolo         | Descrição |
|----------------|-----------------|-----------|
| Aritméticos    | `+`, `-`, `*`, `**`, `/` | Operações matemáticas; `**` = potência |
| Atribuição     | `:=`            | Atribuir valor a variável |
| Diferente de   | `<>` ou `~=`   | Comparação de desigualdade |
| Ref. ao BD     | `@`             | Indica instância de BD distribuído |

---

### 2.5 Declaração e Inicialização de Variáveis

#### Comentários
```sql
-- Comentário de uma linha

/* Comentário de
   mais de uma linha */
```

#### Data do sistema
```sql
data_saida DATE := SYSDATE;
```

#### Declaração de constante
```sql
desconto_padrao CONSTANT NUMBER(3,2) := 8.25;
-- NUMBER(3,2): 3 dígitos totais, 2 casas decimais
```

#### Declaração de valor default
```sql
participante BOOLEAN DEFAULT TRUE;
```

#### Declaração de variável com tipo de atributo de tabela (`%TYPE`)
```sql
<variavel> carro.modelo%TYPE;
```

---

## 3. Blocos PL/SQL

### 3.1 Conceito
- Permitem agrupar várias instruções SQL em um único bloco, enviado como **uma só unidade** ao servidor.
- **Unidade básica** da linguagem — todos os programas são construídos por blocos que podem ser encadeados.
- Cada bloco executa uma **unidade lógica de trabalho**.
- Blocos anônimos são construídos dinamicamente e executados uma vez.

### 3.2 Estrutura Geral

```sql
DECLARE
    /* Seção para declarar variáveis, tipos,
       cursores e subprogramas locais */
BEGIN
    /* Seção executável — comandos procedurais e SQL.
       É a ÚNICA seção obrigatória */
EXCEPTION
    -- Comandos de manipulação de erros
END;
```

### 3.3 Blocos Anônimos
Não possuem nome:
```sql
DECLARE
    <definições de variáveis>
BEGIN
    <comandos>
EXCEPTION
    <tratamento de erros de execução>
END;
/
```
> O símbolo `/` manda executar o bloco.

### 3.4 Blocos Nomeados
Semelhantes aos anônimos, mas possuem um **rótulo**:
```sql
<<l_nome>>
DECLARE
    <definições de variáveis>
BEGIN
    <comandos>
EXCEPTION
    <tratamento de erros de execução>
END l_nome;
/
```

### 3.5 Escopo de Variável

- **Local:** definida dentro do bloco.
- **Global:** definida fora do bloco.

```sql
<<global>>
DECLARE
    sexo CHAR := 'F';   -- variável global
BEGIN
    ...
    DECLARE
        sexo CHAR := 'M';   -- variável local
    BEGIN
        ...sexo...           -- acessa 'M' (local)
        ...global.sexo...    -- acessa 'F' (global, via rótulo)
    END global;
END;
```

### 3.6 Exemplo 3 — Atualizar telefone de cliente

```sql
DECLARE
    v_telefone VARCHAR2(10) := '21268430';
    v_cpf      VARCHAR2(12) := '123456789-34';
BEGIN
    -- Atualiza a tabela CLIENTE
    UPDATE cliente
    SET    telefone = v_telefone
    WHERE  cpf = v_cpf;
EXCEPTION
    -- Se registro não encontrado, insere como nova tupla
    IF SQL%NOTFOUND THEN
        INSERT INTO cliente (cpf, telefone)
        VALUES (v_cpf, v_telefone);
    END IF;
END;
/
```
> **Atenção:** válido apenas se os demais atributos de `Cliente` não forem `NOT NULL` e um dos atributos informados for chave primária.

---

## 4. Controle de Processamento

### 4.1 Estruturas de Controle — IF-THEN-ELSE

```sql
IF <expressão booleana 1> THEN
    <instruções1>
[ELSIF <expressão booleana 2> THEN
    <instruções2>]
[ELSE
    <instruções3>]
END IF;
```

#### Exemplo 4 — Situação do estudante
```sql
IF media >= 7.0 THEN
    situacao := 'Aprovado';
ELSIF media < 5.0 THEN
    situacao := 'Reprovado';
ELSE
    situacao := 'Final';
END IF;
```

---

### 4.2 Estruturas de Controle — CASE

```sql
CASE <seletor>
    WHEN <valor1> THEN <instruções1>;
    ...
    WHEN <valorn> THEN <instruçõesn>;
    [ELSE <instruçõesm>;]
END CASE;
```

#### Exemplo 5 — Cálculo conforme valor de variável
```sql
CASE V
    WHEN 2 THEN i := 2 * V;
    WHEN 5 THEN i := V + 15;
    ELSE        i := V * 3;
END CASE;
```

---

### 4.3 Laços

#### Laço Simples (infinito sem EXIT)
```sql
LOOP
    <instruções>
END LOOP;
```
- Executam infinitamente a menos que haja instrução de saída:
```sql
EXIT [WHEN <condição>];
```

#### Laço WHILE
```sql
WHILE <condição> LOOP
    <instruções>
END LOOP;
```

#### Laço FOR
```sql
FOR <contador> IN [REVERSE] <inferior>..<superior>
LOOP
    <instruções>
END LOOP;
```

#### GOTO e Rótulos
```sql
...
GOTO rot1;
...
<<rot1>>
-- Laços podem ser etiquetados para uso com EXIT
```

---

### 4.4 Exemplo 6 — Tabela virtual com quadrados

```sql
DECLARE
    TYPE elemento IS RECORD (
        id   INTEGER,
        info INTEGER
    );
    TYPE tabela IS TABLE OF elemento
        INDEX BY BINARY_INTEGER;
    i BINARY_INTEGER;
    C tabela;
BEGIN
    i := 1;
    WHILE i**2 < 30 LOOP
        C(i).id   := i;
        C(i).info := i**2;
        i         := i + 1;
    END LOOP;
END;
/
```

### 4.5 Exemplo 7 — Gravar tabela virtual em tabela relacional

```sql
-- Continua do Exemplo 6
FOR m IN 1..i-1 LOOP
    INSERT INTO exemp1(linha, valor)
    VALUES (C(m).id, C(m).info);
END LOOP;
```
> O índice `i` saiu do laço WHILE com uma unidade a mais (quando `i**2 > 30`).

---

## 5. Recuperação de Dados do BD para Variáveis

### Comando SELECT ... INTO

```sql
SELECT <atributo(s)>
INTO   <variável(is)>
FROM   <tabela(s)>
WHERE  ...;
```

#### Regras importantes:
- O **número de variáveis** deve ser igual ao número de atributos.
- Os **tipos** de cada atributo e variável correspondente devem ser compatíveis.
- **Deve ser recuperada uma única tupla** (senão: exceção).
- As variáveis devem ser declaradas previamente.
- **Nunca use:** `<variável> := SELECT ... FROM ...;` — isso é inválido em PL/SQL.

#### Exemplo 8 — Contar empregados
```sql
DECLARE
    qtdEmp NUMBER;
BEGIN
    SELECT COUNT(*) INTO qtdEmp
    FROM Empregado;
END;
```

---

## 6. Saída de Dados

### Habilitar saída
```sql
SET SERVEROUTPUT ON;
```

### Comandos de saída
```sql
DBMS_OUTPUT.PUT('...')       -- Escreve e permanece na mesma linha
DBMS_OUTPUT.PUT_LINE('...')  -- Escreve e muda de linha
```
> Os parâmetros **devem ser cadeias de caracteres (String)**.

### Funções Oracle para manipulação de Strings

| Função | Ação |
|--------|------|
| `UPPER(<string>)` | Converte para maiúscula |
| `LOWER(<string>)` | Converte para minúscula |
| `RTRIM(<string>)` | Remove espaços à direita |
| `LENGTH(<string>)` | Retorna o tamanho da string |
| `INSTR(<string1>, <string2>)` | Posição inicial de `<string2>` em `<string1>` |
| `SUBSTR(<string>, m, n)` | Sub-string das posições m a n |
| `TO_CHAR(<valor>, [<formato>])` | Converte data ou número em string |
| `<string1> \|\| <string2>` | Concatena `<string2>` ao final de `<string1>` |

---

## 7. Tratamento de Exceções

### Conceito
Responde a erros de execução do programa.

### Estrutura
```sql
BEGIN
    ...
EXCEPTION
    WHEN <nome_exceção> THEN
        -- Manipula a condição de erro
        <instruções>;
    [WHEN OTHERS THEN
        -- Trata qualquer erro não listado acima
        <instruções>;]
END;
```

### Exceções Pré-definidas pelo Oracle

| Exceção | Significado |
|---------|-------------|
| `NO_DATA_FOUND` | Nenhuma tupla recuperada |
| `TOO_MANY_ROWS` | Excesso de tuplas recuperadas |
| `INVALID_CURSOR` | Erro de definição de cursor |
| `ZERO_DIVIDE` | Divisão por zero |
| `DUP_VAL_ON_INDEX` | Índice duplicado |

> A opção `WHEN OTHERS` pode ser usada para tratar qualquer erro não listado.

### Exemplo 9 — Buscar modelo de carro ou registrar ausência

```sql
DECLARE
    v_chassi VARCHAR(20)   := '235-456-YWR';
    v_modelo VARCHAR2(20);
BEGIN
    SELECT modelo INTO v_modelo
    FROM   carro
    WHERE  chassi = v_chassi;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        INSERT INTO log_table (info)
        VALUES ('Carro com Chassi 235-456-YWR não existe!');
END;
/
```

---

## 8. Cursores

### 8.1 Conceito
- Utilizados para processar **várias linhas** obtidas a partir do BD (via `SELECT`).
- O programa percorre o conjunto de linhas, devolvendo uma por vez e processando cada uma.
- Podem ser **explícitos** ou **implícitos**.

| Tipo | Gestão |
|------|--------|
| Explícito | Declarado e gerenciado pelo **programador** |
| Implícito | Declarado e gerenciado pelo **Oracle** |

---

### 8.2 Cursores Explícitos — Fluxo de Controle

```
DECLARE → OPEN → FETCH → VAZIO?
                   ↑        | F (não vazio)
                   └────────┘
                            | V (vazio)
                          CLOSE
```

1. **DECLARE** — Cria o cursor
2. **OPEN** — Abre o cursor (realiza a consulta)
3. **FETCH** — Carrega a linha atual em variáveis e aponta para a próxima
4. **CLOSE** — Libera o cursor

---

### 8.3 Utilização de Cursores Explícitos

```sql
-- 1. Declarar o cursor
CURSOR <nome> IS <comando_select>;

-- 2. Abrir o cursor para consulta
OPEN <nome>;

-- 3. Extrair os resultados para variáveis PL/SQL
FETCH <nome> INTO <lista_de_variáveis>;
-- ou
FETCH <nome> INTO <registro>;

-- 4. Fechar o cursor
CLOSE <nome>;
```

---

### 8.4 Propriedades de Cursores Explícitos

| Propriedade | Significado |
|-------------|-------------|
| `%ROWCOUNT` | Quantidade de tuplas recuperadas pelo comando que gerou o cursor |
| `%FOUND` | `TRUE` se alguma tupla foi recuperada |
| `%NOTFOUND` | `TRUE` se nenhuma linha foi recuperada |
| `%ISOPEN` | `TRUE` se o cursor está aberto |

---

### 8.5 Exemplo 10 — Cursor para manipulação de Carros (completo)

```sql
SET SERVEROUTPUT ON;
DECLARE
    v_chassi     carro.chassi%TYPE;
    v_data_carro carro.data_carro%TYPE;
    v_km_carro   carro.km_carro%TYPE;
    v_modelo     carro.modelo%TYPE := 'Clio Sedan';

    CURSOR c_carro IS
        SELECT chassi, km_carro, data_carro
        FROM   carro
        WHERE  modelo = v_modelo;
BEGIN
    OPEN c_carro;
    LOOP
        FETCH c_carro INTO v_chassi, v_km_carro, v_data_carro;
        EXIT WHEN c_carro%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE('Carro: ' || ' ' ||
            TO_CHAR(v_chassi)     || ' ' ||
            TO_CHAR(v_km_carro)   || ' ' ||
            TO_CHAR(v_data_carro));
    END LOOP;
    CLOSE c_carro;
END;
/
```

---

### 8.6 Tipo Registro em Cursor (`%ROWTYPE`)

#### Exemplo 11 — Listar CPF e nome de clientes no Rio

```sql
DECLARE
    CURSOR c_reg IS
        SELECT cpf, nome FROM Cliente
        WHERE  endereco = 'Rio';
    v_reg c_reg%ROWTYPE;
BEGIN
    OPEN c_reg;
    LOOP
        FETCH c_reg INTO v_reg;
        EXIT WHEN c_reg%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE('CPF: ' || v_reg.cpf ||
                             ' ' || 'Nome: ' || v_reg.nome);
    END LOOP;
    CLOSE c_reg;
END;
/
```

---

### 8.7 Cursor com Laços FOR

Forma mais reduzida — faz implicitamente OPEN, FETCH, EXIT e CLOSE, e declara implicitamente a variável de tipo registro.

#### Exemplo 12 — Nome e email de clientes em Recife

```sql
DECLARE
    CURSOR c_reg IS
        SELECT nome, email FROM Cliente
        WHERE  endereco = 'Recife';
BEGIN
    FOR v_reg IN c_reg LOOP
        DBMS_OUTPUT.PUT_LINE(v_reg.nome || ' ' || v_reg.email);
    END LOOP;
END;
/
```

---

### 8.8 Cursor FOR sem Declaração Explícita

#### Exemplo 13 — Nome e telefone de clientes em Salvador

```sql
DECLARE
    ...
BEGIN
    FOR v_reg IN (SELECT nome, telefone FROM Cliente
                  WHERE  endereco = 'Salvador') LOOP
        DBMS_OUTPUT.PUT_LINE(v_reg.nome || ' ' || v_reg.telefone);
    END LOOP;
END;
/
```

---

### 8.9 Cursor com Parâmetros

#### Exemplo 14 — CPF e nome de clientes por endereço paramétrico

```sql
DECLARE
    CURSOR c_reg (p_valor VARCHAR2) IS
        SELECT cpf, nome FROM Cliente
        WHERE  endereco = p_valor;
    v_reg c_reg%ROWTYPE;
BEGIN
    OPEN c_reg('Rio');   -- cada nova passagem requer OPEN/CLOSE
    LOOP
        FETCH c_reg INTO v_reg;
        EXIT WHEN c_reg%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE(v_reg.cpf || ' ' || v_reg.nome);
    END LOOP;
    CLOSE c_reg;
END;
/
```

---

### 8.10 Cursores Implícitos

- Utilizados para processar `INSERT`, `UPDATE`, `DELETE` e `SELECT ... INTO`.

#### Exemplo 15 — Atualizar e-mail ou inserir se não existir

```sql
BEGIN
    UPDATE cliente
    SET    email = 'nina@cin.ufpe.br'
    WHERE  cpf   = '324567876-18';

    -- Cursor implícito: se UPDATE não encontrou tupla, insere
    IF SQL%NOTFOUND THEN
        INSERT INTO cliente (cpf, email)
        VALUES ('324567876-18', 'nina@cin.ufpe.br');
    END IF;
END;
/
```
> Válido se os demais atributos não forem `NOT NULL` e um deles for chave primária.

---

## 9. Subprogramas

Subprogramas são **procedimentos**, **funções** e **pacotes** que são criados e armazenados na base de dados.

- Em geral não são alterados após construídos e são executados muitas vezes.
- São executados **explicitamente**, por chamada.

```
Subprogramas
├── Functions (Funções)
├── Procedures (Procedimentos)
└── Packages (Pacotes)
```

---

### 9.1 Procedimentos (Procedures)

#### Sintaxe
```sql
CREATE [OR REPLACE] PROCEDURE <nome>
    [(parâmetro [{IN | OUT | IN OUT}] tipo, ...)]
IS
    <definições de variáveis>
BEGIN
    <corpo-do-procedimento>
END <nome>;
```

> **Atenção:** Procedimentos **não usam a cláusula `DECLARE`**.

| Modificador | Significado |
|-------------|-------------|
| `IN`        | Parâmetro de **entrada** (default) |
| `OUT`       | Parâmetro de **saída** |
| `IN OUT`    | Parâmetro de **entrada e saída** |

#### Exemplo 16 — Inserir veículo na tabela Carro

```sql
CREATE OR REPLACE PROCEDURE InsereCarro (
    p_chassi     carro.chassi%TYPE,
    p_modelo     carro.modelo%TYPE,
    p_km_carro   carro.km_carro%TYPE,
    p_data_carro carro.data_carro%TYPE
) IS
BEGIN
    INSERT INTO carro (chassi, modelo, km_carro, data_carro)
    VALUES (p_chassi, p_modelo, p_km_carro, p_data_carro);
    COMMIT;
END InsereCarro;
/

-- Chamada em outro bloco PL/SQL:
InsereCarro('235-456-YWR', 'Celta', 100, TO_DATE('15/05/2002','dd/mm/yyyy'));

-- Chamada na interface de caracteres:
EXEC InsereCarro('235-456-YWR', 'Celta', 100, TO_DATE('15/05/2002','dd/mm/yyyy'));
```

---

### 9.2 Funções (Functions)

- Chamadas como **parte de uma expressão**, retornando um valor.
- O corpo **deve** conter o comando `RETURN`.

#### Sintaxe
```sql
CREATE [OR REPLACE] FUNCTION <nome>
    [(parâmetro tipo, ...)]
RETURN <tipo-retorno>
IS
BEGIN
    <corpo-da-função>
    RETURN <expressão>;
END <nome>;
```

> **Não** especificar tamanho na definição do tipo de retorno.

#### Exemplo 17 — Calcular nova chave a partir da maior existente

```sql
CREATE OR REPLACE FUNCTION calcula RETURN INTEGER IS
    retorno INTEGER;
BEGIN
    SELECT MAX(id) INTO retorno FROM categoria;
    retorno := retorno + 1;
    RETURN retorno;
END calcula;
/

-- Chamada em bloco PL/SQL:
v_valor := calcula;

-- Chamada na interface de caracteres:
SELECT calcula FROM dual;
```

---

### 9.3 Packages (Pacotes)

- Fornecem mecanismo para **ampliar o poder da linguagem**.
- Elementos do pacote podem aparecer em qualquer ordem, mas um elemento deve ser declarado **antes de ser referenciado**.
- Na definição do pacote, só é apresentada a **especificação**.
- A **implementação** (corpo) é apresentada à parte.
- Permite **sobrecarga (overload)** de subprogramas.

#### Sintaxe — Especificação do Pacote
```sql
CREATE OR REPLACE PACKAGE <nome> AS
    <especificação de procedimento> |
    <especificação de função>       |
    <declaração de variável>        |
    <definição de tipo>             |
    <declaração de exceção>         |
    <declaração de cursor>
END nome;
```

#### Sintaxe — Corpo do Pacote
```sql
CREATE OR REPLACE PACKAGE BODY <nome> AS
    -- Implementação de cada subprograma declarado
END <nome>;
```

#### Acesso a elementos do pacote (fora dele):
```sql
CadastroPackage.RemoveCarro(...)
```

#### Inicialização de Pacotes
- Código de inicialização para a **primeira execução**.
- Fornecido no corpo do pacote como um bloco `BEGIN...END` no final.

#### Exemplo 18 — Pacote CadastroPackage (resumo estrutural)

```sql
-- Especificação:
CREATE OR REPLACE PACKAGE CadastroPackage AS

    -- Insere veículo em Carro
    PROCEDURE InsereCarro (
        p_chassi     carro.chassi%TYPE,
        p_modelo     carro.modelo%TYPE,
        p_km_carro   carro.km_carro%TYPE,
        p_data_carro carro.data_carro%TYPE
    );

    -- Remove um dado veículo de Carro
    PROCEDURE RemoveCarro (p_chassi IN carro.chassi%TYPE);

    -- Tipo de tabela virtual para armazenar chassis
    TYPE t_chassiTable IS TABLE OF carro.chassi%TYPE
        INDEX BY BINARY_INTEGER;

    -- Exceção levantada por RemoveCarro
    e_carroNaoExistente EXCEPTION;

    -- Retorna tabela virtual com chassis de um dado modelo
    PROCEDURE ListaChassi (
        p_modelo    IN     carro.modelo%TYPE,
        p_chassi    OUT    t_chassiTable,
        p_Numcarros IN OUT BINARY_INTEGER
    );

END CadastroPackage;
/
```

```sql
-- Corpo:
CREATE OR REPLACE PACKAGE BODY CadastroPackage AS

    -- InsereCarro
    PROCEDURE InsereCarro (...) IS
    BEGIN
        INSERT INTO carro (chassi, modelo, km_carro, data_carro)
        VALUES (p_chassi, p_modelo, p_km_carro, p_data_carro);
        COMMIT;
    END InsereCarro;

    -- RemoveCarro
    PROCEDURE RemoveCarro (p_chassi IN carro.chassi%TYPE) IS
    BEGIN
        DELETE FROM carro WHERE chassi = p_chassi;
        IF SQL%NOTFOUND THEN
            RAISE e_carroNaoExistente;
        END IF;
        COMMIT;
    END RemoveCarro;

    -- ListaChassi
    PROCEDURE ListaChassi (
        p_modelo    IN     carro.modelo%TYPE,
        p_chassi    OUT    t_chassiTable,
        p_Numcarros IN OUT BINARY_INTEGER
    ) IS
        v_chassi carro.chassi%TYPE;
        CURSOR c_carros IS
            SELECT chassi FROM carro WHERE modelo = p_modelo;
    BEGIN
        p_Numcarros := 0;
        OPEN c_carros;
        LOOP
            FETCH c_carros INTO v_chassi;
            EXIT WHEN c_carros%NOTFOUND;
            p_Numcarros           := p_Numcarros + 1;
            p_chassi(p_Numcarros) := v_chassi;
        END LOOP;
    END ListaChassi;

END CadastroPackage;
/
```

> Todos os subprogramas declarados no Package devem ter implementação no Body.

---

## 10. Triggers (Gatilhos)

### 10.1 Conceito

- Procedimentos criados e **armazenados no SGBD**, ativados **automaticamente** em resposta a determinadas mudanças no BD.
- Podem ser: **Simple**, **Autonomous** ou **Compound**.
- São executados **implicitamente** sempre que ocorre o evento que os dispara.
- O evento pode estar associado a uma tabela, view, esquema ou ao BD.
- **Não aceitam parâmetros.**
- **Não podem ser locais** em relação a um bloco, procedure, function ou package.

```
Evento → [Condição?] →Sim→ Ação
```

### 10.2 Componentes de um Trigger

Um trigger é composto por **quatro partes**:

| Parte | Descrição |
|-------|-----------|
| **Momento** | Quando o trigger é executado |
| **Evento** | O que dispara o trigger |
| **Tipo** | Linha ou Comando |
| **Ação** | O que o trigger executa |

---

### 10.3 Momento

Triggers do tipo *simple* são disparados em exatamente um ponto:

| Palavra-chave | Ação |
|---------------|------|
| `BEFORE` | Antes do evento (ou de cada tupla afetada) |
| `AFTER` | Após o evento (ou de cada tupla afetada) |
| `INSTEAD OF` | No lugar do evento (somente para **views**) |

> `BEFORE` permite **consulta e alteração** a valores `OLD` (anteriores) e `NEW` (recém-inseridos).  
> `AFTER` só permite **consulta**.

---

### 10.4 Tipo

Aplicável a eventos DML:

| Tipo | Descrição |
|------|-----------|
| **Linha** (`FOR EACH ROW`) | Acionado para cada linha afetada. Para UPDATE de atributo: `UPDATE OF <atributo> ON <tabela>` |
| **Comando** | Acionado uma única vez, independentemente do número de linhas. Não permite acesso às linhas atualizadas. |

---

### 10.5 Eventos

- **DML:** `INSERT`, `DELETE`, `UPDATE`
- **DDL:** `CREATE`, `ALTER`, `DROP`
- **Operações do BD:** `SERVERERROR`, `LOGON`, `LOGOFF`, `STARTUP`, `SHUTDOWN`, `GRANT`, `REVOKE`
- Eventos temporais, externos, combinações

---

### 10.6 Ações

Bloco PL/SQL executado em razão do evento.

#### Predicados Lógicos (quando disparado por mais de um evento DML)

| Predicado | Significado |
|-----------|-------------|
| `INSERTING` | Disparado por `INSERT` |
| `UPDATING` | Disparado por `UPDATE` |
| `DELETING` | Disparado por `DELETE` |

#### Possibilidades de Ação
- Sequência de comandos de modificação/acesso.
- Abortar a transação implicitamente.
- Indicar um `ROLLBACK`.
- Substituir a operação via `INSTEAD OF` (Oracle: só para views).

---

### 10.7 Usos de Triggers

- Manter **restrições de integridade complexas**.
- **Auditoria** de informações (log seletivo).
- Indicar automaticamente a outros programas que uma ação é necessária.
- **Gerar valor de coluna** (atributo).
- Garantir **regras de negócio**.
- **Controle de versões**.
- Garantir **restrições de acesso**.

---

### 10.8 Recomendações de Uso

#### Use triggers preferencialmente para:
- Garantir que, quando uma operação for processada, ações relacionadas serão executadas.
- Impor regras de negócio complexas que não podem ser definidas com restrições de integridade.
- Impor integridade referencial entre tabelas pai/filho em BDs distribuídos.
- Manter replicação síncrona de tabelas.
- Operações centralizadas/globais que devem disparar independente do usuário ou aplicação.
- Modificar dados de tabela quando DML é emitido sobre Views.

#### Use com moderação:
- Limite o tamanho a ~60 linhas. Se precisar de mais, use procedures.

#### Não use triggers para:
- Refazer ações já existentes no SGBD: `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`, `DELETE CASCADE`, `DELETE SET NULL`.
- Não criar triggers **recursivos**.

---

### 10.9 Ativação Condicional — WHEN

- Usada apenas com **row triggers**.
- Expressão booleana SQL avaliada para cada tupla afetada.
- Apenas se o resultado for verdadeiro, a ação é executada.
- **Não** pode incluir: subconsultas, expressões em PL/SQL, funções definidas pelo usuário.

---

### 10.10 Sintaxe Completa

```sql
CREATE [OR REPLACE] TRIGGER <nome>
[BEFORE | AFTER | INSTEAD OF] <evento>    -- INSTEAD OF: só para views
ON <tabela>
[REFERENCING NEW AS <novo_nome>           -- evitar conflito com new/old
             OLD AS <antigo_nome>]
[FOR EACH ROW [WHEN (<condição>)]]        -- trigger de linha; WHEN: ativação condicional
[DECLARE [PRAGMA AUTONOMOUS_TRANSACTION]  -- controle de transação
 [Definição de variáveis locais]]
BEGIN
    <corpo-do-procedimento>
END <nome>;
/
```

---

### 10.11 Restrições para Triggers Simple

- **Não podem** emitir: `COMMIT`, `ROLLBACK`, `SAVEPOINT`.
- **Não podem** chamar funções que façam isso.
- **Não podem** declarar variável `LONG` ou `LONG RAW`.
- Essas ações são **permitidas** para triggers do tipo **Autonomous**.

---

### 10.12 Exemplos de Triggers

#### Exemplo 19 — Trigger de Linha: Atualizar km do carro na devolução

```sql
CREATE OR REPLACE TRIGGER altera_km
AFTER UPDATE ON locacao
FOR EACH ROW
BEGIN
    IF :NEW.data_entrega IS NOT NULL THEN
        UPDATE carro
        SET    km_carro = :NEW.km_final
        WHERE  chassi   = :NEW.chassi;
    END IF;
END;
/
-- :NEW → valores inseridos; :OLD → valores anteriores
```

#### Exemplo 20 — Trigger de Linha: Impedir locação com carro velho

```sql
CREATE OR REPLACE TRIGGER verifica_data
BEFORE INSERT OR UPDATE ON locacao
FOR EACH ROW
DECLARE
    x DATE;
BEGIN
    SELECT data_carro INTO x FROM Carro WHERE chassi = :NEW.chassi_carro;
    IF SYSDATE - x >= 1059 THEN
        RAISE_APPLICATION_ERROR(-20011, 'Carro com mais de três anos de uso');
    END IF;
END;
/
```

#### Exemplo 21 — Trigger de Comando: Impedir remoção no último dia do mês

```sql
CREATE OR REPLACE TRIGGER dia_permitido
BEFORE DELETE ON Carro
DECLARE
    aux       VARCHAR2(2);
    x         VARCHAR2(2);
    y         VARCHAR2(2);
    ultimo_dia EXCEPTION;
BEGIN
    x := EXTRACT(month FROM SYSDATE);
    IF x IN ('1','3','5','7','8','10','12') THEN aux := '1'; END IF;
    IF x IN ('4','6','9','11')              THEN aux := '2'; END IF;
    IF x = 2                                THEN aux := '3'; END IF;

    y := EXTRACT(day FROM SYSDATE);

    CASE aux
        WHEN 1 THEN IF y = '31' THEN RAISE ultimo_dia; END IF;
        WHEN 2 THEN IF y = '30' THEN RAISE ultimo_dia; END IF;
        WHEN 3 THEN IF y = '28' THEN RAISE ultimo_dia; END IF;
    END CASE;

EXCEPTION
    WHEN ultimo_dia THEN
        RAISE_APPLICATION_ERROR(-20324,
            'ÚLTIMO DIA DO MÊS - Não é permitido remover carro do BD');
END dia_permitido;
/
```

---

## 11. Tabela Mutante

### Conceito
- Em alguns triggers é necessário fazer referência a valores da **tabela que está sendo alterada**.
- Ocorre quando um trigger tenta **consultar a própria tabela** que sofre a ação.
- Também ocorre ao consultar tabela pai em `UPDATE/DELETE` cascading.

> ⚠️ **Um Trigger está tentando modificar ou consultar um dado que ainda está sendo modificado.**

### Possíveis Soluções
1. Uso combinado de **trigger de linha** (row) e **trigger de comando** (statement).
2. Uso de **Compound Triggers** (a partir do Oracle 11g release 1).
3. Uso de **Autonomous Transactions**.

---

### 11.1 Solução com Row + Statement Trigger (Exemplo 22 e 23)

**Cenário:** Auditar inserções e modificações na tabela `Carro`, gravando ação, id, instante e total de registros em `Carro_audit`.

#### Tabela de auditoria:
```sql
CREATE TABLE Carro_audit (
    id               NUMBER(10)  NOT NULL,
    acao             VARCHAR2(10) NOT NULL,
    carro_id         NUMBER(7),
    numero_registros NUMBER(10),
    hora_criacao     TIMESTAMP,
    CONSTRAINT carro_audit_pk PRIMARY KEY (id)
);
CREATE SEQUENCE carro_audit_seq;
```

#### Package com abordagem combinada (Exemplo 23):

```sql
-- Especificação
CREATE OR REPLACE PACKAGE trigger_api AS
    PROCEDURE carro_row_change (p_id IN carro.chassi%TYPE, p_acao IN VARCHAR2);
    PROCEDURE carro_statement_change;
END trigger_api;

-- Corpo
CREATE OR REPLACE PACKAGE BODY trigger_api AS
    TYPE t_change_rec IS RECORD (
        id   carro.chassi%TYPE,
        acao carro_audit.acao%TYPE
    );
    TYPE t_change_tab IS TABLE OF t_change_rec;
    g_change_tab t_change_tab := t_change_tab();  -- tabela em memória

    PROCEDURE carro_row_change (p_id IN carro.chassi%TYPE, p_acao IN VARCHAR2) IS
    BEGIN
        g_change_tab.extend;
        g_change_tab(g_change_tab.last).id   := p_id;
        g_change_tab(g_change_tab.last).acao := p_acao;
    END carro_row_change;

    PROCEDURE carro_statement_change IS
        l_count NUMBER(10);
    BEGIN
        FOR i IN g_change_tab.first..g_change_tab.last LOOP
            SELECT COUNT(*) INTO l_count FROM carro;
            INSERT INTO carro_audit (id, acao, carro_id, numero_registros, hora_criacao)
            VALUES (carro_audit_seq.NEXTVAL, g_change_tab(i).acao,
                    g_change_tab(i).id, l_count, SYSTIMESTAMP);
        END LOOP;
        g_change_tab.delete;  -- limpa a memória
    END carro_statement_change;
END trigger_api;
/

-- Trigger de linha (row)
CREATE OR REPLACE TRIGGER carro_trg
AFTER INSERT OR UPDATE ON carro
FOR EACH ROW
BEGIN
    IF INSERTING THEN
        trigger_api.carro_row_change(p_id => :new.chassi, p_acao => 'INSERT');
    ELSE
        trigger_api.carro_row_change(p_id => :new.chassi, p_acao => 'UPDATE');
    END IF;
END;

-- Trigger de comando (statement)
CREATE OR REPLACE TRIGGER carro_st_trg
AFTER INSERT OR UPDATE ON carro
BEGIN
    trigger_api.carro_statement_change;
END;
```

---

### 11.2 Compound Triggers (a partir do Oracle 11g)

- Disparados a partir de **mais de um ponto**.
- Facilitam ações em vários pontos de disparo **compartilhando dados comuns**.

#### Sintaxe
```sql
CREATE [OR REPLACE] TRIGGER <nome>
FOR <evento> ON <tabela>
COMPOUND TRIGGER
    <Declaração de Variáveis Locais>

    BEFORE STATEMENT IS
    BEGIN <bloco> END BEFORE STATEMENT;

    BEFORE EACH ROW IS
    BEGIN <bloco> END BEFORE EACH ROW;

    AFTER EACH ROW IS
    BEGIN <bloco> END AFTER EACH ROW;

    AFTER STATEMENT IS
    BEGIN <bloco> END AFTER STATEMENT;

END <nome>;
```

#### Exemplo 24 — Compound Trigger para auditoria de Carro (com DELETE)

```sql
CREATE OR REPLACE TRIGGER audit_carro
FOR INSERT OR UPDATE OR DELETE ON Carro
COMPOUND TRIGGER
    x       INTEGER;
    cadeia  VARCHAR2(6);

    BEFORE STATEMENT IS
    BEGIN
        SELECT COUNT(*) INTO x FROM carro;
    END BEFORE STATEMENT;

    AFTER EACH ROW IS
    BEGIN
        IF INSERTING THEN
            cadeia := 'INSERT'; x := x + 1;
        ELSIF DELETING THEN
            cadeia := 'DELETE'; x := x - 1;
        ELSE
            cadeia := 'UPDATE';
        END IF;
        INSERT INTO carro_audit (id, acao, carro_id, numero_registros, hora_criacao)
        VALUES (carro_audit_seq.NEXTVAL, cadeia, :NEW.id, x, SYSTIMESTAMP);
    END AFTER EACH ROW;

END audit_carro;
```

---

### 11.3 Autonomous Transactions

- Permite executar uma **nova transação independente** da que dispara o trigger.
- Na seção `DECLARE` do trigger: `PRAGMA AUTONOMOUS_TRANSACTION;`
- A transação do trigger **deve sofrer commit**.
- A tabela vista pela transação autônoma é a **com dados anteriores** à ação da transação que disparou o trigger.
- Sem controle pode levar a **deadlock**, provocando rollback da transação disparadora.

#### Sintaxe
```sql
CREATE [OR REPLACE] TRIGGER <nome>
FOR <evento> ON <tabela>
FOR EACH ROW
DECLARE
    PRAGMA AUTONOMOUS_TRANSACTION;
    ...
BEGIN
    <bloco>
    COMMIT;  -- obrigatório
END <nome>;
```

#### Exemplo 25 — Autonomous Transaction para auditoria de Carro

```sql
CREATE OR REPLACE TRIGGER carro_trg
AFTER INSERT OR UPDATE ON carro
FOR EACH ROW
DECLARE
    PRAGMA AUTONOMOUS_TRANSACTION;
    l_count INTEGER;
BEGIN
    SELECT COUNT(*) INTO l_count FROM Carro;
    IF INSERTING THEN
        l_count := l_count + 1;
        INSERT INTO carro_audit
        VALUES (carro_audit_seq.NEXTVAL, 'INSERT', :new.chassi, l_count, SYSTIMESTAMP);
    ELSE
        INSERT INTO carro_audit
        VALUES (carro_audit_seq.NEXTVAL, 'UPDATE', :new.chassi, l_count, SYSTIMESTAMP);
    END IF;
    COMMIT;
END;
```

---

## 12. Modelo Entidade-Relacionamento (Contexto dos Exemplos)

```
CARRO                   CLIENTE
├── chassi (PK)         ├── cpf (PK)
├── modelo              ├── nome
├── km_carro            ├── endereco
└── data_carro          ├── email
                        └── telefone

LOCACAO (n:m entre CARRO e CLIENTE)
├── km_inicial
├── km_final
├── data_inicial
├── data_final
└── data_entrega  -- se NOT NULL → carro entregue
```

---

## 13. Resumo das Principais Construções PL/SQL

| Construção | Palavras-chave Principais |
|------------|--------------------------|
| Bloco anônimo | `DECLARE`, `BEGIN`, `EXCEPTION`, `END` |
| Variável simples | `<nome> <tipo> [:= <valor>]` |
| Constante | `CONSTANT` |
| Tipo de tabela | `%TYPE`, `%ROWTYPE` |
| Registro | `CREATE TYPE ... IS RECORD` |
| Tabela virtual | `TYPE ... IS TABLE OF ... INDEX BY BINARY_INTEGER` |
| Controle | `IF-ELSIF-ELSE`, `CASE`, `LOOP`, `WHILE`, `FOR` |
| Saída de dados | `DBMS_OUTPUT.PUT_LINE` |
| Recuperação | `SELECT ... INTO` |
| Cursor explícito | `CURSOR`, `OPEN`, `FETCH`, `CLOSE`, `%NOTFOUND` |
| Cursor FOR | `FOR <var> IN <cursor> LOOP` |
| Exceção | `EXCEPTION`, `WHEN`, `RAISE`, `RAISE_APPLICATION_ERROR` |
| Procedure | `CREATE [OR REPLACE] PROCEDURE` |
| Function | `CREATE [OR REPLACE] FUNCTION ... RETURN` |
| Package | `CREATE [OR REPLACE] PACKAGE` + `PACKAGE BODY` |
| Trigger | `CREATE [OR REPLACE] TRIGGER ... BEFORE/AFTER ... FOR EACH ROW` |
| Compound Trigger | `COMPOUND TRIGGER`, `BEFORE/AFTER STATEMENT`, `BEFORE/AFTER EACH ROW` |
| Autonomous | `PRAGMA AUTONOMOUS_TRANSACTION` |

---

*Fonte: Material de aula de Gerenciamento de Dados e Informação — CIn/UFPE. Autores: Robson Fidalgo e Valeria Times.*
