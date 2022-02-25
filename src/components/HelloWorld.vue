<script setup>
import { ref } from "vue";
import CustomTable from "./Table.vue";
import moment from "moment";
import _ from "lodash";

const columns = [
  {
    text: "Sl. No",
    value: "sl_no",
  },
  {
    text: "Picture",
    value: "picture",
  },
  {
    text: "Name",
    value: "name",
  },
  {
    text: "DOB",
    value: "dob",
  },
  {
    text: "Email",
    value: "email",
  },
  {
    text: "Location",
    value: "location",
  },
  {
    text: "Phone",
    value: "phone",
  },
];

const data = ref([]);

const getData = () => {
  fetch("https://randomuser.me/api/?results=50", {
    headers: { "Content-type": "application/json" },
  })
    .then((res) => res.json())
    .then((response) => {
      const results = response.results;

      data.value = _.reverse(results);
    })
    .catch((error) => {
      console.log("Looks like there was a problem: \n", error);
    });
};

function dateTime(value) {
  return moment(value).format("YYYY-MM-DD");
}

getData();
</script>

<template>
  <div>
    <CustomTable :columns="columns" :tdata="data">
      <template v-slot:sl_no="record">
        {{ record.index }}
      </template>
      <template v-slot:picture="{ item }">
        <div v-if="item.picture">
          <img :src="item.picture.thumbnail" />
        </div>
      </template>
      <template v-slot:name="{ item }">
        <p v-if="item.name">
          {{ item.name.title }}<span v-if="item.name.title">.</span>
          {{ item.name.first }} {{ item.name.last }}
        </p>
      </template>
      <template v-slot:dob="{ item }">
        <p v-if="item.dob">
          {{ dateTime(item.dob.date) }}
        </p>
      </template>
      <template v-slot:location="{ item }">
        <div v-if="item.location">
          <p>
            {{ item.location.street.name }}, {{ item.location.street.number }},
            {{ item.location.city }}, {{ item.location.country }} -
            {{ item.location.postcode }}
          </p>
        </div>
      </template>
    </CustomTable>
  </div>
</template>
