# Trabalho_Daniel_RL
StockMaster: Gerenciador de Estoque de E-commerce
Este projeto simula um sistema de gerenciamento de estoque (Inventory Management System) focado no backend para uma loja de e-commerce. O objetivo principal é controlar o catálogo de produtos e suas quantidades, permitindo operações administrativas como cadastro, listagem e movimentação de estoque.

O sistema não é a "vitrine" (storefront) para o cliente final, mas sim a ferramenta interna que o administrador da loja usaria para manter o estoque organizado e atualizado.

Autores
Paulo

Márcia

Miguel

Estrutura de Dados
O núcleo do sistema é construído sobre quatro arrays paralelos. Isso significa que o produto no index 5 do array produtos tem seus dados correspondentes no index 5 dos arrays codigos, precos e estoque.

produtos: (Array de Strings) Armazena os nomes dos produtos.

codigos: (Array de Strings) Armazena o SKU ou código de barras, servindo como o identificador único (chave) de cada produto.

precos: (Array de Números) Armazena o preço de cada produto.

estoque: (Array de Números) Armazena a quantidade disponível de cada produto.

Funcionalidades
O sistema opera através de um menu interativo que oferece as seguintes operações de CRUD (Create, Read, Update, Delete) e gerenciamento:

1. Cadastrar Produto
Permite ao usuário adicionar um novo item ao inventário. O sistema solicita:

Nome

Código (SKU)

Preço

Quantidade inicial em estoque

Após o recebimento, esses dados são adicionados ao final dos quatro arrays correspondentes.

2. Listar Produtos
Exibe no console uma tabela formatada com todos os produtos atualmente cadastrados, mostrando todas as suas informações: Código, Nome, Preço (R$) e Quantidade (Qtd).

3. Buscar Produto por Código
Permite ao usuário localizar um produto específico. O sistema solicita o código (SKU) e, ao encontrá-lo, exibe todos os dados associados àquele item.

4. Dar Entrada no Estoque (Reposição)
Usado para adicionar mais unidades a um produto já existente (ex: recebimento de um fornecedor).

O sistema solicita o código do produto.

Pergunta a quantidade a ser adicionada.

Soma essa quantidade ao valor existente no array estoque para o índice encontrado.

5. Dar Baixa no Estoque (Venda)
Simula a venda de um produto, removendo unidades do inventário.

O sistema solicita o código do produto.

Pergunta a quantidade a ser removida (vendida).

Verificação de Estoque: O sistema confere se a quantidade a ser removida é menor ou igual ao estoque atual (quantidade <= estoque[index]).

Se sim: A quantidade é subtraída do array estoque.

Se não: A operação é bloqueada e o sistema informa: "Estoque insuficiente".

6. Sair
Encerra a execução do programa.