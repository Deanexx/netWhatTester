<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-img
          alt="42Image"
          width="50px"
          :src="require(`../assets/school42.png`)"/>
        <q-toolbar-title>
          NetWhat
        </q-toolbar-title>
        <q-space/>
        <q-btn v-if="$route.name !== 'start'" color="deep-orange" @click="$router.push({name: 'start'})">Redo test</q-btn>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <transition enter-active-class="animated fadeIn"
                  leave-active-class="animated fadeOut"
                  mode="out-in"
                  :duration="400">
        <router-view/>
      </transition>
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',
  data () {
    return {
    }
  }
}
</script>
