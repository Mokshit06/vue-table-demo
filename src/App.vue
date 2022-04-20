<script setup lang="ts">
import { ref } from 'vue';
import { makeData } from './make-data';
import type { Person } from './make-data';
import { createTable, useTable, sortRowsFn } from './vue-table';

const table = createTable<{ Row: Person }>();

const columns = table.createColumns([
  table.createGroup({
    header: 'Name',
    columns: [
      table.createDataColumn('firstName', {
        header: 'First Name',
      }),
      table.createDataColumn('lastName', {
        header: 'Last Name',
      }),
    ],
  }),
  table.createGroup({
    header: 'Info',
    columns: [
      table.createDataColumn('age', {
        header: 'Age',
      }),
      table.createDataColumn('visits', {
        header: 'Visits',
      }),
      table.createDataColumn('status', {
        header: 'Status',
      }),
      table.createDataColumn('progress', {
        header: 'Profile Progress',
      }),
    ],
  }),
]);

const data = ref(makeData(20));

const instance = useTable(table, {
  get data() {
    return data.value;
  },
  columns,
  sortRowsFn,
});
</script>

<template>
  <div class="table">
    <table v-bind="instance.getTableProps()">
      <thead>
        <tr
          v-for="headerGroup of instance.getHeaderGroups()"
          :key="headerGroup.id"
          v-bind="headerGroup.getHeaderGroupProps()"
        >
          <th
            v-for="header of headerGroup.headers"
            :key="header.id"
            v-bind="header.column.getToggleSortingProps()"
          >
            <component :is="header.renderHeader" />
            <span v-if="header.column.getIsSorted()">
              {{ header.column.getIsSorted() === 'desc' ? ' 🔽' : ' 🔼' }}
            </span>
          </th>
        </tr>
      </thead>
      <tbody v-bind="instance.getTableBodyProps()">
        <tr
          v-for="row of instance.getRowModel().rows"
          :key="row.id"
          v-bind="row.getRowProps()"
        >
          <td
            v-for="cell of row.getVisibleCells()"
            :key="cell.id"
            v-bind="cell.getCellProps()"
          >
            <component :is="cell.renderCell" />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
.table {
  padding: 1rem;
}

.table table {
  border-spacing: 0;
  border: 1px solid black;
}

.table table tr:last-child td {
  border-bottom: 0;
}

.table table th,
.table table td {
  margin: 0;
  padding: 0.5rem;
  border-bottom: 1px solid black;
  border-right: 1px solid black;
}

.table table th:last-child,
.table table td:last-child {
  border-right: 0;
}

.table .pagination {
  padding: 0.5rem;
}
</style>
