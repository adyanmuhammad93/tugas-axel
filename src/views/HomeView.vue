<script setup>
import { onMounted, ref } from "vue";
import axios from 'axios';
import { useRouter } from "vue-router";

const router = useRouter()

const items = ref([])
const selectedDeparture = ref(null)
const selectedArrival = ref(null)
const selectedDate = ref(null)


async function fetchData() {
  try {
    const response = await axios.get('https://fancy-beijinho-1834ec.netlify.app/api/v1/airport')
    console.log(response)
    items.value = response.data.map(item => ({
      ...item,
      display: `${item.city}, ${item.country}, ${item.iata}`
    }))
    console.log(items[0].display)
    // people.value = response.data
    // console.log(people.value)
  } catch (error) {
    console.log(error)
  }
}
onMounted(() => {
  fetchData()
})

const form = ref({
  departure: '',
  arrival: '',
  date: ''
})

const searchFlight = async () => {
  try {
    const formData = {
      departure: form.value.departure.iata,
      arrival: form.value.arrival.iata,
      date: form.value.date
    }
    const response = await axios.post('https://fancy-beijinho-1834ec.netlify.app/api/v1/flight-search', formData)
    console.log(response)
    const newData = JSON.stringify(response.data)
    router.push({
      name: 'result', params: {
        flightData: newData
      }
    })
  } catch (error) {
    console.log(error)
  }
}

</script>

<template>
  <main>
    <div class="d-flex flex-column gap-3">
      <div class="p-5 mb-4 bg-body-tertiary rounded-3 bgJumbotron">
        <div class="container-fluid py-5">
          <h1 class="display-5 fw-bold">Swinny Tours</h1>
          <p class="col-md-8 fs-4 jumbotron">Welcome to Swinny Tours, where every journey is an adventure waiting to unfold!
            <br>
            At Swinny Tours, we believe that travel is not just about reaching a destination;
            <br>
            With a commitment to exceptional service and dedication to personalized service.
            <br>
            We ensure from the moment you inquire about a trip to the final farewell, our team of experienced travel
            specialists is here to guide you every step of the way. We handle all the logistics so you can focus on your
            journey.
          </p>
        </div>
      </div>

      <div class="bg-white shadow p-3 rounded-4 w-100">
        <div class="container text-center">
          <form class="row" @submit.prevent="searchFlight">
            <div class="col">
              <label for="inputState" class="form-label">Departure</label>
              <v-combobox v-model="form.departure" :items="items" item-title="display" item-text="display"
                item-value="id" label="Select an Item" return-object>
                <!-- <template v-slot:selection="data">
                  {{ data.item.display }}
                </template> -->
              </v-combobox>
            </div>
            <div class="col">
              <label for="inputState" class="form-label">Arrival</label>
              <v-combobox v-model="form.arrival" :items="items" item-title="display" item-text="display" item-value="id"
                label="Select an Item" return-object>
                <!-- <template v-slot:selection="data">
                  {{ data.item.display }}
                </template> -->
              </v-combobox>
            </div>
            <div class="col">
              <label for="inputState" class="form-label">Date</label>
              <input type="date" class="form-control" v-model="form.date">
            </div>
            <div class="col d-flex justify-content-center align-items-stretch">
              <button type="submit" class="btn btn-light w-100" @click="searchFlight">Explore Now</button>
            </div>
          </form>
        </div>
      </div>
    </div>
    <div class="bg-dark text-white py-5">
      <div class="container">
        <div class="text-center">
          <h2>We Ensure You Fly with Us Forever</h2>
        </div>
        <div class="p-3 rounded-4 w-100">
          <div class="container text-center">
            <div class="row">
              <div class="col">
                <h3>Talented Crew</h3>
                <p>We have crews who possess exceptional skills, expertise, and synergy. Ensuring you'll have the best flight experience .</p>
              </div>
              <div class="col">
                <h3>Safe Guard</h3>
                <p>Rigorous safety protocols, advanced technology employment, and proficient pilot expertise, ensuring the secure transportation of passengers and cargo through the skies.</p>
              </div>
              <div class="col">
                <h3>Best Awards</h3>
                <p>Exceptional achievement, innovation, or superiority proving our experience in this field.</p>
              </div>
              <div class="col">
                <h3>Pickup at Home</h3>
                <p>Personal Pickup from Home to airport. Ensuring convenience and comfort for everyone.</p>
              </div>
            </div>
          </div>
        </div>
        <div></div>
      </div>
    </div>
    <div>
      <div class="container">
        <div class="row">
          <div class="col">
            <div class="card" style="width: 18rem;">
              <div class="card-body">
                <h5 class="card-title">Airport Pickup</h5>
                <p class="card-text">Personal Pickup from Home to airport. Ensuring convenience and comfort for everyone.</p>
              </div>
            </div>
          </div>
          <div class="col">
            <div class="card" style="width: 18rem;">
              <div class="card-body">
                <h5 class="card-title">Airport Lounge</h5>
                <p class="card-text">Free airport lounge acces for resting and waiting for boarding time.</p>
              </div>
            </div>
          </div>
          <div class="col">
            <div class="card" style="width: 18rem;">
              <div class="card-body">
                <h5 class="card-title">Sim Card upon Arrival</h5>
                <p class="card-text">Free sim card upon arrival in your destination.</p>
              </div>
            </div>
          </div>
          <div class="card" style="width: 18rem;">
            <div class="card-body">
              <h5 class="card-title">Extra Baggage</h5>
              <p class="card-text">Free extra 10kg baggage ensuring you can bring all your favorite stuff with you throughout your journey.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.bgJumbotron {
  background-image: url('/src/assets/jumbotronbg.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  filter: grayscale(70%);
}

.jumbotron {
  color: darkblue;
}
</style>