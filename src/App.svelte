<script>
  import InputForm from "./lib/InputForm.svelte";
</script>

<main>
  <h1>BetterSort</h1>
  <p>A simple tool to better sort through a YouTube channel's videos.</p>

  <InputForm />
</main>
<footer>
  fueled by <a href="https://www.santamonicabrewworks.com/"
    >Santa Monica Brew Works inclined IPA</a
  >.
</footer>

<style>
  main {
    text-align: center;
    padding: 1rem;
    max-width: calc(240px + 50vw);
    margin: 0 auto auto auto;
  }

  footer {
    margin-top: auto;
  }

  h1 {
    color: var(--c-red);
    font-size: 4rem;
    font-weight: 100;
    margin: 1rem 0;
    text-align: center;
  }
</style>
