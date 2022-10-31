<script lang="ts">
	import * as Three from 'three'
	import * as Threlte from '@threlte/core'

	export let color: Three.ColorRepresentation

	const { scene } = Threlte.useThrelte()

	scene.background = new Three.Color(color)
</script>
