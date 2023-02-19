<template>
  <div id="app" class="gimmersta-background">
    <div class="content">
      <div>
        <div
          class="person-item"
          v-for="person of people"
          v-on:click="currentPerson = person"
        >
          <div class="person-item-wrapper">
            <div class="avatar-name">{{ person.name.charAt(0) }}</div>
            <div>
              <div>{{ person.name }}</div>
              <p class="text-s gray">{{ person.username }}</p>
              <p class="text-s gray">{{ person.email }}</p>
              <p class="text-s gray">{{ person.company.name }}</p>
            </div>
          </div>
        </div>
      </div>
      <div v-if="currentPerson.name">
        <div class="person-details">
          <div class="person-details-wrapper">
            <div class="map-wrapper">
              <GoogleMap
                :api-key="apiKey"
                class="map"
                :center="coords(currentPerson.address.geo)"
                :zoom="3"
              >
                <Marker
                  :options="{ position: coords(currentPerson.address.geo) }"
                />
              </GoogleMap>
            </div>
            <div>
              <div>{{ currentPerson.name }}</div>
              <p class="text-s gray">{{ currentPerson.username }}</p>
              <p class="text-s gray">{{ currentPerson.email }}</p>
              <p class="text-s gray">{{ currentPerson.phone }}</p>
              <div class="spacer-s row"></div>
              <p class="text-s gray">{{ currentPerson.address.street }}</p>
              <p class="text-s gray">{{ currentPerson.address.suite }}</p>
              <p class="text-s gray">{{ currentPerson.address.city }}</p>
              <p class="text-s gray">{{ currentPerson.address.zipcode }}</p>
              <div class="spacer-s row"></div>
              <p class="text-s gray">{{ currentPerson.website }}</p>
              <div class="spacer-s row"></div>
              <p class="text-s gray">{{ currentPerson.company.name }}</p>
              <p class="text-s gray">{{ currentPerson.company.catchPhrase }}</p>
              <p class="text-s gray">{{ currentPerson.company.bs }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import { GoogleMap, Marker } from "vue3-google-map";

export default defineComponent({
  name: "App",
  components: { GoogleMap, Marker },

  data() {
    return {
      people: [],
      currentPerson: { name: "" },
      apiKey: import.meta.env.VITE_API_KEY,
    };
  },

  async beforeCreate() {
    const url = "https://jsonplaceholder.typicode.com/users";
    const res = await fetch(url);
    this.people = await res.json();
  },

  methods: {
    coords(coords) {
      return { lat: Number(coords.lat), lng: Number(coords.lng) };
    },
  },
});
</script>
