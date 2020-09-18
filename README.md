<div align="center">
  <div><b>somehow-year</b></div>
  <img src="https://user-images.githubusercontent.com/399657/68222691-6597f180-ffb9-11e9-8a32-a7f38aa8bded.png"/>
  <div>— part of <a href="https://github.com/spencermountain/somehow">somehow</a> —</div>
  <div>WIP svelte infographics</div>
  <div align="center">
    <sub>
      by
      <a href="https://spencermounta.in/">Spencer Kelly</a> 
    </sub>
  </div>
</div>
<div align="right">
  <a href="https://npmjs.org/package/somehow-year">
    <img src="https://img.shields.io/npm/v/somehow-year.svg?style=flat-square" />
  </a>
</div>
<img height="25px" src="https://user-images.githubusercontent.com/399657/68221862-17ceb980-ffb8-11e9-87d4-7b30b6488f16.png"/>

**work-in-progress**

renders html year-viz using [spacetime](https://github.com/spencermountain/spacetime) date library

<h4><a href="https://spencermounta.in/somehow-year/">demo:</a></h4>

`npm i somehow-year`

week-centered year from start to end dates, inclusive

```html
<script>
  import { Year, Day } from './src'
</script>

<Year date="2020">
  <Day date="march 28th" color="blue" />
</Year>
<Year date="2021">
  <Day date="march 28th" color="blue" />
</Year>
```

![image](https://user-images.githubusercontent.com/399657/93631446-9ad2e580-f9b9-11ea-8d55-d3ed0739e28b.png)

### Vertical

pass-in vertical={true}
![image](https://user-images.githubusercontent.com/399657/93631491-afaf7900-f9b9-11ea-96f7-b0a2ae394b52.png)

### API

- `<Year date="" vertical={false}/>`
- `<Day date="" color=""/>`

### See also:

- [somehow](https://github.com/spencermountain/somehow) - other svelte dataviz components
- [d3-time](https://github.com/d3/d3-time)
  MIT
