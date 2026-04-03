# Neuro-Defense
Projeto educativo para aprender idiomas.
Neuro-Defense: Documentação do Projeto
1. Visão Geral

Neuro-Defense é um jogo de navegador onde o progresso do jogador depende do seu conhecimento de inglês. O objetivo é navegar por estágios, derrotar inimigos (representando diferentes desafios linguísticos) e restaurar o sistema neural da humanidade.
2. Arquitetura Técnica
A. Estrutura de Arquivos

    index.html: Contém a estrutura de interface, estilos (CSS) e a engine principal (JavaScript).

    database.js: O banco de dados contendo palavras, frases, regras gramaticais e exercícios de lacunas.

    Assets (Imagens/Sons): Sprites dos inimigos (inimigo-cor.png), jogador, porta e efeitos sonoros.

B. Interface (HTML/CSS)

    Canvas: Onde ocorre a renderização do jogo (600x450px).

    HUD: Barra superior que exibe Vida (Sistema Vital), Maestria (Carga Neural), Pontuação e Estágio.

    Modais: Janelas sobrepostas para a História, Batalhas, Game Over e Estatísticas. Utilizam backdrop-filter: blur para imersão.

    Controles: Um D-Pad virtual responsivo para dispositivos móveis, com transparência para não obstruir a visão.

3. Mecânicas de Jogo (Engine)
Movimentação e Grid

O Masmorra é baseada em um grid de 12 colunas por 9 linhas. Cada "tile" possui 50px.

    Sistema de Colisão: O mapa é um array onde 1 é parede e 2 é porta. O jogador não pode atravessar paredes nem portas fechadas.

    Trigger de Batalha: Quando a coordenada do jogador (player.x/y) coincide com a de um inimigo vivo, o jogo pausa e o modal de aprendizado abre.

Sistema de Persistência (Anti-Cheat)

O jogo utiliza localStorage para salvar o estado em tempo real:

    Save de Sala: Ao entrar em uma sala, o mapa e os tipos de inimigos são gerados e salvos. Recarregar a página não muda os inimigos.

    Challenge Lock: Se o jogador atualizar a página durante uma pergunta, o jogo reabre automaticamente na mesma pergunta, impedindo o "reroll" de desafios difíceis.

4. Lógica Pedagógica (Os Inimigos)

Cada cor de alienígena representa um método de ensino diferente:
Inimigo	Nome	Descrição	Regra de Aparição
🟢 Verde	Aprendizado	Escolha a tradução correta. 40% das vezes exibe a tradução no título para estudo passivo.	Sempre
🟡 Amarelo	V ou F	Decisão rápida. 30% verdade, 70% falso. Compara palavras com palavras e frases com frases.	Sempre
🔵 Azul	Escrita	Produção ativa. 60% repetir a palavra em inglês, 40% traduzir do português para o inglês.	Sempre
💜 Roxo	Memória	Memorize 5 itens e responda a posição de um deles. One-shot: Se errar, ele some do mapa após dar dano.	Sempre
🌸 Rosa	Reforço	Traz de volta palavras que o jogador errou anteriormente.	Se houver erros
🟠 Laranja	Sintaxe	O jogador deve clicar nas "pílulas" de palavras para formar a frase na ordem correta.	> 500 Pontos
🔴 Vermelho	Gramática	Exercícios de "Complete a lacuna" com 3 opções.	> 700 Pontos
⚫ Preto	Elite	Desafios de gramática pura. Em caso de erro, exibe uma nota explicativa técnica.	> 700 Pontos
⚪ Branco	Cura	Desafio de tradução reversa. Se acertar, cura 25 de HP.	HP < 50%
🎁 Caixa	Suprimentos	Sequência de 3 desafios (Escrita -> Tradução -> V/F). Dá bônus alto de HP e Pontos.	7% de chance

5. Algoritmos Especiais
Distância de Levenshtein (Escrita Inteligente)

Utilizado no inimigo Azul e no Boss.

    Se o jogador erra apenas 1 letra, o jogo não pune. Ele exibe um aviso: "QUASE LÁ!" e permite que o jogador corrija, tornando o aprendizado menos frustrante e focando na memorização, não apenas na digitação perfeita.

Gerador de Distratores (Opções Erradas)

A função _genOpts garante que as opções erradas sejam do mesmo tipo que a correta:

    Se a resposta é uma palavra, as opções serão outras palavras.

    Se a resposta é uma frase, as opções serão outras frases.
    Isso impede que o jogador use o tamanho do texto para adivinhar a resposta.

6. Fluxo de Experiência do Usuário (UX)

    Loading: Verifica se o database.js e todos os assets de imagem/som foram carregados.

    Intro (Neural Log): Apresenta a história em 5 slides. O texto em inglês prepara o terreno para as palavras que serão encontradas no jogo. Aparece apenas na primeira vez.

    Gameplay: O jogador limpa salas. Após dominar o vocabulário básico, a dificuldade sobe (Laranja, Vermelho, Preto).

    Boss Final: Ativado quando o jogador atinge a maestria total do banco de dados. Uma sequência de 15 desafios aleatórios de todos os tipos.

    Game Over: Exibe a pontuação final e oferece o reinício da unidade.

7. Banco de Dados (database.js)

    VOCAB_ALL: Vocabulário de base (substantivos, verbos, pronomes).

    VOCAB_SENTENCES: Frases de conversação e uso cotidiano.

    VOCAB_GRAMMAR: Regras gramaticais com o campo note (exclamação pedagógica).

    VOCAB_FORM_SENTENCE: Exercícios de completar lacunas.

8. Considerações Finais

O projeto utiliza uma abordagem de Aprendizado Baseado em Desafios. Ao contrário de métodos tradicionais, o erro tem uma consequência mecânica (perda de HP), enquanto o acerto gera dopamina através da progressão no mapa e ganho de pontos, criando um ciclo de feedback positivo para a retenção de memória de longo prazo.
