<template>
  <div>
    <my-header></my-header>
    <h1> This is the id {{ $route.params.id}}</h1><!-- so it can retrieve id from url -->
    <div class="row">
      <div class="col-md-5 col-md-offset-0">
        <figure>
          <img class="product" v-bind:src="product.image" >
        </figure>
      </div>
      <div class="col-md-6 col-md-offset-0 description">
        <h1>{{product.title}}</h1>
        <p v-html="product.description"></p>
        <p class="price">
          {{product.price }}
        </p>
        <button @click="edit">Edit Product</button>
        <router-view></router-view><!-- if new routes come in, the corresponding component goes here wihtout refreshing the page -->
      </div>
    </div>
  </div>
  </template>
  <script>
  import MyHeader from './Header.vue'
  export default {
    components: { MyHeader },
    data() {
    return {
      product: '' // though being empty, created hook will populate it with product
    }
  },
  methods: {
      edit() {
        this.$router.push({name: 'Edit'})
      }
  },
  created: function() {
    axios.get('/static/products.json')
    .then((response) =>{
      this.product = response.data.products.filter(data => data.id == this.$route.params.id)[0]
      this.product.image = '/' + this.product.image;
    });
  }
}
</script>
