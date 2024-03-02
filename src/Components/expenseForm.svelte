<script>
	// @ts-nocheck

	import Title from '../Components/title.svelte';
	export let name = '';
	export let amount = null;
	export let addExpense;
	export let isEditing;
	export let editExpense;
	export let hideForm;

	$: isEmpty = !name || !amount;
	function handleSubmit() {
		if (isEditing) {
			editExpense({ name, amount });
		} else {
			addExpense({ amount, name });
		}

		name = '';
		amount = null;
		hideForm();
	}
	//local Storage
	function setLocalStorage() {
		// @ts-ignore
		localStorage.setItem('expenses', JSON.stringify(expenses));
	}
</script>

<section class="form">
	<Title title="Add expense" />
	<form class="expense-form" on:submit={handleSubmit}>
		<div class="form-control">
			<label for="name">name</label>
			<input type="text" id="name" bind:value={name} />
		</div>
		<div class="form-control">
			<label for="amount">amount</label>
			<input type="number" id="amount" bind:value={amount} />
		</div>
		{#if isEmpty}
			<p class="form-empty">Please fill out all form fields</p>
		{/if}
		<a href="./">
			<button type="submit" class="btn btn-block" class:disabled={isEmpty} disabled={isEmpty} on:click={setLocalStorage}
				>{#if isEditing}edit Expense{:else}add expense{/if}</button
			>
		</a>
		<a href="./">
			<button type="button" class="close-btn" on:click={hideForm}>Close</button>
		</a>
	</form>
</section>
