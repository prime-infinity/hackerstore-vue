<template>
  <div class="layout-row">
    <ProductList v-on:add-to-cart="addToCart" v-on:remove-from-cart="removeFromCart" class="flex-70" :products="products" />
    <Cart v-on:coupon-changed="couponChanged" class="flex-30" :cart="cart" />
  </div>
</template>

<script>
import ProductList from "@/components/ProductList";
import Cart from "@/components/Cart";

export default {
  name: "Checkout",
  components: {Cart, ProductList},
  data() {
    return {
      cart: {
        items: [],
        subTotal: 0,
        totalPrice: 0,
        discount: 0,
        selectedCoupon: 0
      },
      products: []
    }
  },
  created() {
    this.products = PRODUCTS.map((product, index) => {
      product.id = index + 1;
      product.image = `/images/items/${product.name.toLocaleLowerCase()}.png`;
      product.cartQuantity = 0;
      return product;
    });
  },
  methods: {
    addToCart: function(product) {
      //console.log(product);
      //first, find the item in cart by its id
      const index = this.products.findIndex(p => p.id === product.id);

      //then, set the cartQuantity to 1
      this.products[index].cartQuantity = 1;

      //then push a new item into the cart
      this.cart.items.push({
        id: this.products[index].id,
        item: this.products[index].heading,
        price: this.products[index].price,
        quantity: 1
      });

      //add item price to cart subTotal
      this.cart.subTotal = this.cart.subTotal + this.products[index].price

      //update total price
      this.cart.totalPrice = this.cart.subTotal - this.cart.discount
    },
    removeFromCart: function(product) {
      //console.log(product);
      //first, find the item in cart by its id
      const index = this.products.findIndex(p => p.id === product.id);
      //then, set the cartQuantity to 0
      this.products[index].cartQuantity = 0;

      //find the item in the cart with the id
      const cartIndex = this.cart.items.findIndex(c => c.id === product.id);
      //remove the item from cart
      this.cart.items.splice(cartIndex, 1);

      //subtract item price to cart subTotal
      this.cart.subTotal = this.cart.subTotal - this.products[index].price

      //update total price
      this.cart.totalPrice = this.cart.subTotal - this.cart.discount

    },
    couponChanged: function(coupon) {
      //set cart coupon discount
      this.cart.discount = coupon
      this.cart.totalPrice = this.cart.subTotal - this.cart.discount
    },
  }
}
export const PRODUCTS = [
  {
    heading: "Cap - $10",
    name: "Cap",
    price: 10
  },
  {
    heading: "Hand Bag - $30",
    name: "HandBag",
    price: 30
  },
  {
    heading: "Shirt - $30",
    name: "Shirt",
    price: 30
  },
  {
    heading: "Shoes - $50",
    name: "Shoe",
    price: 50
  },
  {
    heading: "Pant - $40",
    name: "Pant",
    price: 40
  },
  {
    heading: "Slipper - $20",
    name: "Slipper",
    price: 20
  }
]

</script>

<style>
.flex-70 {
  flex: 0 0 70%;
  width: 70%;

}

.flex-30 {
  flex: 0 0 30%;
  width: 30%;
}
</style>