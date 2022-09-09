<template cla>
  <div class="container mt-5 mx-3 bg" >
    <b-col class="mt-5">
      <b-row class="justify-content-center">
        <b-col cols="8">
          <h4 class="my-3 text-white font-weight-bold">Complete os campos  para finalizar seu cadastro</h4> <br/>
          <b-form-group
              id="first-name"
              class="text-white font-weight-bold"
              label="Nome Completo"
              label-for="first-name">
            <b-form-input
                id="first-name"
                type="text"
                required
                placeholder="First Name"
                v-model="customer.nome"
            ></b-form-input>
          </b-form-group>
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
        <b-col cols="8">
          <b-form-group
              id="cpf"
              label="CPF: "
              class="text-white font-weight-bold"
              label-for="cpf">
            <b-form-input
                id="cpf"
                required
                v-mask="['###.###.###-##', '##.###.###/####-##']"
                placeholder="000.000.000-00"
                v-model="customer.cpf"
            ></b-form-input>
          </b-form-group>
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
        <b-col cols="8">
          <b-form-group
              id="telefone"
              label="Telefone: "
              class="text-white font-weight-bold"
              label-for="telefone">
            <b-form-input
                id="telefone"
                required
                v-mask="['(##) ####-####', '(##) #####-####']"
                placeholder="(99)9999-9999"
                v-model="customer.telefone"
            ></b-form-input>
          </b-form-group>
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
        <b-col cols="8">
          <b-form-group
              id="email"
              label="E-Mail"
              class="text-white font-weight-bold"
              label-for="email">
            <b-form-input
                id="email"
                type="email"
                required
                placeholder="example@crm.com"
                v-model="customer.email"
            ></b-form-input>
          </b-form-group>
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
        <b-col cols="8" class="mt-2">
          <b-form-checkbox
              id="lgpd"
              v-model="customer.lgpd"
              name="lgpd"
              value="active"
              requiered
              unchecked-value="inactive"
              class="text-white font-weight-bold"
          >
           Você concorda com nossos termos e politica de privacidade?
          </b-form-checkbox>
          
        </b-col>
      </b-row>
      <b-row class="justify-content-center">
          
          <b-col class="mt-5 p-3" cols="0">
          
            <b-button variant="primary" class="px-5" @click="addNewCustomer"
            >Cadastrar
            </b-button
            >
          </b-col>
      </b-row>
    </b-col>
  </div>
</template>

<script>
import axios from "axios";
import {mask} from 'vue-the-mask'

export default {
  name: "CreateCustomerModal",
  data() {
    return {
      success: false,
      customer: {},
    };
  },
  directives: {
        mask
    },
  methods: {
    addNewCustomer() {
      axios
          .post("http://localhost:3000/customers/", this.customer)
          .then((response) => {
            console.log(response.data);
            this.$emit("reloadDataTable");
            this.$emit("showSuccessAlert");
            this.success = true
            this.$router.push('/cadastros')
          })
          .catch((error) => {
            console.log(error);
          });
    },
  },
};
</script>