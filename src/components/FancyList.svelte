<script lang="ts">
	export let items: string[];
</script>

<!-- FancyList.svelte -->
<ul>
	{#each items as item}
		<li class="fancy">
			<slot name="item" {item} />
		</li>
	{/each}
</ul>

<slot name="footer" />
