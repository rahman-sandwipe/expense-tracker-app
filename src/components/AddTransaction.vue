<script setup>
  import { ref } from 'vue';
  
  const title = ref('');
  const amount = ref(0);
  const type = ref('income');
  const amountError = ref('');
  
  const emit = defineEmits(['add-transaction']);
  
  const handleSubmit = () => {
    if (amount.value <= 0) {
      amountError.value = 'Amount must be greater than 0.';
      return;
    }
    const newTransaction = {
      id: Date.now(),
      title: title.value,
      amount: parseFloat(amount.value),
      type: type.value,
    };
    emit('add-transaction', newTransaction);
    title.value = '';
    amount.value = 0;
    type.value = 'income';
    amountError.value = '';
  };
</script>


<template>
    <div class="mt-3">
        <form @submit.prevent="handleSubmit" class="row g-3">
            <div class="col-md-3">
                <input type="text" v-model="title" class="form-control" id="title" placeholder="title" required />
            </div>
            <div class="col-md-3">
                <input type="number" v-model="amount" class="form-control" id="amount" placeholder="0" required />
                <small v-if="amountError" class="text-danger">{{ amountError }}</small>
            </div>
            <div class="col-md-3">
                <select v-model="type" class="form-select" id="type" required>
                    <option value="income">Income</option>
                    <option value="expense">Expense</option>
                </select>
            </div>
            <div class="col-md-3">
                <label>&nbsp;</label>
                <button type="submit" class="btn btn-primary">Add</button>
            </div>
        </form>
    </div>
</template>
  
