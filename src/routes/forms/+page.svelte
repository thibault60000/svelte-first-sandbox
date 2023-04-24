<script lang="ts">
	import { onMount } from 'svelte';

	let canvasElement: HTMLCanvasElement;

	onMount(() => {
		const ctx = canvasElement.getContext('2d');

		if (ctx) {
			ctx.beginPath();
			ctx.arc(20, 20, 10, 0, 2 * Math.PI);
			ctx.fillStyle = 'red';
			ctx.fill();
		}
	});

	let user = {
		first_name: 'John',
		last_name: 'Doe',
		age: 31,
		size: 183,
		email: 'john.doe@gmail.com',
		job: 'developer',
		description: 'A 31 years old male named John Doe',
		male: true,
		birthCity: 'Paris',
		passion: 'svelte',
		sports: ['football', 'tennis'],
		files: []
	};

	const options = [
		{ value: 'svelte', label: 'Svelte' },
		{ value: 'react', label: 'React' },
		{ value: 'vue', label: 'Vue' }
	];

	const sports_groups = [
		{
			value: 'football',
			label: 'football'
		},
		{
			value: 'tennis',
			label: 'tennis'
		},
		{
			value: 'rugby',
			label: 'rugby'
		}
	];

	let files: any = [];

	let text_visible = true;

	const btn_is_active = true;

	export let text_with_use_action: string;

	$: {
		user.files = files;
		console.log('user', user);
	}
	function handleSubmit(event: Event) {
		event.preventDefault();
		console.log('submitted');
	}
	function handleFileSelect(event: Event) {
		const target = event.target as HTMLInputElement;
		const files = target.files;
		console.log('files', files);
	}

	function textAction(node: HTMLElement, text_with_use_action: string) {
		console.log('mounted', node);
		console.log('value', text_with_use_action);

		if (!text_with_use_action) {
			node.textContent = 'Click me';
		}

		function update() {
			console.log('update');
			if (text_with_use_action) {
				node.textContent = text_with_use_action;
			} else {
				node.textContent = 'Click me';
			}
		}

		update();

		node.addEventListener('click', () => {
			console.log("I'm clicked");
			text_with_use_action = "I'm clicked";
			update();
		});

		return {
			update,
			destroy() {
				console.log('destroyed');
			}
		};
	}
</script>

<svelte:head>
	<title>Forms</title>
	<meta name="description" content="Forms" />
</svelte:head>

<form on:submit|preventDefault={handleSubmit}>
	<label>
		FirstName <input bind:value={user.first_name} />
	</label>

	<label>
		LastName <input bind:value={user.description} />
	</label>

	<label>
		Job <textarea bind:value={user.description} />
	</label>

	<label>
		Age <input type="number" bind:value={user.age} />
	</label>

	<label>
		Size {user.size}cm <input type="range" bind:value={user.size} />
	</label>

	<label>
		IsMale <input type="checkbox" bind:checked={user.male} />
	</label>

	<label for="avatar"
		>Upload a picture:
		<input
			accept="image/png, image/jpeg"
			bind:files
			on:change={handleFileSelect}
			id="avatar"
			name="avatar"
			type="file"
		/>
	</label>

	<label>
		Ville de naissance
		<input
			on:input={(value) => console.log('Old value:', value)}
			bind:value={user.birthCity}
			on:input={(value) => console.log('New value:', value)}
		/>
	</label>

	<label>
		Passions
		<select bind:value={user.passion}>
			{#each options as passion, i (`passion-${i}`)}
				<option value={passion.value}>{passion.label}</option>
			{:else}
				Aucune passion disponible
			{/each}
		</select>
	</label>

	<p>Pratique :</p>
	{#each sports_groups as sport, i (`sport-${i}`)}
		<label style="display: flex; gap: 0.3rem;">
			<input type="checkbox" bind:group={user.sports} value={sport.value} />
			<span>{sport.label}</span>
		</label>
	{:else}
		Aucune sport disponible
	{/each}

	<p>Active button class</p>
	<div style="display: flex; gap: 0.3rem;">
		<button type="button" on:click={() => (text_visible = !text_visible)}> toggle text </button>
		<button type="button" class:btn_is_active>2 </button>
		<button type="button"> 3 </button>
	</div>

	{#if text_visible}<p use:textAction={text_with_use_action} /> {/if}
</form>

<!-- Use 'this' to bind 'canvasElement' to the canvas element -->
<canvas bind:this={canvasElement} />

<style>
	form {
		display: flex;
		flex-direction: column;
	}
	form > * {
		margin: 0.5rem 0;
		width: 90%;
	}
	* {
		box-sizing: border-box;
		margin: 0;
		padding: 0;
		font-family: inherit;
	}

	label {
		display: block;
		margin-bottom: 0.5rem;
		font-weight: bold;
	}

	textarea {
		resize: vertical;
	}

	input,
	textarea,
	select {
		display: block;
		width: 100%;
		text-align: left;
		padding: 0.5rem;
		border: 1px solid #ccc;
		border-radius: 4px;
		font-size: 1rem;
		max-width: 500px;
		line-height: 1.5;
	}
	input[type='checkbox'],
	input[type='radio'] {
		width: fit-content;
	}
	button[type='button'] {
		background-color: white;
		color: black;
		border: 1px solid black;
		border-radius: 3px;
		padding: 0.3rem 0.9rem;
	}
	button[type='button'].btn_is_active {
		background-color: blue;
		color: white;
	}
</style>
