<template>
  <div class="container">
    <div class="date-title">
      <h1>{{ formattedDate }}</h1>
    </div>
    <!-- {{ day }} -->
    <div class="column-container">
    <div class="column-left">
        <div class="card">
            <div class="card-header">
                <p class="save-message" v-if="hasBeenSaved">Les informations ont été sauvegardées.</p>
                <button 
                    v-if="!isDataAlreadySaved" 
                    :class="['save-btn', { 'save-btn-disabled': !isDataComplete }]" 
                    @click="isDataComplete && save()"
                    :disabled="!isDataComplete" >
                    <img class="save-icon" src="../assets/floppy-disk-solid.svg" alt="Sauvegarder">
                </button>
            
            </div>
            <h2>A faire absolument :</h2>
            <ul>
                <li><input placeholder="Corder une application" type="text" :disabled="isExistingDay" v-model="day.tasks_todo[0]"></li>
                <li><input placeholder="Faire un design" type="text" :disabled="isExistingDay" v-model="day.tasks_todo[1]"></li>
                <li><input placeholder="Lire 500 pages" type="text" :disabled="isExistingDay" v-model="day.tasks_todo[2]"></li>
            </ul>
            <h2>A faire si possible :</h2>
            <ul>
                <li><input placeholder="Faire du sport" type="text" :disabled="isExistingDay" v-model="day.tasks_if_possible[0]"></li>
                <li><input placeholder="Dessiner 20 minutes" type="text" :disabled="isExistingDay" v-model="day.tasks_if_possible[1]"></li>
            </ul>
            <h2>A faire optionnellement :</h2>
            <ul>
                <li><input placeholder="Faire une balade" type="text" :disabled="isExistingDay" v-model="day.tasks_optional[0]"></li>
            </ul>
        </div>
    </div>
    <div class="column-right">
        <div class="card">
            <div class="note-container">
            <h2>Note :</h2>
                <button  v-for="n in 10"  :key="n"  :class="['notes-btn', { 'notes-btn-selected': day.note === n }]" 
                  @click="day.note === null && (day.note = n)" :disabled="day.note !== null" >{{ n }}</button>
            </div>
        </div>
    </div>
</div>
  </div>
</template>
<script>
export default {
    props: {
        day: {
            type: Object,
            required: true,
        },
    },
    computed: {
        formattedDate() {
            const date = new Date(this.day.date);
            const options = { day: 'numeric', month: 'long', year: 'numeric' };
            return date.toLocaleDateString('fr-FR', options);
        },
        shouldShowSaveButton() {
            return (
                this.day.id === null ||
                this.day.tasks_todo.slice(0, 3).some(task => task === '') ||
                this.day.tasks_if_possible.slice(0, 2).some(task => task === '') ||
                this.day.tasks_optional[0] === '' ||
                this.day.note === null
            );
        },
        isDataAlreadySaved() {
            return this.day.id !== null;
        },
        isDataComplete() {
            return (
                this.day.tasks_todo.slice(0, 3).every(task => task !== '') &&
                this.day.tasks_if_possible.slice(0, 2).every(task => task !== '') &&
                this.day.tasks_optional[0] !== '' &&
                this.day.note !== null
            );
        },
        isExistingDay() {
            return this.day.id !== null;
        }
    },
    data() {
        return {
            hasBeenSaved: false,
            newTask: '',
        };
    },
    methods: {
        save() {
            fetch('http://localhost:3000/days', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(this.day),
            })
            .then(response => response.json())
            .then(data => {
                this.hasBeenSaved = true;
                setTimeout(() => {
                    this.hasBeenSaved = false;
                }, 3000);
            })
            .catch(error => {
                console.error('Erreur lors de la sauvegarde :', error);
            });
        }
    },
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap');

* {
    font-family: 'Montserrat', sans-serif;
}

.container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.column-container {
    display: flex;
    width: 100%;
    align-items: stretch; /* Ceci étire les enfants pour qu'ils aient la même hauteur */
}

.column-left {
    flex: 4; /* Prend 4 parts de l'espace disponible */
    margin-right: 2rem;
    display: flex;
    flex-direction: column;
}

.column-right {
    flex: 1; /* Prend 1 part de l'espace disponible */
    display: flex;
    flex-direction: column;
}

.card {
    background-color: #1B1B1B;
    border-radius: 0.7rem;
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.5);
    padding: 1rem;
    flex-grow: 1; /* Permet à la carte de grandir pour remplir l'espace disponible */
    display: flex;
    flex-direction: column;
}

.card-header {
    display: flex;
    justify-content: flex-end;
}

.date-title {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.1rem;
    font-weight: bold;
}

h2 {
    color: #fff;
    font-size: 1.3rem;
    font-weight: bold;
    margin: 0.5rem;
}

li {
    color: #fff;
}

input {
    background-color: #1B1B1B;
    color: #fff;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    margin: 1rem;
}

.save-btn,
.notes-btn,
.notes-btn-selected {
    border: none;
    border-radius: 5px;
    cursor: pointer;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

.save-btn {
    background-color: #309939;
    color: #fff;
}

.save-btn-disabled {
    background-color: #808080;
    cursor: not-allowed;
}

.save-icon {
    width: 1rem;
    height: 1rem;
    filter: brightness(0) invert(1);
}

.save-message {
    color: #fff;
    font-size: 0.8rem;
    font-weight: 300;
}

.note-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex-grow: 1; /* Permet au conteneur de notes de grandir pour remplir l'espace disponible */
    justify-content: center; /* Centre verticalement le contenu */
}

.notes-btn,
.notes-btn-selected {
    width: 4rem;
    height: 2.5rem;
    margin: 0.5rem;
}

.notes-btn {
    background-color: #309939;
    color: #000;
}

.notes-btn-selected {
    background-color: #14e626;
    color: #fff;
    box-shadow: 0 0 15px rgba(214, 214, 214, 0.5);
}

.notes-btn:disabled {
    cursor: default;
    opacity: 0.7; /* Optionnel : réduit l'opacité des boutons désactivés */
}
</style>