<script lang="ts">
  import { onMount } from "svelte";
  import { Capacitor } from "@capacitor/core";

  interface LocationData {
    label: string;
    value: string;
  }

  let locationData: LocationData[] = [];

  // Verifica se está em uma plataforma nativa
  const isNative: boolean = Capacitor.isNativePlatform();

  if (isNative) {
    // Aqui você usaria as APIs do Capacitor (ex: Geolocation Plugin)
    console.log("API do Capacitor");
  } else {
    if ("geolocation" in navigator) {
      console.log("Geolocalização disponível");

      function getLocation() {
        if (navigator.geolocation) {
          navigator.geolocation.getCurrentPosition(
            (position) => {
              locationData = [
                {
                  label: "Latitude",
                  value: position.coords.latitude.toString(),
                },
                {
                  label: "Longitude",
                  value: position.coords.longitude.toString(),
                },
                { label: "Precisão", value: `${position.coords.accuracy}  m` },
                {
                  label: "Velocidade",
                  value: (position.coords.speed ?? "0").toString(),
                },
              ];
            },
            (error) => {
              console.error(error);
            },
          );
        } else {
          alert("Geolocalização não é suportada neste navegador.");
        }
      }

      // Chama a função de geolocalização quando o componente é montado
      onMount(() => {
        getLocation();
      });
    } else {
      alert(
        "I'm sorry, but geolocation services are not supported by your browser.",
      );
    }
  }
</script>

<main>
  <div class="flex flex-col justify-center items-center min-h-screen">
    <h1 class="text-3xl font-bold">Check-In</h1>
    <!-- card -->
    <div class="m-2 p-8 shadow-md w-2/3 rounded-md h-auto bg-white">
      <h2 class="text-2xl font-semibold text-left">Dados de Geolocalização:</h2>
      {#if locationData.length > 0}
        <!-- Verifica se há dados de localização -->
        {#each locationData as { label, value }}
          <p class="font-sm p-1">
            <strong>{label}:</strong>
            {value}
          </p>
        {/each}
      {/if}
      Lorem ipsum dolor, sit amet consectetur adipisicing elit. Autem ipsum expedita labore aliquid officia cum dicta illum, blanditiis sed delectus, quas sit minus reiciendis commodi cumque perspiciatis similique assumenda voluptatum.

      <!-- <button class="btn btn-accent">Tirar foto</button> -->

      <video id="camera" width="320" height="240" autoplay style="display:none;"
      ></video>
      <button class="btn btn-accent captureButton">Capturar Foto</button>
      <canvas id="canvas" style="display:none;"></canvas>
      <img id="photoPreview" alt="Preview da foto" />
    </div>
  </div>
</main>
