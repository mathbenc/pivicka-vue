<template>
  <v-data-table
    :headers="headers"
    :items="coronaData"
    :pagination.sync="pagination"
    item-key="name"
    class="elevation-1"
  >
    <template slot="items" slot-scope="props">
      <tr @click="rowClick(props.item.name)">
        <td>{{ props.item.name }}</td>
        <td class="text-xs-right">{{ props.item.calories }}</td>
        <td class="text-xs-right">{{ props.item.fat }}</td>
        <td class="text-xs-right">{{ props.item.carbs }}</td>
        <td class="text-xs-right">{{ props.item.protein }}</td>
        <td class="text-xs-right">{{ props.item.iron }}</td>
      </tr>
    </template>
  </v-data-table>
</template>

<script>
import axios from "axios";

export default {
  name: "Table",
  data: () => ({
    options: {
      sortBy: "name"
    },
    headers: [
      { text: "Country", value: "country", width: "40%", align: "left" },
      { text: "Infected", value: "cases", width: "15%", align: "right" },
      { text: "Infected today", value: "todayCases", width: "15%", align: "right" },
      { text: "Dead", value: "deaths", width: "15%", align: "right" },
      { text: "Cured", value: "recovered", width: "15%", align: "right" },
    ],
    coronaData: []
  }),
  methods: {
    async getCoronaData() {
      try {
        const response = await axios.get("https://coronavirus-19-api.herokuapp.com/countries")
        this.coronaData = response.data;
      } catch(err) {
        console.log(err)
      }
    },

    rowClick(name) {
      this.selectedItem = name;
    }
  },
  created: function() {
    this.getCoronaData();
    console.log(this.coronaData);
  }
};
</script>