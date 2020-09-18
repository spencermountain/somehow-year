<script>
  import spacetime from 'spacetime'
  import { days } from './stores'
  import { setContext } from 'svelte'
  import { onMount } from 'svelte'
  export let date = ''

  // pre-work
  let today = spacetime.now()
  date = spacetime(date)
  let year = date.year()
  setContext('year', year)

  // compute the year
  const calculate = function(date) {
    let start = date
      .startOf('year')
      .startOf('week')
      .minus(1, 'second')
    // get each day in the year, by week #
    let weeks = start.every('week', date.endOf('year'))
    weeks = weeks.map(mon => {
      let sun = mon.endOf('week').add(1, 'second')
      return mon.every('day', sun)
    })
    // render data
    return weeks.map(week => {
      return week.map(d => {
        let noday = d.year() !== year
        let day = d.day()
        let iso = d.format('iso-short')
        let meta = $days[iso] || {}
        return {
          iso: iso,
          noday: noday,
          today: d.isSame(today, 'day'),
          color: meta.color || 'none',
          weekend: !noday && (day === 0 || day === 1)
        }
      })
    })
  }

  let weeks = []
  onMount(() => {
    weeks = calculate(date)
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
    border-radius: 2px;
    flex: 1;
    box-shadow: 1px 1px 2px 0px rgba(0, 0, 0, 0.2);
    margin: 1px;
  }
  .noday {
    flex: 1;
    box-shadow: none;
  }
  .weekend {
    background-color: #f5f5f5;
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
          style="background-color:{d.color};"
          title={d.iso} />
      {/each}
    </div>
  {/each}
</div>
<slot />
