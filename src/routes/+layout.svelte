<script>
  import { page } from "$app/stores";
  import {
    Content,
    Header,
    HeaderNav,
    HeaderNavItem,
    SideNav,
    SideNavItems,
    SideNavLink,
    SkipToContent,
  } from "carbon-components-svelte";
  import "carbon-components-svelte/css/all.css";

  let isSideNavOpen = false;
</script>

<Header company="Odyn Land" bind:isSideNavOpen>
  <svelte:fragment slot="skip-to-content">
    <SkipToContent />
  </svelte:fragment>
  <HeaderNav>
    <HeaderNavItem text="Tic Tac Toe" href="/tictactoe" />
    <HeaderNavItem text="Memory Match" href="/memorymatch" />
    <HeaderNavItem text="Simon" href="/simon" />
  </HeaderNav>
</Header>
<SideNav bind:isOpen={isSideNavOpen}>
  <SideNavItems>
    <SideNavLink text="Tic Tac Toe" href="/tictactoe" />
    <SideNavLink text="Memory Match" href="/memorymatch" />
    <SideNavLink text="Simon" href="/simon" />
  </SideNavItems>
</SideNav>

<Content>
  <slot />
</Content>
