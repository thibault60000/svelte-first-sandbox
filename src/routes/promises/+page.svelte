<script lang="ts">
	interface Post {
		id: number;
		title: string;
		body: string;
		userId: number;
	}

	let isLoading = false;
	let posts: Post[] = [];
	let error: string | null = null;

	let count: number = 0;

	function incrementeCount(event: MouseEvent) {
		count += 1;
	}

	const fetchData = async () => {
		isLoading = true;
		try {
			const response = await fetch('https://jsonplaceholder.typicode.com/posts');
			if (!response.ok) {
				throw new Error('Error retrieving data from server');
			}
			const json = await response.json();
			posts = json;
			console.log('posts', posts);
			isLoading = false;
			return posts;
		} catch (err: any) {
			error = err.message;
			isLoading = false;
		}
	};
</script>

<!-- {@debug posts} -->

<button on:click={fetchData}>Fetch Data</button>

{#if !posts.length && !error}
	<p>Click the button to fetch data</p>
{:else if isLoading}
	<p>Loading...</p>
{:else if error}
	<p>Error: {error}</p>
{:else}
	<ul>
		{#each posts as post (post.id)}
			<li><strong>{post.id}</strong> {post.title}</li>
		{/each}
	</ul>
{/if}

<span> Count : {count}</span>

<button on:click|preventDefault={incrementeCount}> Incremente Count </button>
