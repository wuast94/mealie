<template>
  <div>
    <div class="d-md-flex" style="gap: 10px">
      <v-select v-model="inputDay" :items="MEAL_DAY_OPTIONS" :label="$t('meal-plan.rule-day')"></v-select>
      <v-select v-model="inputEntryType" :items="MEAL_TYPE_OPTIONS" :label="$t('meal-plan.meal-type')"></v-select>
    </div>

    <div class="mb-5">
      <RecipeOrganizerSelector v-model="inputCategories" selector-type="categories" />
      <RecipeOrganizerSelector v-model="inputTags" selector-type="tags" />
      <GroupHouseholdSelector
        v-model="inputHouseholds"
        multiselect
        :description="$tc('meal-plan.mealplan-households-description')"
      />
    </div>

    <!-- TODO: proper pluralization of inputDay -->
    {{ $t('meal-plan.this-rule-will-apply', {
         dayCriteria: inputDay === "unset" ? $t('meal-plan.to-all-days') : $t('meal-plan.on-days', [inputDay]),
         mealTypeCriteria: inputEntryType === "unset" ? $t('meal-plan.for-all-meal-types') : $t('meal-plan.for-type-meal-types', [inputEntryType])
        }) }}
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, useContext } from "@nuxtjs/composition-api";
import GroupHouseholdSelector from "~/components/Domain/Household/GroupHouseholdSelector.vue";
import RecipeOrganizerSelector from "~/components/Domain/Recipe/RecipeOrganizerSelector.vue";
import { PlanCategory, PlanHousehold, PlanTag } from "~/lib/api/types/meal-plan";

export default defineComponent({
  components: {
    GroupHouseholdSelector,
    RecipeOrganizerSelector,
  },
  props: {
    day: {
      type: String,
      default: "unset",
    },
    entryType: {
      type: String,
      default: "unset",
    },
    categories: {
      type: Array as () => PlanCategory[],
      default: () => [],
    },
    tags: {
      type: Array as () => PlanTag[],
      default: () => [],
    },
    households: {
      type: Array as () => PlanHousehold[],
      default: () => [],
    },
    showHelp: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, context) {
    const { i18n } = useContext();

    const MEAL_TYPE_OPTIONS = [
      { text: i18n.t("meal-plan.breakfast"), value: "breakfast" },
      { text: i18n.t("meal-plan.lunch"), value: "lunch" },
      { text: i18n.t("meal-plan.dinner"), value: "dinner" },
      { text: i18n.t("meal-plan.side"), value: "side" },
      { text: i18n.t("meal-plan.type-any"), value: "unset" },
    ];

    const MEAL_DAY_OPTIONS = [
      { text: i18n.t("general.monday"), value: "monday" },
      { text: i18n.t("general.tuesday"), value: "tuesday" },
      { text: i18n.t("general.wednesday"), value: "wednesday" },
      { text: i18n.t("general.thursday"), value: "thursday" },
      { text: i18n.t("general.friday"), value: "friday" },
      { text: i18n.t("general.saturday"), value: "saturday" },
      { text: i18n.t("general.sunday"), value: "sunday" },
      { text: i18n.t("meal-plan.day-any"), value: "unset" },
    ];

    const inputDay = computed({
      get: () => {
        return props.day;
      },
      set: (val) => {
        context.emit("update:day", val);
      },
    });

    const inputEntryType = computed({
      get: () => {
        return props.entryType;
      },
      set: (val) => {
        context.emit("update:entry-type", val);
      },
    });

    const inputCategories = computed({
      get: () => {
        return props.categories;
      },
      set: (val) => {
        context.emit("update:categories", val);
      },
    });

    const inputTags = computed({
      get: () => {
        return props.tags;
      },
      set: (val) => {
        context.emit("update:tags", val);
      },
    });

    const inputHouseholds = computed({
      get: () => {
        return props.households;
      },
      set: (val) => {
        context.emit("update:households", val);
      },
    });

    return {
      MEAL_TYPE_OPTIONS,
      MEAL_DAY_OPTIONS,
      inputDay,
      inputEntryType,
      inputCategories,
      inputTags,
      inputHouseholds,
    };
  },
});
</script>
