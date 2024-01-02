<template>
    <div class="text">
        {{ text }}
    </div>
</template>
  
<script>
export default {
    name: 'Text',
    props: {
        text: String
    }
}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.topbar {
    width: 100%;
    height: 50px;
    background: #fff;
}
</style>
  