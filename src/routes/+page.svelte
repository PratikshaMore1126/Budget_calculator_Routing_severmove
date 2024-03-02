<script>
	// @ts-nocheck

	import { setContext, onMount, afterUpdate } from 'svelte';

	//component
	import Navbar from '../Components/navbar.svelte';
	import ExpenseList from '../Components/expenseList.svelte';
	// @ts-ignore
	import Total from '../Components/total.svelte';
	import ExpenseForm from '../Components/expenseForm.svelte';
	// import expensesData from "./Components/expenses";

	import Modal from '../Components/modal.svelte';
	//variable
	// let expenses = [...expensesData];
	// @ts-ignore
	let expenses = [];
	//set editing variables
	let setName = '';
	// @ts-ignore
	let setAmount = null;
	// @ts-ignore
	let setId = null;

	// toggle form variables
	let isFormOpen = false;

	//reactive
	// @ts-ignore
	$: isEditing = setId ? true : false;
	// @ts-ignore
	$: total = expenses.reduce((acc, curr) => {
		return (acc += curr.amount);
	}, 0);

	//functions

	function showForm() {
		isFormOpen = true;
	}
	function hideForm() {
		isFormOpen = false;
		setAmount = null;
		setName = '';
		setId = null;
	}

	// @ts-ignore
	function removeExpense(id) {
		// @ts-ignore
		expenses = expenses.filter((item) => item.id !== id);
		// setLocalStorage();
	}
	function clearExpenses() {
		expenses = [];
		// setLocalStorage();
	}
	// @ts-ignore
	function addExpense({ name, amount }) {
		let expense = { id: Math.random() * Date.now(), name, amount };
		// @ts-ignore
		expenses = [expense, ...expenses];
		// setLocalStorage();
	}
	// @ts-ignore
	function setModifiedExpense(id) {
		// @ts-ignore
		let expense = expenses.find((item) => item.id === id);

		setId = expense.id;
		setName = expense.name;
		setAmount = expense.amount;
		showForm();
	}
	// @ts-ignore
	function editExpense({ name, amount }) {
		// @ts-ignore
		expenses = expenses.map((item) => {
			// @ts-ignore
			return item.id === setId ? { ...item, name, amount } : { ...item };
		});
		setId = null;
		setAmount = null;
		setName = '';
		// setLocalStorage();
	}

	//context
	setContext('remove', removeExpense);
	setContext('modify', setModifiedExpense);

	//local Storage
	function setLocalStorage() {
		// @ts-ignore
		localStorage.setItem('expenses', JSON.stringify(expenses));
	}

	onMount(() => {
		expenses = localStorage.getItem('expenses')
			? // @ts-ignore
			  JSON.parse(localStorage.getItem('expenses'))
			: [];
	});

	afterUpdate(() => {
		setLocalStorage();
	});
</script>

<!-- <Navbar {showForm} /> -->

<nav class="nav">
	<div class="nav-center">
		<h1 class="nav-title">budget calculator</h1>
		<a href="/form">
			<button type="button" class="nav-btn" on:click={showForm}> add item </button>
		</a>
	</div>
</nav>

<main class="content">
	<!-- {#if isFormOpen}
	  <Modal>
		<ExpenseForm
		  {addExpense}
		  name={setName}
		  amount={setAmount}
		  {isEditing}
		  {editExpense}
		  {hideForm}
		/>
	  </Modal>
	{/if} -->
	<Total title="total expenses" {total} />

	<ExpenseList {expenses} />
	<button type="button" class="btn btn-primary btn-block clear" on:click={clearExpenses}>
		clear expenses
	</button>
</main>

<!-- <Modal
	>
	<h1 slot="header">hello world</h1>
	<p slot="footer">
	  Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ipsam, quos
	  laboriosam. Obcaecati velit quam cupiditate veniam, facere eos hic nemo
	  veritatis odio, incidunt sequi architecto sapiente reprehenderit! Sint, in
	  nobis!
	</p></Modal
  > -->

<style>
	main {
		text-align: center;
		/* padding: 1em; */
		max-width: 240px;
		margin: 0 auto;
	}
	/* 
	h1 {
	  color: #ff3e00;
	  text-transform: uppercase;
	  font-size: 2em;
	  font-weight: 100;
	} */

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
	.clear {
		width: 900px;
		margin-left: 50px;
	}
</style>
