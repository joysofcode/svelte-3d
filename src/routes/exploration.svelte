<script lang="ts">
	import { onDestroy, onMount } from 'svelte'
	import { spring, tweened } from 'svelte/motion'
	import { DEG2RAD } from 'three/src/math/MathUtils'
	import {
		AmbientLight,
		Canvas,
		HemisphereLight,
		OrbitControls,
		Pass,
		PerspectiveCamera,
		PointLight,
		TransformControls,
	} from '@threlte/core'
	import { GLTF } from '@threlte/extras'

	// import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass'

	let posY: number
	let scale = tweened(0.01)

	const m = { x: 0, y: 0 }

	function mapRange(value, a, b, c, d) {
		value = (value - a) / (b - a)
		return c + value * (d - c)
	}

	function mousey(event) {
		m.x = mapRange(event.clientX, 0, 640, 0, 360)
		m.y = mapRange(event.clientY, 0, 792, 0, 360)
	}

	function tick() {
		posY = Math.sin(Date.now() / 1000) + 1
		requestAnimationFrame(tick)
	}

	onMount(() => tick())
</script>

<svelte:head>
	<title>Threlte</title>
</svelte:head>

<svelte:window on:mousemove={mousey} />

<div class="scene">
	<Canvas>
		<PerspectiveCamera position={{ x: 10, y: 10 }} fov={50}>
			<!-- <OrbitControls autoRotate autoRotateSpeed={4} /> -->
			<OrbitControls />
		</PerspectiveCamera>

		<HemisphereLight skyColor="orange" groundColor="gray" intensity={0.6} />
		<AmbientLight intensity={0.2} />

		<!-- <PointLight color="blue" position={{ x: 10, y: 10, z: 10 }} />
		<PointLight color="orange" position={{ x: -10, y: 10, z: 0 }} /> -->

		<GLTF
			url={'models/halloween_pumpkin.glb'}
			on:pointerenter={() => {
				$scale = 0.014
			}}
			on:pointerleave={() => {
				$scale = 0.01
			}}
			interactive
			position={{ y: posY }}
			rotation={{ y: DEG2RAD * 90 }}
			scale={$scale}
		/>

		<!-- position={{ y: posY }} -->
		<!-- rotation={{ y: (45 * Math.PI) / 2 }} -->

		<!-- <Mesh
			castShadow
			geometry={new BoxGeometry($width, $height, $depth)}
			material={new MeshStandardMaterial({ color: 'aqua' })}
			position={{ y: posY }}
		>
			<TransformControls />
		</Mesh> -->

		<!-- <Mesh
			receiveShadow
			geometry={new PlaneGeometry(4, 4)}
			material={new MeshStandardMaterial({ color: 'white', side: DoubleSide })}
			rotation={{ x: Math.PI / 2 }}
			position={{ y: -0.5 }}
		/> -->
	</Canvas>
</div>

<style>
	:global(body) {
		background: radial-gradient(hsl(220 14% 20%), hsl(220 20% 10%));
		background-attachment: fixed;
	}

	.scene {
		width: 100%;
		height: 100%;
		position: absolute;
		inset: 0;
	}
</style>
