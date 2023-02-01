<template>
  <form :data-service="action" class="formulaireAjoutFacturier">
    <fieldset class="titreFormulaireFacturier">
      <legend>Etat du dossier</legend>
      <input type="hidden" name="__base__" value="cerfa" />
      <input type="hidden" name="__id__" value="0" />
      <div class="detailApprenti">
        <div class="_inputBoxFacturier">
          <span class="detailFacturier"> </span>
          <select name="etat" required @change="setEtat">
            <option value="0">Choisir</option>
            <option value="0">INITIAL (0)</option>
            <option value="TRANSMISSION_EN_COURS">TRANSMISSION EN COURS</option>
            <option value="TRANSMIS">TRANSMIS</option>
            <option value="EN_COURS_INSTRUCTION">INSTRUCTION EN COURS</option>
            <option value="ENGAGE">ENGAG&Eacute;</option>
            <option value="ANNULE">ANNUL&Eacute;</option>
            <option value="REFUSE">REFUS&Eacute;</option>
            <option value="RUPTURE">RUPTURE</option>
            <option value="SOLDE">SOLD&Eacute;</option>
          </select>
        </div>
        <div class="_inputBoxFacturier" v-if="etat == 'TRANSMISSION_EN_COURS'">
          <span class="detailFacturier"
            >En cochant cette case, vous confirmez que vous voulez transmettre
            le dossier sélectionné à l'OPCO concerné.</span
          >
          <input name="__changeetat__" type="checkbox" required />
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
        data-formid="formMaitre"
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
import Maitre from '@/components/Model/MaitreJS.Class';
import configuration from '@/administration/configuration.vue';
import connexionAPIService from '@/services/connexionAPI.service.vue';

export default {
  name: 'FormulairOpco',
  components: {
    BoutonBase,
    BoutonSubmit,
  },

  data() {
    return {
      etat: '',
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
    setEtat(e) {
      this.etat = e.target.value;
    },
    async effacerFormulaire() {
      for (let valeur of document.getElementsByClassName('detailMaitre')[0]
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
</style>
