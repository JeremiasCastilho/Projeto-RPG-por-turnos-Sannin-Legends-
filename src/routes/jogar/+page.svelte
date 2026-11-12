<script lang="ts">
    type DeckName = "Jiraiya" | "Tsunade" | "Orochimaru";

    type Card = {
        name: string;
        description: string;
        damage: number;
        healing: number;
        cost: number;
    };

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

    let selectedDeck: DeckName = "Jiraiya";

    let playerHp: number = 100;
    let enemyHp: number = 100;
    let chakra: number = 3;

    let started: boolean = false;
    let gameOver: boolean = false;

    let message: string = "Choose your Sannin deck to begin!";

        // Cards available to the player
    let hand: Card[] = [];

        // Cards waiting in the deck
    let drawPile: Card[] = [];

    function startGame(): void {
        playerHp = 100;
        enemyHp = 100;
        chakra = 3;

            // Shuffle the selected deck
        const shuffledDeck: Card[] = [...decks[selectedDeck]]
            .sort(() => Math.random() - 0.5);

        // Draw the first three cards
        hand = shuffledDeck.slice(0, 3);

        // Keep the remaining cards in the deck
        drawPile = shuffledDeck.slice(3);

        gameOver = false;
        started = true;

        message = "Battle started! Choose your card.";
    }

    
    function playCard(index: number): void {

        if (!started || gameOver || chakra < hand[index].cost) {
            return;
        }

        // Get the selected card
        const card: Card = hand[index];

        // Apply the card effects
        enemyHp = Math.max(0, enemyHp - card.damage);

        playerHp = Math.min(100, playerHp + card.healing);

        chakra -= card.cost;

        message = card.name + " used!";

        // Remove the used card from the hand
        hand = hand.filter((_, cardIndex) => cardIndex !== index);

        // Draw a new card if the deck is not empty
        if (drawPile.length > 0) {
            hand = [...hand, drawPile[0]];
            drawPile = drawPile.slice(1);
        }

        // Check player victory
        if (enemyHp <= 0) {
            message = "Victory! You won the battle!";
            gameOver = true;
            return;
        }

        // Computer counterattack
        const enemyDamage: number =
            Math.floor(Math.random() * 11) + 10;

        playerHp = Math.max(0, playerHp - enemyDamage);

        // Check player defeat
        if (playerHp <= 0) {
            message = "Defeat! Your opponent won!";
            gameOver = true;
            return;
        }

        // Restore chakra for the next turn
        chakra = 3;

        message = "Opponent attacked for " +
            enemyDamage + " damage! Your turn.";
    }


</script>

<div class="fundo-menu battle-page">

    <div class="battle-container">

        <h1>LEGENDARY SANNIN</h1>

        {#if !started}

            <h2>Choose your Sannin</h2>

            <select bind:value={selectedDeck}>
                <option value="Jiraiya">Jiraiya</option>
                <option value="Tsunade">Tsunade</option>
                <option value="Orochimaru">Orochimaru</option>
            </select>

            <button on:click={startGame}>
                START BATTLE
            </button>

        {:else}

            <div class="battle-status">

                <div class="fighter">
                    <h2>{selectedDeck}</h2>

                    <p>Player HP: {playerHp}/100</p>

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

                    <div class="health-bar">
                        <div
                            class="health-fill enemy-health"
                            style="width: {enemyHp}%"
                        ></div>
                    </div>
                </div>

            </div>

            
    <h2>Your Cards</h2>

    <p>Cards remaining in deck: {drawPile.length}</p>

    <div class="card-list">
        {#each hand as card, index}
            <div class="game-card">
                <h3>{card.name}</h3>
                <p>{card.description}</p>
                <p>Damage: {card.damage}</p>
                <p>Healing: {card.healing}</p>
                <p>Chakra Cost: {card.cost}</p>

                <button
                    on:click={() => playCard(index)}
                    disabled={gameOver || chakra < card.cost}
                >
                    USE CARD
                </button>
            </div>
        {/each}
    </div>

{#if hand.length === 0}
    <p>No cards left in your hand!</p>
{/if}


            <div class="battle-message">
                <p>{message}</p>
            </div>

            {#if gameOver}
                <button on:click={startGame}>
                    PLAY AGAIN
                </button>
            {/if}

        {/if}

        <a class="menu" href="/">
            BACK TO MENU
        </a>

    </div>

</div>
