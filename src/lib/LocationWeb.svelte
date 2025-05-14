<script lang="ts">
  import { onMount } from "svelte";

  export let position: { lat: number; lng: number } | null = null;
  export let locationData: Array<{ label: string; value: string }> = [];

  function getLocation() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (pos) => {
          const { latitude, longitude, accuracy, speed } = pos.coords;

          locationData = [
            { label: "Latitude", value: latitude.toString() },
            { label: "Longitude", value: longitude.toString() },
            { label: "Precisão", value: `${accuracy} m` },
            { label: "Velocidade", value: (speed ?? "0").toString() },
          ];

          position = {
            lat: latitude,
            lng: longitude,
          };
        },
        (error) => {
          alert(error);
        }
      );
    }
  }

  onMount(() => {
    getLocation();
  });
</script>