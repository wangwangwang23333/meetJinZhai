<template>
  <div class="home">
    begin to work
  </div>
</template>

<script>


export default {
  name: 'Home',
  components: {
    
  },
  created(){
    this.$message({
      message: '欢迎来到「遇见金寨」',
      type: 'success'
    });
  }
}
</script>
