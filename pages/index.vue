<template>
	<div class="stage"/>
	<div class="landscape-disabled" v-show="isAppInPortraitMode"/>
</template>

<script setup lang="ts">
import * as PIXI from 'pixi.js';

let app;
let isAppInPortraitMode = ref(false);

function initPixi()
{
	app = new PIXI.Application({
		backgroundColor: 0x1099bb,
		antialias: true,
		resolution: 1
	});
	
	app.renderer.resize(window.innerWidth, window.innerHeight - 4);
	document.querySelector('.stage').appendChild(app.view);
}

function onWindowResize()
{
	const threshold: number = 300;
	app.renderer.resize(window.innerWidth, window.innerHeight - 4);
	isAppInPortraitMode.value = window.innerHeight > (window.innerWidth - threshold);
}

function defineWindowHooks()
{
	window.addEventListener('resize', onWindowResize);
	onWindowResize();
}

onMounted(() => {
	initPixi();
	defineWindowHooks();
});
onUnmounted(() =>
{
	app.destroy(true);
	window.removeEventListener('resize', onWindowResize);
});
</script>
