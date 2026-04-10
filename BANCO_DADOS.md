# 💾 BANCO DE DADOS - Guardando as Informações

Nesta seção, vamos entender como **guardar as informações permanentemente** usando um banco de dados.

---

## 🎯 O Que É Um Banco de Dados?

Um **banco de dados** é como um **armário organizado** onde guardamos informações:

```
ANTES (sem banco de dados):
  ❌ Informações só na memória do computador
  ❌ Se desligar o computador, perdem tudo
  ❌ Difícil de organizar

DEPOIS (com banco de dados):
  ✅ Informações guardadas em arquivo
  ✅ Mesmo desligando o computador, continua lá
  ✅ Fácil de organizar e buscar
```

---

## 📦 Banco de Dados Que Vamos Usar: SQLite

### Por Que SQLite?

| Vantagem | Explicação |
|----------|-----------|
| **Simples** | Tudo num arquivo, sem complicação |
| **Leve** | Não precisa de servidor rodando |
| **Grátis** | Código aberto, sem custos |
| **Funciona em Java** | Tem driver JDBC para conectar |
| **Aprendizado** | Perfeito para começar |

---

## 📋 Tabelas Que Vamos Criar

Um banco de dados SQLite é organizado em **tabelas**, como planilhas Excel:

### TABELA 1: clientes

Guardará os dados de cada cliente que fez uma compra.

```
┌────┬─────────────┬──────────────┬────────────────────┐
│ id │ nome        │ telefone     │ email              │
├────┼─────────────┼──────────────┼────────────────────┤
│ 1  │ João Silva  │ 11987654321  │ joao@email.com     │
│ 2  │ Maria       │ 11912345678  │ maria@email.com    │
│ 3  │ Pedro Costa │ 11955555555  │ pedro@email.com    │
└────┴─────────────┴──────────────┴────────────────────┘
```

**SQL para criar esta tabela:**
```sql
CREATE TABLE clientes (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT NOT NULL,
    telefone TEXT,
    email TEXT NOT NULL
);
```

**Explicação:**
- `id`: Número único para cada cliente (aumenta automaticamente)
- `nome`: Texto obrigatório
- `telefone`: Texto opcional
- `email`: Texto obrigatório

---

### TABELA 2: produtos

Guardará todos os produtos que podem ser vendidos.

```
┌────┬───────────────────┬───────┬──────────────────────┐
│ id │ nome              │ preco │ descricao            │
├────┼───────────────────┼───────┼──────────────────────┤
│ 1  │ Suco de Laranja   │ 5.50  │ Suco natural 250ml   │
│ 2  │ Bolo de Chocolate │ 8.00  │ Bolo caseiro         │
│ 3  │ Pão de Queijo     │ 3.50  │ Tradicional          │
│ 4  │ Café              │ 2.50  │ Coado na hora        │
└────┴───────────────────┴───────┴──────────────────────┘
```

**SQL para criar esta tabela:**
```sql
CREATE TABLE produtos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT NOT NULL,
    preco REAL NOT NULL,
    descricao TEXT
);
```

**Explicação:**
- `id`: Número único para cada produto
- `nome`: Texto obrigatório
- `preco`: Número decimal (REAL) obrigatório
- `descricao`: Texto opcional

---

### TABELA 3: vendas

Guardará cada venda realizada (ligação entre cliente e produtos).

```
┌────┬───────────┬─────────────────────┬────────┐
│ id │ clienteId │ dataVenda           │ total  │
├────┼───────────┼─────────────────────┼────────┤
│ 1  │ 1         │ 2026-04-10 10:30:00 │ 19.00  │
│ 2  │ 2         │ 2026-04-10 10:35:00 │ 13.50  │
│ 3  │ 1         │ 2026-04-10 11:00:00 │ 5.50   │
└────┴───────────┴─────────────────────┴────────┘
```

**SQL para criar esta tabela:**
```sql
CREATE TABLE vendas (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    clienteId INTEGER NOT NULL,
    dataVenda DATETIME DEFAULT CURRENT_TIMESTAMP,
    total REAL NOT NULL,
    FOREIGN KEY (clienteId) REFERENCES clientes(id)
);
```

**Explicação:**
- `id`: Número único para cada venda
- `clienteId`: Número que referencia qual cliente (ligação com tabela clientes)
- `dataVenda`: Data e hora (automática se não especificar)
- `total`: Número decimal da soma final
- `FOREIGN KEY`: Garante que o cliente existe

---

### TABELA 4: itens_venda

Guardará quais produtos foram vendidos em cada venda (e a quantidade).

