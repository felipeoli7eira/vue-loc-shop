<template>
  <div id="app">

    <div class="container mt-5">

      <div class="text-center mx-auto p-3 rounded shadow my-5 title-screen">
        <h2>CRUD de produtos</h2>
      </div>

      <div class="row shop">

        <div class="col col-4 p-4 rounded bg-white shadow">
          <form class="product-data needs-validation" novalidate>
            <div class="form-group mb-2">
              <label>Produto</label>
              <input
                v-model="product.name"
                type="text"
                class="form-control input-prod-name"
                placeholder="..."
                required
              />
              <div class="invalid-feedback">O produto precisa de um nome</div>
            </div>

            <div class="form-group mb-2">
              <label>Descrição</label>
              <input
                v-model="product.desc"
                type="text"
                class="form-control input-prod-desc"
                placeholder="..."
                required
              />
              <div class="invalid-feedback">Descreva o produto</div>
            </div>

            <div class="form-group mb-2">
              <label>Preço</label>
              <input
                v-model="product.price"
                type="text"
                class="form-control input-prod-price"
                placeholder="R$ 00,00"
                required
              />
              <div class="invalid-feedback">Precifique o produto</div>
            </div>

            <button
              @click="submit()"
              type="button"
              class="btn btn-primary">
              {{create ? 'salvar' : 'atualizar'}}
            </button>
          </form>
        </div>

        <div class="col col-8">
          <div class="text-center">
            <h4 class="rounded shadow p-3 bg-white">Lista de produtos</h4>
          </div>

          <table class="table table-striped">
              <thead>
                <tr>
                  <th class="bg-white border-0 rounded" scope="col">produto</th>
                  <th class="bg-white border-0 rounded" scope="col">descrição</th>
                  <th class="bg-white border-0 rounded" scope="col">preço</th>
                  <th class="bg-white border-0 rounded" scope="col">ação</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="product in products" :key="product._id">
                  <td>{{product.name}}</td>
                  <td>{{product.desc}}</td>
                  <td>R$ {{product.price}}</td>
                  <td>
                    <button @click="destroy(product._id)" class="btn btn-danger btn-sm me-1">apagar</button>
                    <button @click="edit(product._id)" class="btn btn-primary btn-sm">atualizar</button>
                  </td>
                </tr>
              </tbody>
            </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

  /* eslint-disable */

  export default {
    name: 'App',
    data() {
      return {
        create: true,

        product: {
          _id: undefined,
          name: undefined,
          desc: undefined,
          price: ''
        },

        products: []
      }
    },

    methods: {

      save()
      {
        localStorage.__products__ = JSON.stringify(this.products)
      },

      submit() // create and update
      {
        if (! this.product.name || ! this.product.desc || ! this.product.price)
          document.querySelector('form').classList.add('was-validated')

        if (this.create === true)
        {
          this.product._id = new Date().getTime()
          this.products.push(this.product)
          this.save()
          this.resetForm()
        }
        else
        {
          this.products.forEach( (product, index) => {

            if (product._id === this.product._id)
              this.products[ index ] = this.product
          })

          this.save()
          this.resetForm()
        }
      },

      getProducts()
      {
        this.products = localStorage.__products__ === undefined ? [] : JSON.parse(localStorage.__products__)
      },

      destroy(id)
      {
        this.products = this.products.filter(product => product._id !== id)
        this.save()
        this.resetForm()
      },

      edit(id)
      {
        this.create = false
        let productForEdit = this.products.filter(product => product._id == id)
        this.product = productForEdit [0]
      },

      resetForm()
      {
        this.product = { _id: undefined, name: undefined, desc: undefined, price: '' }
        this.create = create
      }
    },

    created()
    {
      this.getProducts()
    }
  }
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Raleway:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

*
{
  font-family: 'Raleway', sans-serif;
}

body
{
  background-color: #eaeaea !important;
}

.title-screen
{
  background-color: #fff;
}

</style>
