<template>
  <div>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">#</th>
            <th class="text-left">Image</th>
            <th class="text-left">Title</th>
            <th class="text-left">Description</th>
            <th class="text-left">Price</th>
            <th class="text-left">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="product in products.slice(
              (page - 1) * perPage,
              page * perPage
            )"
            :key="product.id"
          >
            <td>{{ product.id }}</td>
            <td>
              <v-img
                :src="product.image"
                max-width="80"
                max-height="80"
              ></v-img>
            </td>
            <td>{{ product.title }}</td>
            <td>{{ product.description }}</td>
            <td>{{ product.price }}</td>
            <td></td>
            <td>
              <v-btn color="error" @click="deleteProduct(product.id)"
                >Delete</v-btn
              >
              >
            </td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>

    <div class="text-center">
      <v-pagination
        v-model="page"
        total-visible="7"
        :length="lastPage"
      ></v-pagination>
    </div>
  </div>
</template>

<script lang="ts">
import axios from 'axios';
export default {
  name: 'Products',
  data() {
    return {
      products: [],
      page: 1,
      perPage: 10,
      lastPage: 0,
    };
  },
  async mounted() {
    const { data } = await axios.get('products');
    this.products = data;
    this.lastPage = Math.max(data.length / this.perPage);
  },
  methods: {
    async deleteProduct(id: number) {
      if (confirm('Are you sure you want to delete')) {
        await axios.delete(`products/${id}`);

        this.products = this.products.filter((p) => p.id !== id);
      }
    },
  },
};
</script>

<style></style>
