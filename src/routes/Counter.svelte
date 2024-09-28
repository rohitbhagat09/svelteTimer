<script>
  let hours = 0;
  let minutes = 0;
  let seconds = 0;

  let timerInterval; // Store the interval ID for the countdown timer
  let isRunning = false; // Track whether the timer is running
  let isTimerFinished = false; // Track when the timer reaches zero

  // Scroll event handler to adjust hours, minutes, and seconds
  function handleScroll(event, unit) {
    event.preventDefault();
    if (isRunning) return;

    if (event.deltaY > 0) {
      // Scroll down
      if (unit === "hours") hours = (hours + 1) % 24;
      if (unit === "minutes") minutes = (minutes + 1) % 60;
      if (unit === "seconds") seconds = (seconds + 1) % 60;
    } else {
      // Scroll up
      if (unit === "hours") hours = (hours - 1 + 24) % 24;
      if (unit === "minutes") minutes = (minutes - 1 + 60) % 60;
      if (unit === "seconds") seconds = (seconds - 1 + 60) % 60;
    }
  }

  
  // Start or stop the countdown timer
  function startCountdown() {
    if (isRunning) {
      clearInterval(timerInterval);
      isRunning = false;
    } else {
      if (hours === 0 && minutes === 0 && seconds === 0) return; // Don't start if the timer is set to zero

      isRunning = true;
      isTimerFinished = false;
      timerInterval = setInterval(() => {
        if (seconds > 0) {
          seconds--;
        } else if (minutes > 0) {
          minutes--;
          seconds = 59;
        } else if (hours > 0) {
          hours--;
          minutes = 59;
          seconds = 59;
        } else {
          // Timer reached zero
          clearInterval(timerInterval);
          isRunning = false;
          isTimerFinished = true;
        }
      }, 1000);
    }
  }

  // Reset the timer values and stop countdown
  function resetTimer() {
    clearInterval(timerInterval);
    isRunning = false;
    isTimerFinished = false;
    hours = 0;
    minutes = 0;
    seconds = 0;
  }
</script>

<div class="time-picker">
  <div class="time-unit" on:wheel={(e) => handleScroll(e, "hours")}>
    <strong>{String(hours).padStart(2, "0")}</strong>
    <span class="unit-label">HH</span>
  </div>

  <div class="separator">:</div>

  <div class="time-unit" on:wheel={(e) => handleScroll(e, "minutes")}>
    <strong>{String(minutes).padStart(2, "0")}</strong>
    <span class="unit-label">MM</span>
  </div>
  <div class="separator">:</div>
  <div class="time-unit" on:wheel={(e) => handleScroll(e, "seconds")}>
    <strong>{String(seconds).padStart(2, "0")}</strong>
    <span class="unit-label">SS</span>
  </div>
</div>

<!-- Timer Control Buttons -->
<div class="controls">
  <button on:click={startCountdown}>
    {isRunning ? "Stop Timer" : "Start Countdown"}
  </button>
  <button on:click={resetTimer} disabled={isRunning}>Reset</button>
</div>

<!-- Show when the countdown is complete -->
{#if isTimerFinished}
  <div class="finished-message">
    <p>Time's up!</p>
  </div>
{/if}

<style>
  .time-picker {
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 1rem 0;
  }

  .time-unit {
    position: relative;
    width: 3em;
    height: 3em;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    font-weight: 400;
    color: var(--color-theme-1);
    background: var(--color-bg-1);
    border: 1px solid var(--color-border);
    border-radius: 8px;
    cursor: pointer;
    overflow: hidden;
    transition: transform 0.3s ease-out;
  }

  .separator {
    font-size: 3rem;
    font-weight: 600;
    color: var(--color-theme-2);
    margin: 0 0.5em;
  }

  .time-unit span,
  .time-unit strong {
    display: block;
    width: 100%;
    text-align: center;
    padding: 0.5em 0;
  }

  .time-unit strong {
    font-size: 3rem;
    font-weight: 600;
  }

  .time-unit span {
    color: var(--color-text-light);
  }

  .unit-label {
    font-size: 0.75rem;
    color: var(--color-text-muted);
    position: absolute;
    bottom: 0.5em;
  }

  .time-unit:hover {
    background: var(--color-bg-2);
  }

  .controls {
    display: flex;
    justify-content: center;
    margin-top: 1rem;
  }

  .controls button {
    margin: 0 1rem;
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
    color: white;
    background: var(--color-theme-1);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease;
  }

  .controls button:disabled {
    background: var(--color-disabled);
	color: red;
    cursor: not-allowed;
  }

  .controls button:hover:not(:disabled) {
    background: var(--color-theme-2);
  }

  .finished-message {
    text-align: center;
    margin-top: 1rem;
    font-size: 1.5rem;
    color: var(--color-success);
  }
</style>
