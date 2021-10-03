<script>
  import {data} from "./store"

	let title = "";
	let editing = false;

	const add = () => {
			$data = [...$data, {
				id: Math.random(),
				title
			}];
		title = "";
	};

	const remove = (item) => {
		$data = $data.filter((x) => x !== item);
	};

	const edit = (item) => {
		editing = !editing;
	}

	const cancel = () => {
		editing = !editing;
	}
</script>

<form autocomplete="off" on:submit|preventDefault={add}>
	<input bind:value={title} />
	<button type="submit">+</button>
</form>

{#if editing}
	I'm editing!
	<button on:click={() => cancel()}>cancel</button>
{:else}
	<ul>
		{#each $data as item}
			<li>{item.title}
			<button on:click={() => edit(item)}>edit</button>
			<button on:click={() => remove(item)}>-</button>
		</li>
		{/each}
	</ul>
{/if}


