<template>
<div class="popup-wrapper">
     <div class="popup-body">

      <LazyYoutube src="https://www.youtube.com/watch?v=TcMBFSGVi1c" />

    <button type="button" @click="close"> close video </button>
  </div> 
</div>

</template>
  
<script>
import LazyYoutube from 'vue-lazytube'
  export default {
    name: 'PopUp',
    methods: {
      close() {
        this.$emit('close');
      },
    },
    components: {
        LazyYoutube,
    },
  };
</script>

<style>
  .popup-wrapper{
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.13);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .popup-body{
    background: white;
    border-radius: 20px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    min-width: 300px;
    min-height: 200px;
  }
</style>