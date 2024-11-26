<template>
    <h3>Add New Contact</h3>
    <form id="form" @submit.prevent="submitTransaction">
      <div class="form-control">
        <label for="text">Name</label>
        <input type="text" id="text" placeholder="Enter Name..." v-model="form.name" />
      </div>
      <div class="form-control">
        <label for="fone">Fone</label>
        <input type="text" id="fone" placeholder="Enter Fone..." v-model="form.fone" />
      </div>
      <div class="form-control">
        <label for="fone">Address</label>
        <input type="text" id="address" placeholder="Enter Address..." v-model="form.address" />
      </div>
      <button class="btn">{{ isEditing ? 'Update' : 'Add' }}</button>
    </form>
  </template>
  <script setup>
  import { useToast } from 'vue-toastification';
  import { ref, defineProps, watch } from 'vue';
  
  const props = defineProps({
    transaction: {
      type: Object,
      required: true,
    }
  })
  
  // Get toast interface
  const toast = useToast();
  
  const emit = defineEmits(['add', 'update']);
  
  const form = ref({ id: '', name: '', fone:'', address: '' });
  
  const isEditing = ref(false);
  
  const resetForm = () => {
    form.value = {
      id: '',
      name: '',
      fone: '',
      address:'',
    };
    isEditing.value = false;
  };
  
  watch(
    () => props.transaction,
    (newTransaction) => {
      if (newTransaction) {
        form.value = { ...newTransaction };
        isEditing.value = true;
      } else {
        resetForm();
      }
    },
    { immediate: true, deep: true }
  );
  
  const submitTransaction = () => {
  
    if (!form.value.name || !form.value.fone) {
      // Display a toast error message if either field is empty
      toast.error('Both fields must be filled.');
      return;
    }
    if (isEditing.value) {
      emit('update', { ...form.value });
    } else {
      emit('add', { ...form.value });
    }
    // Clear form fields
    resetForm();
  };
  </script>