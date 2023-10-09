<script lang="ts">
  const cards = ["🐶", "🐭", "🐯", "🦁", "🐧", "🐔", "🐵", "🐨"];
  let board: string[] = [];
  let shownCards: number[] = [];
  let matchedCards: number[] = [];
  let winner = false;

  function startGame() {
    winner = false;
    matchedCards = [];
    board = [];
    let unusedCards = Array.from([...cards, ...cards]);
    while (unusedCards.length > 0) {
      const cardIndex = Math.floor(Math.random() * unusedCards.length);
      const card = unusedCards[cardIndex];
      unusedCards = [
        ...unusedCards.slice(0, cardIndex),
        ...unusedCards.slice(cardIndex + 1),
      ];
      board.push(card);
    }
  }
  function showCard(index: number) {
    if (shownCards.length == 2 || shownCards.includes(index)) {
      return;
    }
    shownCards = [...shownCards, index];
    if (shownCards.length == 1) {
      return;
    } else if (
      shownCards.length == 2 &&
      board[shownCards[0]] === board[shownCards[1]]
    ) {
      matchedCards = [...matchedCards, ...shownCards];
      shownCards = [];
      winner = matchedCards.length === cards.length * 2;
    } else {
      setTimeout(() => {
        shownCards = [];
      }, 1000);
    }
  }
  function onKeyDown(e: KeyboardEvent, i: number) {
    if (e.code != "Enter") {
      return;
    }
    showCard(i);
  }

  startGame();
</script>

<h2>MemoryMatch</h2>
<div class="board">
  {#if winner}
    <div class="win-container">
      <h2>You win!</h2>
      <button on:click={startGame}>New Game?</button>
    </div>
  {/if}
  {#each board as card, index (card + index)}
    <div class="scene">
      <div
        class={"card" +
          (shownCards.includes(index) || matchedCards.includes(index)
            ? " is-flipped"
            : "")}
        role="button"
        tabindex="0"
        on:keydown={(e) => onKeyDown(e, index)}
        on:click={() => showCard(index)}
      >
        <svg viewBox="0 0 20 20" class="card-face card-back">
          <text x="2" y="15">{card}</text>
        </svg>
        <div class="card-face card-front" />
        <!-- <div class="card-face card-front" /> -->
      </div>
    </div>
  {/each}
</div>

<style>
  .board {
    display: grid;
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
    aspect-ratio: 1;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
    position: relative;
  }

  .win-container {
    position: absolute;
    z-index: 2;
    background-color: white;
    border: 1px solid #1ac9e8;
    width: 20%;
    height: 20%;
    left: 40%;
    top: 40%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .win-container h2 {
    margin-top: 0;
  }

  .scene {
    perspective: 600px;
  }

  .card {
    width: 80%;
    height: 80%;
    margin-top: 10%;
    margin-left: 10%;
    border: 1px solid gray;
    box-shadow: 5px 5px 5px #1ac9e8;
    position: relative;
    transition: transform 1s;
    transform-style: preserve-3d;
  }

  .card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
  }

  .card-back {
    transform: rotateY(180deg);
  }

  .card.is-flipped {
    transform: rotateY(180deg);
  }
</style>
