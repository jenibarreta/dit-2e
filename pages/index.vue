<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-data-table
        :headers="headers"
        :items="customers"
        :items-per-page="10"
        class="elevation-1"
        :loading="loading"
        loading-text="Laoding... Please wait"
      >
        <template v-slot:item.profilePicture="{ item }">

          <v-badge
          color="purple"
          content="VIP"
          v-if="items.isVip == true"
        >
        <v-avatar size="60">
          <v-img
            lazy-src="https://picsum.photos/id/11/10/6"
            :src="item.profilePicture"
          ></v-img>
        </template>
      </v-data-table>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      headers: [
        {
          align: "start",
          sortable: false,
        },
        { text: "Photo", value: "profilePicture" },
        { text: "Name", value: "name" },
        { text: "Email ", value: "email" },
        { text: "Phone ", value: "phone" },
        { text: "Address", value: "address" },
        { text: "Age", value: "age" },
        { text: "Status", value: "isVip" },
      ],
      customers: [],
      loading: true
    };
  },
  methods: {
    getCustomers() {
      this.$axios
        .get("/customers")
        .then((response) => {
          console.log(response.data);
          this.customers = response.data;
          this.loading = false
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },

  mounted() {
    this.getCustomers();
  },
};
</script>
