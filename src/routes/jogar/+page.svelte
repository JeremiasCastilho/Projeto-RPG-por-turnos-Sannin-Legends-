<script lang="ts">
    // Define os nomes dos três personagens que podem ser escolhidos pelo jogador.
    type DeckName = "Jiraiya" | "Tsunade" | "Orochimaru";

    // Define a estrutura que todas as cartas do jogo devem possuir.
    type Card = {
        name: string;
        description: string;
        damage: number;
        healing: number;
        cost: number;
    };

    // Armazena os baralhos dos personagens e as características de cada carta.
    const decks: Record<DeckName, Card[]> = {
        Jiraiya: [
            {
                name: "Rasengan",
                description: "A powerful chakra attack.",
                damage: 25,
                healing: 0,
                cost: 1
            },
            {
                name: "Fire Style",
                description: "Burn the opponent.",
                damage: 20,
                healing: 0,
                cost: 1
            },
            {
                name: "Sage Mode",
                description: "Recover some health.",
                damage: 0,
                healing: 20,
                cost: 2
            },
            {
                name: "Gamabunta",
                description: "Summon the giant toad.",
                damage: 40,
                healing: 0,
                cost: 3
            }
        ],

        Tsunade: [
            {
                name: "Monster Strength",
                description: "A devastating punch.",
                damage: 30,
                healing: 0,
                cost: 1
            },
            {
                name: "Medical Ninjutsu",
                description: "Restore your health.",
                damage: 0,
                healing: 25,
                cost: 2
            },
            {
                name: "Chakra Punch",
                description: "A powerful physical attack.",
                damage: 35,
                healing: 0,
                cost: 2
            },
            {
                name: "Katsuyu",
                description: "Summon the giant slug.",
                damage: 0,
                healing: 40,
                cost: 3
            }
        ],

        Orochimaru: [
            {
                name: "Snake Attack",
                description: "Attack with snakes.",
                damage: 25,
                healing: 0,
                cost: 1
            },
            {
                name: "Forbidden Jutsu",
                description: "A dangerous technique.",
                damage: 35,
                healing: 0,
                cost: 2
            },
            {
                name: "Reanimation",
                description: "Use forbidden techniques.",
                damage: 30,
                healing: 0,
                cost: 2
            },
            {
                name: "Manda",
                description: "Summon the giant snake.",
                damage: 45,
                healing: 0,
                cost: 3
            }
        ]
    };

    // Define o personagem selecionado inicialmente pelo jogador.
    let selectedDeck: DeckName = "Jiraiya";

    // Armazena os pontos de vida do jogador e do adversário, além do chakra disponível.
    let playerHp: number = 100;
    let enemyHp: number = 100;
    let chakra: number = 3;

    // Controla se a partida já começou e se foi encerrada.
    let started: boolean = false;
    let gameOver: boolean = false;

    // Armazena a mensagem que informa ao jogador o que está acontecendo na partida.
    let message: string = "Choose your Sannin deck to begin!";

    // Armazena as cartas que estão disponíveis na mão do jogador.
    let hand: Card[] = [];

    // Armazena as cartas que ainda não foram compradas durante a partida.
    let drawPile: Card[] = [];

    // Inicia uma nova partida e reinicia os atributos dos personagens.
    function startGame(): void {
        playerHp = 100;
        enemyHp = 100;
        chakra = 3;

        // Cria uma cópia do baralho escolhido e embaralha a ordem das cartas.
        const shuffledDeck: Card[] = [...decks[selectedDeck]]
            .sort(() => Math.random() - 0.5);

        // Separa as três primeiras cartas para formar a mão inicial do jogador.
        hand = shuffledDeck.slice(0, 3);

        // Guarda as cartas restantes para serem compradas durante a partida.
        drawPile = shuffledDeck.slice(3);

        // Atualiza o estado da partida para indicar que uma nova batalha começou.
        gameOver = false;
        started = true;

        // Exibe a mensagem inicial da batalha.
        message = "Battle started! Choose your card.";
    }

    // Executa a jogada do jogador utilizando a carta selecionada.
    function playCard(index: number): void {

        // Impede a jogada se a partida não começou, já terminou ou falta chakra.
        if (!started || gameOver || chakra < hand[index].cost) {
            return;
        }

        // Obtém a carta escolhida usando sua posição na mão do jogador.
        const card: Card = hand[index];

        // Aplica o dano da carta ao adversário, impedindo que a vida fique negativa.
        enemyHp = Math.max(0, enemyHp - card.damage);

        // Recupera a vida do jogador sem ultrapassar o limite de 100 pontos.
        playerHp = Math.min(100, playerHp + card.healing);

        // Desconta do chakra disponível o custo necessário para utilizar a carta.
        chakra -= card.cost;

        // Informa qual carta foi utilizada pelo jogador.
        message = card.name + " used!";

        // Remove da mão a carta que acabou de ser utilizada.
        hand = hand.filter((_, cardIndex) => cardIndex !== index);

        // Verifica se ainda existem cartas no baralho para comprar uma nova.
        if (drawPile.length > 0) {
            // Adiciona a primeira carta restante à mão do jogador.
            hand = [...hand, drawPile[0]];

            // Remove do baralho a carta que acabou de ser comprada.
            drawPile = drawPile.slice(1);
        }

        // Verifica se o adversário ficou sem pontos de vida.
        if (enemyHp <= 0) {
            message = "Victory! You won the battle!";
            gameOver = true;
            return;
        }

        // Calcula o dano do contra-ataque do computador, entre 10 e 20 pontos.
        const enemyDamage: number =
            Math.floor(Math.random() * 11) + 10;

        // Aplica o dano do adversário sem permitir que a vida do jogador fique negativa.
        playerHp = Math.max(0, playerHp - enemyDamage);

        // Verifica se o jogador ficou sem pontos de vida.
        if (playerHp <= 0) {
            message = "Defeat! Your opponent won!";
            gameOver = true;
            return;
        }

        // Restaura o chakra para três pontos no início do próximo turno.
        chakra = 3;

        // Informa o dano causado pelo adversário e libera a próxima jogada.
        message = "Opponent attacked for " +
            enemyDamage + " damage! Your turn.";
    }
