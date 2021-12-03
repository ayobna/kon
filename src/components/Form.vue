<template>
  <b-col md="4">
    <div class="view">
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          id="input-group-1"
          label="Email address:"
          label-for="input-1"
        >
          <b-form-input
            id="input-1"
            v-model="form.email"
            type="email"
            placeholder="Enter email"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
          <b-form-input
            id="input-2"
            v-model="form.name"
            placeholder="Enter name"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-3"
          label="Your phone:"
          label-for="input-3"
        >
          <b-form-input
            id="input-3"
            v-model="form.phone"
            type="tel"
            placeholder="Enter phone"
            required
          ></b-form-input>
        </b-form-group>
        <div class="btnSend">
          <b-button type="submit" variant="primary">Send</b-button>
        </div>
      </b-form>
    </div>
  </b-col>
</template>

<script>
export default {
  name: "Form",
  props: ["idProduct"],
  data() {
    return {
      desc: "",
      form: {
        email: "",
        name: "",
        phone: "",
        desc: "",
      },
      show: true,
    };
  },
  methods: {
    async viewProduct() {
      try {
        const requestOptions = {
          mode: "no-cors",
          method: "POST",

          headers: {
            "Content-Type": "application/json",
            "Access-Control-Allow-Origin": "*",
          },
          body: JSON.stringify({
            id_Product: this.idProduct,
            name: this.form.name,
            email: this.form.email,
            phone: this.form.phone,
          }),
        };
        let response = await fetch(
          "	http://webhook.site/611974d4-2ef3-4e68-8fb5-05f948d3769c",
          requestOptions
        );
        let data = await response.json();
        console.log(`data`, data);
           alert("נשלח בהצלחה");
              alert(data);
        if (data === 1) {
          alert("נשלח בהצלחה");
        }
      } catch (err) {
        this.postResult = err.message;
      }
    },
    onSubmit(event) {
      event.preventDefault();
      this.viewProduct();

      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
        this.onReset(event);
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";
      this.form.phone = "";
    },
  },
  created() {
    this.Product = this.$route.params.Product;

    this.Product.images[0] !== undefined
      ? (this.src = this.Product.images[0].url)
      : (this.src =
          "https://ecommerce.laraship.com/assets/corals/images/default_product_image.png");
  },
};
</script>
