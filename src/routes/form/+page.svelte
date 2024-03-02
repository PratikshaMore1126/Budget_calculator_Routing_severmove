<script>
// @ts-nocheck

   import ExpenseForm from "../../Components/expenseForm.svelte";
	
  
	import Modal from "../../Components/modal.svelte";
	//variable
	// let expenses = [...expensesData];
	// @ts-ignore
	let expenses = [];
	//set editing variables
	let setName = "";
	// @ts-ignore
	let setAmount = null;
	// @ts-ignore
	let setId = null;
  
	// toggle form variables
	let isFormOpen = true;
  
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
	  setName = "";
	  setId = null;
	}
    function addExpense({ name, amount }) {
	  let expense = { id: Math.random() * Date.now(), name, amount };
	  // @ts-ignore
	  expenses = [expense, ...expenses];
	  // setLocalStorage();
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
	  setName = "";
	  // setLocalStorage();
	}
	

	
</script>

{#if isFormOpen}

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
	{/if}