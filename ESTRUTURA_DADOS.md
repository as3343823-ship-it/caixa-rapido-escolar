# 🏗️ ESTRUTURA DE DADOS - As Classes do Projeto

Nesta seção, vamos entender as **3 principais classes** que vamos criar.

Lembre-se: Uma **classe é um molde** que define características (atributos) e ações (métodos).

---

## 1️⃣ CLASSE: Cliente

### 🎯 O Que É?
É o molde que define **dados de quem está comprando**.

### 📋 Características (Atributos)

```
Cliente
├─ id: número (identificador único)
├─ nome: texto (nome da pessoa)
├─ telefone: texto (telefone para contato)
└─ email: texto (email para contato)
```

### 💡 Exemplo Real

```
Cliente #1
├─ id: 1
├─ nome: "João Silva"
├─ telefone: "11987654321"
└─ email: "joao@email.com"

Cliente #2
├─ id: 2
├─ nome: "Maria Santos"
├─ telefone: "11912345678"
└─ email: "maria@email.com"
```

### 🔧 Ações (Métodos) que vamos criar

```
Cliente joao = new Cliente();

// Métodos para DEFINIR informações
joao.setNome("João Silva");
joao.setTelefone("11987654321");
joao.setEmail("joao@email.com");

// Métodos para OBTER informações
String nome = joao.getNome();
String telefone = joao.getTelefone();
String email = joao.getEmail();

// Método para verificar se tudo está correto
boolean valido = joao.validar();
```

### ✅ Validações

Quando criamos um Cliente, precisamos verificar:
- ✓ Nome não pode estar vazio
- ✓ Telefone ou Email tem que estar preenchido
- ✓ Nome deve ter pelo menos 3 caracteres

---

## 2️⃣ CLASSE: Produto

### 🎯 O Que É?
É o molde que define **dados de cada item que pode ser vendido**.

### 📋 Características (Atributos)

```
Produto
├─ id: número (identificador único)
├─ nome: texto (nome do produto)
├─ preco: número decimal (quanto custa)
└─ descricao: texto (informação extra)
```

### 💡 Exemplo Real

```
Produto #1
├─ id: 1
├─ nome: "Suco de Laranja"
├─ preco: 5.50
└─ descricao: "Suco natural 250ml"

Produto #2
├─ id: 2
├─ nome: "Bolo de Chocolate"
├─ preco: 8.00
└─ descricao: "Bolo caseiro"

Produto #3
├─ id: 3
├─ nome: "Pão de Queijo"
├─ preco: 3.50
└─ descricao: "Tradicional"
```

### 🔧 Ações (Métodos) que vamos criar

```
Produto suco = new Produto();

// Métodos para DEFINIR informações
suco.setNome("Suco de Laranja");
suco.setPreco(5.50);
suco.setDescricao("Suco natural 250ml");

// Métodos para OBTER informações
String nome = suco.getNome();
double preco = suco.getPreco();
String descricao = suco.getDescricao();

// Método para validar
boolean valido = suco.validar();
```

### ✅ Validações

- ✓ Nome não pode estar vazio
- ✓ Preço tem que ser maior que zero
- ✓ Preço deve ter máximo 2 casas decimais

---

## 3️⃣ CLASSE: Venda

### 🎯 O Que É?
É o molde que **agrupa um cliente com seus produtos comprados**.

### 📋 Características (Atributos)

```
Venda
├─ id: número (identificador único)
├─ cliente: Cliente (quem comprou)
├─ dataVenda: data e hora (quando comprou)
├─ produtos: lista de produtos (o que comprou)
├─ quantidades: lista de números (quanto de cada)
└─ total: número decimal (quanto custou tudo)
```

### 💡 Exemplo Real

```
Venda #1
├─ id: 1
├─ cliente: João Silva (ID: 1)
├─ dataVenda: 2026-04-10 às 10:30
├─ Produtos comprados:
│  ├─ Suco de Laranja x2 → 5.50 cada = 11.00
│  └─ Bolo de Chocolate x1 → 8.00 cada = 8.00
└─ TOTAL: 19.00
```

### 🔧 Ações (Métodos) que vamos criar

```
Venda venda = new Venda();

// DEFINIR cliente
venda.setCliente(joao);

// ADICIONAR produtos
venda.adicionarProduto(suco, 2);        // Suco x2
venda.adicionarProduto(bolo, 1);        // Bolo x1

// CALCULAR o total automaticamente
venda.calcularTotal();

// OBTER informações
double total = venda.getTotal();
Cliente cliente = venda.getCliente();
List<Produto> produtos = venda.getProdutos();

// GERAR um recibo (texto formatado)
String recibo = venda.gerarRecibo();
System.out.println(recibo);

// VALIDAR se tudo está OK
boolean valido = venda.validar();
```

### 📄 Exemplo de Recibo Gerado

```
═══════════════════════════════════
        CAIXA RÁPIDO ESCOLAR
═══════════════════════════════════

Cliente: João Silva
Telefone: 11987654321
Email: joao@email.com

Data: 10/04/2026 - 10:30

───────────────────────────────────
PRODUTOS:
───────────────────────────────────
Suco de Laranja        x2    R$ 11,00
Bolo de Chocolate      x1    R$  8,00

───────────────────────────────────
TOTAL:                        R$ 19,00
═══════════════════════════════════

Obrigado pela compra!
```

### ✅ Validações

- ✓ Cliente tem que estar definido
- ✓ Tem que haver pelo menos 1 produto
- ✓ Total deve ser maior que zero

---

## 🔗 Como as Classes Se Relacionam?

```
┌──────────────────────────────────────────┐
│            VENDA                         │
│  ┌─────────────────────────────────────┐ │
│  │ Cliente: João Silva                 │ │
│  │ (Objeto da classe Cliente)          │ │
│  └─────────────────────────────────────┘ │
│                                          │
│  ┌─────────────────────────────────────┐ │
│  │ Produtos:                           │ │
│  │  • Suco x2 (Classe Produto)        │ │
│  │  • Bolo x1 (Classe Produto)        │ │
│  └─────────────────────────────────────┘ │
│                                          │
│  Total: R$ 19,00                         │
└──────────────────────────────────────────┘
```

Uma **Venda contém um Cliente** e **vários Produtos**.

---

## 📊 Diagrama de Classes

```
┌─────────────────┐
│    Cliente      │
├─────────────────┤
│ - id            │
│ - nome          │
│ - telefone      │
│ - email         │
├─────────────────┤
│ + getNome()     │
│ + validar()     │
└─────────────────┘
       △
       │
       │ possui
       │
┌─────────────────┐          ┌──────────────┐
│     Venda       │◄────────►│   Produto    │
├─────────────────┤   tem    ├──────────────┤
│ - id            │          │ - id         │
│ - cliente       │          │ - nome       │
│ - data          │          │ - preco      │
│ - produtos      │          │ - descricao  │
│ - total         │          ├──────────────┤
├─────────────────┤          │ + getPreco() │
│ + adicionar...()│          │ + validar()  │
│ + calcular...() │          └──────────────┘
│ + gerar...()    │
└─────────────────┘
```

---

## 🎓 Próximo Passo

👉 Leia o arquivo **BANCO_DADOS.md** para entender como guardar essas informações!

---

**Última atualização:** 2026-04-10