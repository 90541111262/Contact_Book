<template>
    <h3>Conctact List</h3>

    <DataTable :data="transactions" :columns="columns" :options="options" class="table table-striped table-bordered" style="width:100%">
        <thead>
            <tr>
                <th>Name</th>
                <th>Fone</th>
                <th>Address</th>
            </tr>
        </thead>
        <template #action="props">
          <Button
              class='btn btn-light-primary'
                :text="`Col 1: ${props.cellData}`"
                @click="() => $emit('edit',props.rowData)"
            >Edit</Button>
            <Button
              class='btn btn-light-primary'
                :text="`Col 2: ${props.cellData}`"
                @click="() => $emit('delete',props.cellData.id)"
            >Delete</Button>
        </template>        
    </DataTable>


  </template>
  <script setup>
  import { defineProps, defineEmits } from 'vue';
  
  const props = defineProps({
    transactions: {
      type: Array,
      required: true,
    },    
  });

const emit = defineEmits(['delete', 'edit']);

import DataTable from 'datatables.net-vue3';
import DataTablesCore from 'datatables.net';
 
DataTable.use(DataTablesCore);
 
const columns = [
  { data: 'name', title: 'Name' },
  { data: 'fone', title: 'fone' },
  { data: 'address', title: 'Address' },
  {
    data: null,
    render: '#action',
    title: 'Action'
  },  
];

const options = {
  responsive: true,
  select: true,
};
  
  
</script>

<style>
@import 'bootstrap';
@import 'datatables.net-bs5';
</style>