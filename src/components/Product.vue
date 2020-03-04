<template>
  <div class="product">
    <div class="product__image">
      <img v-bind:src="product.image">
    </div>
    <div class="product__info">
      <div class="product__info__title">
        {{ product.title }} <b v-if="product.discount">- Save {{ product.discount }}</b>
      </div>
      <div class="product__info__price">
        <template v-if="product.discount">Was {{ product.price | currency }} | Now {{ (product.price - (product.price * (parseFloat(product.discount.slice(0,-1)/100)))).toFixed(2) | currency }}</template>
        <template v-else>{{ product.price | currency }}</template>
      </div>
      <div class="product__info__description">
        {{ product.description }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Product",
  props: {
    product: {
      type: Object
    }
  },
  filters: {
    currency: function(value) {
      if (!value) {
        return "";
      }
      return new Intl.NumberFormat("en-GB", {
        style: "currency",
        currency: "GBP"
      }).format(value);
    }
  }
};
</script>

<style lang="scss">
.product {
  flex: 0 0 25%;
  position: relative;
  margin-bottom: 20px;
  @media screen and (max-width: 1024px) and (min-width: 801px) {
    flex: 0 0 33.33333%;
  }
  @media screen and (max-width: 800px) and (min-width: 601px) {
    flex: 0 0 50%;
  }
  @media screen and (max-width: 600px) {
    flex: 0 0 100%;
  }

  &__image {
    padding: 0 10px;
    line-height: 0;

    img {
      max-width: 100%;
    }
  }

  &__info {
    position: absolute;
    bottom: 0;
    left: 10px;
    right: 10px;
    padding: 6px 10px 8px;
    text-align: left;
    font-size: 14px;
    background: rgba(0, 0, 0, 0.8);
    color: white;

    &__price,
    &__title {
      font-weight: bold;
    }

    &__description {
      color: #bbb;
    }
  }
}
</style>
