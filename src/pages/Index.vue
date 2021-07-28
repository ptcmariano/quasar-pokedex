<template>
  <q-page class="flex flex-center">
  
      <div class="q-pa-md" style="max-width: 500px">
        <q-input
          v-model="pokename"
          debounce="500"
          filled
          placeholder="Search"
          @keyup.enter="getPokemon"
        >
          <template v-slot:append>
            <q-icon name="search" />
          </template>
        </q-input>
        
        <q-img
          v-if="imgfront !== null"
          :src="imgfront"
          :ratio="1"
          class="q-mt-md"
          style="width: 150px"
          placeholder-src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJYAAACWBAMAAADOL2zRAAAAG1BMVEXMzMyWlpaqqqq3t7fFxcW+vr6xsbGjo6OcnJyLKnDGAAAACXBIWXMAAA7EAAAOxAGVKw4bAAABAElEQVRoge3SMW+DMBiE4YsxJqMJtHOTITPeOsLQnaodGImEUMZEkZhRUqn92f0MaTubtfeMh/QGHANEREREREREREREtIJJ0xbH299kp8l8FaGtLdTQ19HjofxZlJ0m1+eBKZcikd9PWtXC5DoDotRO04B9YOvFIXmXLy2jEbiqE6Df7DTleA5socLqvEFVxtJyrpZFWz/pHM2CVte0lS8g2eDe6prOyqPglhzROL+Xye4tmT4WvRcQ2/m81p+/rdguOi8Hc5L/8Qk4vhZzy08DduGt9eVQyP2qoTM1zi0/uf4hvBWf5c77e69Gf798y08L7j0RERERERERERH9P99ZpSVRivB/rgAAAABJRU5ErkJggg=="
        />

        <q-input
          v-model="resultAPI"
          filled
          type="textarea"
        />
      </div>

  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'PageIndex',
  setup () {
    let imgfront = ref(null)
    let pokename = ref(null)
    let resultAPI = ref(null)

    return {
      pokename: pokename,
      imgfront: imgfront,
      resultAPI: resultAPI,
      getPokemon() {
        if (pokename.value) {
          fetch(`https://pokeapi.co/api/v2/pokemon/${pokename.value}`, {
            "headers": {
              "accept": "*/*",
              "sec-fetch-dest": "empty"
            },
            "method": "GET"
          })
          .then((response) => {
            response.json().then((json) => {
              resultAPI.value = JSON.stringify(json.species)
              imgfront.value = json.sprites.front_default
            });
          })
        }
      }
    }
  }
})
</script>
