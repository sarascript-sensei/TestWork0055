<template>
  <div class="col-md-4">
    <div class="thumbnail">
      <img src="https://cdn2.iconfinder.com/data/icons/marketing-25/64/product_presentation-256.png" alt="">
      <div class="caption">
        {{ product.user_id }}
        <h3>{{ product.name }}</h3>
        <p>{{ product.price }} €</p>
        <p>{{ product.description}}</p>
        <p>
          <a href="#" class="btn btn-success">
            Order
          </a>

          <hr>

          <p v-if="product.user_id == authenticatedUser.id">
            <button @click="$emit('delete-product')" class="btn btn-danger">
              Delete
            </button>
            <router-link
              class="btn btn-success"
              :to="'/products/' + product.id + '/edit'">
              Edit
            </router-link>
          </p>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    props: ['product', 'authenticatedUser'],
  }
</script>
<style>
</style>
