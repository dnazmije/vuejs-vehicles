<template>
  <b-container>
    <h2 class="mb-5">List Vehicles</h2>

    <b-col>
      <b-form-group
        label="Filter"
        label-cols-sm="6"
        label-align-sm="right"
        label-for="filterInput"
        class="mb-10"
      >
        <b-input-group>
          <b-form-input
            v-model="filter"
            type="search"
            id="filterInput"
            placeholder="Type to Search"
          ></b-form-input>
          <b-input-group-append>
            <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </b-col>

    <b-table
      show-empty
      small
      stacked="md"
      :items="items"
      :fields="fields"
      :filter="filter"
      :filterIncludedFields="filterOn"
    >
      <template v-slot:cell(name)="row">{{ row.value.first }} {{ row.value.last }}</template>

      <template v-slot:cell(actions)="row">
        <b-button
          size="sm"
          @click="row.toggleDetails"
          class="mr-1"
        >{{ row.detailsShowing ? 'Hide' : 'Show' }} Details & Edit</b-button>
        <b-button size="sm" @click="deleteItem(row.item, row.index)" class="mr-1">Delete</b-button>
      </template>

      <template v-slot:row-details="row">
        <b-card>
          <b-row m-10>
            <b-col sm="6">
              <label>vehicleName</label>
              <b-form-input type="text" name="vehicleType" v-model="items[row.index].vehicleName"></b-form-input>
            </b-col>
            <b-col sm="6">
              <label>vehicleType</label>
              <b-form-input type="text" name="vehicleType" v-model="items[row.index].vehicleType"></b-form-input>
            </b-col>
          </b-row>
          <b-row m-10>
            <b-col sm="6">
              <label>plateNumber</label>
              <b-form-input type="text" name="plateNumber" v-model="items[row.index].plateNumber"></b-form-input>
            </b-col>
            <b-col sm="6">
              <label>model</label>
              <b-form-input type="text" name="model" v-model="items[row.index].model"></b-form-input>
            </b-col>
          </b-row>
        </b-card>
      </template>
    </b-table>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          vehicleName: "vehicle",
          vehicleType: "Car",
          plateNumber: "SK-1234-AB",
          model: "Audi"
        },
        {
          vehicleName: "vehicle-2",
          vehicleType: "Van",
          plateNumber: "SK-5555-AB",
          model: "Mercedes"
        },
        {
          vehicleName: "vehicle-3",
          vehicleType: "Truck",
          plateNumber: "ST-9852-AB",
          model: "BMW"
        },
        {
          vehicleName: "vehicle-4",
          vehicleType: "Trailer",
          plateNumber: "PP-7412-AB",
          model: "Citroen"
        },
        {
          vehicleName: "vehicle-5",
          vehicleType: "Container",
          plateNumber: "SK-3654-AB",
          model: "Opel"
        }
      ],
      fields: [
        { key: "vehicleName", label: "vehicleName" },
        { key: "vehicleType", label: "vehicleType" },
        { key: "plateNumber", label: "plateNumber" },
        { key: "model", label: "model" },
        { key: "actions", label: "actions" }
      ],
      filter: null,
      filterOn: []
    };
  },
  methods: {
    deleteItem(item, index) {
      this.items.splice(index, 1);
    }
  }
};
</script>