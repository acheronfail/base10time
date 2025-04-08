<script lang="ts">
  const b10HourTickCount = 10;
  const b10MinuteTickCount = 100;
  const b10SecondTickCount = 100;

  const minutesPerDay = 1440;
  const minutesPerB10Hour = minutesPerDay / b10HourTickCount;
  const secondsPerB10Second = 60 / b10SecondTickCount;

  let b10Hour = $state(0);
  let b10Minute = $state(0);
  let b10Second = $state(0);

  setInterval(() => {
    const now = new Date();
    const minutes = now.getHours() * 60 + now.getMinutes();
    b10Hour = minutes / minutesPerB10Hour;
    b10Minute = ((minutes % minutesPerB10Hour) * 100) / minutesPerB10Hour;
    b10Second = (now.getSeconds() + now.getMilliseconds() / 1000) / secondsPerB10Second;
  }, secondsPerB10Second * 1000);
</script>

<main>
  <header class="text-center text-[2rem]">
    <h1 class="font-bold">Base 10 Time!</h1>
    <p class="text-gray-500">
      <em>
        It's the future. Start using it now!
      </em>
    </p>
  </header>

  <div class="flex w-full items-center justify-center font-mono">
    <svg
      class="w-1/2 h-1/2"
      viewBox="0 0 100 100"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
    >
      <circle cx="50" cy="50" r="45" stroke="black" fill="#eee" />
      <circle cx="50" cy="50" r="2" fill="black" />
      <!-- text in the center -->
      <text x="50" y="33" text-anchor="middle" font-size="3" class="fill-gray-500" transform="translate(0 2)">
        Base 10 Analog
      </text>
      <!-- hour ticks -->
      {#each Array(b10HourTickCount) as _, i}
        <line
          x1="50"
          y1="5"
          x2="50"
          y2="10"
          stroke="black"
          stroke-width="2"
          transform="rotate({(i * 360) / b10HourTickCount} 50 50)"
        />
        <text
          x="50"
          y="14"
          text-anchor="middle"
          font-size="4"
          transform="
          rotate({(i * 360) / b10HourTickCount} 50 50)
          rotate({-(i * 360) / b10HourTickCount} 50 14)
        "
          fill="black"
        >
          {i}
        </text>
      {/each}
      <!-- minute ticks -->
      {#each Array(b10MinuteTickCount) as _, i}
        <line
          x1="50"
          y1="5"
          x2="50"
          y2="7"
          stroke="black"
          stroke-width="1.5"
          transform="rotate({(i * 360) / b10MinuteTickCount} 50 50)"
        />
      {/each}
      <!-- second -->
      <line
        x1="50"
        y1="50"
        x2="50"
        y2="15"
        stroke="black"
        stroke-width="0.5"
        transform="rotate({(Math.floor(b10Second) * 360) / b10SecondTickCount} 50 50)"
      />
      <!-- minute -->
      <line
        x1="50"
        y1="50"
        x2="50"
        y2="15"
        stroke="black"
        stroke-width="1"
        transform="rotate({(b10Minute * 360) / b10MinuteTickCount} 50 50)"
      />
      <!-- hour -->
      <line
        x1="50"
        y1="50"
        x2="75"
        y2="50"
        stroke="black"
        stroke-width="1.5"
        transform="rotate({(b10Hour * 360) / b10HourTickCount - 90} 50 50)"
      />
    </svg>

    <div class="flex flex-col items-center font-mono">
      <p class="text-lg text-gray-500">Base 10 Digital</p>
      <p class=" text-[2rem] font-bold">
        {@render renderNumber(b10Hour)}:{@render renderNumber(b10Minute)}:{@render renderNumber(b10Second)}
      </p>
    </div>
  </div>
</main>

{#snippet renderNumber(n: number)}
  {#if Math.floor(n) == 0}
    <span class="text-gray-400">00</span>
  {:else if Math.floor(n) < 10}
    <span class="text-gray-400">0</span>{Math.floor(n)}
  {:else}
    {Math.floor(n)}
  {/if}
{/snippet}
