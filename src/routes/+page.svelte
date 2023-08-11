<div class="container">
	<h1>Examples</h1>
	<ul>
		<li>
			<a href="/sphere">Sphere</a>
		</li>
		<li>
			<a href="/models">Models</a>
		</li>
	</ul>
</div>

<style>
	:global(body) {
		font-family: sans-serif;
		color: hsl(0 0% 98%);
	}

	.container {
		height: 100%;
		display: grid;
		place-content: center;
	}

	h1 {
		font-size: 4rem;
	}

	ul {
		margin: 0;
		padding: 0;
		font-size: 2rem;
		list-style-position: inside;

		& a {
			color: hsl(0 0% 98%);
			text-decoration: none;
		}
	}
</style>
