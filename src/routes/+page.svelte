<script lang="ts">
  import { fade } from "svelte/transition";
  import { cubicIn } from "svelte/easing";
  const date = new Date("01/04/2024 20:00:00");
  let now = new Date();
  setInterval(() => {
    now = new Date();
  }, 500);

  const config = {
    seconds: { color: "#FFFFFF", icon: 127789 },
    minutes: { color: "#D94194", icon: 127835 },
    hours: { color: "#4D728F", icon: 127840 },
    days: { color: "#BFA454", icon: 129368 },
    weeks: { color: "#BF6854", icon: 129365 },
  } as const;

  $: diff = date.getTime() - now.getTime();

  $: weeks = Math.floor(diff / (1000 * 60 * 60 * 24 * 7));
  $: days = Math.floor(
    (diff - weeks * 1000 * 60 * 60 * 24 * 7) / (1000 * 60 * 60 * 24),
  );
  $: hours = Math.floor(
    (diff - weeks * 1000 * 60 * 60 * 24 * 7 - days * 1000 * 60 * 60 * 24) /
      (1000 * 60 * 60),
  );
  $: minutes = Math.floor(
    (diff -
      weeks * 1000 * 60 * 60 * 24 * 7 -
      days * 1000 * 60 * 60 * 24 -
      hours * 1000 * 60 * 60) /
      (1000 * 60),
  );
  $: seconds = Math.floor(
    (diff -
      weeks * 1000 * 60 * 60 * 24 * 7 -
      days * 1000 * 60 * 60 * 24 -
      hours * 1000 * 60 * 60 -
      minutes * 1000 * 60) /
      1000,
  );

  $: dotCounts = (
    diff > 0
      ? [
          new Array(weeks).fill("weeks"),
          new Array(days).fill("days"),
          new Array(hours).fill("hours"),
          new Array(minutes).fill("minutes"),
          new Array(seconds).fill("seconds"),
        ]
      : []
  ) as ("seconds" | "minutes" | "hours" | "days" | "weeks")[][];
</script>

{#if diff < 0}
  <div class="text-center w-full h-screen flex justify-center items-center">
    <h1 class="text-4xl text-white font-extralight tracking-tight">
      Merry Christmas
    </h1>
  </div>
{:else}
  <div class="grid grid-cols-4 mx-auto gap-4 gap-y-14 w-80 my-12">
    {#each dotCounts as unit}
      {#each unit as dot, dotI}
        <div
          style={`color: ${config[dot].color}`}
          class="flex flex-col items-center justify-center"
          transition:fade={{ duration: 300, easing: cubicIn }}
        >
          <div class=" text-8xl text-center font-thin" style="line-height: 0;">
            *
          </div>
          <span>{dotI + 1}</span>
        </div>
      {/each}
    {/each}
  </div>
{/if}
