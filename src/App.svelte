<script>
  import { v4 } from 'uuid';
  import Noty  from 'noty';
  import 'noty/lib/noty.css';
  import 'noty/lib/themes/sunset.css';
  let estadoEditar = false;
  let products = [
    {
      id: 1,
      name: "Audifonos",
      descripcion: "Diadema",
      categoria: "Tecnologia",
      url: "https://advance.com.ec/26501-large_default/038001mavhg8929.jpg",
    },
    {
      id: 2,
      name: "Macbook Pro 16'",
      descripcion: "Retina display",
      categoria: "Tecnologia",
      url:
        "https://www.apple.com/v/macbook-pro-16/b/images/meta/og__csakh451i0eq_large.png?202006121746",
    },
  ];

  let product = {
    id: "",
    name: "",
    descripcion: "",
    categoria: "",
    url: "",
  };

  const limpiar = () => {
    product.id = "";
    product.name = "";
    product.descripcion = "";
    product.categoria = "";
    product.url = "";
  };

  const guadar = () => {
    console.log(product);
    const newProduct = {
      id: v4(),
      name: product.name,
      descripcion: product.descripcion,
      categoria: product.categoria,
      url: product.url,
    };
    products = products.concat(newProduct);
    limpiar();
    console.log(products);
  }

  const update = () => {
	  let editado = {
		id: product.id,
      	name: product.name,
      	descripcion: product.descripcion,
      	categoria: product.categoria,
      	url: product.url,
	  }
	  const i = products.findIndex(p => p.id === product.id);
	  products[i] = editado;
  }

  const onSubmit = (e) => {
	if(estadoEditar) {
		update();
		estadoEditar = false;
		limpiar();
		new Noty({
			theme: 'sunset',
			type: 'success',
			timeout: 3000,
			text: 'Producto Actualizado Satisfactoriamente'
		}).show();
	} else {
		guadar();
	}
  };

  const eliminar = (id) => {
    // se añaden los productos menos el que coincida por el id
    products = products.filter((pro) => pro.id !== id);
  };

  const editar = (productEditado) => {
	estadoEditar = true;
	product = productEditado;
  };

</script>

<style>

</style>

<main>

  <div class="container p-4">
    <div class="row">
      <div class="col-md-6">
        {#each products as pro}
          <div class="card">
            <div class="row">
              <div class="col-md-4">
                {#if pro.url}
                  <img src={pro.url} class="img-fluid p-2" alt="producto" />
                {:else}
                  <img
                    src="img/no-product.png"
                    class="img-fluid p-2"
                    alt="producto" />
                {/if}
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5>
                    <strong>{pro.name}</strong>
                    <span>
                      <small>{pro.categoria}</small>
                    </span>
                  </h5>
                  <p class="card-text">{pro.descripcion}</p>
                  <button on:click={eliminar(pro.id)} class="btn btn-danger">
                    Eliminar
                  </button>
                  <button on:click={editar(pro)} class="btn btn-success">
                    Editar
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>

      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit|preventDefault={onSubmit}>
              <div class="form-group">
                <input
                  bind:value={product.name}
                  type="text"
                  placeholder="nombre"
                  id="product-name"
                  class="form-control" />
              </div>
              <div class="form-group">
                <textarea
                  bind:value={product.descripcion}
                  id="product-description"
                  cols="30"
                  rows="10"
                  placeholder="descripcion"
                  class="form-control" />
              </div>
              <div class="form-group">
                <input
                  bind:value={product.url}
                  type="url"
                  id="producto-url"
                  placeholder="https://facebook.com"
                  class="form-control" />
              </div>
              <div class="form-group">
                <select
                  bind:value={product.categoria}
                  id="category"
                  class="form-control">
                  <option value="tecnologia">Tecnologia</option>
                  <option value="hogar">Hogar</option>
                  <option value="otros">Otros</option>
                </select>
              </div>
              <button class="btn btn-primary">
				  {#if !estadoEditar}
				  	Guardar
				  {:else}
					Editar
				  {/if}
			  </button>
            </form>
          </div>
        </div>
      </div>
    </div>

  </div>

</main>
