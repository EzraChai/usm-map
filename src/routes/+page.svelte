<script lang="ts">
	import {
		Control,
		ControlButton,
		GeoJSON,
		Marker,
		Popup,
		ControlGroup,
		LineLayer,
		MapLibre
	} from 'svelte-maplibre';

	import { busARoute, busBRoute, busCRoute, busDRoute, busERoute } from '$lib/index.js';

	let clickedName = $state('');
	let busRoute = $state<'' | 'A' | 'B' | 'C' | 'D' | 'E'>('');

	const routes = {
		A: { route: busARoute, color: '#ff6b6b' },
		B: { route: busBRoute, color: '#4ade80' },
		C: { route: busCRoute, color: '#60a5fa' },
		D: { route: busDRoute, color: '#facc15' },
		E: { route: busERoute, color: '#c084fc' }
	};

	const markers: Array<{
		lngLat: [number, number];
		name: string;
	}> = [
		{
			lngLat: [100.2925, 5.3564],
			name: 'Desasiswa Tekun'
		},
		{
			lngLat: [100.2887, 5.3566],
			name: 'Kafe Restu Saujana'
		},
		{
			lngLat: [100.289, 5.3553],
			name: 'Bus Stop Desasiswa Restu Saujana'
		},
		{
			lngLat: [100.29333471751437, 5.3564138776637265],
			name: 'Padang Kawad'
		},
		{
			lngLat: [100.29519498025712, 5.355836522440214],
			name: 'Desasiswa Indah Kembara - Padang Kawad'
		},
		{
			lngLat: [100.30050921032773, 5.356252304653339],
			name: 'Stor Pusat Kimia Universiti'
		},
		{
			lngLat: [100.30096485334201, 5.357164034858201],
			name: 'Desasiswa Bakti Fajar Permai Petas'
		},
		{
			lngLat: [100.3025164430502, 5.358815645492939],
			name: 'Pusat Sejahtera'
		},
		{
			lngLat: [100.297, 5.355],
			name: 'Desasiswa Aman Damai'
		},
		{
			lngLat: [100.29645706555931, 5.354341193712926],
			name: 'Aman Damai'
		},
		{
			lngLat: [100.2979734619704, 5.355059367602834],
			name: 'Jabatan Keselamatan'
		},
		{
			lngLat: [100.304, 5.355],
			name: 'PHS2'
		},
		{
			lngLat: [100.30267708758373, 5.354766270538676],
			name: 'Kompleks Eureka'
		},
		{
			lngLat: [100.3067, 5.3573],
			name: 'SOLLAT'
		},
		{
			lngLat: [100.3039996244243, 5.356634402877361],
			name: 'Dewan Budaya'
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
			lngLat: [100.30412963199535, 5.358016217530732],
			name: 'Kompleks Dewan Kuliah C23'
		},
		{
			lngLat: [100.30033349500121, 5.3547882386114765],
			name: 'Desasiswa Fajar Harapan'
		},
		{
			lngLat: [100.29879287365395, 5.355033026358754],
			name: 'Hadapan INFORMM'
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
		maxZoom={18}
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
				onclick={() => (clickedName = name)}
				class="grid h-8 w-8 place-items-center rounded-full border border-gray-200  text-black shadow-2xl focus:outline-2 focus:outline-black"
			>
				<span class="h-3 w-3 rounded-full bg-white"> </span>
				<Popup openOn="click" bind:open={open[i]} offset={[0, -10]}>
					<div class="text-lg font-bold">{name}</div>
				</Popup>
			</Marker>
		{/each}

		{#if busRoute != ''}
			<GeoJSON id="busRoute" data={routes[busRoute].route}>
				<LineLayer
					layout={{ 'line-cap': 'round', 'line-join': 'round' }}
					paint={{
						'line-width': 3,
						'line-color': routes[busRoute].color,
						'line-opacity': 0.8
					}}
				/>
			</GeoJSON>
		{/if}
		<Control class="flex flex-col gap-y-2">
			<ControlGroup>
				<ControlButton onclick={() => (busRoute = busRoute == 'A' ? '' : 'A')}>A</ControlButton>
				<ControlButton onclick={() => (busRoute = busRoute == 'B' ? '' : 'B')}>B</ControlButton>
				<ControlButton onclick={() => (busRoute = busRoute == 'C' ? '' : 'C')}>C</ControlButton>
				<ControlButton onclick={() => (busRoute = busRoute == 'D' ? '' : 'D')}>D</ControlButton>
				<ControlButton onclick={() => (busRoute = busRoute == 'E' ? '' : 'E')}>E</ControlButton>
			</ControlGroup>
		</Control>
	</MapLibre>
</div>

<style>
	:global(.map) {
		height: 100vh;
	}
</style>
