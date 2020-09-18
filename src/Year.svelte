<script>
  import spacetime from 'spacetime'
  export let date = ''
  date = spacetime(date)
  let year = date.year()
  let start = date
    .startOf('year')
    .startOf('week')
    .minus(1, 'second')
  let weeks = start.every('week', date.endOf('year'))
  weeks = weeks.map(mon => {
    let sun = mon.endOf('week').add(1, 'second')
    return mon.every('day', sun)
  })

  let today = spacetime.now()
  weeks = weeks.map(week => {
    return week.map(d => {
      let noday = d.year() !== year
      let day = d.day()
      return {
        iso: d.format('iso-short'),
        noday: noday,
        today: d.isSame(today, 'day'),
        weekend: !noday && (day === 0 || day === 1)
      }
    })
  })
</script>

<style>
  .year {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: stretch;
    text-align: center;
    flex-wrap: nowrap;
    align-self: stretch;
    height: 100%;
    min-height: 100px;
  }
  .day {
    flex: 1;
    box-shadow: 1px 1px 2px 0px rgba(0, 0, 0, 0.2);
    margin: 1px;
  }
  .noday {
    flex: 1;
    box-shadow: none;
  }
  .weekend {
    background-color: #f7f7f7;
    opacity: 0.5;
  }
  .week {
    flex: 1;
    display: flex;
    flex-direction: column-reverse;
  }
  .label {
    font-size: 0.9rem;
  }
</style>

<div class="label">{year}</div>
<div class="year">
  {#each weeks as week}
    <div class="week">
      {#each week as d}
        <div
          class="day"
          class:weekend={d.weekend}
          class:noday={d.noday}
          title={d.iso} />
      {/each}
    </div>
  {/each}
</div>
