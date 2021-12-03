<template>
  <div id="Home">
    <div v-if="loading" class="loading" style="margin-top: 20%">Loading...</div>
    <div v-else>
      <b-container class="bv-example-row">
        <b-input-group>
          <b-form-input
            type="search"
            v-model="search"
            placeholder="Search by name"
          >
          </b-form-input>
          <b-dropdown v-model="selected" :text="DropdownText">
            <b-dropdown-item
              :value="AllProudct"
              @click="onChange(AllProudct)"
              >{{ AllProudct }}</b-dropdown-item
            >

            <b-dropdown-item
              v-for="(category, key) in CategoryList"
              :key="key"
              :value="category"
              @click="onChange(category)"
              >{{ category }}</b-dropdown-item
            >
          </b-dropdown>
        </b-input-group>
        <h1>{{ selected }}</h1>
        <b-row class="justify-content-md-center">
          <Product
            v-for="product in filteredProduct"
            :key="product.id"
            :prod="product"
          />
        </b-row>
      </b-container>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Product from "@/components/Product.vue";

export default {
  name: "Home",

  data: function () {
    return {
      products: [],
      productsTemp: [],
      findProduct: "",
      search: "",
      CategoryList: [],
      selected: "",
      DropdownText: "Select by Category",
      AllProudct: "כל המוצרים",
      loading: false,
      post: null,
      error: null,
    };
  },
  components: {
    Product,
  },
  beforeMount: async function () {
    this.loading = true;

    try {
      let response = await fetch(
        "https://api.konimbo.co.il/v1/items?token=9c1a92bf8cefc59e4ec9fa7c53bba0f90dd8b15850bef1062dbf32c5e8fd3a08"
      );
      let data = await response.json();
      if (data != null) {
        this.products = data;
        this.productsTemp = data;
        this.CategoryList = new Set(
          data.map((item) => item.store_category_title)
        );
      }
      this.loading = false;
    } catch (err) {
      this.error = err.message;
      alert(this.error);
    }
  },
  computed: {
    filteredProduct: function () {
      var filtering = new RegExp(this.search, "i");
      return this.products.filter((product) => {
        return product.title.match(filtering);
      });
    },
  },
  methods: {
    onItem4Click(evt) {
      console.log("click item 4", evt);
    },
    onChange(value) {
      this.products = this.productsTemp;
      this.selected = value;
      this.DropdownText = value;
      if (this.selected != "" && this.selected !== this.AllProudct) {
        this.products = this.products.filter((product) => {
          return product.store_category_title === this.selected;
        });
      }
    },
  },
};
</script>
<style>
#Home {
  justify-content: center;
  text-align: center;
  flex-direction: row;
  margin-top: 20;
}
</style>
