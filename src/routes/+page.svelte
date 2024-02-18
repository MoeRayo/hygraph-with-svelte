<script>
  import { onMount } from 'svelte';
  import { gql, GraphQLClient } from 'graphql-request';
 import "../app.css";
  import ProductCard from '../components/ProductCard.svelte';
  export let products = [];

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
          category
          countInStock
          description
          id
          image
          name
          numReviews
          price
          rating
        }
      }
    `;

    try {
      const data = await client.request(query);
      products = data.productDetails;
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
</div>