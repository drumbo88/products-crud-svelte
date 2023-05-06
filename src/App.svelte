<script>
  import { v4 } from 'uuid'
  const emptyProduct = {
    id: '',
    name: '',
    description: '',
    category: '',
    imageURL: '',
  }
  let product = { ...emptyProduct }
  let productIndex = null
  const cleanProduct = () => { product = { ...emptyProduct } }

  let products = [
    {
      id: '1',
      name: 'HP Pavilion Notebook',
      description: 'HP Laptop',
      category: 'laptops',
    },
    {
      id: '2',
      name: 'Mouse Razer',
      description: 'Gaming mouse',
      category: 'peripherials',
    },
  ]

  const onSubmitHandler = () => {
    if (productIndex >= 0) {
        updateProduct()
    }
    else {
        addProduct()
    }
  }

  const addProduct = () => {
    products = [...products, { id: v4(), ...product }]
    cleanProduct()
  }

  const editHandler = id => {
    productIndex = products.findIndex(p => p.id === id)
    if (productIndex == -1) {
        alert('Producto no encontrado.')
        return
    }
    product = products[productIndex]
  }
  const updateProduct = () => {
    products[productIndex] = product
    finishUpdate()
  }
  const finishUpdate = () => {
    productIndex = null
    cleanProduct()
  }
  const deleteHandler = id => {
    if (confirm('Are you sure you want to delete this product?'))
        products = products.filter(p => p.id !== id)
  }
</script>

<style>

</style>

<main>

  <div class="container">
    <div class="row">
      <div class="col-md-6">
        {#each products as { id, name, description, imageURL, category }}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4">
                <img src="{imageURL ?? 'images/no-product-image.png'}" alt="" class="img-fluid p-2" />
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5>
                    <strong>{name}</strong>
                    <span>
                      <small>{category}</small>
                    </span>
                  </h5>
                  <p class="card-text">{description}</p>
                  <button class="btn btn-danger" on:click={deleteHandler(id)}>Delete</button>
                  <button class="btn btn-secondary" on:click={editHandler(id)}>Edit</button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit|preventDefault={onSubmitHandler}>
              <div class="form-group">
                <input
                  bind:value={product.name}
                  class="form-control"
                  type="text"
                  placeholder="Product name"
                  id="product-name" />
              </div>
              <div class="form-group">
                <textarea
                  bind:value={product.description}
                  class="form-control"
                  id="product-description"
                  rows="3"
                  placeholder="Product description" />
              </div>
              <div class="form-group">
                <input
                  bind:value={product.imageURL}
                  class="form-control"
                  type="url"
                  placeholder="https://..."
                  id="product-image-url" />
              </div>
              <div class="form-group">
                <select
                  bind:value={product.category}
                  class="form-control"
                  id="category">
                  <option value="laptops">Laptops</option>
                  <option value="peripherials">Periferials</option>
                  <option value="servers">Servers</option>
                </select>
              </div>
              {#if productIndex === null}
                <button class="btn btn-primary" on:click={addProduct}>Save product</button>
              {:else}
                <button class="btn btn-primary" on:click={updateProduct}>Update product</button>
                <button class="btn btn-secondary" on:click={finishUpdate}>Cancel</button>
              {/if}
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>

</main>
