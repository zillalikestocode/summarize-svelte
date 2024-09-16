<script lang="ts">
  import { Globe, Loader2 } from "lucide-svelte";
  import snarkdown from "snarkdown";

  let url = "";
  let loading = false;
  let summary: string;

  async function summarize() {
    loading = true;
    const response = await fetch(
      "http://qw4g8soscwcsw44sscgoow4s.135.148.139.0.sslip.io/api/v1/summary",
      {
        method: "POST",
        body: JSON.stringify({ url }),
        headers: { "Content-type": "application/json" },
      },
    ).then((res) => res.json());
    console.log(response);
    summary = response.summary[0];
    loading = false;
  }
</script>

<svelte:head>
  <meta
    http-equiv="Content-Security-Policy"
    content="upgrade-insecure-requests"
  />
</svelte:head>

<div class="flex min-h-screen p-5 flex-col justify-center items-center">
  <div class="flex w-full md:w-72 flex-col items-center">
    <h4 class="font-['Azeret_Mono'] mb-2.5 font-bold text-2xl">summarize.</h4>
    <div class=" w-full flex items-center gap-2 border rounded-xl p-2.5">
      <Globe size={20} />
      <input
        bind:value={url}
        type="text"
        placeholder="Enter Link"
        class="w-full bg-transparent focus:ring-0 focus:outline-0"
      />
    </div>
    <button
      on:click={summarize}
      class="font-semibold bg-[oklch(45.7784%_0.283496_273.786971_/1)] text-white p-5 py-2.5 rounded-full w-full mt-2.5"
      >summarize</button
    >
  </div>
  {#if loading}
    <Loader2 class="animate-spin mt-10" />
  {:else if summary}
    <div class="border rounded-xl p-4 mt-5 text-sm">
      {@html snarkdown(summary)}
    </div>
  {/if}
</div>
