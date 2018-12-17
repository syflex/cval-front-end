<template>
  <q-layout view="lHh Lpr lFf">
    <q-layout-header>
      <q-toolbar
        color="primary"
        :glossy="$q.theme === 'mat'"
        :inverted="$q.theme === 'ios'"
      >
        <q-btn
          flat
          dense
          round :to="{name: 'Home'}"
          aria-label="home"
        >
          <q-icon name="menu" />
        </q-btn>

        <q-toolbar-title>
          CVAL
          <div slot="subtitle">Running on v{{ $q.version }}</div>
        </q-toolbar-title>
        <q-btn  label="crops" :to="{name: 'Crop'}"/>
        <q-btn  label="view crops" :to="{name: 'Crops'}"/>
        <q-btn  label="Structure" :to="{name: 'Structure'}"/>
        <q-btn  label="view Structure" :to="{name: 'Structures'}"/>
      </q-toolbar>
    </q-layout-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { openURL } from 'quasar'

export default {
  name: 'MyLayout',
  data () {
    return {
      leftDrawerOpen: this.$q.platform.is.desktop
    }
  },
  methods: {
    openURL
  }
}
</script>

<style>
</style>