```
┌────┬─────────┬───────────┬───────────┐
│ id │ vendaId │ produtoId │ quantidade│
├────┼─────────┼───────────┼───────────┤
│ 1  │ 1       │ 1         │ 2         │  ← Venda 1: Suco x2
│ 2  │ 1       │ 2         │ 1         │  ← Venda 1: Bolo x1
│ 3  │ 2       │ 4         │ 1         │  ← Venda 2: Café x1
│ 4  │ 3       │ 1         │ 1         │  ← Venda 3: Suco x1
└────┴─────────┴───────────┴───────────┘
```

**SQL para criar esta tabela:**
```sql
CREATE TABLE itens_venda (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    vendaId INTEGER NOT NULL,
    produtoId INTEGER NOT NULL,
    quantidade INTEGER NOT NULL,
    FOREIGN KEY (vendaId) REFERENCES vendas(id),
    FOREIGN KEY (produtoId) REFERENCES produtos(id)
);
```

**Explicação:**
- `id`: Número único para cada item
- `vendaId`: Qual venda este item pertence
- `produtoId`: Qual produto foi vendido
- `quantidade`: Quantas unidades

---

## 🔗 Como as Tabelas Se Relacionam?

```
CLIENTE                  VENDA                 PRODUTO
┌──────────┐           ┌──────────┐         ┌──────────┐
│ id: 1    │◄──┐       │ id: 1    │         │ id: 1    │
│ João     │   └──┬────│clienteId:1        │ Suco     │
└──────────┘      │    │total:19  │        │ preco:5.50
                  │    └──────────┘        └──────────┘
                  │         △
                  │         │ itens_venda
         VENDA 1  │         │ conecta
      (João comprou  ITEM 1: produto 1, qtd 2 (Suco x2)
       2 sucos e     ITEM 2: produto 2, qtd 1 (Bolo x1)
       1 bolo)           │
                  │      └──────> PRODUTO 2 (Bolo)
                  └─────────────────────────┘
```

---

## 🛠️ Operações Principais no Banco

### 1. INSERIR (Guardar novo dado)

```sql
-- Adicionar novo cliente
INSERT INTO clientes (nome, telefone, email)
VALUES ('João Silva', '11987654321', 'joao@email.com');

-- Adicionar novo produto
INSERT INTO produtos (nome, preco, descricao)
VALUES ('Suco', 5.50, 'Suco natural');

-- Criar uma nova venda
INSERT INTO vendas (clienteId, total)
VALUES (1, 19.00);
```

### 2. BUSCAR (Obter dados)

```sql
-- Buscar todos os clientes
SELECT * FROM clientes;

-- Buscar um cliente específico
SELECT * FROM clientes WHERE id = 1;

-- Buscar todos os produtos
SELECT * FROM produtos;

-- Buscar uma venda específica
SELECT * FROM vendas WHERE id = 1;
```

### 3. ATUALIZAR (Modificar dados existentes)

```sql
-- Atualizar email de um cliente
UPDATE clientes 
SET email = 'newemail@email.com' 
WHERE id = 1;

-- Atualizar preço de um produto
UPDATE produtos 
SET preco = 6.00 
WHERE id = 1;
```

### 4. DELETAR (Apagar dados)

```sql
-- CUIDADO! Apaga um cliente
DELETE FROM clientes WHERE id = 1;
```

---

## 💻 Como Java Vai Usar o Banco de Dados?

### Fluxo:

```
JAVA CODE
  │
  ├─ Cria um cliente: João
  │
  ├─ Conecta ao banco
  │   (usando JDBC)
  │
  ├─ Escreve SQL:
  │   "INSERT INTO clientes..."
  │
  ├─ Envia para banco
  │   SQLite
  │
  ├─ Banco executa
  │
  └─ Java recebe resposta
     (sucesso ou erro)
```

---

## 🔐 Boas Práticas

| Prática | Descrição |
|---------|-----------|
| **IDs Únicos** | Cada registro tem um ID diferente |
| **Relacionamentos** | Usar FOREIGN KEY para conectar tabelas |
| **Validação** | Verificar dados antes de guardar |
| **Backup** | Copiar arquivo do banco regularmente |
| **Não Deletar** | Melhor marcar como "inativo" que deletar |

---

## 📁 Arquivo do Banco de Dados

O arquivo será salvo como:
```
caixa-rapido-escolar/
├── dados/
│   └── caixa_rapido.db  ← Arquivo do banco (SQLite)
```

Este arquivo `.db` contém TODAS as tabelas e dados.

---

## 🎓 Próximo Passo

👉 Leia o arquivo **GUIA_IMPLEMENTACAO.md** para começar a criar o código!

---

**Última atualização:** 2026-04-10 17:04:39