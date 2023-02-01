<template>
  <form :data-service="action" class="formulaireAjoutFacturier">
    <fieldset class="titreFormulaireFacturier">
      <legend>
        S&eacute;lection OPCO 
      </legend>
      <input type="hidden" name="__base__" value="" />
      <input type="hidden" name="__id__" value="0" />
      <div class="detailApprenti">
        <div class="_inputBoxFacturier">
          <span class="detailFacturier"> </span>
          <select
            name="opco"
            required
          >
          <option value="0">Choisir</option>
            <option v-for="o in listeOpcos" :value="o.nom">{{o.nom}}</option>
          </select>
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
      listeOpcos:this.$parent.opcos,
      action: construitURLService.methods.construitURLConnectionBack(
        'dossier',
        configuration.data().urlPossibles.modifier
      )
    };
  },
  mounted() {
    this.$parent.initFormulaire();
  },
  methods: {
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

