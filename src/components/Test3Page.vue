<template>
  <div style="height: 100%">
    <AgGridVue
      style="width: 1000px; height: 500px"
      class="ag-theme-alpine"
      :gridOptions="gridOptions"
    />
  </div>
</template>

<script setup lang="ts">
import { AgGridVue } from 'ag-grid-vue3';
import { dummy_data } from '../../dummy.ts';

import {
  GridOptions,
  RowSpanParams,
  CellClassParams,
  ColDef,
} from 'ag-grid-community';

import 'ag-grid-community/styles/ag-grid.css';
import 'ag-grid-community/styles/ag-theme-alpine.css';

const countByDate = {};

dummy_data.forEach(({ TRANS_DATE }, index) => {
  if (!countByDate[TRANS_DATE]) {
    countByDate[TRANS_DATE] = {
      data: TRANS_DATE,
      count: 0,
      startIndex: index,
    };
  }
  countByDate[TRANS_DATE].count++;
});

const columnDefs: ColDef[] = [
  {
    headerName: '명세일자',
    field: 'TRANS_DATE',
    rowSpan: rowSpan,
    cellClassRules: {
      'cell-span': (params: CellClassParams) => {
        const data = params.data.TRANS_DATE;

        return (
          data === countByDate[data].data &&
          params.node?.rowIndex === countByDate[data].startIndex
        );
      },
    },
  },
  { headerName: '명세번호', field: 'TRANS_SEQ' },
  {
    headerName: '매입일자',
    field: 'WORK_DATE',
  },
  { headerName: '매입구분', field: 'WORK_TYPE_NM' },
  { headerName: '창고이름', field: 'STOCK_NM' },
];

const rowData = dummy_data;

const gridOptions: GridOptions = {
  columnDefs,
  rowData,
  suppressRowTransform: true,
};

function rowSpan(params: RowSpanParams) {
  const data = params.data.TRANS_DATE;
  console.log(countByDate);

  console.log(params);

  // console.log(data);
  // console.log(countByDate[data]);
  if (
    data === countByDate[data].data &&
    params.node?.rowIndex === countByDate[data].startIndex
  ) {
    return countByDate[data].count;
  }
}
</script>

<style scoped></style>
