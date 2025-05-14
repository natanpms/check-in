<script lang="ts">
  import { Capacitor } from "@capacitor/core";
  import LocationWeb from "./lib/LocationWeb.svelte";

  const isNative: boolean = Capacitor.isNativePlatform();

  let locationData = [];
  let position: { lat: number; lng: number } | null = null;
</script>

<main>
  <div class="flex flex-col justify-center items-center min-h-screen">
    <h1 class="text-3xl font-bold">Check-In</h1>

    <div class="m-2 p-8 shadow-md w-4/5 rounded-md h-auto bg-white">
      <h2 class="text-2xl mb-4 text-center font-semibold ">Dados de Geolocalização:</h2>

      {#if isNative}
        <p>teste</p>
      {:else}
        <LocationWeb bind:locationData bind:position />
      {/if}

      {#if locationData.length > 0}
        {#each locationData as { label, value }}
          <p class="text-center m-2 font-sm p-1">
            <strong>{label}:</strong>
            {value}
          </p>
        {/each}
      {/if}

      {#if position}
        <iframe
          title="locationMap"
          width="100%"
          height="450"
          frameborder="0"
          scrolling="no"
          marginheight="0"
          marginwidth="0"
          src={"https://www.openstreetmap.org/export/embed.html?" +
            `bbox=${position.lng - 0.005},${position.lat - 0.005},${position.lng + 0.005},${position.lat + 0.005}` +
            `&marker=${position.lat},${position.lng}`}
          style="border: 1px solid black"
        ></iframe>
        <br />
        <small>
          <a class="font-semibold hover:underline"
            href={`https://www.openstreetmap.org/?mlat=${position.lat}&mlon=${position.lng}#map=15/${position.lat}/${position.lng}`}
            target="_blank"
          >
            Ver mapa maior
          </a>
        </small>
      {/if}
    </div>
  </div>
</main>
