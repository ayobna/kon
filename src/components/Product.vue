<template>
  <b-card no-body class="mb-3 mt-5" style="max-width: 300px; max-hight: 300px">
    <p>{{ prod.title }}</p>
    <b-card-img
      id="image"
      :src="
        prod.images[0]
          ? prod.images[0].url
          : `https://ecommerce.laraship.com/assets/corals/images/default_product_image.png`
      "
    />
    <b-card-text v-if="prod.desc !== desc">
      Description: {{ prod.desc }}
    </b-card-text>

    <b-card-text> Price: ${{ prod.price }} </b-card-text>

    <b-button @click="viewProduct" variant="primary">About</b-button>
  </b-card>
</template>

<script>
export default {
  name: "Product",
  props: ["prod"],
  data() {
    return {
      desc: "",
    };
  },
  methods: {
    viewProduct() {
      this.$router.push({ name: "About", params: { Product: this.prod } });
    },
  },
};
</script>

<style scoped>
#image {
  width: 200px;
  height: 200px;
  min-width: 200px;
  min-height: 200px;
}
</style>
