<template>
  <div style="height: 100%">
    <button @click="changeHandler">Change</button>
    <div class="container">
      <AgGridVue
        style="width: 1000px; height: 600px"
        class="ag-theme-alpine"
        :gridOptions="gridOptions"
      />
      <div class="example-header">
        Page Size:
        <select @change="onPageSizeChanged" id="page-size">
          <option value="10">10</option>
          <option value="20">20</option>
          <option value="50">50</option>
          <option value="100">100</option>
        </select>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { AgGridVue } from 'ag-grid-vue3';
import { dummy_data } from '../../dummy.js';

import 'ag-grid-community/styles/ag-grid.css';
import 'ag-grid-community/styles/ag-theme-alpine.css';
import { GridOptions } from 'ag-grid-community';
import { GridApi } from 'ag-grid-community';
import { ref } from 'vue';

let gridApi: GridApi | null = null;

const _header = ref('명세일자' as string);

const columnDefs = [
  {
    headerName: _header.value,
    field: 'TRANS_DATE',
    sortable: true,
  },
  { headerName: '명세번호', field: 'TRANS_SEQ', sortable: true },
  { headerName: '매입일자', field: 'WORK_DATE' },
  { headerName: '매입구분', field: 'WORK_TYPE_NM' },
  { headerName: '창고이름', field: 'STOCK_NM' },
];

const onGridReady = (params: any) => {
  gridApi = params.api;
};

const rowData = dummy_data;

const gridOptions: GridOptions = {
  columnDefs: columnDefs,
  rowData: rowData,
  pagination: true,
  paginationPageSize: 10,
  onGridReady: onGridReady,
};

function onPageSizeChanged(e: Event) {
  const target = e.target as HTMLSelectElement;

  gridApi?.paginationSetPageSize(Number(target.value));
}

const changeHandler = async () => {
  _header.value = '테스트';

  gridApi?.refreshHeader();
};
</script>

<style scoped>
.container {
  position: relative;
}
.example-header {
  position: absolute;
  bottom: 15px;
  left: 15px;
}
</style>
