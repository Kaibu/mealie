<template>
  <div class="text-center">
    <v-dialog v-model="dialog" width="70%">
      <v-card>
        <v-app-bar dark color="primary mb-2">
          <v-icon large left>
            {{ $globals.icons.import }}
          </v-icon>
          <v-toolbar-title class="headline">
            {{ $t("settings.backup.import-summary") }}
          </v-toolbar-title>
          <v-spacer></v-spacer>
        </v-app-bar>
        <v-card-text class="mb-n4">
          <v-row>
            <div v-for="values in allNumbers" :key="values.title">
              <v-card-text>
                <div>
                  <h3>{{ values.title }}</h3>
                </div>
                <div class="success--text">{{ $t("general.success-count", { count: values.success }) }}</div>
                <div class="error--text">{{ $t("general.failed-count", { count: values.failure }) }}</div>
              </v-card-text>
            </div>
          </v-row>
        </v-card-text>
        <v-tabs v-model="tab" show-arrows="">
          <v-tab>{{ $t("general.recipes") }}</v-tab>
          <v-tab>{{ $t("general.themes") }}</v-tab>
          <v-tab>{{ $t("general.settings") }}</v-tab>
          <v-tab> {{ $t("settings.pages") }} </v-tab>
          <v-tab>{{ $t("user.users") }}</v-tab>
          <v-tab>{{ $t("group.groups") }}</v-tab>
        </v-tabs>
        <v-tabs-items v-model="tab">
          <v-tab-item v-for="(table, index) in allTables" :key="index">
            <v-card flat>
              <DataTable :data-headers="importHeaders" :data-set="table" />
            </v-card>
          </v-tab-item>
        </v-tabs-items>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import DataTable from "@/components/ImportSummaryDialog/DataTable";
export default {
  components: {
    DataTable,
  },
  data: () => ({
    tab: null,
    dialog: false,
    recipeData: [],
    themeData: [],
    settingsData: [],
    userData: [],
    groupData: [],
    pageData: [],
    allDataTables: [],
  }),

  computed: {
    importHeaders() {
      return [
        {
          text: this.$t("general.status"),
          value: "status",
        },
        {
          text: this.$t("general.name"),
          align: "start",
          sortable: true,
          value: "name",
        },
        {
          text: this.$t("general.exception"),
          value: "data-table-expand",
          align: "center",
        },
      ];
    },
    recipeNumbers() {
      return this.calculateNumbers(this.$t("general.recipes"), this.recipeData);
    },
    settingsNumbers() {
      return this.calculateNumbers(this.$t("general.settings"), this.settingsData);
    },
    themeNumbers() {
      return this.calculateNumbers(this.$t("general.themes"), this.themeData);
    },
    userNumbers() {
      return this.calculateNumbers(this.$t("user.users"), this.userData);
    },
    groupNumbers() {
      return this.calculateNumbers(this.$t("group.groups"), this.groupData);
    },
    pageNumbers() {
      return this.calculateNumbers(this.$t("settings.pages"), this.pageData);
    },
    allNumbers() {
      return [
        this.recipeNumbers,
        this.themeNumbers,
        this.settingsNumbers,
        this.pageNumbers,
        this.userNumbers,
        this.groupNumbers,
      ];
    },
    allTables() {
      return [this.recipeData, this.themeData, this.settingsData, this.pageData, this.userData, this.groupData];
    },
  },

  methods: {
    calculateNumbers(title, list_array) {
      if (!list_array) return;
      let numbers = { title: title, success: 0, failure: 0 };
      list_array.forEach(element => {
        if (element.status) {
          numbers.success++;
        } else numbers.failure++;
      });
      return numbers;
    },
    open(importData) {
      this.recipeData = importData.recipeImports;
      this.themeData = importData.themeImports;
      this.settingsData = importData.settingsImports;
      this.userData = importData.userImports;
      this.groupData = importData.groupImports;
      this.pageData = importData.pageImports;
      this.dialog = true;
    },
  },
};
</script>

<style></style>
