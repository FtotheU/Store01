<template>
  <div>
    <b-container fluid>
      <b-row style="margin-bottom:30px; padding-top:30px;">
        <b-col>
          <h2>Products</h2>
        </b-col>
      </b-row>

      <b-row>
        <b-col v-for="product in products" :key="product.id" style="my-3">
          <b-card
            :title="product.name"
            :title-size="h4"
            :img-src="require(`@/assets/pic${product.id}.jpeg`)"
            img-alt="Card image"
            img-left
            class="mb-3 ml-3 card"
            style="width:450px; height:10rem;"
            img-fluid
          >
            <b-row>
              <b-col md="6">
                <b-card-text> {{ product.price }} ₩ </b-card-text>
              </b-col>
              <b-col md="6">
                <b-button
                  v-if="!product.cart"
                  @click="add(product)"
                  href="#"
                  variant="outline-success"
                  size="sm"
                  pill
                  style="width:70px;"
                  >Add</b-button
                >
                <b-button
                  v-if="product.cart"
                  @click="add(product)"
                  :disabled="product.cart"
                  href="#"
                  variant="success"
                  size="sm"
                  pill
                  style="width:70px;"
                  >Added
                </b-button>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
      </b-row>

      <b-row style="margin-bottom:30px;">
        <b-col>
          <h2>Shopping Cart</h2>
        </b-col>
      </b-row>

      <b-row style="margin-bottom:30px;">
        <b-col>
          <b-table bordered hover :items="cart" :fields="fields">
            <template slot="#" slot-scope="data">
              {{ data.index + 1 }}
            </template>
            <template slot="price" slot-scope="data">
              {{ data.item.price * data.item.quantity }}
            </template>
            <template slot="remove" slot-scope="data">
              <b-button
                @click="remove(data.item.id)"
                variant="danger"
                class="mr-2"
              >
                X
              </b-button>
            </template>
            <template slot="quantity" slot-scope="data">
              <b-row>
                <b-col cols="5">
                  <b-button
                    :disabled="data.item.quantity <= 1"
                    variant="primary"
                    @click="decrement(data.item.id)"
                    class="mr-2"
                  >
                    -
                  </b-button>
                </b-col>
                <b-col cols="2">
                  <h4>{{ data.item.quantity }}</h4>
                </b-col>
                <b-col cols="5">
                  <b-button
                    variant="primary"
                    @click="increment(data.item.id)"
                    class="mr-2"
                  >
                    +
                  </b-button>
                </b-col>
              </b-row>
            </template>

            <template slot="image" slot-scope="data">
              <b-img
                style="max-width: 5rem;"
                :src="require(`@/assets/pic${data.item.id}.jpeg`)"
                fluid
                alt="Responsive image"
              ></b-img>
            </template>
          </b-table>
        </b-col>
      </b-row>
      <b-row v-if="cart.length > 0">
        <b-col></b-col>
        <b-col></b-col>
        <b-col></b-col>
        <b-col></b-col>
        <b-col><h3>Total</h3></b-col>
        <b-col
          ><h3> {{ total }}.00 ₩</h3></b-col
        >
      </b-row>
      <b-row v-if="cart.length > 0">
        <b-col>
          <b-button @click="clean" variant="info" block class="mr-2">
            Clean
          </b-button>
        </b-col>
        <b-col></b-col>
        <b-col cols="4"></b-col>

        <b-col> </b-col>
        <b-col>
          <b-button @click="buy" variant="success" block class="mr-2">
            Purchase
          </b-button>
        </b-col>
      </b-row>
      <b-modal
        hide-header-close
        no-close-on-esc
        no-close-on-backdrop
        ref="modal-1"
        centered
        title="Purchase Completed"
      >
        <template slot="modal-footer">
          <b-button class="mt-3" variant="success" block @click="clean"
            >Accept</b-button
          >
        </template>
        <p class="my-4">Products</p>
        <ul v-for="productFinal in ticket.products" :key="productFinal.id">
          <li>Product name: {{ productFinal.name }}</li>
          <li>Quantity: {{ productFinal.quantity }}</li>
          <li>Price: {{ productFinal.price }}</li>
          <li>Total: {{ productFinal.price * productFinal.quantity }}</li>
          <hr />
        </ul>
        <h2 class="my-4">Total: {{ ticket.total }}.00 ₩</h2>
      </b-modal>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "Cart",
  props: {
    msg: String,
  },
  data() {
    return {
      ticket: {
        products: null,
        total: 0,
      },
      counter: 0,
      products: [
        {
          id: 1,
          img: "@/assets/pic1.jpeg",
          name: "Cravity season 1. hideout: remember who we are",
          price: 14900,
          cart: false,
          quantity: 1,
        },
        {
          id: 2,
          img: "@/assets/pic2.jpeg",
          name: "Cravity season 2. hideout: the new day we step into",
          price: 14900,
          cart: false,
          quantity: 1,
        },
        {
          id: 3,
          img: "@/assets/pic3.jpeg",
          name: "Cravity season 3. hideout: be our voice",
          price: 16300,
          cart: false,
          quantity: 1,
        },
        {
          id: 4,
          img: "@/assets/pic4.jpeg",
          name: "Cravity light stick",
          price: 30000,
          cart: false,
          quantity: 1,
        },
        {
          id: 5,
          img: "@/assets/pic5.jpeg",
          name: "Cravity season's greeting",
          price: 29900,
          cart: false,
          quantity: 1,
        },
        {
          id: 6,
          img: "@/assets/pic6.jpeg",
          name: "Oneus : Light us",
          price: 14500,
          cart: false,
          quantity: 1,
        },
        {
          id: 7,
          img: "@/assets/pic7.jpeg",
          name: "Oneus : Raise us",
          price: 14500,
          cart: false,
          quantity: 1,
        },
        {
          id: 8,
          img: "@/assets/pic8.jpeg",
          name: "Oneus : Fly with us",
          price: 14500,
          cart: false,
          quantity: 1,
        },
        {
          id: 9,
          img: "@/assets/pic9.jpeg",
          name: "Oneus : Time with us",
          price: 14900,
          cart: false,
          quantity: 1,
        },
        {
          id: 10,
          img: "@/assets/pic10.jpeg",
          name: "Oneus : Lived",
          price: 16300,
          cart: false,
          quantity: 1,
        },
        {
          id: 11,
          img: "@/assets/pic10.jpeg",
          name: "Oneus : Devil",
          price: 16300,
          cart: false,
          quantity: 1,
        },
        {
          id: 12,
          img: "@/assets/pic10.jpeg",
          name: "Oneus light stick",
          price: 16300,
          cart: false,
          quantity: 1,
        },
      ],
      cart: [],
      fields: ["#", "image", "name", "price", "quantity", "remove"],
    }; // data return
  },
  methods: {
    add(product) {
      this.products[product.id - 1].cart = true;
      this.cart.push(product);
      this.counter++;
    },
    clean() {
      this.cart = [];

      for (const key in this.products) {
        this.products[key].cart = false;
        this.products[key].quantity = 1;
      }
      this.$refs["modal-1"].hide();
    },
    remove(id) {
      for (let index = 0; index < this.products.length; index++) {
        if (this.products[index].id == id) {
          this.products[index].cart = false;
        }
      }
      for (let index = 0; index < this.cart.length; index++) {
        if (this.cart[index].id == id) {
          this.cart.splice(index, 1);
        }
      }
    },
    buy() {
      this.ticket = {
        products: this.cart,
        total: this.total,
      };
      this.$refs["modal-1"].show();
    },
    increment(id) {
      for (let index = 0; index < this.cart.length; index++) {
        if (this.cart[index].id == id) {
          this.cart[index].quantity++;
        }
      }
    },
    decrement(id) {
      for (let index = 0; index < this.cart.length; index++) {
        if (this.cart[index].id == id) {
          this.cart[index].quantity--;
        }
      }
    },
  },
  computed: {
    total() {
      let t = 0;
      for (let index = 0; index < this.cart.length; index++) {
        t += this.cart[index].price * this.cart[index].quantity;
      }
      return t;
    },
  },
};
</script>

<style scoped></style>
