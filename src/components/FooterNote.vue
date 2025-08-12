<template>
  <footer class="m-8 text-ctp-subtext0/40 text-center text-xs flex flex-col gap-1">
    <p class="">
      Built with
      <span class="text-transparent text-shadow-2xs text-shadow-(color:--catppuccin-color-mauve)/75">💜</span>
      by <a class="text-ctp-mauve/50" href="https://unseen.ninja/">lemon</a>.
    </p>
    <p>
      <b>Disclaimer</b>: This project is an independent work and is <b>not affiliated with</b>, or sponsored by
      <a class="text-ctp-mauve/50" href="https://catppuccin.com/">Catppuccin</a>.
    </p>
  </footer>
</template>

<style>
@media (width < 500px) {
  footer {
    place-self: center;
    flex-direction: column-reverse;
    max-width: 30ch;
    font-size: 15px;
  }
}
</style>
