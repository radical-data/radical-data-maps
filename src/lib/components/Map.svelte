<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import pkg from 'maplibre-gl';
	const { Map } = pkg;
	import 'maplibre-gl/dist/maplibre-gl.css';

	let map;
	let mapContainer: HTMLDivElement;

	onMount(() => {
		const initialState = { lng: 0, lat: 0, zoom: 2 };

		map = new Map({
			container: mapContainer,
			style: 'https://demotiles.maplibre.org/style.json',
			center: [initialState.lng, initialState.lat],
			zoom: initialState.zoom
		});
	});

	onDestroy(() => {
		if (map) {
			map.remove();
		}
	});
</script>

<div class="map" bind:this={mapContainer}></div>

<style>
	.map {
		position: absolute;
		width: 100%;
		height: 100%;
		margin: 0;
	}
</style>
