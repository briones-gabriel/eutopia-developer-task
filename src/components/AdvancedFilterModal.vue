<template>
  <div>
    <!--Modal-->
    <v-dialog
      v-model="showModal"
      width="90%"
      persistent
      @click:outside="toggleAdvancedFilter"
    >
      <!--Main Card-->
      <v-card>
        <!--Tabs-->
        <v-tabs
          fixed-tabs
          background-color="green lighten-2"
          color="black"
          light
        >
          <v-tab disabled>Overview</v-tab>
          <v-tab href="#sector">Sector</v-tab>
          <!--Sector Tab-->
          <v-tab-item value="sector">
            <!--Industries & Tags section-->
            <v-container>
              <h3>INDUSTRIES & TAGS</h3>
              <v-divider></v-divider>
              <br />
              <v-autocomplete
                outlined
                clearable
                multiple
                dense
                deletable-chips
                hide-selected
                small-chips
                solo
                flat
                append-icon=""
                no-data-text="No results found"
                color="grey"
                menu-props="closeOnClick"
                v-model="selectedTags"
                label="Type to search"
                :items="sortedTagOptions"
              ></v-autocomplete>
            </v-container>
            <!--Industries section-->
            <v-container>
              <h3>INDUSTRIES</h3>
              <v-divider></v-divider>
              <v-row class="no-gutters">
                <v-col
                  cols="3"
                  v-for="industry in industriesOptions"
                  :key="industry"
                >
                  <v-checkbox
                    multiple
                    hide-details=""
                    v-model="selectedIndustries"
                    :label="industry"
                    :value="industry"
                  ></v-checkbox>
                </v-col>
              </v-row>
            </v-container>
          </v-tab-item>
          <!--End of sector tab-->
          <v-tab disabled>Climate Impact</v-tab>
          <!--Financials Tab-->
          <v-tab href="#financials">Financials</v-tab>
          <v-tab-item value="financials">
            <!--Date Picker-->
            <v-container>
              <h3>DATE PICKER</h3>
              <v-divider></v-divider>
              <br />
              <v-row>
                <!--Date picker (from date)-->
                <v-col cols="3">
                  <v-menu
                    offset-y
                    ref="fromDateMenu"
                    v-model="fromDateMenu"
                    return-value.sync="fromDate"
                    transition="scale-transition"
                    :close-on-content-click="false"
                    min-width="auto"
                  >
                    <!--Text input to display the information-->
                    <template v-slot:activator="{ on }">
                      <v-text-field
                        readonly
                        v-model="fromDate"
                        label="From"
                        prepend-icon="mdi-calendar"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <!--Dinamic calendar-->
                    <v-date-picker v-model="fromDate" no-title></v-date-picker>
                  </v-menu>
                </v-col>
                <!--Date picker (to date)-->
                <v-col cols="3">
                  <v-menu
                    offset-y
                    ref="toDateMenu"
                    v-model="toDateMenu"
                    return-value.sync="toDate"
                    transition="scale-transition"
                    :close-on-content-click="false"
                    min-width="auto"
                  >
                    <!--Text input to display the information-->
                    <template v-slot:activator="{ on }">
                      <v-text-field
                        readonly
                        v-model="toDate"
                        label="To"
                        prepend-icon="mdi-calendar"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <!--Dinamic calendar-->
                    <v-date-picker v-model="toDate" no-title></v-date-picker>
                  </v-menu>
                </v-col>
              </v-row>
            </v-container>
            <br />
          </v-tab-item>
          <!--End of Financials tab-->
          <v-tab disabled>Wildcard</v-tab>
        </v-tabs>
        <br />
        <v-divider></v-divider>
        <!--Buttons-->
        <v-container class="text-right">
          <!--Apply-->
          <v-btn
            elevation="0"
            color="green lighten-1"
            class="white--text"
            @click="apply"
          >
            Apply
          </v-btn>
          <!--Clear all-->
          <v-btn outlined color="grey" class="mx-4" @click="clearAll">
            Clear all
          </v-btn>
          <!--Cancel-->
          <v-btn text plain @click="toggleAdvancedFilter"> Cancel </v-btn>
        </v-container>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: "AdvancedFilterModal",
  props: ["showModal", "toggleAdvancedFilter"],
  data: () => ({
    fromDate: null,
    toDate: null,
    fromDateMenu: false,
    toDateMenu: false,
    selectedTags: [],
    selectedIndustries: [],
    industriesOptions: [
      "Agriculture",
      "Building",
      "Constructions",
      "Energy",
      "Financial services",
      "Food & beverage",
      "Forestry",
      "Healthcare",
      "Logistics",
      "Manufacturing",
      "Mining",
      "Other activities",
      "Public administration",
      "Telecommunications & ICS",
      "Transportation",
      "Utilities (electricity, water, waste)",
    ],
    tagOptions: [
      "3D printing",
      "Advanced materials",
      "Advanced metering",
      "Air vehicles",
      "Air-to-water",
      "Algae",
      "Packaging",
      "Alternative proteins",
      "Animal farming",
      "Animal welfare",
      "Apiculture",
      "Apps",
      "Aquaculture",
      "Artificial intelligence (AI)",
      "Automotive",
      "Autonomus vehicles",
      "Aviation",
      "Banking",
    ],
  }),
  methods: {
    apply() {
      // Emit variables and exit
      this.$emit("apply", [
        this.selectedTags,
        this.selectedIndustries,
        this.fromDate,
        this.toDate,
      ]);
      this.toggleAdvancedFilter();
    },
    clearAll() {
      // Clear all variable and emit to save them
      this.selectedTags = [];
      this.selectedIndustries = [];
      this.fromDate = null;
      this.toDate = null;
      this.$emit("apply", [
        this.selectedTags,
        this.selectedIndustries,
        this.fromDate,
        this.toDate,
      ]);
    },
  },
  computed: {
    sortedTagOptions() {
      return this.tagOptions.sort();
    },
  },
};
</script>

<style>
</style>
