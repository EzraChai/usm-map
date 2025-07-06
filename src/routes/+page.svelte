<script>
	import { MapLibre } from 'svelte-maplibre';
	// @ts-ignore
	import Marker from 'svelte-maplibre/Marker.svelte';
	// @ts-ignore
	import Popup from 'svelte-maplibre/Popup.svelte';

	let clickedName = $state('');

	const markers = [
		{
			lngLat: [100.2925, 5.3564],
			name: 'Tekun'
		},
		{
			lngLat: [100.2887, 5.3566],
			name: 'Kafe Restu Saujana'
		},
		{
			lngLat: [100.289, 5.3553],
			name: 'Bus Stop Restu Saujana'
		},
		{
			lngLat: [100.29335, 5.3565],
			name: 'Padang Kawad'
		},
		{
			lngLat: [100.2951, 5.3558],
			name: 'Indah Kembara'
		},
		{
			lngLat: [100.3004, 5.356],
			name: 'Stor Kimia'
		},
		{
			lngLat: [100.301, 5.3572],
			name: 'Bakti'
		},
		{
			lngLat: [100.3026, 5.3588],
			name: 'Pusat Sejahtera'
		},
		{
			lngLat: [100.2972, 5.355],
			name: 'Aman Damai'
		},
		{
			lngLat: [100.2965, 5.3543],
			name: 'Damai'
		},
		{
			lngLat: [100.2979, 5.355],
			name: 'JK'
		},
		{
			lngLat: [100.304, 5.355],
			name: 'PHS2'
		},
		{
			lngLat: [100.3027, 5.3548],
			name: 'Eureka'
		},
		{
			lngLat: [100.3067, 5.3573],
			name: 'SOLLAT'
		},
		{
			lngLat: [100.304, 5.3567],
			name: 'Subaidah'
		},
		{
			lngLat: [100.3078, 5.3566],
			name: 'GSB'
		},
		{
			lngLat: [100.3068, 5.3551],
			name: 'HBP'
		},
		{
			lngLat: [100.3041, 5.358],
			name: 'DKSK'
		},
		{
			lngLat: [100.3004, 5.3548],
			name: 'FAJAR'
		}
	];

	let open = $state(markers.map(() => false));
</script>

<div class=" min-h-screen">
	<MapLibre
		center={[100.2971, 5.3573]}
		zoom={14.9}
		class="map"
		standardControls
		maxZoom={16}
		minZoom={14.9}
		maxBounds={[
			[100.2871, 5.3473], // Southwest corner
			[100.3102, 5.3673] // Northeast corner
		]}
		style="https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json"
	>
		{#each markers as { lngLat, name }, i (name)}
			<Marker
				{lngLat}
				anchor="center"
				on:click={() => (clickedName = name)}
				class="grid h-8 w-8 place-items-center rounded-full border border-gray-200  text-black shadow-2xl focus:outline-2 focus:outline-black"
			>
				<span class="h-3 w-3 rounded-full bg-white"> </span>
				<Popup openOn="click" bind:open={open[i]} offset={[0, -10]}>
					<div class="text-lg font-bold">{name}</div>
				</Popup>
			</Marker>
		{/each}
	</MapLibre>
</div>

<style>
	:global(.map) {
		height: 100vh;
	}
</style>
