<template>
  <div id="app">
    <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ" target="_blank">
      <div class="gradient-info">{{ currentGradientName }}</div>
    </a>
    <div v-if="days.length === 0 ">
    <div v-if="loading">
      <p class="loading-msg">
        Chargement en cours...
      </p>
      <p class="loading-msg">
        Le chargement peut être (très) long. Pour vous faire patienter, voici une petite citation : 
      </p>
      <p class="loading-msg">
        "Parfois, l'attente est l'occasion parfaite pour faire une pause. Que pourriez-vous découvrir si vous preniez un instant pour observer ce qui vous entoure ? Le calme d'aujourd'hui prépare souvent les succès de demain."
      </p>
      <p class="loading-msg">
        Merci de votre patience :)</p>
    </div>
    </div>
    <div v-else>
      <button class="btn" @click="addDay">Ajouter une journée</button>
      <div v-for="day in days" :key="day.id">
        <Orga :day="day" />
      </div>
    </div>
  </div>
</template>

<script>
import Orga from './components/Orga.vue';

export default {
  name: 'App',
  components: {
    Orga
  },
  data() {
    return {
      loading: true,
      days: [],
      currentGradientIndex: 0,
      gradients: [
        { name: 'Gris profond', value: 'linear-gradient(135deg, #2C2C2C 0%, #242424 50%, #1B1B1B 100%)' },
        { name: 'Bleu nuit étoilée', value: 'linear-gradient(135deg, #1e3c72 0%, #2a5298 50%, #7303c0 100%)' },
        { name: 'Vert forêt', value: 'linear-gradient(135deg, #134E5E 0%, #71B280 100%)' },
        { name: 'Coucher de soleil', value: 'linear-gradient(135deg, #ff9966 0%, #ff5e62 100%)' },
        { name: 'Ciel d\'été', value: 'linear-gradient(135deg, #2980B9 0%, #6DD5FA 50%, #FFFFFF 100%)' },
        { name: 'Violet mystique', value: 'linear-gradient(135deg, #42275a 0%, #734b6d 100%)' },
        { name: 'Océan profond', value: 'linear-gradient(135deg, #0f2027 0%, #203a43 50%, #2c5364 100%)' },
      ]
    };
  },
  computed: {
    currentGradientName() {
      return this.gradients[this.currentGradientIndex].name;
    }
  },
  methods: {
    getDays() {
      fetch('https://backorgajournaliere.onrender.com/days')
        .then(response => response.json())
        .then(data => {
          this.days = data;
          this.loading = false
        });
    },
    addDay() {
      const newDay = { 
        "id": null, 
        "date": new Date().toISOString().split('T')[0], 
        data: {}, 
        "note": null, 
        "tasks_todo": ["", "", ""], 
        "tasks_if_possible": ["", ""], 
        "tasks_optional": ["", ""] 
      };
      this.days = [newDay, ...this.days];
    },
    setRandomGradient() {
      this.currentGradientIndex = Math.floor(Math.random() * this.gradients.length);
      document.body.style.background = this.gradients[this.currentGradientIndex].value;
    }
  },
  created() {
    this.getDays();
    this.setRandomGradient();
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap');

body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
  transition: background 0.5s ease;
}

#app {
  font-family: 'Montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #ffffff;
  min-height: 100vh;
  padding: 20px;
  box-sizing: border-box;
}

.btn {
  background: linear-gradient(315deg, #1B1B1B 0%, #242424 50%, #2C2C2C 100%);
  color: #fff;
  font-size: 1rem;
  font-weight: 600;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  border-radius: 5px;
  padding: 15px 30px;
  text-transform: uppercase;
  letter-spacing: 1px;
  transition: all 0.3s ease;
}

.loading-msg {
  font-size: 1rem;
  font-weight: regular;
}

.btn:hover {
  transform: translateY(-2px);
  background: linear-gradient(315deg, #242424 0%, #2C2C2C 50%, #333333 100%);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.4);
}

.btn:active {
  transform: translateY(1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.gradient-info {
  position: fixed;
  top: 10px;
  left: 10px;
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  padding: 5px 10px;
  border-radius: 5px;
  font-size: 0.8rem;
  z-index: 1000;
}

.loading-msg {
  color: #ffffff;
  width: 40%;
  margin: 1rem auto;
}


</style>