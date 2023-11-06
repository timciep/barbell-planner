<script>
    import Barbell from '../components/Barbell.svelte';
</script>

<svelte:head>
	<title>Barbell Planner</title>
	<meta name="description" content="Calculate barbell weights." />
</svelte:head>

<section>
	<h1>
		Barbell Planner
	</h1>

	<Barbell />
</section>

<style>
	/* section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	} */

	h1 {
		width: 100%;
		margin: 0 0 1rem 0;
	}
</style>
