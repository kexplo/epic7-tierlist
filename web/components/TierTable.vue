<template>
  <v-card>
    <v-card-title>
      {{ title }}
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="search"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="items"
      :search="search"
      :pagination.sync="pagination"
    >
      <template v-slot:items="props">
        <td><img :src="props.item.img"></td>
        <td class="text-xs-center pa-1">{{ props.item.name }}</td>
        <td class="text-xs-center pa-1">{{ props.item.star }}</td>
        <td class="text-xs-center pa-1">{{ props.item.element }}</td>
        <td class="text-xs-center pa-1"><img :src="props.item.class"></td>
        <td class="text-xs-center pa-1">{{ props.item.hunt }}</td>
        <td class="text-xs-center pa-1">{{ props.item.abyss }}</td>
        <td class="text-xs-center pa-1">{{ props.item.raid }}</td>
        <td class="text-xs-center pa-1">{{ ((props.item.hunt + props.item.abyss + props.item.raid) / 3).toFixed(2) }}</td>
        <td class="text-xs-center pa-1">{{ props.item.recommended_set }}</td>
        <td class="text-xs-center pa-1">{{ props.item.neck }}</td>
        <td class="text-xs-center pa-1">{{ props.item.suggested_roles }}</td>
        <td class="text-xs-center pa-1"><img :src="props.item.recommended_artifact"></td>
        <td class="text-xs-center pa-1"><img :src="props.item.alternate_artifact"></td>
        <td class="text-xs-center pa-1"><img :src="props.item.alternate_artifact2"></td>
        <td class="text-xs-left pa-1">{{ props.item.note }}</td>
      </template>
      <template v-slot:no-results>
        <v-alert :value="true" color="error" icon="warning">
          Your search for "{{ search }}" found no results.
        </v-alert>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  name: 'tier-table',
  props: {
    headers: {
      type: Array,
      default: () => []
    },
    items: {
      type: Array,
      required: true,
      default: () => []
    },
    pagination: {
      type: Object,
      default: () => {}
    },
    search: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    }
  }
}
</script>
