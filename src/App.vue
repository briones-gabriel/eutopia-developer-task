<template>
  <v-app>
    <v-container fill-height>
      <v-main class="text-center">
        <h1 class="text-h2 green--text">Eutopia Developer Task</h1>
        <br />
        <v-btn
          color="green lighten-1"
          elevation="0"
          class="white--text"
          @click="toggleAdvancedFilter"
          large
        >
          Advanced Filter
        </v-btn>
        <v-container>
          <br />
          <v-row class="d-flex justify-center">
            <!--Industries & Tags-->
            <v-col v-if="checkForTags" cols="4">
              <h4 class="text-h5">Industries & Tags selected</h4>
              <v-container>
                <v-chip-group column>
                  <v-chip
                    color="green"
                    outlined
                    label
                    v-for="tag in selectedTags"
                    :key="tag"
                  >
                    #{{ tag }}
                  </v-chip>
                </v-chip-group>
              </v-container>
            </v-col>
            <!--Industries-->
            <v-col v-if="checkForIndustries" cols="4">
              <h4 class="text-h5">Industries selected</h4>
              <v-container>
                <v-chip-group column>
                  <v-chip
                    color="green"
                    outlined
                    label
                    v-for="industry in selectedIndustries"
                    :key="industry"
                  >
                    {{ industry }}
                  </v-chip>
                </v-chip-group>
              </v-container>
            </v-col>
            <!--Calendar Dates-->
            <v-col v-if="checkForDates" cols="4">
              <h4 class="text-h5">Calendar dates selected</h4>
              <v-container>
                <v-chip outlined label color="green" large>
                  {{ fromDate }} -- {{ toDate }}
                </v-chip>
              </v-container>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
      <AdvancedFilterModal
        :showModal="showModal"
        :toggleAdvancedFilter="toggleAdvancedFilter"
        @apply="displayResults"
      ></AdvancedFilterModal>
    </v-container>
  </v-app>
</template>

<script>
import AdvancedFilterModal from "./components/AdvancedFilterModal.vue";

export default {
  name: "App",
  components: { AdvancedFilterModal },
  data: () => ({
    showModal: false,
    selectedTags: [],
    selectedIndustries: [],
    fromDate: null,
    toDate: null,
  }),
  methods: {
    toggleAdvancedFilter() {
      this.showModal = !this.showModal;
    },
    displayResults(results) {
      this.selectedTags = results[0];
      this.selectedIndustries = results[1];
      this.fromDate = results[2];
      this.toDate = results[3];
      console.log(this.selectedIndustries.length);
    },
  },
  computed: {
    checkForDates() {
      return true ? this.fromDate && this.toDate : false;
    },
    checkForTags() {
      return true ? this.selectedTags.length : false;
    },
    checkForIndustries() {
      return true ? this.selectedIndustries.length : false;
    },
  },
};
</script>
