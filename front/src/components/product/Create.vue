<template>
  <div class="row">
    <div class="col-md-8 col-md-offset-2">
      <div class="panel panel-default">
        <div class="panel-body">
          <form @submit.prevent="create">
            <div class="form-group">
              <label for="name">Name:</label>
              <input name="name" type="text" class="form-control"
                v-validate="'required'"
                v-model="product.name">
                <div class="help-block alert alert-danger" v-show="errors.has('name')">
                  {{ errors.first('name') }}
                </div>
            </div>
            <div class="form-group">
              <label for="price">Price:</label>
              <input name="price" type="number" class="form-control" v-model="product.price"
                v-validate="'max_value:100000|min_value:1'"
                >
              <div class="help-block alert alert-danger" v-show="errors.has('price')">
                {{ errors.first('price') }}
              </div>
            </div>
            <div class="form-group">
              <label for="description">Description:</label>
              <textarea type="text" class="form-control" v-model="product.description"></textarea>
            </div>
            <input type="submit" class="btn btn-success pull-right" value="create">
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      product: {
        name: '',
        price: 0,
        description: ''
      }
    }
  },
  methods: {
    create () {
        this.$http.post('api/products', this.product)
            .then(response => {
              this.$router.push('/feed');
      })
    }
  }
}
</script>
