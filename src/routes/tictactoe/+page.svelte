<script lang="ts">
  const cleanBoard = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  let board = Array.from(cleanBoard.map((row) => Array.from(row)));

  let currentPlayer = "x";
  let winner = "";

  function reset() {
    currentPlayer = "x";
    winner = "";
    board = Array.from(cleanBoard.map((row) => Array.from(row)));
  }

  function checkWin() {
    if (
      board.some((row) => {
        const value = row[0];
        return value !== "" && row.every((cell) => cell === value);
      })
    ) {
      winner = currentPlayer;
      return;
    }
    if (
      board.some((_row, rowIndex) => {
        const value = board[0][rowIndex];
        return (
          value !== "" &&
          board.map((row) => row[rowIndex]).every((cell) => cell === value)
        );
      })
    ) {
      winner = currentPlayer;
      return;
    }
    if (
      board[1][1] !== "" &&
      ((board[0][0] === board[1][1] && board[0][0] === board[2][2]) ||
        (board[0][2] === board[1][1] && board[0][2] === board[2][0]))
    ) {
      winner = currentPlayer;
      return;
    }
  }

  function onCellSelected(rowIndex: number, cellIndex: number) {
    if (board[rowIndex][cellIndex] != "" || winner) {
      return;
    }
    board[rowIndex][cellIndex] = currentPlayer;
    checkWin();
    currentPlayer = currentPlayer === "x" ? "o" : "x";
  }

  function onKeyDown(e: KeyboardEvent, rowIndex: number, cellIndex: number) {
    if (e.code != "Enter") {
      return;
    }
    onCellSelected(rowIndex, cellIndex);
  }
</script>

<h1>My TicTacToe in Svelte</h1>
{#if !winner}
  <h2>{currentPlayer.toUpperCase()}'s turn</h2>
{:else}
  <h2>{winner.toUpperCase()} wins!</h2>
  <button on:click={reset}>New Game?</button>
{/if}
<div class="board">
  {#each board as row, rowIndex}
    <div class="row">
      {#each row as cell, cellIndex}
        <svg
          role="button"
          tabindex="0"
          viewBox="0 0 10 10"
          class="cell"
          on:keydown={(e) => onKeyDown(e, rowIndex, cellIndex)}
          on:click={() => onCellSelected(rowIndex, cellIndex)}
        >
          <text x="1" y="9">{cell}</text>
        </svg>
      {/each}
    </div>
  {/each}
</div>

<style>
  .board {
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    width: 500px;
    max-width: calc(100vw - 16px);
    aspect-ratio: 1;
  }

  .row {
    display: flex;
    width: 100%;
    height: calc(100% / 3);
  }

  .cell {
    border: 1px solid red;
    width: calc(100% / 3);
    display: flex;
    justify-content: space-around;
    align-items: center;
  }
</style>
