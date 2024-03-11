<script setup lang="ts">
import EvaluationItem from './EvaluationItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import NumOneIcon from './icons/IconOne.vue'
import NumTwoIcon from './icons/IconTwo.vue'
import axios from 'axios'
import { ref } from 'vue'
const userName = ref('')
let activeColor = ref('green')

function getUser() {
  axios.get('https://randomuser.me/api/')
  .then(function (response) {
    const name = response.data.results[1].name;
    userName.value = name.title + ' ' + 
                     name.first + ' ' + 
                     name.last;
  })
  .catch(function (error) {
    activeColor = ref('red')
    userName.value = error
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

    Vous trouverez ci-dessous une liste de tâches de difficultés croissantes, afin d'évaluer votre maitrise de : <u>Javascript, HTML, CSS et VueJS</u>.<br/>
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
    <p :style="{color: activeColor}"> {{ userName }} </p>
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
</style>