# Relatório Técnico — Explicação do JavaScript (StockMaster)

## 1) Visão Geral da Arquitetura do JS

O JavaScript organiza a lógica do sistema em três blocos principais:

- **Dados** — arrays paralelos e constantes;
- **Funções auxiliares** — pequenas utilidades reutilizáveis;
- **Funções principais** — as seis operações pedidas na prova:
  - listar  
  - cadastrar  
  - buscar  
  - entrada  
  - baixa  
  - sair  

---

## 2) Dados Iniciais (Arrays Paralelos)

Os **arrays paralelos** mantêm as informações do mesmo produto na **mesma posição (índice)**:

```javascript
let produtos  = [...]; // nomes
let codigos   = [...]; // códigos únicos (SKU)
let precos    = [...]; // preço unitário (número)
let estoque   = [...]; // quantidade disponível (número)
let ultimaAtualizacao = [...]; // data/hora de modificação
```

A variável `LIMITE_BAIXO` define o ponto onde um item passa a ser considerado com **“estoque baixo”**.

---

## 3) Funções Auxiliares

### 3.1) mostrar(id, botao)

Responsável por alternar as **abas (views)** da interface.

**Passos:**
1. Remove a classe CSS `show` de todas as seções com `class="view"`;
2. Adiciona `show` apenas na seção cujo `id` foi recebido (ex.: `"listar"`);
3. Remove `active` de todos os botões de aba e adiciona no botão clicado.

**Resultado:** apenas a aba selecionada fica visível, e o botão ativo ganha destaque.

---

### 3.2) dataAgora()

Retorna uma string fixa simulando uma data/hora:  
`"02/11/2025 10:00"`

Essa abordagem **evita o uso de APIs de data**, mantendo o foco em lógica e manipulação de arrays.

---

### 3.3) formatarPreco(v)

Recebe um número e devolve uma string com `R$` na frente.  
Troca ponto por vírgula, se existir.

**Exemplo:**
`3500.00 → "R$ 3.500,00"`

Mantém o formato simples e adequado para exibição.

---

### 3.4) procurarIndice(cod)

Busca **linearmente** no array `codigos` e retorna o índice onde encontrou.  
Se não encontrar, retorna `-1`.

**Complexidade:** `O(n)`  
É suficiente para um catálogo pequeno e ajuda na compreensão da busca por posição.

---

### 3.5) kpiFlash(id)

Aplica uma **animação CSS** nos indicadores (KPIs) sempre que o valor muda.  
Remove a classe `flash`, força o navegador a recalcular o layout, e adiciona novamente — isso faz a animação reiniciar.

---

### 3.6) desenharSparkline()

Desenha um **mini-gráfico (sparkline)** no KPI de “Valor Total em Estoque”, usando apenas **SVG**.

**Funcionamento:**
- Mantém um vetor `historicoValor` com o valor total do estoque em diferentes momentos;
- Calcula o valor mínimo e máximo para normalizar os pontos no intervalo `[0, 1]`;
- Converte cada valor em coordenadas `(x, y)` dentro da viewBox `100x28`;
- Atualiza o `<polyline>` com os pontos e posiciona um ponto final (`<circle>`).

---

## 4) Funções Principais (Requisitos da Prova)

### 4.1) listarProdutos()

**Responsável por:**
- Limpar e remontar a tabela com todos os produtos;
- Calcular e exibir os KPIs (Total de produtos, Valor total e Estoque baixo);
- Atualizar o histórico de valores e redesenhar o sparkline.

**Passo a passo:**
1. Zera o conteúdo da tabela (`tbodyProdutos.innerHTML = ""`);
2. Para cada produto, monta uma `<tr>` com código, nome, preço, quantidade, status e data;
3. Soma `preco × quantidade` em uma variável `total`;
4. Conta quantos itens estão abaixo do `LIMITE_BAIXO`;
5. Atualiza os elementos `#kpiTotalProdutos`, `#kpiValorTotal` e `#kpiEstoqueBaixo`;
6. Aplica a animação com `kpiFlash()` e redesenha o gráfico com `desenharSparkline()`.

---

### 4.2) cadastrarProduto()

**Fluxo:**
1. Lê os valores digitados nos campos (nome, código, preço e quantidade);
2. Valida:
   - Se algum campo está vazio → mensagem de erro;
   - Se o código já existe → erro de duplicidade;
3. Adiciona os valores ao final dos arrays com `push()`;
4. Grava a data atual no array `ultimaAtualizacao`;
5. Mostra mensagem de sucesso e chama `listarProdutos()` para atualizar a tela.

---

### 4.3) buscarProduto()

