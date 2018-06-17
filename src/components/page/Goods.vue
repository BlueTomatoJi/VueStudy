<template>
  <div>
    <slot name="header"><span>这里是默认header</span></slot>
    <span>{{name}}</span>
    <span class="o-price">¥{{price}}</span>
    <slot><span>这里是默认Slot</span></slot>
    <slot name="footer"><span>这里是默认footer</span></slot>
  </div>
</template>

<script>
    export default {
      name: "Goods",
      props: {
        name: String,
        price: Number,
      },
      data() {
        return {

        };
      },
    }
</script>

<style scoped>

</style>
