<script>
  import { onMount, tick } from "svelte";
  import "leaflet/dist/leaflet.css";
  import Btn from "./button/Btn.svelte";
  import Input from "./input/Input.svelte";

  let map, L;
  let userLocation = [-6.92222, 107.60694];
  let loading = true;
  let angkotMarkers = [];
  let mapContainer; // Gunakan bind:this di elemen HTML nanti

  const handleFindTrayek = async () => {
    console.log("tes");
  };

  onMount(async () => {
    const leaflet = await import("leaflet");
    L = leaflet;
    loading = false;
    
    await tick(); // Tunggu hingga DOM ter-render

    if (!mapContainer) {
      console.error("Map container not found!");
      return;
    }

    map = L.map(mapContainer).setView(userLocation, 15);

    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      attribution: "&copy; OpenStreetMap contributors",
    }).addTo(map);

    // Tambahkan marker posisi user
    L.circle(userLocation, {
      radius: 200,
      color: "blue",
      fillOpacity: 0.3,
    }).addTo(map);
    L.marker(userLocation, {
      icon: L.divIcon({ className: "user-marker" }),
    }).addTo(map);

    // Simulasi angkot
    const angkotLocations = [
      [-6.92222, 107.60694],
      [-6.92322, 107.60894],
      [-6.92422, 107.61094],
    ];
    angkotLocations.forEach((loc) => {
      const marker = L.marker(loc, {
        icon: L.divIcon({ className: "angkot-marker" }),
      });
      marker.addTo(map);
      angkotMarkers.push(marker);
    });
  });
</script>

<div class="h-screen w-full bg-gray-100 flex flex-col">
  <div class="flex items-center justify-between px-4 py-3 bg-white shadow-md">
    <button class="p-2 bg-gray-200 rounded-full">&#9776;</button>
    <h1 class="text-xl font-semibold">Search Angkot</h1>
    <div class="w-6 h-6"></div>
  </div>

  <div class="flex-1 relative">
    <div bind:this={mapContainer} class="h-full w-full"></div>
  </div>

  <div class="bg-white shadow-md p-4 rounded-t-lg">
    <div class="flex justify-between items-center mb-2">
      <p class="text-gray-500">Searching for you on the map...</p>
      <span class="text-green-500">✔ Online</span>
    </div>
    <Input
      className="w-full p-2 border rounded-md mb-2"
      type="text"
      placeholder="From"
    />
    <Input
      className="w-full p-2 border rounded-md mb-2"
      type="text"
      placeholder="To"
    />
    <Btn className="w-full bg-green-500 text-white py-2 rounded-md" label="Find Trayek" onClick={handleFindTrayek}/>
  </div>
</div>
