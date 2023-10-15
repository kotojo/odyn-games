<script lang="ts">
  import { AspectRatio } from "carbon-components-svelte";

  const colors = ["yellow", "blue", "red", "green"] as const;
  type colors = (typeof colors)[number];
  let gameRunning = false;
  let gameLost = false;
  let showingColor = "";
  let simonSays: colors[] = [];
  let colorGuesses: colors[] = [];
  function onButtonClicked(color: colors) {
    colorGuesses.push(color);
    checkLoss();
    if (colorGuesses.length === simonSays.length) {
      colorGuesses = [];
      chooseNextColor();
    }
  }
  function checkLoss() {
    if (
      simonSays.slice(0, colorGuesses.length).some((val, i) => {
        return colorGuesses[i] != val;
      })
    ) {
      gameLost = true;
      gameRunning = false;
    }
  }
  function startGame() {
    if (gameRunning) {
      return;
    }
    gameLost = false;
    gameRunning = true;
    simonSays = [];
    console.log("starting game");
    colorGuesses = [];
    chooseNextColor();
  }
  function chooseNextColor() {
    if (gameLost) {
      return;
    }
    simonSays = [...simonSays, getRandomColor()];
    console.log("color choosen", simonSays);
    showColors();
  }
  function getRandomColor() {
    const index = Math.floor(Math.random() * 4);
    return colors[index];
  }
  async function showColors() {
    let showingIndex = 0;
    while (showingIndex < simonSays.length) {
      showingColor = simonSays[showingIndex];
      console.log("showing ", showingColor);
      await sleep(800);
      showingColor = "";
      await sleep(200);
      showingIndex++;
    }
    console.log("done showing");
    showingColor = "";
  }
  async function sleep(time: number) {
    return new Promise<void>((res) => {
      setTimeout(() => {
        res();
      }, time);
    });
  }
</script>

<div class="container">
  <AspectRatio ratio="1x1">
    <div class="simon">
      <button
        on:click={() => onButtonClicked("yellow")}
        class={`yellow ${showingColor === "yellow" ? "active" : ""}`}
      />
      <button
        on:click={() => onButtonClicked("blue")}
        class={`blue ${showingColor === "blue" ? "active" : ""}`}
      />
      <button
        on:click={() => onButtonClicked("red")}
        class={`red ${showingColor === "red" ? "active" : ""}`}
      />
      <button
        on:click={() => onButtonClicked("green")}
        class={`green ${showingColor === "green" ? "active" : ""}`}
      />
      <button class="start" on:click={startGame}>
        {gameRunning ? simonSays.length : "Go"}
      </button>
    </div>
  </AspectRatio>
</div>
{#if gameLost}
  <h2>GAME LOST</h2>
  <button on:click={startGame}>Restart</button>
{/if}

<style>
  .simon {
    position: relative;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    width: 80%;
    max-width: 800px;
    margin: auto;
    aspect-ratio: 1;
  }

  .start {
    position: absolute;
    width: 10%;
    min-width: var(--cds-spacing-09);
    aspect-ratio: 1;
    border-radius: 100%;
    top: calc(min(45%, calc(50% - var(--cds-spacing-09) / 2)));
    left: calc(min(45%, calc(50% - var(--cds-spacing-09) / 2)));
  }

  button {
    border: none;
    cursor: pointer;
    border: 5px solid black;
  }

  button:active {
    box-shadow: inset 0 0 100px 100px rgba(255, 255, 255, 0.6);
  }

  button.active {
    box-shadow: inset 0 0 100px 100px rgba(255, 255, 255, 0.6);
  }

  button.active.yellow {
    box-shadow: inset 0 0 100px 100px rgba(0, 0, 0, 0.3);
  }

  .yellow {
    background-color: yellow;
    border-top-left-radius: 100%;
  }
  .yellow:active {
    box-shadow: inset 0 0 100px 100px rgba(0, 0, 0, 0.3);
  }
  .blue {
    background-color: blue;
    border-top-right-radius: 100%;
  }
  .red {
    background-color: red;
    border-bottom-left-radius: 100%;
  }
  .green {
    background-color: green;
    border-bottom-right-radius: 100%;
  }
</style>
