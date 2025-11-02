#Relatório Técnico — Explicação do JavaScript (StockMaster)

#1) Visão Geral da Arquitetura do JS
O JavaScript organiza a lógica do sistema em três blocos principais:
• Dados (arrays paralelos e constantes);
• Funções auxiliares (pequenas utilidades reutilizáveis);
• Funções principais (as 6 operações pedidas: listar, cadastrar, buscar, entrada, baixa e sair).

#2) Dados Iniciais (Arrays Paralelos)
Os arrays paralelos mantêm a informação do mesmo produto no mesmo índice (i):
let produtos  = [...];   // nomes
let codigos   = [...];   // códigos únicos (SKU)
let precos    = [...];   // preço unitário (número)
let estoque   = [...];   // quantidade disponível (número)
let ultimaAtualizacao = [...]; // texto com data/hora de modificação

A variável LIMITE_BAIXO define o ponto onde um item passa a ser considerado 'estoque baixo'.

#3) Funções Auxiliares
3.1) mostrar(id, botao)
Responsável por alternar as abas (views) da interface.
Passos:
1. Remove a classe CSS 'show' de todas as seções com class='view';
2. Adiciona 'show' apenas na seção cujo id foi recebido (ex.: 'listar');
3. Remove 'active' de todos os botões de aba e adiciona em quem foi clicado (botao).
Resultado: só a view selecionada fica visível e a aba ativa fica destacada.
3.2) dataAgora()
Retorna uma string fixa ('02/11/2025 10:00') simulando uma data/hora. Decisão didática: evita uso de APIs de data, mantendo o foco em lógica e arrays.
3.3) formatarPreco(v)
Recebe um número e devolve uma string com 'R$ ' na frente. Troca ponto por vírgula se existir (ex.: 3500.00 → 'R$ 3500,00'). Mantém o formato simples e adequado para exibição.
3.4) procurarIndice(cod)
Busca linear no array 'codigos'. Percorre do início ao fim e retorna o índice onde encontrou; se não encontrar, retorna -1. Complexidade: O(n). É suficiente para nosso catálogo pequeno e facilita o entendimento do aluno.
3.5) kpiFlash(id)
Aplica uma animação CSS nos KPIs quando o valor muda. Remove a classe 'flash', força reflow e adiciona novamente para reexecutar a animação.
3.6) desenharSparkline()
Desenha um mini-gráfico (sparkline) no KPI de Valor Total usando SVG puro.
Como funciona:
• Mantém um vetor 'historicoValor' com valores totais em instantes sucessivos;
• Calcula mínimo e máximo para normalizar os pontos no intervalo [0, 1];
• Converte cada valor em coordenadas (x,y) dentro da viewBox 100x28;
• Atualiza o elemento <polyline> com a lista de pontos e posiciona um ponto final (<circle>).

#4) Funções Principais (Requisitos da Prova)
4.1) listarProdutos()
Responsável por:
• Limpar e remontar o <tbody> da tabela com todos os produtos;
• Calcular e exibir os KPIs do topo (total de produtos, valor total e quantidade de itens com estoque baixo);
• Atualizar o histórico do valor total e redesenhar o sparkline.

