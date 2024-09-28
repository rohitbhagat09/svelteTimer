<script>
  import Counter from "./Counter.svelte";
  import welcome from "$lib/images/svelte-welcome.webp";
  import welcome_fallback from "$lib/images/svelte-welcome.png";
  import BreakTimer from "./BreakTimer.svelte";
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
  <h1>
    <span class="welcome">
      <picture>
        <source srcset={welcome} type="image/webp" />
        <img src={welcome_fallback} alt="Welcome" />
      </picture>
    </span>

    Pomodoro Timer
  </h1>

  <h2>Todo task</h2>

  <Counter />
</section>
<div class="break-timer">
  <h1 class="breakHeading">Break Timer</h1>
  <BreakTimer />
</div>

<style>
  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 0.6;
    padding: 0em 5em;
    /* max-width: 50rem; */
  }

  h1 {
    width: 100%;
  }
  h2 {
    font-size: 1.5em;
  }
  .breakHeading {
    font-size: 1.5em;
  }

  .welcome {
    display: block;
    position: relative;
    width: 100%;
    height: 0;
    padding: 0 0 calc(100% * 495 / 2048) 0;
  }

  .welcome img {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    display: block;
  }

  .break-timer {
    position: absolute;
    display: flex;
    flex-direction: column;
    right: 3em;
    bottom: 5em;
  }
</style>
