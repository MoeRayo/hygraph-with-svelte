<script>
 import "../app.css";
  import { onMount } from 'svelte';
  import { gql, GraphQLClient } from 'graphql-request';
  import ProductCard from '../components/ProductCard.svelte';
  export let products = [];
  export let stores = [];


  onMount(async () => {
    console.log(import.meta.env.VITE_HYGRAPH_URL)
    const hygraphURL = import.meta.env.VITE_HYGRAPH_URL;

    const client = new GraphQLClient(hygraphURL, {
      headers: {
       
      }
    });

    const query = gql`
      query products {
        productDetails {
          description
          id
          image
          name
          price
        }
        stores {
          city
          name
        }
      }
    `;

    try {
      const data = await client.request(query);
      products = data.productDetails;
       stores = data.stores;
    } catch (error) {
      console.error('Error fetching products:', error);
    }
  });
</script>

<div class="container mx-auto p-4">
  <h1 class="text-3xl font-semibold mb-8">Product List</h1>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
    {#each products as product}
      <ProductCard {product} key={product.id} />
    {/each}
  </div>

   <h1 class="text-3xl font-semibold mb-8">Stores</h1>
  <table class="min-w-full divide-y divide-gray-200">
    <thead class="bg-gray-50">
      <tr>
        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Name</th>
        <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">City</th>
      </tr>
    </thead>
    <tbody class="bg-white divide-y divide-gray-200">
      {#each stores as store}
        <tr>
          <td class="px-6 py-4 whitespace-nowrap">{store.name}</td>
          <td class="px-6 py-4 whitespace-nowrap">{store.city}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>