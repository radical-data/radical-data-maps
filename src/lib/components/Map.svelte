<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import maplibregl, { type StyleSpecification } from 'maplibre-gl';
	const { Map, addProtocol, NavigationControl, GeolocateControl } = maplibregl;
	import 'maplibre-gl/dist/maplibre-gl.css';
	import { Protocol } from 'pmtiles';
	import tileStyle from '$lib/tileStyle.json';

	let map: typeof Map.prototype;
	let mapContainer: HTMLDivElement;

	let protocol = new Protocol();
	addProtocol('pmtiles', protocol.tile);

	onMount(() => {
		const initialState = { lng: 0, lat: 0, zoom: 2 };

		map = new Map({
			container: mapContainer,
			style: tileStyle as StyleSpecification,
			center: [initialState.lng, initialState.lat],
			zoom: initialState.zoom,
			attributionControl: false,
			hash: true
		});

		map.addControl(new NavigationControl({ showCompass: false }), 'bottom-right');
		map.addControl(
			new GeolocateControl({
				positionOptions: {
					enableHighAccuracy: true
				},
				trackUserLocation: true
			}),
			'bottom-right'
		);
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
