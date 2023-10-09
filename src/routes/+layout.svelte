<script>
  import { page } from "$app/stores";
</script>

<nav class="navbar">
  <h2><a href="/" class:active={$page.url.pathname === "/"}>My Site</a></h2>
  <ul class="nav-list">
    <li class:active={$page.url.pathname === "/tictactoe"}>
      <a href="/tictactoe"><span>Tic tac toe</span></a>
    </li>
    <li class:active={$page.url.pathname === "/memorymatch"}>
      <a href="/memorymatch"><span>Memory Match</span></a>
    </li>
    <li class:active={$page.url.pathname === "/simon"}>
      <a href="/simon"><span>Simon</span></a>
    </li>
  </ul>
</nav>
<div class="container">
  <slot />
</div>

<style>
  :global(body) {
    margin: 0;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
  }
  .container {
    margin: 0 auto;
    width: 100vw;
    max-width: 960px;
  }

  .navbar {
    display: flex;
    flex-direction: row;
    align-items: center;
    background-color: #0f59f5;
    padding: 0 5rem;
  }

  h2 {
    margin: 0;
  }

  li {
    font-size: 1.2rem;
    text-align: center;
  }

  li a {
    display: flex;
    align-items: center;
    padding: 0.5rem;
    height: 2.5rem;
  }

  .navbar .active {
    background-color: #1ac9e8;
    height: 100%;
  }

  .navbar a {
    color: white;
    text-decoration: none;
  }

  .nav-list {
    list-style-type: none;
    height: 100%;
    margin: 0;
    display: flex;
    flex-direction: row;
  }
</style>
