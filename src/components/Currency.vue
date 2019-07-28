<template>
  <div class="hello">
    <div class="container">
      <div class="row">
        <div class="col-lg-10 ml-auto mr-auto">
          <div class="row">
            <div class="col-lg-3">
              <label>From</label>
              <input type="text" class="form-control" placeholder="From" />
            </div>
            <div class="col-lg-1">
              <i class="fa fa-exchange" aria-hidden="true"></i>
            </div>
            <div class="col-lg-3">
              <label>To</label>
              <input type="text" class="form-control" placeholder="To" />
            </div>
            <div class="col-lg-3">
              <label>Amount</label>
              <input type="number" v-model="amount" class="form-control"/>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      amount: 1
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.fa-exchange {
  position: relative;
  top: 65%;
  transform: translateY(-50%);
}
</style>
