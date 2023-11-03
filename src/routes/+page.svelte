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
    return year * 365 * 24 * 60 * 60
  }

  /** @type {number} */
  var hours_drive;

  /** @type {number} */
  var life_expect;

  let today = new Date();

  /** @type {number} */
  const my_life_expect = 74.6;

  /** @type {number} */
  var age = today.getFullYear() - 2000 - ((today.getMonth() > 5) ? 0 : 1);
</script>

<main>

  <hgroup>
    <!--
    <p>It's important to remember</p>
    -->
    <p>please remember</p>
    <h1>We're all dying always</h1>
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
    <h1>You are wasting valuable time on earth</h1>
  </hgroup>

  I am currently {age} and my life expectancy {my_life_expect} years. 
  {#if age > my_life_expect}
    Therefore, I am probably dead.
  {:else}
    This means, that the average amount of time I loose by dying in a flight is {yr2sec((my_life_expect - age) / 2e7).toPrecision(4)} seconds. 

    Driving to Chicago from NYC takes 10 hours longer than flying. This means that by driving rather flying, whien I reach Chicago I will be {(yr2sec(10) - yr2sec((my_life_expect - age) / 2e7)).toPrecision(4)} seconds closer to dying.
  {/if}

  <hgroup>
    <p>These are facts</p>
    <h1>There are rarely exceptions</h1>
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

  hgroup h1 {
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

  h1 {
    font-size: 1.8rem;
  }
</style>
