<script lang="ts">
	import { Pane } from 'tweakpane'
	import { browser } from '$app/environment'

	import * as Threlte from '@threlte/core'
	import * as Three from 'three'
	import * as Extra from '@threlte/extras'
	import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass'

	import Background from './background.svelte'

	type Camera = keyof typeof camera
	type PointLight = keyof typeof pointLight
	type Ghost = keyof typeof ghost

	let camera = {
		position: { x: 10, y: 10, z: 10 },
		zoom: 40,
	}

	let pointLight = {
		position: { x: 0, y: 2, z: 2 },
		color: 'hsl(60, 100%, 50%)',
	}

	let ghost = {
		position: { x: 0, y: 1.9, z: 0 },
		scale: 0.4,
	}

	function float() {
		const timeInSeconds = Date.now() / 1000
		const offsetY = 2.9
		ghost.position.y = Math.sin(timeInSeconds) + offsetY
		requestAnimationFrame(float)
	}

	if (browser) {
		float()

		const pane = new Pane({ title: 'Scene' })

		const cameraControls = pane.addFolder({ title: 'Camera' })
		cameraControls.addInput(camera, 'position')
		cameraControls.addInput(camera, 'zoom')

		cameraControls.on('change', ({ presetKey, value }) => {
			camera[presetKey as Camera] = value as any
		})

		const pointLightControls = pane.addFolder({ title: 'Point Light' })
		pointLightControls.addInput(pointLight, 'position')
		pointLightControls.addInput(pointLight, 'color')

		pointLightControls.on('change', ({ presetKey, value }) => {
			pointLight[presetKey as PointLight] = value as any
		})

		const ghostControls = pane.addFolder({ title: 'Ghost' })
		ghostControls.addInput(ghost, 'position')
		ghostControls.addInput(ghost, 'scale')

		ghostControls.on('change', ({ presetKey, value }) => {
			ghost[presetKey as Ghost] = value as any
		})
	}

	const gridHelper = new Three.GridHelper(20, 10)
	const axisHelper = new Three.AxesHelper(10)
</script>

<Threlte.Canvas rendererParameters={{ antialias: true }}>
	<Threlte.Object3DInstance object={gridHelper} />
	<Threlte.Object3DInstance object={axisHelper} />

	<Threlte.Pass
		pass={new UnrealBloomPass(new Three.Vector2(1, 1), 4, 1, 0.7)}
	/>

	<Background color="orangered" />

	<Threlte.OrthographicCamera {...camera}>
		<Threlte.OrbitControls />
	</Threlte.OrthographicCamera>

	<Threlte.AmbientLight color="blue" intensity={4} />
	<Threlte.PointLight intensity={0.2} {...pointLight} />

	<Extra.GLTF url="models/ghost.glb" {...ghost} />
	<Extra.GLTF url="models/garden.glb" />
</Threlte.Canvas>
