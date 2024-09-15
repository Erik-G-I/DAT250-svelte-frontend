<script>
  import { createEventDispatcher } from "svelte";
  import Poll from "./poll.svelte";
  import User from "./user.svelte";

  const dispatch = createEventDispatcher();

  let count = 0;

  function handleClick() {
    count = count + 1;
  }
</script>

<main>
  <User />
  <Poll />
</main>
