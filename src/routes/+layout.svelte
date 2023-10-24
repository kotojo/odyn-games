<script>
  import {
    Content,
    Header,
    SideNav,
    SideNavItems,
    SideNavLink,
    Theme,
  } from "carbon-components-svelte";

  import { activeTab } from "$lib/active-tab";
  let activeTabName = "";
  activeTab.subscribe((tab) => {
    activeTabName = tab;
  });

  let isSideNavOpen = false;
</script>

<Theme
  theme="g90"
  tokens={{
    background: "#2f52d9",
  }}
>
  <Header
    company="Odyn Land"
    platformName={activeTabName && `- ${activeTabName}`}
    bind:isSideNavOpen
  />
  <SideNav bind:isOpen={isSideNavOpen}>
    <SideNavItems>
      <SideNavLink text="Tic Tac Toe" href="/tictactoe" />
      <SideNavLink text="Memory Match" href="/memorymatch" />
      <SideNavLink text="Simon" href="/simon" />
    </SideNavItems>
  </SideNav>

  <Content>
    <div class="app-container">
      <slot />
    </div>
  </Content>
</Theme>

<style>
  .app-container {
    width: 100%;
    height: 100%;
    max-height: calc(100vh - 6rem);
  }
</style>
