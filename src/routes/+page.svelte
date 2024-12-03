<script>
  import Katex from 'svelte-katex'

  /**
   * @param {number} hour
   * @return {number}
   */
  function hr2yr(hour) {
    let year = hour / ( 24 * 365 )
    return year
  }

  /**
   * @param {number} year
   * @return {number}
   */
  function yr2sec(year) {
    return year * 365.25 * 24 * 60 * 60
  }

  /**
   * @param {number} sec
   * @return {number}
   */
  function sec2hr(sec) {
    return sec / ( 60 * 60 )
  }

  /** @type {number} */
  var hours_drive;

  /** @type {number} */
  var life_expect;

  let today = new Date();
  let birth = new Date('June 19, 2000');

  /** @type {number} */
  const my_life_expect = 74.6;

  /** @type {number} */
  var age = ( (today - birth) / ( 1000 * 60 * 60 * 24 * 365.25 ) ).toPrecision(4);
  // var age = today.getFullYear() - 2000 - ((today.getMonth() > 5) ? 0 : 1);
</script>

<main>

  <h1>Flight Safety Using Weighted Sums</h1>

  <hgroup>
    <!--
    <p>It's important to remember</p>
    -->
    <p>please remember</p>
    <h2>We're all dying always</h2>
  </hgroup>

  If there is one commercial plane crash every 20 million flights, and you've got <input placeholder="x" type="number" bind:value={life_expect} min="0" /> more years to live in general, we can find your average lifespan if you take this flight.

  <Katex displayMode>
    {#if life_expect}
      {
        "\\text{your average lifespan after taking this flight} = \\frac{(20 \\text{ million} - 1) *" 
        + String(life_expect) 
        + " + 0}{20 \\text{ million}}"
      }
    {:else}
      {"\\text{your average lifespan after taking this flight} = \\frac{(20 \\text{ million} - 1) * x + 0}{20 \\text{ million}}"}
    {/if}
  </Katex>

  20 million - 1 of you keep living for 
  {#if life_expect}
    {String(life_expect) + " more " + (life_expect == 1 ? " year" : " years")}
  {:else}
    <Katex>x</Katex> longer
  {/if}
  while 1 of you dies on the plane, living 0 more time.

  <br />
  <br />

  By subtracting this from your predicted lifespan, we can find the average amount of time it'll cost you to take this flight.

  <Katex displayMode>
    {#if life_expect}
      {
        "\\begin{align*}"
        + "\\text{Average time lost by dying in a flight} &= x - \\frac{20 \\text{ million}-1}{20 \\text{ million}}x \\newline"
        + "&= \\frac{1}{20 \\text{ million}} * " + String(life_expect) + "\\text{" + (life_expect == 1 ? " year" : " years") + "} \\newline"
        + "&= " + String( +yr2sec(life_expect / 2e7).toPrecision(4) ) + "\\text{ seconds}"
        + "\\end{align*}"
      }
    {:else}
      {
        "\\begin{align*}"
        + "\\text{Average time lost by dying in a flight} &= x - \\frac{20 \\text{ million}-1}{20 \\text{ million}}x \\newline"
        + "&= \\frac{1}{20 \\text{ million}}x"
        + "\\end{align*}"
      }
    {/if}
  </Katex>

  <hgroup>
    <p>By driving</p>
    <h2>You are wasting valuable time on earth</h2>
  </hgroup>

  I am currently {age} years old and my life expectancy is {my_life_expect} years. 
  {#if age > my_life_expect}
    Therefore, I am probably dead.
  {:else}
    So, I have roughly {(my_life_expect - age).toPrecision(4)} years left to live and the average amount of time I loose by dying in a flight each time I take one is {yr2sec((my_life_expect - age) / 2e7).toPrecision(4)} seconds. 
    <br />
    <br />
    Driving to Chicago from NYC takes 10 hours longer than flying. This means that if I drove, I would reach Chicago (on average) {sec2hr(10 * 60 * 60 - yr2sec((my_life_expect - age) / 2e7)).toPrecision(4)} hours closer to dying than if I had taken a flight ({(100 * (10 * 60 * 60 - yr2sec((my_life_expect - age) / 2e7)) / (today - birth) / 1000).toExponential(3)}% of my remaining life).
  {/if}

  <hgroup>
    <p>For driving to be worth it,</p>
    <h2>How long would you need to live?<h2>
  </hgroup>

  So if it takes <input placeholder="y" type="number" bind:value={hours_drive} min="0" /> more hours for you to drive to your destination, you would need to be planning to live to

  <Katex displayMode>
    {#if hours_drive}
      { String(+hr2yr(20000000 * hours_drive).toPrecision(5)) + "\\text{ years} = 20 \\text{ million} * " + String(hours_drive) + "\\text{ hours}" }
    {:else}
      {"x = 20 \\text{ million} * y"}
    {/if}
  </Katex>

  in order to have a greater average loss of time than the extra time driving

</main>

<style>
  main {
    display: block;
    margin-left: auto;
    margin-right: auto;

    width: 95%;
    max-width: 1006px
  }

  input {
    max-width: 50px;
  }

  hgroup {
    margin-top: 2rem;
    margin-bottom: 1rem;
  }

  hgroup h2 {
    margin-top: 0;
    margin-bottom: 0;
    text-transform: uppercase;
  }

  hgroup p {
    font-size: 1rem;
    margin-bottom: 0;
    text-transform: capitalize;
    font-weight: bold;
    color: red;
  }

  h2 {
    font-size: x-large;
  }

  h1 {
    font-size: xx-large;
    margin-top: 3rem;
    text-align: center;
  }
</style>
