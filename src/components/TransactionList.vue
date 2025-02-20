<template>
    <div>
        <div class="mt-3">
            <label class="custom-padding">Filter by: </label>
            <!-- Radio Buttons for Filtering -->
            <label class="custom-padding">
                <input type="radio" class="form-check-input" v-model="filterType" value="all" /> All
            </label>
            <label class="custom-padding">
                <input type="radio" class="form-check-input" v-model="filterType" value="income" /> Income
            </label>
            <label class="custom-padding">
                <input type="radio" class="form-check-input" v-model="filterType" value="expense" /> Expense
            </label>
        </div>
        <table class="table table-bordered table-dark mt-3">
            <thead>
                <tr>
                    <th>Title</th>
                    <th>Amount</th>
                    <th>Type</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="transaction in paginatedTransactions" :key="transaction.id">
                    <td>{{ transaction.title }}</td>
                    <td :class="{ 'text-success': transaction.type === 'income', 'text-danger': transaction.type === 'expense', 'fw-bold': transaction.amount >= 500 }">
                        ${{ transaction.amount }}
                    </td>
                    <td>{{ transaction.type.toUpperCase() }}</td>
                    <td>
                        <button @click="deleteTransaction(transaction.id)" class="btn btn-danger btn-sm">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <p v-if="filteredTransactions.length === 0">No transactions recorded yet.</p>
    
        <!-- Pagination Controls -->
        <div class="pagination">
            <button @click="previousPage" :disabled="currentPage === 1" class="btn btn-secondary btn-sm">Previous</button>
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            <button @click="nextPage" :disabled="currentPage === totalPages" class="btn btn-secondary btn-sm">Next</button>
        </div>
    </div>
</template>
  
<script setup>
  import { ref, computed } from 'vue';
  
  const props = defineProps({
    transactions: Array,
  });
  
  const filterType = ref('all');
  const currentPage = ref(1); // Current page number
  const itemsPerPage = ref(5); // Number of items per page
  
  // Filtered transactions based on the selected filter type
  const filteredTransactions = computed(() => {
    if (filterType.value === 'all') {
      return props.transactions;
    } else {
      return props.transactions.filter(t => t.type === filterType.value);
    }
  });
  
  // Paginated transactions for the current page
  const paginatedTransactions = computed(() => {
    const start = (currentPage.value - 1) * itemsPerPage.value;
    const end = start + itemsPerPage.value;
    return filteredTransactions.value.slice(start, end);
  });
  
  // Total number of pages
  const totalPages = computed(() => {
    return Math.ceil(filteredTransactions.value.length / itemsPerPage.value);
  });
  
  // Navigate to the previous page
  const previousPage = () => {
    if (currentPage.value > 1) {
      currentPage.value--;
    }
  };
  
  // Navigate to the next page
  const nextPage = () => {
    if (currentPage.value < totalPages.value) {
      currentPage.value++;
    }
  };
  
  const emit = defineEmits(['delete-transaction']);
  
  const deleteTransaction = (id) => {
    emit('delete-transaction', id);
    // Reset to the first page after deletion
    currentPage.value = 1;
  };
</script>
  
<style scoped>
    .pagination {
        margin-top: 20px;
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 10px;
    }
    .custom-padding {
        padding-right: 1rem; /* Adjust the value as needed */
    }
    .form-check-input:checked {
        background-color: #28a745; /* Green background for checked state */
        border-color: #28a745;
    }

    .form-check-label {
        margin-left: 0.5rem; /* Add spacing between radio button and label */
    }
</style>