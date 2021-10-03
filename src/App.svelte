<script>
  import { writable } from "svelte/store";

	const localStore = (key, initial) => {
  // receives the key of the local storage and an initial value

		const toString = (value) => JSON.stringify(value, null, 2); // helper function
		const toObj = JSON.parse; // helper function

		if (localStorage.getItem(key) === null) {
			// item not present in local storage
			localStorage.setItem(key, toString(initial)); // initialize local storage with initial value
		}

		const saved = toObj(localStorage.getItem(key)); // convert to object

		const { subscribe, set, update } = writable(saved); // create the underlying writable store

		return {
			subscribe,
			set: (value) => {
				localStorage.setItem(key, toString(value)); // save also to local storage as a string
				return set(value);
			},
			update,
			};
  };

	let initialize = [];

	const data = localStore("data", initialize);

	let title = "";

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
</script>

<form autocomplete="off" on:submit|preventDefault={add}>
	<input bind:value={title} />
	<button type="submit">+</button>
</form>

<ul>
	{#each $data as item}
		<li>{item.title}
		<button on:click={() => remove(item)}>-</button>
	</li>
	{/each}
</ul>
