<script>
  import { Year, Day } from './src'
</script>

<style>
  .container {
    margin: 50px;
  }
  .m3 {
    margin: 3rem;
  }
</style>

<div>
  <div class="m3">
    <a href="https://github.com/spencermountain/somehow-year">somehow-year</a>
    <span class="f08 grey ">
      - its a svelte html year-infographic component using spacetime.
    </span>
  </div>
  <div class="container">
    <Year date="march 2020" vertical={false}>
      <Day date="march 28th" />
    </Year>
    <div class="m3" />
    <Year date="march 2021">
      <Day date="march 28th" />
    </Year>
  </div>
</div>
