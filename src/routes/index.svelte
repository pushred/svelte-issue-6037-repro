<script>
  import { tick } from "svelte";

  let isModalOpen = false;
  let hasWrapper = true;

  const embedHtml = `
    <blockquote class="twitter-tweet">
      <p lang="en" dir="ltr">
        we are all muppets now, but, like, OFFICIALLY <a
          href="https://t.co/FX9Ir76yGi"
        >
          pic.twitter.com/FX9Ir76yGi
        </a>
      </p>
      &mdash; Svelte Society 🧡 (@SvelteSociety)
      <a
        href="https://twitter.com/SvelteSociety/status/1386318870378532865?ref_src=twsrc%5Etfw"
      >
        April 25, 2021
      </a>
    </blockquote>
  `;

  async function handleModalOpen() {
    isModalOpen = true;

    await tick();

    if (typeof twttr?.widgets?.load === "function") {
      twttr.widgets.load();
    }
  }

  function handleModalClose() {
    isModalOpen = false;
  }
</script>

<svelte:head>
  <script async src="https://platform.twitter.com/widgets.js"></script>
</svelte:head>

<label>
  <input type="checkbox" bind:checked={hasWrapper} />
  Wrap
  <code>@html</code>
  in a div
</label>

<hr />

{#if isModalOpen && hasWrapper}
  <div>
    {@html embedHtml}
  </div>
  <button on:click={handleModalClose}>Close</button>
{:else if isModalOpen}
  {@html embedHtml}
  <button on:click={handleModalClose}>Close</button>
{:else}
  <button on:click={handleModalOpen}>Open Modal</button>
{/if}
