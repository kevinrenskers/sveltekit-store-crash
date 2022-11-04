<script lang="ts">
  import {
    get,
    writable,
    readable,
    type Writable,
    type Readable,
  } from "svelte/store";
  import { browser } from "$app/environment";

  const locationsStore: Writable<{ id: number; name: string }[]> = writable();

  let locations: Readable<{ id: number; name: string }[]> = readable([]);

  async function fetchContent(
    store: Writable<{ id: number; name: string }[]>
  ): Promise<Readable<{ id: number; name: string }[]>> {
    if (browser && get(store)) {
      return store;
    }

    const fetchedValues = [{ id: 1, name: "Test" }];

    if (browser) {
      store.set(fetchedValues);
      return store;
    } else {
      return readable(fetchedValues);
    }
  }

  fetchContent(locationsStore).then(content => {
    locations = content;
  });
</script>

{#each $locations as location}
  {location.id}: {location.name}
{/each}
