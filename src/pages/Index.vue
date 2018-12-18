<template>
  <q-page class="row flex flex-center">
      <div class="row gutter-sm" style="max-width: 500px">
        <q-btn :to="{name: 'Crop'}" label="Add Crop" class="col-6">
          <img src="/statics/icons/crop.png" width="50%">
        </q-btn>
        <q-btn :to="{name: 'Crops'}" label="View Crop" class="col-6">
          <img src="/statics/icons/crop.png" width="50%">
        </q-btn>
        <q-btn :to="{name: 'Structure'}" label="Add Structure" class="col-6">
          <img src="/statics/icons/home.png" width="50%">
        </q-btn>
        <q-btn :to="{name: 'Structures'}" label="View Structure" class="col-6">
          <img src="/statics/icons/home.png" width="50%">
        </q-btn>
      </div>
  </q-page>
</template>

<style>
</style>

<script>
export default {
  name: 'PageIndex'
}
</script>
