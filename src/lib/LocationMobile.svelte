<script lang="ts">
    import { onMount } from "svelte";
    import { Geolocation } from "@capacitor/geolocation";

    export let position: { lat: number; lng: number } | null = null;
    export let locationData: Array<{ label: string; value: string }> = [];

    async function getLocation() {
        const permissionStatus = await Geolocation.checkPermissions();

        if (permissionStatus.location !== "granted") {
            const requestStatus = await Geolocation.requestPermissions();

            if (requestStatus.location !== "granted") {
                alert("Permissão de localização negada pelo usuário");
            }
        }

        // Obtém a posição atual
        const coord = await Geolocation.getCurrentPosition();
        let {latitude, longitude, accuracy, speed} = coord.coords;
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

        console.log("capacitor:", coord);
     
    }

    onMount(() => {
        getLocation();
    });
</script>
