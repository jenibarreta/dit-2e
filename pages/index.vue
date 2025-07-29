<template>
  <v-row justify="center" align="center">
    <v-col cols="12">

      <v-card>
      <v-toolbar color="transparent" elevation="0">
        <v-toolbar-title>List of Customers</v-toolbar-title>
        <v-spacer />
        <v-btn color="primary" @click="createDialog = !createDialog"><v-icon left>mdi-plus</v-icon> Create</v-btn>

      </v-toolbar>
        <v-divider></v-divider>
      <v-data-table
    :headers="headers"
    :items="customers"
    :items-per-page="10"
    class="elevation-1"
    :loading="loading"
    loading-text="Loading...Please wait"
  >
  <template v-slot:item.profilePicture="{ item }">
    <v-badge
    color="purple"
    overlap
    content="VIP"
    offset-x="10"
    offset-y="40"
    bordered
    v-if="item.isVip == true"
    >
      <v-avatar size="40">
        <v-img
         lazy-src="https://picsum.photos/id/11/10/6"
         :src="item.profilePicture">
        </v-img>
      </v-avatar>
    </v-badge>
    <div v-else>
      <v-avatar size="40">
        <v-img
          lazy-src="https://picsum.photos/id/11/10/6"
          :src="item.profilePicture">
        </v-img>
      </v-avatar>
    </div>
      </template>
       <template v-slot:item.actions="{ item }">
        <v-btn color="primary" @click="getCustomersDetails(item)">View</v-btn>
       </template>
    </v-data-table>
    </v-card>

    <v-dialog v-model="customerDialog" width="500">
      <v-card>
        <v-toolbar color="primary">
          <v-toolbar-title><v-icon left>mdi-information</v-icon>Customer Information</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn @click="customerDialog = false" icon><v-icon>mdi-close</v-icon></v-btn>
        </v-toolbar>
        </v-card-title>
        <v-card-text class="mt-5">
        <v-row>
          <v-col cols="12" md="4">
            <v-img max-height="151" max-width="151" :src="customerPhoto"></v-img>
          </v-col>
          <v-col cols="12" md="8">
            <v-list dense>
              <v-list-item two-line>
                <v-list-item-content>
                  <v-list-item-subtitle>Name</v-list-item-subtitle>
                  <v-list-item-title>{{ customerName }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            
              <v-list-item two-line>
                <v-list-item-content>
                  <v-list-item-subtitle>Email</v-list-item-subtitle>
                  <v-list-item-title>{{ customerEmail }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>

              <v-list-item two-line>
                <v-list-item-content>
                  <v-list-item-subtitle>Phone</v-list-item-subtitle>
                  <v-list-item-title>{{ customerPhone }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>

              <v-list-item two-line>
                <v-list-item-content>
                  <v-list-item-subtitle>Age</v-list-item-subtitle>
                  <v-list-item-title>{{ customerAge }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>

            </v-list>
          </v-col>
        </v-row>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer align="center">
          <v-btn
        depressed
        color="dark blue"
        class="my-1"
      >
          UPDATE
    </v-btn>

    <v-btn
    depressed
    color="red"
    >
    DELETE
  </v-btn>
</v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="createDialog" width="500">
      <v-card>
        <v-toolbar>
          <v-toolbar-title><v-icon left>mdi-account</v-icon>Create Customer</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn @click="createDialog = false" icon><v-icon>mdi-close</v-icon></v-btn>
        </v-toolbar>
        <v-card-text>
          <v-form class="mt-5">
            <v-text-field label="Name" outlined></v-text-field>
            <v-text-field label="Email Address" outlined></v-text-field>
            <v-text-field label="Contact Number" outlined></v-text-field>
            <v-textarea outlined label="Address" rows="2"></v-textarea>
            <v-text-field label="Age" type="number" outlined></v-text-field>
            <v-checkbox label="Is VIP"></v-checkbox>
          </v-form>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-btn color="primary" block class="my-5">Submit</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    </v-col>
  </v-row>
  </template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return{
      headers: [
         // {
         //   text: 'ID',
         //   align: 'start',
         //   sortable: false,
         //   value: 'id',
         // },
          { text: 'Photo', value: 'profilePicture' },
          { text: 'Name', value: 'name' },
          { text: 'Email', value: 'email' },
          { text: 'Phone', value: 'phone' },
          { text: 'Address', value: 'address' },
          { text: 'Age', value: 'age'},
          { text: 'Status', value: 'isVip'},
          { text: "", value: 'actions'},
        ],
        customers: [],
        loading: true,
        customerDialog: false,
        createDialog: false,
        customerName: "",
        customerEmail: "",
        customerPhone: "",
        customerAge: 0,
        customerPhoto: ""
    }
  },
  methods: {
    getCustomers() {
      this.$axios.get('/customers')
      .then(response => {
        console.log(response.data);
        this.customers = response.data;
        this.loading = false;
      })
      .catch(err => {
        console.log(err);
      });
    },

    getCustomersDetails(item) {
      console.log(item);

    this.customerName = item.Name;
    this.customerEmail = item.Email;  
    this.customerPhone = item.Phone;
    this.customerAge = item.Age;
    this.customerPhoto = item.Photo;  
    this.customerDialog = true;
    }
  },

  mounted() {
    this.getCustomers();
  },
};
</script>