Passo a passo:
1) Zera o conteúdo do <tbody> (tbodyProdutos.innerHTML = '');
2) Para cada índice i: monta uma <tr> com código, nome, preço (formatado), quantidade, status e última atualização;
3) Soma preço*quantidade em 'total';
4) Conta quantos itens estão abaixo do LIMITE_BAIXO;
5) Atualiza os elementos #kpiTotalProdutos, #kpiValorTotal e #kpiEstoqueBaixo;
6) Aplica animação com kpiFlash() e atualiza o sparkline.
4.2) cadastrarProduto()
Fluxo:
1) Lê os valores dos inputs (nome, código, preço e quantidade);
2) Validação: se houver campo vazio → mensagem de erro; se o código já existir → erro;
3) Adiciona cada dado no final dos arrays paralelos usando push();
4) Grava a data de atualização no array ultimaAtualizacao;
5) Mostra mensagem de sucesso e chama listarProdutos() para refletir mudanças na interface.
4.3) buscarProduto()
Recebe um código via input. Usa procurarIndice() para encontrar o índice. Se -1, exibe 'Produto não encontrado'; caso contrário, mostra nome, código, preço, quantidade e última atualização.
4.4) registrarEntrada()
Objetivo: registrar reposição.
1) Lê código e quantidade a adicionar; valida se o produto existe e se a quantidade é válida (>0);
2) Soma a quantidade ao estoque[i];
3) Atualiza ultimaAtualizacao[i] e chama listarProdutos().
4.5) registrarBaixa()
Objetivo: registrar venda/consumo.
1) Lê código e quantidade a remover; valida existência e quantidade (>0);
2) Verifica se qtd <= estoque[i]; se não, exibe 'Estoque insuficiente';
3) Subtrai e atualiza ultimaAtualizacao[i];
4) Chama listarProdutos() para atualizar tela e KPIs.
4.6) sair()
Simula o encerramento da sessão: mostra um alert e desabilita todos os botões (for sobre getElementsByTagName('button')).

#5) Mapa Requisito → Implementação
1. Cadastrar Produto → cadastrarProduto()
2. Listar Produtos → listarProdutos()
3. Buscar por Código → buscarProduto() + procurarIndice()
4. Dar Entrada → registrarEntrada()
5. Dar Baixa → registrarBaixa()
6. Sair → sair()

#6) Regras de Validação e Mensagens
• Campos obrigatórios (nome, código, preço e quantidade) não podem ficar vazios;
• Código (SKU) deve ser único (checagem via procurarIndice);
• Quantidades de entrada/baixa devem ser maiores que zero;
• Na baixa, a quantidade solicitada não pode ultrapassar o estoque atual;
• Mensagens de erro/sucesso são exibidas nos elementos #msgCadastro, #msgEntrada e #msgBaixa.

#7) Interação com o DOM (Elementos da Tela)
Leitura/Escrita de valores e classes visuais:
• document.getElementById('...').value → lê valores dos inputs;
• element.innerHTML → escreve o conteúdo das tabelas e mensagens;
• element.classList.add()/remove() → alterna visibilidade das views e estados das abas;
• getElementsByTagName('button') → usado em sair() para desabilitar todos os botões.

#8) Complexidade e Escalabilidade
• As funções de busca usam varredura linear O(n), adequada para listas pequenas e didática.
• Para catálogos grandes, recomenda-se usar um mapa (objeto ou Map) de código→índice, reduzindo buscas para O(1).

#9) Casos de Teste Sugeridos
1) Cadastrar com todos os campos vazios → deve bloquear.
2) Cadastrar com código já existente → deve bloquear.
3) Cadastrar item válido → deve aparecer na lista e atualizar KPIs.
4) Buscar por código inexistente → deve mostrar 'não encontrado'.
5) Entrada com quantidade 0 ou negativa → bloquear.
6) Baixa com estoque insuficiente → bloquear.
7) Baixa válida → reduzir estoque e, se ficar abaixo do limite, status virar 'Estoque baixo'.
8) Sair → todos os botões ficam desativados.

#10) Pseudocódigo Resumido
fun listarProdutos():
  limpar tabela
  total ← 0; baixos ← 0
  para i de 0 até (tamanho-1):
    inserir linha com dados[i]
    total ← total + (precos[i] * estoque[i])
    se estoque[i] < LIMITE_BAIXO então baixos ← baixos + 1
  exibir KPIs(total, baixos)
  adicionar total ao historicoValor e desenharSparkline()

fun cadastrarProduto():
  ler campos
  validar
  push em cada array
  listarProdutos()

fun registrarEntrada()/registrarBaixa():
  validar código e quantidade
  somar/subtrair
  atualizar data e listarProdutos()

#11) Conclusão
O JS do StockMaster foi construído para evidenciar o domínio de arrays, índices, laços e condições. Cada função responde diretamente a um requisito da prova, e o código mantém uma ligação clara entre os elementos de interface e a lógica de negócio. O sparkline e as animações dos KPIs são extras visuais que não complicam a lógica central, mas enriquecem a apresentação.
