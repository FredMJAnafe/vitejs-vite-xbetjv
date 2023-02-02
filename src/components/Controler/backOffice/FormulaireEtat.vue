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
            <option value="TRANSMIS">TRANSMIS</option>
            <option value="EN_COURS_INSTRUCTION">INSTRUCTION EN COURS</option>
            <option value="ENGAGE">ENGAG&Eacute;</option>
            <option value="ANNULE">ANNUL&Eacute;</option>
            <option value="REFUSE">REFUS&Eacute;</option>
            <option value="RUPTURE">RUPTURE</option>
            <option value="SOLDE">SOLD&Eacute;</option>
          </select>
        </div>
        <div class="_inputBoxFacturier" v-if="etat == 'TRANSMIS'">
          <span class="detailFacturier"
            >En cochant cette case, vous confirmez que vous voulez transmettre
            le dossier sélectionné à l'OPCO concerné.</span
          >
          <input
            name="changeetat"
            type="checkbox"
            class="nontransmis"
            required
          />
          <input
            name="__commandedistante__"
            type="hidden"
            value="dossier.cerfa.etat"
            required
          />
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
  <div class="erreur" v-if="!!erreur">Retour OPCO : {{ erreur }}</div>
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
      erreur: '',
      etat: '',
      action: construitURLService.methods.construitURLConnectionBack(
        'dossier',
        configuration.data().urlPossibles.modifier
      ),
    };
  },
  mounted() {
    this.$parent.initFormulaire();
    this.$el.parentNode.addEventListener(
      'onEspaceSubmitSuccessB',
      this.onSubmit.bind(this)
    );
  },
  methods: {
    onSubmit(e) {
      let infoDistante = e.detail.reponse.dist_info;
      if (infoDistante) {
        infoDistante = infoDistante.extra_info;
      }
      if (infoDistante && e.detail.reponse.extra_info) {
        let i = this.$parent.items[this.$parent.indexCourant];
        infoDistante = infoDistante[i.opco];
      }
      if (infoDistante) {
        this.erreur = infoDistante.erreur;
        if(this.erreur) {
          e.stopImmediatePropagation();
        }
      }
    },
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
.erreur {
  margin:5px;
  padding:3px;
  display:block;
  background:red;
  color:white;
}

/*section{
  display: flex;
  padding: 10px;
}*/
</style>
