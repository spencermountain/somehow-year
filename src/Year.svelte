<script>
  import spacetime from 'spacetime'
  export let date = ''
  date = spacetime(date)
  let start = date.startOf('year').minus(1, 'second')
  let weeks = start.every('week', date.endOf('year'))
  weeks = weeks.map(w => {
    return w.every('day', date.endOf('week'))
  })
</script>

<style>
  .row {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    text-align: center;
    flex-wrap: nowrap;
    align-self: stretch;
  }
  .col {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    text-align: center;
    flex-wrap: nowrap;
    align-self: stretch;
  }
  .day {
    width: 15px;
    height: 15px;
    border: 1px solid grey;
  }
</style>

<div class="row">
  {#each weeks as week}
    <div class="col">
      {#each week as d}
        <div class="day">
          <slot />
        </div>
      {/each}
    </div>
  {/each}
</div>
