<template>
  <div class="main_app">
    <h1 class="text-center">Tic Tac Toe</h1>
    <hr />
    <Board />
    <footer>
      Developed by
      <a
        href="https://sushil-kamble.netlify.app/"
        class="link-style"
        target="_blank"
      >
        Sushil Kamble
      </a>
    </footer>
  </div>
</template>

<script>
import Board from './components/Board'

export default {
  components: { Board }
}
</script>

<style>
body {
  background: rgb(25, 25, 25);
  background: linear-gradient(
    90deg,
    rgba(25, 25, 25, 1) 0%,
    rgba(88, 91, 107, 1) 72%
  );
  color: white;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.text-center {
  text-align: center;
}

.link-style {
  color: white;
  text-decoration: none;
}

footer {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  color: white;
  background: rgb(90, 90, 90);
  text-align: center;
}
</style>