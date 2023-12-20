<script lang="ts">
  import { fade } from "svelte/transition";
  import { cubicIn } from "svelte/easing";
  import FoodApple from "svelte-material-icons/FoodApple.svelte";
  import Hamburger from "svelte-material-icons/Hamburger.svelte";
  import Noodles from "svelte-material-icons/Noodles.svelte";
  import HotDog from "svelte-material-icons/FoodHotDog.svelte";
  import Cups from "svelte-material-icons/FoodTurkey.svelte";

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

  const Thing = FoodApple;

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

  $: dotCounts = [
    new Array(weeks).fill("weeks"),
    new Array(days).fill("days"),
    new Array(hours).fill("hours"),
    new Array(minutes).fill("minutes"),
    new Array(seconds).fill("seconds"),
  ] as ("seconds" | "minutes" | "hours" | "days" | "weeks")[][];
</script>

<div class="flex flex-wrap items-center gap-12 mx-auto my-12 w-96">
  {#each dotCounts as unit}
    {#each unit as dot, dotI}
      <div
        class="flex flex-col items-center justify-center"
        transition:fade={{ duration: 300, easing: cubicIn }}
      >
        <div
          class="flex flex-col text-center"
          style={`color: ${config[dot].color}`}
        >
          <div
            class=" text-8xl text-center font-thin leading-"
            style="line-height: 0;"
          >
            *
            <!-- {@html "&#" + config[dot].icon + ";"} -->
          </div>
          <span>{dotI + 1}</span>
        </div>
      </div>
    {/each}
  {/each}
</div>
