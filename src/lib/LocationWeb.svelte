<script lang="ts">
  // Componente de localização da web

  import { onMount } from "svelte";

  interface LocationData {
    label: string;
    value: string;
  }

  let locationData: LocationData[] = [];

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
</script>

<div>
  {#if locationData.length > 0}
    {#each locationData as { label, value }}
      <p class="font-sm p-1">
        <strong>{label}:</strong>
        {value}
      </p>
    {/each}
  {/if}
</div>
