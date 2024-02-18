<script>
  export let product;
</script>

<div class="bg-white rounded-lg overflow-hidden shadow-lg p-6 m-4">
  <img src={product.image} alt={product.name} class="w-full h-48 object-cover mb-4">
  <h2 class="text-xl font-semibold">{product.name}</h2>
  <p class="text-gray-600 mb-2">{product.category}</p>
  <p class="text-gray-800">${product.price}</p>
  <p class="text-sm text-gray-500">{product.description}</p>
</div>