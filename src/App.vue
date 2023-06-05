<template>
  <h1 class="page-title">Оформление заказа</h1>
  <Cart/>
</template>

<script>
  import Cart from "@/components/Cart.vue";
  
  export default {
    components: {
      Cart,
    }
  }
</script>

<style>
  #app {
    font-family: 'Inter';
    font-style: normal;
    font-weight: 600;
    font-size: 24px;
    color: #228C22;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  button {

    margin-top: 30px;
    font-family: 'Inter';
    font-style: normal;
    font-weight: 600;
    font-size: 18px;
    padding: 10px 20px;
    background: #228C22;
    border-radius: 20px;
    line-height: 24px;
    color: #FFFFFF;
    border: none;
  }

  button:hover {
    box-shadow: inset 0 0 100px 100px rgba(255, 255, 255, 0.1);
    cursor: pointer;
}
</style>
