<script>
    import Barbell from '../components/Barbell.svelte';
</script>

<svelte:head>
	<title>Barbell Planner</title>
	<meta name="description" content="Calculate weightlifting plates to load on a barbell, given a bar weight and a target total weight." />
</svelte:head>

<section>
	<div class="title-section">
		<div>
			<img src="/left.png" alt="left" />
		</div>

		<div>
			<h1>
				Barbell Planner
			</h1>
		</div>

		<div>
			<img src="/right.png" alt="right" />
		</div>
	</div>

	<Barbell />
</section>

<style>
	.title-section {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		flex: 0.4;
		gap: 1rem;
	}

	.title-section img {
		width: 3rem;
	}

	/* section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	} */

	h1 {
		width: 100%;
		margin: 0 0 0 0;
		font-size: 1.8rem;
	}
</style>