Recebe um código via input e usa `procurarIndice()` para encontrar o produto.  
Se o índice for `-1`, exibe **“Produto não encontrado”**.  
Caso contrário, mostra nome, código, preço, quantidade e última atualização.

---

### 4.4) registrarEntrada()

**Objetivo:** adicionar novas unidades (reposição).

**Etapas:**
1. Lê código e quantidade a adicionar;
2. Verifica se o produto existe e se a quantidade é válida (>0);
3. Soma ao estoque existente (`estoque[i] += qtd`);
4. Atualiza `ultimaAtualizacao[i]`;
5. Chama `listarProdutos()`.

---

### 4.5) registrarBaixa()

**Objetivo:** remover unidades (venda ou consumo).

**Etapas:**
1. Lê código e quantidade a remover;
2. Verifica se o produto existe e se a quantidade é válida (>0);
3. Se `qtd > estoque[i]`, mostra **“Estoque insuficiente”**;
4. Caso contrário, subtrai e atualiza `ultimaAtualizacao[i]`;
5. Chama `listarProdutos()`.

---

### 4.6) sair()

Simula o encerramento da sessão.  
Mostra um **alert** e desativa todos os botões com:

```javascript
let botoes = document.getElementsByTagName("button");
for (let i = 0; i < botoes.length; i++) botoes[i].disabled = true;
```

---

## 5) Mapa Requisito → Implementação

| Requisito da Prova | Função no Código |
|---------------------|------------------|
| Cadastrar Produto | `cadastrarProduto()` |
| Listar Produtos | `listarProdutos()` |
| Buscar por Código | `buscarProduto()` + `procurarIndice()` |
| Dar Entrada | `registrarEntrada()` |
| Dar Baixa | `registrarBaixa()` |
| Sair | `sair()` |

---

## 6) Regras de Validação e Mensagens

- Todos os campos (nome, código, preço e quantidade) são obrigatórios.  
- O **código SKU** deve ser único (verificação via `procurarIndice`).  
- Quantidades de entrada/baixa devem ser **maiores que zero**.  
- Na baixa, a quantidade não pode exceder o estoque atual.  
- Mensagens de erro/sucesso são exibidas nos elementos `#msgCadastro`, `#msgEntrada` e `#msgBaixa`.

---

## 7) Interação com o DOM (Tela e Código)

O sistema usa os comandos básicos do **DOM** para conectar lógica e interface:

- `document.getElementById('id').value` → lê valores dos campos.  
- `element.innerHTML` → insere conteúdo nas tabelas e mensagens.  
- `element.classList.add()` / `.remove()` → controla a exibição das abas.  
- `getElementsByTagName('button')` → desativa todos os botões ao sair.

---

## 8) Complexidade e Escalabilidade

- As buscas são **lineares** (`O(n)`), ideais para listas pequenas e fins didáticos.  
- Para estoques grandes, pode-se usar um **objeto** ou **Map** para transformar a busca em `O(1)` (acesso direto por código).

---

## 9) Casos de Teste Sugeridos

1. Cadastrar com todos os campos vazios → deve bloquear.  
2. Cadastrar com código duplicado → deve bloquear.  
3. Cadastrar produto válido → aparece na lista e atualiza KPIs.  
4. Buscar código inexistente → mostra “não encontrado”.  
5. Entrada com quantidade zero ou negativa → bloqueia.  
6. Baixa com estoque insuficiente → bloqueia.  
7. Baixa válida → reduz estoque e atualiza status.  
8. Sair → desativa todos os botões.

---

## 10) Pseudocódigo Resumido

```text
fun listarProdutos():
  limpar tabela
  total ← 0; baixos ← 0
  para i de 0 até n-1:
    inserir linha com dados[i]
    total ← total + (precos[i] * estoque[i])
    se estoque[i] < LIMITE_BAIXO então baixos ← baixos + 1
  exibir KPIs(total, baixos)
  adicionar total ao historicoValor
  desenharSparkline()

fun cadastrarProduto():
  ler campos
  validar
  push em cada array
  listarProdutos()

fun registrarEntrada()/registrarBaixa():
  validar código e quantidade
  somar/subtrair
  atualizar data e listarProdutos()
```

---

## 11) Conclusão

O **JavaScript do StockMaster** evidencia o domínio de:
- Arrays, índices e laços;
- Condições e validações lógicas;
- Conexão entre dados e interface (DOM);
- Simplicidade no fluxo e clareza pedagógica.

Cada função atende diretamente a um requisito da prova.  
As animações e o sparkline enriquecem a apresentação sem alterar a lógica central.

---

### 💡 Extensões Futuras

- Armazenar dados com `localStorage`;
- Adicionar login de administrador;
- Gerar relatórios PDF;
- Criar filtros e ordenações por preço, estoque e data.
