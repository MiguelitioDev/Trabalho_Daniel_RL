# 📦 StockMaster: Gerenciador de Estoque

> **Sistema de Gerenciamento de Estoque (Inventory Management System) para E-commerce.**

Este projeto simula a lógica de backend de uma loja virtual. O foco não é a vitrine para o cliente final, mas sim a ferramenta administrativa utilizada para controlar o catálogo, preços e movimentação de mercadorias.

🔗 **Acesse o projeto:** [trabalhorl.netlify.app](https://trabalhorl.netlify.app/)

---

## 👥 Autores

Projeto desenvolvido para a disciplina de Raciocínio Lógico (Prof. Daniel) por:
* **Paulo**
* **Marcia**
* **Miguel**

---

## ⚙️ Estrutura de Dados (Lógica)

O núcleo do sistema foi construído utilizando o conceito de **Arrays Paralelos**. Isso significa que o índice (index) é o conector entre as diferentes propriedades de um produto.

*Exemplo: O produto no `index 5` do array `produtos` tem seu preço no `index 5` do array `precos`.*

Os dados são organizados nos seguintes vetores:

* `produtos`: **(Array de Strings)** Nomes dos produtos.
* `codigos`: **(Array de Strings)** SKU/Código de barras (Identificador único).
* `precos`: **(Array de Números)** Valor unitário (R$).
* `estoque`: **(Array de Números)** Quantidade disponível.

---

## 🚀 Funcionalidades

O sistema conta com um menu interativo que permite operações completas de CRUD e gestão:

### 1. 📝 Cadastrar Produto
Adiciona um novo item ao catálogo preenchendo os 4 arrays simultaneamente.
* **Dados solicitados:** Nome, Código (SKU), Preço e Quantidade Inicial.

### 2. 📋 Listar Produtos
Gera um relatório visual no console/tela com todos os itens cadastrados, formatados em tabela para fácil visualização.

### 3. 🔍 Buscar por Código
Localiza um produto específico através do seu SKU único e exibe seus detalhes.

### 4. ➕ Entrada no Estoque (Reposição)
Atualiza a quantidade de um item existente (ex: chegada de fornecedor).
* Solicita o código -> Localiza o índice -> Soma a nova quantidade ao saldo atual.

### 5. ➖ Baixa no Estoque (Venda)
Simula a venda e saída de produtos. Possui **validação de segurança**:
* O sistema verifica: `Quantidade Solicitada <= Estoque Atual?`
    * ✅ **Sim:** Subtrai do saldo.
    * ❌ **Não:** Bloqueia a operação e exibe "Estoque insuficiente".

### 6. 🚪 Sair
Encerra a execução do loop do programa.

---

## 🛠 Tecnologias
* HTML5
* CSS3
* JavaScript (Lógica de Arrays e Manipulação de Dados)

---
