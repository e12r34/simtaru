<template>
    <!-- <div class="bg-cover md:bg-contain bg-center h-64 w-64" style="background-image: url('/layout/images/bg.png');"> -->
      
  <!-- Content goes here -->
<!-- </div> -->
<!-- <div class="absolute"> -->
<!-- </div> -->
<div class="">
  <div class="absolute middle w-6" style="margin-top: 10%; margin-left: 28%;">
    <div class="grid">
      <div class="col-2">
        <img src='/layout/images/rajaampat.gif' class="w-10">
      </div>
      <div class="col-10">
        <h3 style="margin-bottom: -0.5em;"><b class="text-orange-400">SISTEM INFORMASI TATA RUANG</b></h3>
        <h3 class="text-white">Pemerintah Kabupaten Raja Ampat</h3>
        <p class="text-white">Jalan Raya Waisai, Waisai, Distrik Kota Waisai, Kabupaten Raja Ampat, Papua Bar. 98482</p>
      </div>
      <div class="col-12" style="margin-top: -2em;">
        <hr class="border-b-1 border-gray-300 my-2 block">
      </div>
      <div class="col-12">
        <h1 class="text-white text-7xl">Jelajahi Kabupaten Raja Ampat</h1>
      </div>
      <div class="col-3"></div>
      <div class="col-6">
        <Button size="large" @click="goMain" rounded>Jelajahi Sekarang</Button>
      </div>
      <div class="col-3"></div>
      
    </div>
  </div>
</div>

  
</template>
  
<script setup>
import { useRouter } from 'vue-router';
const router=useRouter()

function goMain() {
  router.push({path:'/map'})
}
</script>

<style module>
body {
  background-image: url('/layout/images/bg.png');
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: 100% 100%;
}

</style>