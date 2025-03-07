<script setup lang="ts">
import EvaluationItem from './EvaluationItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import NumOneIcon from './icons/IconOne.vue'
import NumTwoIcon from './icons/IconTwo.vue'
import axios from 'axios'
import { ref, onMounted } from 'vue'
const userName = ref('')
let activeColor = ref('green')
const vueJsText = ref('VueJS')
const cssText = ref('CSS')
const isJsHovered = ref(false)
const userEmail = ref('');
const userImage = ref('');
function updateCssText() {
  let index = 0
  const cssVariations = ['SCSS', 'tailwind', 'bootstrap', 'CSS']
  
  setInterval(() => {
    cssText.value = cssVariations[index]
    index = (index + 1) % cssVariations.length
  }, 3000)
}
onMounted(() => {
  updateCssText()
})
function getUser() {
  axios.get('https://randomuser.me/api/')
  .then(function (response) {
    const user = response.data.results[0];
    if (user) {
      userName.value = user.name.title + ' ' + 
                       user.name.first + ' ' + 
                       user.name.last;
      
      userEmail.value = user.email;
      
      userImage.value = user.picture.medium;
      
      activeColor.value = 'red';
    } else {
      throw new Error('Aucun utilisateur trouvé');
    }
  })
  .catch(function (error) {
    activeColor = ref('red')
    userName.value = error
    userName.value = error.toString();
    userEmail.value = '';
    userImage.value = '';
  })
}

const ageOfRegistered = '';

</script>

<template>
  <EvaluationItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>Présentation</template>

    Vous trouverez ci-dessous une liste de tâches de difficultés croissantes, afin d'évaluer votre maitrise de : 
    <u>
      <span 
        class="js-text" 
        @mouseenter="isJsHovered = true" 
        @mouseleave="isJsHovered = false"
      >{{ isJsHovered ? 'JS' : 'Javascript' }}</span>, 
      
      <span class="html-text">HTML</span>, 
      
      <span class="css-text">{{ cssText }}</span> 
      
      et 
      
      <span class="vuejs-text">{{ vueJsText }}</span>
    </u>.
    <br/>
    <br/>    

  </EvaluationItem>

  <EvaluationItem>
    <template #icon >
      <NumOneIcon class="icon"/>
    </template>
    <template #heading>Chargement dynamique</template>

    Votre première mission sera de modifier dynamiquement les 4 mots soulignés de la présentation ci-dessus, en utilisant le langage ou la technologie dont elle porte le nom.<br/>
    <i>Exemple: Le mot 'CSS' devra avoir un attribut, comme sa couleur, modifié en CSS. 'VueJS' sera chargé depuis une variable reactive. Etc.</i><br/>
    <b>Faites preuve d'imagination et montrez que vous maitrisez chaque sujet.</b>

  </EvaluationItem>

  <EvaluationItem>
    <template #icon>
      <NumTwoIcon class="icon"/>
    </template>
    <template #heading>Débogage Javascript</template>
    Ici, il y a un bogue qui empêche d'afficher le nom d'une personne. Vous devez comprendre le résultat de la requête et corriger ce bogue. <br/>
    En bonus, vous devrez afficher l'email et l'image au format vignette de la personne.<br/>
    <button variant="primary" v-on:click="getUser">Obtenir un nom</button>
    <br/>
    <div v-if="userName" class="user-info">
      <p :style="{color: activeColor}">{{ userName }}</p>
      <p v-if="userEmail" class="user-email">Email: {{ userEmail }}</p>
      <div v-if="userImage" class="user-image-container">
        <img :src="userImage" alt="Photo de profil" class="user-image">
      </div>
    </div>
  </EvaluationItem>

  <EvaluationItem>
    <template #icon>
      <NumTwoIcon class="icon"/>
    </template>
    <template #heading>Actualiser le résultat</template>
    D'abord, le numéro de cette étape devrait être un 3, donc à vous de corriger ça !<br/>
    Ensuite, vous devrez créer une propriété Computed, qui calcule et actualise l'age qu'avait la personne ci-dessus, lors de son inscription.<br/>
    <div v-if="ageOfRegistered != ''" class="age">
      Son age à l'inscription était {{ ageOfRegistered }} ans.
    </div>
    <br/>
  </EvaluationItem>

</template>

<style scoped>
.icon{
  width: 24px;
  height: 24px;
}
.age{
  color: blue;
}
.js-text {
  cursor: pointer;
  transition: all 0.3s ease;
}

.html-text {
  color: red;
}

.css-text {
  transition: all 0.5s ease;
}

.vuejs-text {
  color: green;
  font-weight: bold;
}
.button-container {
  display: flex;
  justify-content: center;
  margin-top: 15px;
}

.action-button {
  background-color: #4CAF50;
  border: none;
  color: white;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.action-button:hover {
  background-color: #45a049;
}

.user-info {
  margin: 15px auto;
  padding: 15px;
  border-radius: 8px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  max-width: 300px;
  text-align: center;
}

.user-name {
  font-weight: bold;
  font-size: 1.2em;
  margin-bottom: 10px;
}

.user-email {
  margin: 10px 0;
  font-size: 0.9em;
  color: #555;
  word-break: break-all;
}

.user-image-container {
  display: flex;
  justify-content: center;
  margin: 15px 0 5px;
}

.user-image {
  border-radius: 50%;
  border: 3px solid #ddd;
  width: 100px;
  height: 100px;
  object-fit: cover;
  box-shadow: 0 3px 6px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
}

.user-image:hover {
  transform: scale(1.05);
  border-color: #4CAF50;
}
</style>