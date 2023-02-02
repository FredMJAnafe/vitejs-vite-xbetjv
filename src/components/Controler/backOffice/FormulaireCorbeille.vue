<template>
  <form :data-service="action" class="formulaireAjoutFacturier">
    <fieldset class="titreFormulaireFacturier">
      <legend>
        Formulaire Corbeille (<a
          target="_blank"
          href="https://www.formulaires.service-public.fr/gf/getNotice.do?cerfaNotice=51649&cerfaFormulaire=10103"
          style="font-weight: bold"
          >Aide</a
        >)
      </legend>
      <input type="hidden" name="__base__" value="" />
      <input type="hidden" name="__id__" value="0" />
      <div class="detailApprenti">
        <div class="_inputBoxFacturier">
          <span class="detailFacturier"
            >Êtes-vous certain de vouloir mettre ce dossier dans la corbeille
            ?</span
          >
          <input type="checkbox" name="corbeille" />
        </div>
      </div>
      <BoutonBase
        class="BoutonBaseRecherche"
        intituleBouton="effacer"
        v-on:click="this.effacerFormulaire"
      ></BoutonBase>
      <BoutonSubmit
        class="BoutonBaseRecherche"
        intituleBouton="Soumettre"
        data-formid="formCorbeille"
      ></BoutonSubmit>
      <div v-if="afficheErreurs" class="erreur">
        <p>{{ messageErreur }}</p>
      </div>
    </fieldset>
  </form>
</template>

<script>
import BoutonBase from '@/components/Controler/elementsHTML/bouton/BoutonBase.vue';
import BoutonSubmit from '@/components/Controler/elementsHTML/bouton/BoutonSubmit.vue';
import construitURLService from '@/services/construitURL.service.vue';
//import {Corbeille} from "@/components/Model/Corbeille.Class";
//import {Corbeille} from "@/components/Model/CorbeilleTS.Class";
//import Corbeille from '@/components/Model/CorbeilleJS.Class';
import creationJSONService from '@/services/creationJSON.service.vue';
import configuration from '@/administration/configuration.vue';
import connexionAPIService from '@/services/connexionAPI.service.vue';

export default {
  name: 'FormulaireCorbeille',
  components: {
    BoutonBase,
    BoutonSubmit,
  },

  data() {
    return {
      action: construitURLService.methods.construitURLConnectionBack(
        'dossier',
        configuration.data().urlPossibles.modifier
      ),
    };
  },
  mounted() {
    this.$parent.initFormulaire();
  },
  methods: {
    async effacerFormulaire() {
      for (let valeur of document.getElementsByClassName('detailCorbeille')[0]
        .children) {
        /*console.log(valeur.lastChild.value);*/
        valeur.lastChild.value = '';
      }
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

/*section{
  display: flex;
  padding: 10px;
}*/

.detailCorbeille {
  width: 90%;
  max-width: content-box;
  border-radius: 5px;
  padding: 10px;
  background: white;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 0.2rem 0.5rem;
  box-shadow: 0px 5px 5px -5px;
}

.detailCorbeilleMineur {
  width: 90%;
  max-width: content-box;
  border-radius: 5px;
  padding: 10px;
  background: white;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 0.2rem 0.5rem;
  box-shadow: 0px 5px 5px -5px;
}

.etudiantMineur {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.detailCorbeilleMineur .etudiantMineur div {
  margin: auto;
}

.ligneHorizontaleFormulaire {
  margin: 1em;
  height: 3px;
  background: linear-gradient(#e66465, #9198e5);
  width: 95%;
}

.titreFormulaireFacturier {
  text-align: center;
}

.inputBoxFacturier {
  display: flex;
  flex-direction: column;
  margin-right: auto;
}

/*.detailCorbeille div:last-child{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
}*/
</style>

/* Suggestions : -prévoir d'utiliser une API pour rentrer des données
géographiques -prévoir d'utiliser une API pour rentrer des données d'entreprise
-faire une validation des données afin de vérifier quelles sont bien dans le bon
format */
