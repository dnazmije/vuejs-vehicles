<template>
    <b-container>
        <h2 class="mb-5">Add Vehicle</h2>

        <b-form @submit="onSubmit">

            <label>Name *</label>
            <b-form-input v-model="form.name" type="text" placeholder="Vehicle Name" class="mb-3"></b-form-input>
            <p v-if="errors.includes('name')" class="text-red">Name is required.</p>

            <label>Vehicle Type *</label>
            <b-form-select v-model="form.type" :options="types" class="mb-3"></b-form-select>
            <p v-if="errors.includes('type')" class="text-red">Type is required.</p>

            <label>Model</label>
            <b-form-input list="models"  v-model="form.model" placeholder="Vehicle Model" class="mb-3"></b-form-input>
            <datalist id="models">
                <option v-for="model in models" v-bind:key="model" name="models" >{{ model }}</option>
            </datalist>

            <b-form-group
                label="Licence plate number" label-for="lpn" class="mb-3"
                description="XX-NNNN-XX format where X is an alphabet chacarter and N is a digit." >
                <b-form-input id="lpn" v-model="form.lpn"
                    placeholder="Licence plate number" v-on:blur="validateLpn()" ></b-form-input>
            </b-form-group>
            <p v-if="errors.includes('lpn')" class="text-red">Licence plate number is required.</p>
            <p v-if="errors.includes('lpnNotValid')" class="text-red">Licence plate number format is not valid.</p>

            <label>Last Registration Date *</label>
            <b-form-datepicker v-model="form.lastRegDate" :max="dateToday" locale="en" name="lastRegDate" class="mb-3"></b-form-datepicker>
            <p v-if="errors.includes('lastRegDate')" class="text-red">Last registration date is required.</p>

            <label>Date of Registration Submition *</label>
            <b-form-datepicker v-model="form.regSubmitionDate" :min="form.lastRegDate" locale="en" name="regSubmitionDate" class="mb-3"></b-form-datepicker>
            <p v-if="errors.includes('regSubmitionDate')" class="text-red">Date of registration submition is required.</p>

             <b-button type="submit" variant="primary">Add Vehicle</b-button>
            </b-form>
    </b-container>
    
</template>


<script>
  export default {
    data() {
      return {
        form: {
          name: null,
          type: null,
          model: null,
          lpn: null,
          lastRegDate: null,
          regSubmitionDate: new Date(),
        },
        errors: [],
        isValid: false,

        // (Car, Van, Truck, Container, Trailer, Dupmer)
        types: [
            { text: 'Select Vehicle Type', value: null },
            { text: 'Car', value: 'Car' }, 
            { text: 'Van', value: 'Van' }, 
            { text: 'Truck', value: 'Truck' }, 
            { text: 'Container', value: 'Container' }, 
            { text: 'Trailer', value: 'Trailer' }, 
            { text: 'Dupmer', value: 'Dupmer' },  
        ],
        models: [ 'Audi', 'BMW', 'Citroen', 'Honda', 'Hyundai', 'Mercedes', 'Nissan', 'Opel', 'Suzuki', 'Toyota', 'Volkswagen'],
        dateToday: new Date()
      }
    },
    methods: {
        onSubmit(evt) {
            evt.preventDefault();
            if (this.isFormValid()) {
                alert(JSON.stringify(this.form));
            }
        },
        // regex for the licence plate number `[A-Za-z][A-Za-z][\-][0-9][0-9][0-9][0-9][\-][A-Za-z][A-Za-z]+`
        validateLpn() {
            const lpnRegex = /^[A-Za-z][A-Za-z][-][0-9][0-9][0-9][0-9][-][A-Za-z][A-Za-z]$/;
            if(lpnRegex.test(this.form.lpn)) {
                console.log(`match`)
                return true;
            } else {
                console.log(`not a match`)
                return false;
            }
        },

        isFormValid() {
            this.errors = [];

            if (!this.form.name) this.errors.push('name');
            if (!this.form.type) this.errors.push('type');
            if (!this.form.lpn) this.errors.push('lpn');
            if (!this.validateLpn(this.form.lpn)) this.errors.push('lpnNotValid');
            if (!this.validateLpn(this.form.lastRegDate)) this.errors.push('lastRegDate');
            if (!this.validateLpn(this.form.regSubmitionDate)) this.errors.push('regSubmitionDate');

            if (!this.errors.length) return true; else return false;
        }
    }
  }
</script>

<style scoped>
    .text-red {
        color: rgb(255, 38, 0);
        font-size: 80%;
    }
</style>