<template>
  <div>
    <div class="container">
      <div class="main-panel">
        <img
          class="product-image"
          :src="product.imageUrl"
          :alt="product.name"
        />
      </div>
      <div class="side-panel">
        <p class="name">{{ product.name }}</p>
        <p class="price">{{ product.price }}</p>
        <button type="button" @click="addToCart">카트에 담기</button>
      </div>
    </div>
  </div>
</template>

<script>
import { createCartItem, fetchProductById } from '@/api';
export default {
  async asyncData(context) {
    const response = await fetchProductById(context.params.id);
    const product = response.data;
    return { product };
  },
  head() {
    return {
      title: `Shopping Item Detail - ${this.product.name}`,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: `이 상품은 - ${this.product.name}입니다.`,
        },
        {
          hid: 'og:title',
          name: 'og:title',
          content: `Shopping Item Detail - ${this.product.name}`,
        },
        {
          hid: 'og:description',
          name: 'og:description',
          content: `이 상품은 - ${this.product.name}입니다.`,
        },
        {
          hid: 'og:image',
          name: 'og:image',
          content: 'http://placeimg.com/640/480/fashion',
        },
      ],
    };
  },
  methods: {
    async addToCart() {
      const response = await createCartItem(this.product);
      console.log(response);

      // this.$store.commit('addCartItem', this.product);
      this.$router.push('/cart');
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
}
.product-image {
  width: 500px;
  height: 375px;
}
.side-panel {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 220px;
  text-align: center;
  padding: 0 1rem;
}
</style>
