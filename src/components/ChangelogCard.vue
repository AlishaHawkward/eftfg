<template>
  <v-card>
    <v-card-title class="card-header font-weight-bold">
      EFTFG Updates
    </v-card-title>
    <v-card-text class="pt-4">
      <p class="font-weight-bold">
        Updated: {{ format($static.changelogs.edges[0].node.date) }}
      </p>
      <ul>
        <li v-for="(item, index) in $static.changelogs.edges[0].node.summary" :key="index">
          {{ item }}
        </li>
      </ul>
      <v-divider class="my-3" />
      <v-dialog v-model="changelogDialog" width="750" scrollable>
        <template v-slot:activator="{ on }">
          <v-btn class="ml-4 black--text" color="primary" v-on="on">
            View Full Changelog
          </v-btn>
        </template>
        <v-card>
          <v-card-title class="card-header font-weight-bold" primary-title>
            EFTFG Full Changelog
          </v-card-title>
          <v-card-text class="mt-4">
            <div v-for="(update, index) in $static.changelogs.edges" :key="index">
              <h3>
                {{ format(update.node.date) }}
              </h3>
              <ul>
                <li v-for="(change,index) in update.node.changes" :key="`${update.node.date}-${index}`">
                  {{ change }}
                </li>
              </ul>
            </div>
          </v-card-text>
          <v-divider />
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="changelogDialog = false">
              Close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-divider class="my-3" />
      <p>If you notice any errors on the site or have a feature request, tweet at me!</p>
      <a href="https://twitter.com/ChewyDinosaur">
        @ChewyDinosaur
      </a>
    </v-card-text>
  </v-card>
</template>

<static-query>
query {
  changelogs: allChangelog {
    edges {
      node {
        date,
        summary,
        changes
      }
    }
  }
}
</static-query>

<script>
import { formatDate } from '../../helpers/dateHelpers'

export default {
  data() {
    return {
      changelogDialog: false
    }
  },

  methods: {
    format(date) {
      return formatDate(date)
    }
  }
}
</script>

<style>

</style>