<template>
  <div>
    <div style="justify-content: flex-end; text-align: flex-end">
      <b-container>
        <b-card
          no-body
          class="overflow-hidden"
          style="
            max-width: 100%;
            max-height: 20%;
            margin-top: 10%;
            justify-content: center;
            align-items: center;
          "
        >
          <b-row no-gutters>
            <b-col>
              <b-row md="12">
                <b-card-img
                  :src="src"
                  alt="Image"
                  style="max-width: 50%"
                  class="rounded-0"
                ></b-card-img>
              </b-row>
              <b-button-group
                style="justify-content: center; align-items: center"
              >
                <b-card-img
                  v-for="img in Product.images"
                  :key="img.id"
                  @click="setSrc(img.url)"
                  :src="img.url"
                  alt="Image"
                  style="max-width: 40px"
                  class="rounded-0"
                ></b-card-img>
              </b-button-group>
            </b-col>

            <b-col
              md="3"
              style="justify-content: flex-end; align-items: flex-end"
            >
              <div class="title">
                <b-card-body :title="Product.title">
                  <b-card-text>
                    {{ Product.store_category_title }}
                  </b-card-text>

                  <b-card-text v-if="Product.desc !== desc">
                    Description: {{ Product.desc }}
                  </b-card-text>
                  <b-card-text> Price: ${{ Product.price }} </b-card-text>
                </b-card-body>
              </div>
            </b-col>
            <Form :idProduct="Product.id" />
          </b-row>
        </b-card>
        <div><b-button @click="navigate" variant="danger">Back</b-button></div>
      </b-container>
    </div>
  </div>
</template>
<script>
import router from "../router";
import Form from "@/components/Form.vue";
export default {
  name: "about",
  data() {
    return {
      Product: [],
      slide: 0,
      sliding: null,
      src: "",
       desc: "",
    };
  },
  components: {
    Form,
  },
  methods: {
    navigate() {
      router.go(-1);
    },
    setSrc(url) {
      this.src = url;
    },
  },
  created() {
    this.Product = this.$route.params.Product;
    if (this.Product === undefined) {
      router.go(-1);
    }
    this.Product.images[0] !== undefined
      ? (this.src = this.Product.images[0].url)
      : (this.src =
          "https://ecommerce.laraship.com/assets/corals/images/default_product_image.png");
  },
};
</script>
<style>
#Img {
  font-size: 20;
}
.title {
  margin: 1%;
  margin-top: 10%;
  max-height: 20%;
}
.view {
  margin: 1%;
  margin-top: 15%;
}
.btnSend {
  margin-top: 5%;
}
</style>
