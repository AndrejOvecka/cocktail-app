<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-buttons>
          <ion-back-button></ion-back-button>
        </ion-buttons>

        <ion-title>{{ state.drink.strDrink }}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="state.loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true">
      <DrinkCard :drink="state.drink" />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { reactive } from "vue";
import { useRoute } from "vue-router";
import DrinkCard from "@/components/DrinkCard.vue";
import axios from "axios";
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonButtons,
  IonBackButton,
  IonSpinner,
} from "@ionic/vue";

import IDrinkDetails from "../interfaces/IDrinkDetails";

export default {
  name: "Drink",
  components: {
    IonPage,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    DrinkCard,
    IonButtons,
    IonBackButton,
    IonSpinner,
  },
  setup() {
    const route = useRoute();
    const drinkId = route.params.id as string;

    const state = reactive({
      drink: {} as IDrinkDetails,
      loading: false,
    });

    const fetchDrinkById = async (drinkId: string) => {
      state.loading = true;

      const res = await axios.get(
        `https://www.thecocktaildb.com/api/json/v1/1/lookup.php?i=${drinkId}`
      );

      if (res.data) {
        state.drink = res.data?.drinks[0];
      }

      state.loading = false;
    };

    fetchDrinkById(drinkId);
    {
      state;
    }
  },
};
</script>
