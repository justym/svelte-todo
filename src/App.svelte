<div class="wrapper">
	<h1>TODO List</h1>
	<div class="filter-buttons">
		<button on:click={() => {condition = true}}>Done</button>
		<button on:click={() => {condition = false}}>Yet</button>	
		<button on:click={() => {condition = null}}>All</button>
	</div>
	<div class="form">
		<input type="text" bind:value={title} bind:this={initFocus}>
		<button type="submit" on:click={() => add()}>Add</button>
	</div>
	<div class="list">
		{#each filterList(todoList,condition) as item, i}
			<li>
				<input type="checkbox" bind:checked={item.done}> {item.name}
				<button on:click={remove(item.id)}>Delete</button>
			</li>
		{/each}
	</div>
</div>

<script>
	import {onMount} from 'svelte'

	let initFocus = null;

	function init(){
		title = '',
		initFocus.focus();
	}

	onMount(() => {
		init()
	})

	let todoList = [
		{
			id: 0,
			name: 'test1',
			done: false
		},
		{
			id: 1,
			name: 'test2',
			done: false
		},
		{
			id: 2,
			name: 'test3',
			done: false,
		},
	];

	let title = '';
	function add(){
		todoList = [
			...todoList,
			{	
				id: todoList.length,
				name: title,
				done: false
			}
		]
		init()
	}

	function remove(id){
		todoList = todoList.filter(t => t.id !== id)
	}
	
	let condition = null;
	$: filterList = (todoList,condition) => {
		return condition === null ? todoList : todoList.filter(t => t.done === condition)
	}
</script>

<style>
	.wrapper{
		display: flex;
		flex-direction: column;
		align-items: center;
		font-size: 2em;
	}
</style>