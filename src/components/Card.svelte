<div class="card">
	<slot name="title" />
	{#if $$slots.description}
		<!-- This <hr> and slot will render only if a slot named "description" is provided. -->
		<hr style="padding-top: 1px solid black;" />
		<slot name="description" />
		<footer style="display: flex; font-size: 7pt;">
			<slot name="footer" />
		</footer>
	{/if}
</div>

<style>
	div.card {
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: fit-content;
		box-shadow: 0 0 0 1px rgba(0, 0, 0, 0.1), 0 2px 4px rgba(0, 0, 0, 0.1);
	}
</style>
