<template>
    <!-- <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header> -->
    <!-- <header>
        <h2>Zoo On The Loose</h2>
    </header> -->

    <RouterView />
    <!-- <footer>
        <div>
            Icons made by <a href="https://www.freepik.com" title="Freepik">Freepik</a> from
            <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a>
        </div>
    </footer> -->
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router';
</script>

<style lang="scss">
#app {
    height: 100%;
    width: 100%;
    background: $background;

    // display: grid;
    // grid-template-rows: 40px auto 40px;

    header,
    footer {
        padding: 8px;
        display: flex;
        align-items: center;
        justify-content: center;

        background: $ss-color-zed;
        color: $ss-color-white;
    }
}
</style>
