<script>
  import spacetime from 'spacetime'
  import { days } from './stores'
  import { setContext } from 'svelte'
  import { onMount } from 'svelte'
  export let date = ''
  export let vertical = false

  // pre-work
  let today = spacetime.now()
  date = spacetime(date)
  let year = date.year()
  setContext('year', year)

  export let label = year

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
      let monthChange = week[0].month() !== week[week.length - 1].month()
      let clean = false
      if (week[0].date() === 1) {
        monthChange = true
        clean = true
      }
      let month = week[0].format('month')
      return week.map(d => {
        let inMonth = true
        if (monthChange && d.month() !== week[0].month()) {
          inMonth = false
        }
        if (clean) {
          inMonth = false
        }
        let noday = d.year() !== year
        let day = d.day()
        let iso = d.format('iso-short')
        let meta = $days[iso] || {}
        return {
          iso: iso,
          noday: noday,
          inMonth: inMonth,
          month: month,
          monthChange: monthChange,
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
    position: relative;
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
  .vertical {
    flex-direction: column !important;
  }
  .week {
    position: relative;
    flex: 1;
    display: flex;
    flex-direction: column-reverse;
  }
  .vertWeek {
    flex-direction: row;
  }
  .day {
    border-radius: 2px;
    flex: 1;
    box-shadow: 1px 1px 2px 0px rgba(0, 0, 0, 0.2);
    margin: 1px;
  }
  .square {
    width: 100%;
  }
  .square:after {
    content: '';
    display: block;
    padding-bottom: 100%;
  }
  .noday {
    flex: 1;
    box-shadow: none !important;
  }
  .weekend {
    /* background-color: #f5f5f5; */
    background-color: rgba(245, 245, 245, 0.6);
  }
  .label {
    font-size: 0.9rem;
  }
  /* horizontal */
  .twoMonthHor {
    padding-left: 15px;
  }
  .inMonthHor {
    position: relative;
    left: -15px;
  }
  /* vertical */
  .twoMonthVer {
    padding-top: 35px;
  }
  .inMonthVer {
    position: relative;
    top: -35px;
  }
  .line {
    font-size: 8px;
    color: steelblue;
    margin-bottom: 5px;
    box-shadow: 2px 2px 1px 0px steelblue;
    height: 1px;
  }
  .noline {
    font-size: 8px;
    margin-bottom: 5px;
    height: 1px;
    min-width: 1px;
  }
  /* medium */
  @media only screen and (max-width: 800px) {
    /* horizontal */
    .twoMonthHor {
      padding-left: 8px;
    }
    .inMonthHor {
      left: -8px;
    }
  }
  @media only screen and (max-width: 650px) {
    /* horizontal */
    .twoMonthHor {
      padding-left: 2px;
    }
    .inMonthHor {
      left: -2px;
    }
  }
  @media only screen and (max-width: 400px) {
    .day {
      border-radius: 2px;
      margin: 0px;
      box-shadow: 1px 1px 2px 0px rgba(0, 0, 0, 0.1);
    }
    /* vertical */
    .twoMonthVer {
      padding-top: 10px;
    }
    .inMonthVer {
      top: -10px;
    }
  }
</style>

<div class="label">{label}</div>
<div class="year" class:vertical>
  {#each weeks as week, i}
    <div
      class="week"
      class:vertWeek={vertical}
      class:twoMonthHor={!vertical && week[0].monthChange}
      class:twoMonthVer={vertical && week[0].monthChange}>
      {#each week as d}
        <div
          class="day square"
          class:weekend={d.weekend}
          class:noday={d.noday}
          class:inMonthHor={!vertical && d.monthChange && d.inMonth}
          class:inMonthVer={vertical && d.monthChange && d.inMonth}
          style="background-color:{d.color};"
          title={d.iso} />
      {/each}
      {#if i !== weeks.length - 1}
        <div class="line" />
      {:else}
        <div class="noline" />
      {/if}
    </div>
  {/each}
</div>
<slot />
