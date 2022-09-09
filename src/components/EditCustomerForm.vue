<template>
  <b-form class="mt-5">
    <h4 class="text-center">Atualize seu contato</h4>
    <b-row class="justify-content-center">
    
        <b-col cols="8">
          <p>Nome:</p>
            <b-form-input
                id="first-name"
                type="text"
                required
                placeholder="First Name"
                v-model="customer.nome"
            ></b-form-input>
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
        <b-col cols="8">
          <p>CPF:</p>
            <b-form-input
                id="cpf"
                required
                v-mask="['###.###.###-##', '##.###.###/####-##']"
                placeholder="000.000.000-00"
                v-model="customer.cpf"
            ></b-form-input>
        </b-col>
      </b-row>
      <b-row class="justify-content-center mt-1">
        <b-col cols="8">
          <p>Telefone:</p>
            <b-form-input
                id="telefone"
                required
                v-mask="['(##) ####-####', '(##) #####-####']"
                placeholder="(99)9999-9999"
                v-model="customer.telefone"
            ></b-form-input>
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
        <b-col cols="8">
          <p>Nome:</p>
            <b-form-input
                id="email"
                type="email"
                required
                placeholder="example@crm.com"
                v-model="customer.email"
            ></b-form-input>
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
        <b-col cols="8">
          <b-form-checkbox
                id="customer_status"
                v-model="customer.lgpd"
                name="customer-status"
                value="active"
                unchecked-value="inactive"
            >
            Você concorda com nossos termos e politica de privacidade?
          </b-form-checkbox>
        </b-col>
      </b-row>
    <b-row class="mt-4 ">
      <b-col cols="5">
      </b-col>
      <b-col>
         <b-button variant="primary" class="px-5 mx-2" @click="updateCustomer"
        >Salvar</b-button
        >
        <b-button variant="warning" @click="triggerClose">Close</b-button>
      </b-col>
    </b-row>
  </b-form>
</template>

<script>
import axios from "axios";

export default {
  name: "CreateCustomerModal",
  props: {
    customerId: Number,
  },
  data() {
    return {
      customer: {},
    };
  },
  mounted() {
    this.getCusomterByID();
  },
  methods: {
    triggerClose() {
      this.$emit("closeEditModal");
    },
    getCusomterByID() {
      axios
          .get(`http://localhost:3000/customers/${this.customerId}`)
          .then((response) => {
            this.customer = response.data;
          })
          .catch((error) => {
            console.log(error);
          });
    },
    updateCustomer() {
      axios
          .put(
              `http://localhost:3000/customers/${this.customerId}`,
              this.customer
          )
          .then((response) => {
            console.log(response.data);
            this.$emit("closeEditModal");
            this.$emit("reloadDataTable");
            this.$emit("showSuccessAlert");
          })
          .catch((error) => {
            console.log(error);
          });
    },
  },
};
</script>