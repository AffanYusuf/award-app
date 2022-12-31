<template>
  <div>
    <div class="card shadow card-award">
      <div class="card-header">
        <span :class="['btn btn-sm float-right', award.type == 'VOUCHERS' ? vouchersClass : productsClass]">{{ award.type }}</span>
      </div>
      <img :src=" 'assets/images/' + award.image " class="card-img-top" alt="..." />
      <div class="card-body">
        <span
          class="card-text"
        >{{ formatPoint(award.point) }} Point</span>
      </div>
    </div>
    <b class="card-title">{{ award.name }}</b>
  </div>
</template>

<script>
export default {
  name: "CardAward",
  props: ["award"],
  data() {
    return {
      vouchersClass: 'btn-primary',
      productsClass: 'btn-warning'
    }
  },
  methods: {
    formatPoint(value) {
      return (value/1).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    }
  }
};

</script>
  
<style>
</style>