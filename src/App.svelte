<script lang="ts">
  import { Capacitor } from "@capacitor/core";
  import LocationWeb from "./lib/LocationWeb.svelte";

  interface LocationData {
    label: string;
    value: string;
  }

  let locationData: LocationData[] = [];

  // Verifica se está em uma plataforma nativa
  const isNative: boolean = Capacitor.isNativePlatform();

 
</script>

<main>
  <div class="flex flex-col justify-center items-center min-h-screen">
    <h1 class="text-3xl font-bold">Check-In</h1>
    <!-- card -->
    <div class="m-2 p-8 shadow-md w-2/3 rounded-md h-auto bg-white">
      <h2 class="text-2xl font-semibold text-left">Dados de Geolocalização:</h2>

      {#if isNative}
        <p>teste</p>
      {:else}
        <LocationWeb />
      {/if}
      

      <button class="btn btn-accent captureButton">Capturar Foto</button>
      <canvas id="canvas" style="display:none;"></canvas>
      <img id="photoPreview" alt="Preview da foto" />
    </div>
  </div>
</main>
