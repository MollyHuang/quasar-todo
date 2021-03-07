<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <q-page class="flex column">

        <div class="col q-pt-lg q-px-md">
          <q-input
            v-model="search"
            placeholder="Search"
            dark
            borderless
          >
            <template v-slot:before>
              <q-icon name="my_location" />
            </template>

            <template v-slot:append>
              <q-btn round dense flat icon="search" />
            </template>
          </q-input>
        </div>

        <div class="col text-white text-center">
          <div class="text-h4 text-weight-light">
            Manchester
          </div>
          <div class="text-h6 text-weight-light">
            Rain
          </div>
          <div class="text-h1 text-weight-thin q-my-lg relative-position">
            <span>8</span>
            <span class="text-h4 relative-position degree">&deg;</span>
          </div>
        </div>

      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'Weather',
  data () {
    return {
      search: ''
    }
  }
}
</script>

<style lang="sass" scoped>
  .q-page
    background: linear-gradient(to bottom, #136a8a, #267871)
  .degree
    top: -44px
</style>
