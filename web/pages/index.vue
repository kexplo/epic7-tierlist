<template>
  <tier-table v-model="search" :headers="headers" :items="tiers" title="Epic 7 Tier List (PvE)" />
</template>

<script>
export default {
  components: {
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>

<script>
import TierTable from '~/components/TierTable.vue'
import pve_data from '~/static/pve.json'

export default {
  components: {
    TierTable
  },

  data() {
    return {
      search: '',
      pagination: {
        descending: true,
        sortBy: 'star'
      },
      headers: [
        { text: '', align: 'left', sortable: false, value: 'img', class: 'pa-1', type: 'img', width: 150, fixed: true },
        { text: 'Name', value: 'name', class: 'pa-1', sortable: true, width: 150 },
        { text: '★', value: 'star', class: 'pa-1', sortable: true, width: 70 },
        { text: 'Element', value: 'element', class: 'pa-1' },
        { text: 'Class', value: 'class', class: 'pa-1', type: 'img' },
        { text: 'Hunt', value: 'hunt', class: 'pa-1', sortable: true, width: 70 },
        { text: 'Abyss', value: 'abyss', class: 'pa-1', sortable: true, width: 70 },
        { text: 'Raid', value: 'raid', class: 'pa-1', sortable: true, width: 70 },
        { text: 'PvE AVR', value: 'pve_average', class: 'pa-1', sortable: true, width: 50 },
        { text: 'Recommended Set', value: 'recommended_set', class: 'pa-1' },
        { text: 'Neck', value: 'neck', class: 'pa-1' },
        { text: 'Suggested Roles', value: 'suggested_roles', class: 'pa-1', width: 150 },
        { text: 'Recommended Artifact', value: 'recommended_artifact', class: 'pa-1', width: 160, type: 'img' },
        { text: 'Alternative Artifact', value: 'alternate_artifact', class: 'pa-1', width: 160, type: 'img' },
        { text: 'Alternative Artifact', value: 'alternate_artifact2', class: 'pa-1', width: 160, type: 'img' },
        { text: 'Note', value: 'note', class: 'pa-1 cl-tier-note', width: 400 }
      ],

      tiers: [
      ]
    }
  },
  mounted() {
    this.tiers = pve_data;
  }
}
</script>
