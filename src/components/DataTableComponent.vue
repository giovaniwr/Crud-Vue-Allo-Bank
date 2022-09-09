<template>
  <b-container class="card">
    <b-row>
      <b-alert v-model="showSuccessAlert" variant="success" class="alert" dismissible>
        {{ alertMessage }}
      </b-alert>
    </b-row>

    <b-table
        striped
        hover
        :items="items"
        :fields="fields"
        :per-page="perPage"
        :current-page="currentPage"
        class="text-center"
    >
      <template #cell(contact_name)="data">
        {{ `${data.item.contact_firstname} ` }}
      </template>
      <template #cell(customer_status)="data">
        <b-icon-bookmark-check-fill
            variant="success"
            v-if="data.item.customer_status === 'active'"
        ></b-icon-bookmark-check-fill>
        <b-icon-bookmark-x-fill
            variant="danger"
            v-else
        ></b-icon-bookmark-x-fill>
      </template>
      <template #cell(actions)="data">
        <b-row>
          <b-col cols="7">
            <b-icon-pencil-square
                class="action-item"
                variant="primary"
                @click="getRowData(data.item.id)"
            ></b-icon-pencil-square>
          </b-col>
          <b-col cols="1">
            <b-icon-trash-fill
                class="action-item"
                variant="danger"
                @click="deleteCadastro(data.item.id)"
            ></b-icon-trash-fill>
          </b-col>
        </b-row>
      </template>
    </b-table>

    <div class="overflow-auto">
      <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-table"
      ></b-pagination>
    </div>

    <!-- Modal for updating customers -->
    <b-modal
        ref="edit-customer-modal"
        size="xl"
        hide-footer
    >
      <edit-customer-form
          @closeEditModal="closeEditModal"
          @reloadDataTable="getCustomerData"
          @showSuccessAlert="showAlertUpdate"
          :customerId="customerId"
      ></edit-customer-form>
    </b-modal>

  </b-container>
</template>

<script>
import axios from "axios";
import EditCustomerForm from "@/components/EditCustomerForm.vue";

export default {
  components: {
    EditCustomerForm,

  },
  data() {
    return {
      perPage: 7,
      currentPage: 7,
      fields: [
        {
          key: "id",
          label: "",
        },
        {
          key: "nome",
          label: "Nome Completo",
          sortable: false,
        },
        {
          key: "email",
          label: "Email",
          sortable: false,
        },
        {
          key: "telefone",
          label: "Telefone",
          sortable: false,
        },
        {
          key: "cpf",
          label: "CPF",
          sortable: false,
        },
        {
          key: "lgpd",
          label: "Aceite LGPD",
          sortable: false,
        },
        "actions",
      ],
      items: [],
      customerId: 0,

      showSuccessAlert: false,
      alertMessage: "",
    };
  },
  mounted() {
    this.getCustomerData();
  },
  methods: {
    getCustomerData() {
      axios
          .get("http://localhost:3000/customers/")
          .then((response) => {
            this.items = response.data;
          })
          .catch((error) => {
            console.log(error);
          });
    },
    getRowData(id) {
      this.$refs["edit-customer-modal"].show();
      this.customerId = id;
    },
    closeEditModal() {
      this.$refs["edit-customer-modal"].hide();
    },
    showAlertCreate() {
      this.showSuccessAlert = true;
      this.alertMessage = "Customer was created successfully!";
    },
    showAlertUpdate() {
      this.showSuccessAlert = true;
      this.alertMessage = "O Cadastro foi Atualizado";
    },
    deleteCadastro(id) {
         axios
        .delete(`http://localhost:3000/customers/${[id]}`)
        .then((response) => {
            console.log(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  computed: {
    rows() {
      return this.items.length
    }
  }
};
</script>

<style>
.action-item:hover {
  cursor: pointer;
}
.alert{
  width: 100%;
  text-align: center;
}
</style>