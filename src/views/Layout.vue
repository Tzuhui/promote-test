<template>
  <div class="container-fluid bg-dark position-absolute w-100" style="height: 100vh;">
    <!-- <div class="row " >
      <div class="col-8 bg-primary"></div>
      <div class="col-4 bg-dark"></div>
    </div> -->
    <div class="container h-100">
      <div class="row h-100">
        <div class="col-5">
            <SelectBar></SelectBar>
        </div>
        <div class="col-6"></div>
        <div class="col-1 d-flex flex-column align-items-center h-100 py-4">
          <router-link to="/"><i class="material-icons h2">clear</i></router-link>
          <h3 class="text-mode-vertical text-white mb-0 mt-auto">POMODORO</h3>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import SelectBar from '@/components/SelectBar.vue';
// import Nav from '@/components/Nav.vue';

export default {
  components: {
    SelectBar,
    // Nav,
  },
};
</script>
