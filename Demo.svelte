<script>
  import { Year, Day } from './src'
</script>

<style>
  .container {
    max-width: 1200px;
  }
</style>

<div>
  <div>
    <a href="https://github.com/spencermountain/somehow-year">somehow-year</a>
    <span class="f08 grey">
      - its a svelte html year-infographic component using spacetime.
    </span>
  </div>
  <div class="container">
    <Year date="march 2020">
      <Day date="march 28th" />
    </Year>
  </div>
</div>