</script>

<div class="fundo-menu battle-page">

    <div class="battle-container">

        <h1>LEGENDARY SANNIN</h1>

        {#if !started}

            <h2>Choose your Sannin</h2>

            <!-- Permite selecionar qual personagem será utilizado na batalha. -->
            <select bind:value={selectedDeck}>
                <option value="Jiraiya">Jiraiya</option>
                <option value="Tsunade">Tsunade</option>
                <option value="Orochimaru">Orochimaru</option>
            </select>

            <!-- Inicia a partida utilizando o personagem selecionado. -->
            <button on:click={startGame}>
                START BATTLE
            </button>

        {:else}

            <!-- Exibe as informações de vida e chakra dos participantes. -->
            <div class="battle-status">

                <div class="fighter">
                    <h2>{selectedDeck}</h2>

                    <p>Player HP: {playerHp}/100</p>

                    <!-- A largura da barra acompanha a quantidade atual de vida do jogador. -->
                    <div class="health-bar">
                        <div
                            class="health-fill player-health"
                            style="width: {playerHp}%"
                        ></div>
                    </div>

                    <p>Chakra: {chakra}/3</p>
                </div>

                <div class="fighter">
                    <h2>Opponent</h2>

                    <p>Enemy HP: {enemyHp}/100</p>

                    <!-- A largura da barra acompanha a quantidade atual de vida do adversário. -->
                    <div class="health-bar">
                        <div
                            class="health-fill enemy-health"
                            style="width: {enemyHp}%"
                        ></div>
                    </div>
                </div>

            </div>

            <h2>Your Cards</h2>

            <!-- Mostra quantas cartas ainda estão disponíveis para compra. -->
            <p>Cards remaining in deck: {drawPile.length}</p>

            <!-- Percorre as cartas da mão e apresenta seus atributos na tela. -->
            <div class="card-list">
                {#each hand as card, index}
                    <div class="game-card">
                        <h3>{card.name}</h3>
                        <p>{card.description}</p>
                        <p>Damage: {card.damage}</p>
                        <p>Healing: {card.healing}</p>
                        <p>Chakra Cost: {card.cost}</p>

                        <!-- Executa a jogada com a posição da carta selecionada.
                             O botão fica desativado se a partida terminou ou falta chakra. -->
                        <button
                            on:click={() => playCard(index)}
                            disabled={gameOver || chakra < card.cost}
                        >
                            USE CARD
                        </button>
                    </div>
                {/each}
            </div>

            <!-- Informa quando não existem mais cartas disponíveis na mão. -->
            {#if hand.length === 0}
                <p>No cards left in your hand!</p>
            {/if}

            <!-- Exibe as mensagens de resultado e andamento da batalha. -->
            <div class="battle-message">
                <p>{message}</p>
            </div>

            <!-- Mostra a opção de iniciar outra partida quando a batalha termina. -->
            {#if gameOver}
                <button on:click={startGame}>
                    PLAY AGAIN
                </button>
            {/if}

        {/if}

        <!-- Permite retornar à página inicial do jogo. -->
        <a class="menu" href="/">
            BACK TO MENU
        </a>

    </div>

</div>
