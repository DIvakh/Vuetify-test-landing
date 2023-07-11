<template>
  <v-container class="container">
    <h1 v-if="!isDataSent">Voice Provisioning</h1>
    <v-form v-if="!isDataSent" ref="form" @submit.prevent="submitForm">
      <v-row>
        <v-col class="py-0 py-md-3" cols="12" md="12">
          <v-select
            v-model="formData.selectedPartner"
            label="Select a Partner"
            :items="partners"
            item-title="name"
            item-value="id"
            variant="solo-filled"
          ></v-select>
        </v-col>
      </v-row>
      <v-row>
        <v-col class="py-0 py-md-3" cols="12" md="12">
          <v-text-field
            :rules="[(v) => !!v || 'This field is required']"
            type="number"
            v-model="formData.accountNumber"
            label="Account Number"
            variant="solo-filled"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col class="py-0 py-md-3" cols="12" sm="6" md="6">
          <v-text-field
            :rules="[(v) => !!v || 'This field is required']"
            v-model="formData.firstName"
            label="First Name"
            variant="solo-filled"
          ></v-text-field>
        </v-col>
        <v-col class="py-0 py-md-3" cols="12" sm="6" md="6">
          <v-text-field
            :rules="[(v) => !!v || 'This field is required']"
            v-model="formData.lastName"
            label="Last Name"
            variant="solo-filled"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col class="py-0 py-md-3" cols="12" sm="6" md="3">
          <v-text-field
            v-model="formData.address"
            label="Address"
            variant="solo-filled"
          ></v-text-field>
        </v-col>
        <v-col class="py-0 py-md-3" cols="12" sm="6" md="3">
          <v-text-field
            v-model="formData.city"
            label="City"
            variant="solo-filled"
          ></v-text-field>
        </v-col>
        <v-col class="py-0 py-md-3" cols="12" sm="6" md="3">
          <v-text-field
            v-model="formData.state"
            label="State"
            variant="solo-filled"
          ></v-text-field>
        </v-col>
        <v-col class="py-0 py-md-3" cols="12" sm="6" md="3">
          <v-text-field
            v-model="formData.zip"
            label="Zip"
            variant="solo-filled"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col class="py-0 py-md-3" cols="12" sm="6" md="6">
          <v-text-field
            v-model="formData.phone"
            label="Contact Phone"
            variant="solo-filled"
            type="tel"
            :rules="[(v) => !!v || 'This field is required']"
          ></v-text-field>
        </v-col>
        <v-col class="py-0 py-md-3" cols="12" sm="6" md="6">
          <v-text-field
            v-model="formData.email"
            :rules="[(v) => !!v || 'This field is required']"
            label="Contact Email"
            variant="solo-filled"
            type="email"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col class="py-0 py-md-3" cols="12" md="4">
          <v-select
            v-model="formData.customerType"
            label="Customer Type"
            placeholder=" "
            :items="customers"
            item-title="name"
            item-value="id"
            variant="solo-filled"
          ></v-select>
        </v-col>
        <v-col class="py-0 py-md-3" cols="12" md="4">
          <v-text-field
            v-model="formData.phoneToProvision"
            label="Phone Number to Provision"
            variant="solo-filled"
            type="phone"
          ></v-text-field>
        </v-col>
        <v-col class="py-0 py-md-3" cols="12" md="4">
          <v-text-field
            v-model="formData.password"
            label="Sip Password"
            variant="solo-filled"
            type="password"
            :rules="[(v) => !!v || 'This field is required']"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col class="py-0 py-md-3" cols="12" md="4"
          ><v-select
            v-model="formData.ported"
            label="Ported"
            placeholder=" "
            :items="portedOptions"
            item-title="name"
            item-value="id"
            variant="solo-filled"
          ></v-select
        ></v-col>
      </v-row>
      <v-row>
        <v-col class="py-0 py-md-3" cols="12" md="4">
          <v-checkbox
            v-model="formData.isBypassChecked"
            label="Bypass E911 Address Automation"
          ></v-checkbox>
        </v-col>
        <v-col class="py-0 py-md-3" cols="12" sm="12" md="8">
          <v-alert
            variant="outlined"
            type="warning"
            icon="$warning"
            lines="two"
            border="top"
            border-color="warning"
            prominent
            text='(By checking "Bypass E911 Address Automation" you agree to manually add this phone number and address to Bandwidth)'
          >
          </v-alert>
        </v-col>
      </v-row>
      <v-row
        ><v-col
          ><v-btn
            :disabled="!isFormValid"
            type="submit"
            variant="flat"
            :color="!isFormValid ? 'lightgray' : 'indigo-darken-4'"
            >PROVISION</v-btn
          ></v-col
        ></v-row
      >
    </v-form>
    <h3 v-else>Your data was successfully sent!</h3>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      sendDataUrl: null,
      isDataSent: false,
      partners: [
        { name: 'Sprout', id: '1' },
        { name: 'Cisco', id: '2' },
        { name: 'Ringostat', id: '3' }
      ],
      customers: [
        { id: 1, name: 'Retail Customer' },
        { id: 2, name: 'Wholesale Customer' },
        { id: 3, name: 'Corporate Customer' },
        { id: 4, name: 'Government Customer' },
        { id: 5, name: 'Nonprofit Customer' }
      ],
      portedOptions: [
        { id: 1, name: 'Number Portability' },
        { id: 2, name: 'Porting Request' },
        { id: 3, name: 'Ported Service Activation' },
        { id: 4, name: 'Ported Service Deactivation' },
        { id: 5, name: 'Ported Service Modification' }
      ],
      requiredFields: [
        'selectedPartner',
        'accountNumber',
        'firstName',
        'lastName',
        'phone',
        'email',
        'password',
        'isBypassChecked'
      ],
      isFormValid: false,
      formData: {
        isBypassChecked: false,
        selectedPartner: null,
        accountNumber: null,
        ported: null,
        customerType: null,
        firstName: null,
        lastName: null,
        phoneToProvision: null,
        password: null,
        email: null,
        phone: null,
        address: null,
        city: null,
        state: null,
        zip: null
      }
    };
  },
  computed: {
    areAllFieldsFilled() {
      for (const field of this.requiredFields) {
        if (!this.formData[field]) {
          return false;
        }
      }
      return true;
    }
  },
  watch: {
    areAllFieldsFilled(value) {
      this.isFormValid = value;
    }
  },
  methods: {
    async submitForm() {
      const dataToSend = {};

      for (const field of Object.keys(this.formData)) {
        if (this.formData[field] && field !== 'isBypassChecked') {
          dataToSend[field] = this.formData[field];
        }
      }
      console.log(dataToSend);
      try {
        const response = await fetch(this.sendDataUrl, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(dataToSend)
        });
        response.ok ? (this.isDataSent = true) : (this.isDataSent = false);
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>
<style lang="scss">
// ====

@import './assets/scss/container.scss';
h1 {
  text-align: center;
  margin-bottom: $x3;
}

h3 {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 50vh;
}
</style>
