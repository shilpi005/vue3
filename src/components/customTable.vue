<template>
  <table class="neumorphic">
    <thead>
      <tr>
        <th v-for="column in columns" :key="column.field">
          <div @click="sort(column.field)" style="cursor: pointer">
            {{ column.label }}
          </div>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in tableData.data" :key="index">
        <td>{{ returnIndex(item.id.value) }}</td>
        <td><img :src="item.picture.medium" /></td>
        <td>
          {{ item.name.title }}. {{ item.name.first }} {{ item.name.last }}
        </td>
        <td>{{ display_date_formate(item.dob.date) }}</td>
        <td>{{ item.email }}</td>
        <td>
          <div>
            {{ item.location.street.number }}, {{ item.location.street.name }}
          </div>
          {{ item.location.city }},{{ item.location.state }},
          {{ item.location.country }}, {{ item.location.postcode }}
        </td>
        <td>{{ item.phone }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { defineProps, ref } from "vue";
let tableData = ref([]);
const props = defineProps(["data"]);
tableData.value = props;
let is_sort_dec = true;
const columns = [
  { label: "slno.", field: "slno" },
  { label: "Picture", field: "picture" },
  { label: "Name", field: "name" },
  { label: "DOB", field: "dob" },
  { label: "Email", field: "email" },
  { label: "Location", field: "location" },
  { label: "Phone", field: "phone" },
];

function returnIndex(id) {
  let index = props.data?.findIndex((item) => item.id.value === id);
  return index + 1;
}
function display_date_formate(date) {
  const dateObject = new Date(date);

  // Extract day, month, and year
  const day = dateObject.getUTCDate().toString().padStart(2, "0");
  const month = (dateObject.getUTCMonth() + 1).toString().padStart(2, "0");
  const year = dateObject.getUTCFullYear().toString().slice(-2);

  // Form the dd-mm-yy format
  const formattedDate = `${day}-${month}-${year}`;

  return formattedDate;
}
const sort = (value) => {
  if (is_sort_dec) {
    is_sort_dec = false;
    switch (value) {
      case (value = "name"):
        tableData.value.data.sort((a, b) => {
          let ga = a.name.first.toLowerCase(),
            gb = b.name.first.toLowerCase();
          if (ga > gb) {
            return -1;
          }
          if (ga < gb) {
            return 1;
          }
          return 0;
        });
        break;
      case (value = "location"):
        tableData.value.data.sort((a, b) => {
          let la = a.location.street.name.toLowerCase(),
            lb = b.location.street.name.toLowerCase();
          if (la > lb) {
            return -1;
          }
          if (la < lb) {
            return 1;
          }
          return 0;
        });
        break;
      case (value = "email"):
        tableData.value.data.sort((a, b) => {
          let da = a.email.toLowerCase(),
            db = b.email.toLowerCase();
          if (da > db) {
            return -1;
          }
          if (da < db) {
            return 1;
          }
          return 0;
        });
        break;
      case (value = "dob"):
        tableData.value.data.sort((a, b) => {
          let da = new Date(a.dob.date),
            db = new Date(b.dob.date);
          return db - da;
        });
        break;
      case (value = "phone"):
        tableData.value.data.sort((a, b) => {
          return b.phone - a.phone;
        });
        break;
    }
  } else {
    is_sort_dec = true;
    switch (value) {
      case (value = "name"):
        tableData.value.data.sort((a, b) => {
          let ua = a.name.first.toLowerCase(),
            ub = b.name.first.toLowerCase();

          if (ua < ub) {
            return -1;
          }
          if (ua > ub) {
            return 1;
          }
          return 0;
        });
        break;
      case (value = "location"):
        tableData.value.data.sort((a, b) => {
          let ha = a.location.street.name.toLowerCase(),
            hb = b.location.street.name.toLowerCase();
          if (ha > hb) {
            return -1;
          }
          if (ha < hb) {
            return 1;
          }
          return 0;
        });
        break;
      case (value = "email"):
        tableData.value.data.sort((a, b) => {
          let ta = a.email.toLowerCase(),
            tb = b.email.toLowerCase();

          if (ta < tb) {
            return -1;
          }
          if (ta > tb) {
            return 1;
          }
          return 0;
        });
        break;

      case (value = "dob"):
        tableData.value.data.sort((a, b) => {
          let da = new Date(a.dob.date),
            db = new Date(b.dob.date);
          return da - db;
        });
        break;
      case (value = "phone"):
        tableData.value.data.sort((a, b) => {
          return a.phone - b.phone;
        });
        break;
      default:
        tableData.value.data.reverse();
        console.log(tableData.value.data.reverse());
    }
  }
};
</script>
<style scoped>
html,
body {
  height: 100%;
}

body {
  margin: 0;
  background: #e0e5ec;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: 100;
}

table.neumorphic {
  width: 100%;
  border-spacing: 0;
  color: #212121;
  text-align: center;
  overflow: hidden;
  box-shadow: 9px 9px 16px rgba(163, 177, 198, 0.6),
    -9px -9px 16px rgba(255, 255, 255, 0.6);
}
table.neumorphic thead {
  box-shadow: 9px 9px 16px rgba(163, 177, 198, 0.6);
}
table.neumorphic th {
  padding: 7px;
}

table.neumorphic > tbody > tr > td {
  padding: 10px;
  font-size: 14px;
  position: relative;
}

table.neumorphic > tbody > tr:hover {
  padding: 20px;
  box-shadow: 9px 9px 16px rgba(163, 177, 198, 0.6),
    -9px -9px 16px rgba(255, 255, 255, 0.6);
}

table.neumorphic tr td:first-child::before {
  content: "";
  position: absolute;
  padding: 7px;
  top: 0;
  left: -5000px;
  width: 10000px;
  height: 100%;
  z-index: -10;
}

table.neumorphic td:hover::after {
  content: "";
  position: absolute;

  left: 0;
  top: -5000px;
  height: 10000px;
  width: 100%;
  z-index: -1;
}
</style>
