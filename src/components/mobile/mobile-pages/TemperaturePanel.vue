<template>
  <v-ons-page>
    <custom-toolbar v-bind="toolbarInfo"></custom-toolbar>
    <v-ons-list>
      <v-ons-list-item v-for="(temperature, index) in getThingsList" 
        v-if="searchType(temperature.type)" 
        :key="temperature.uuid"
        modifier="chevron"
        @click="transition('default', temperature, false, index)"
        tappable
      >
        {{ temperature.name }}
      </v-ons-list-item>
    </v-ons-list>
    <v-ons-fab position="bottom right" @click="transition('default', null, true, 0)">
        <v-ons-icon icon="md-plus"></v-ons-icon>
    </v-ons-fab>
  </v-ons-page>
</template>

<script>
const transitionPage = {
  template: `
     <v-ons-page v-if="addMode === false">
      <custom-toolbar :backLabel="$t('temperature')">
        {{ temperature.name }}
      </custom-toolbar>
      <mobile-thing :thing="temperature" :addMode="false" :index="index"></mobile-thing>
    </v-ons-page>
    <v-ons-page v-else>
      <custom-toolbar :backLabel="$t('all_things')">
        {{$t('add_new_thing')}}
      </custom-toolbar>
      <mobile-thing :thing="temperature" :addMode="true" :index="index"></mobile-thing>
    </v-ons-page>
    `
}

export default {
  props: ['pageStack', 'setOptions'],

  data () {
    return {}
  },

  computed: {
    getThingsList: function () {
      return this.$store.state.thingsList
    }
  },

  methods: {
    searchType: function (thingType) {
      var match = thingType.match(/Thermometer|Thermostat/)
      return match
    },
    transition (name, item, addMode, index) {
      this.setOptions({
        animation: name,
        temperature: item,
        addMode: addMode,
        index: index,
        callback: () => this.setOptions({})
      })

      this.pageStack.push({
        extends: transitionPage,
        data () {
          return {
            animation: name,
            temperature: item,
            addMode: addMode,
            index: index
          }
        }
      })
    }
  }
}
</script>
