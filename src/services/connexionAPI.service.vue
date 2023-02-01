<script>
var ErreurHTML = '';
export default {
  name: 'connexionAPI.service',
  methods: {
    async requete(url, form) {
      le nomsASupprimer = [];
      if(form) {
        Array.from(form.querySelectorAll('.nontransmis')).reduce((a,c) => {
          a.push(c.name)
          return a;
        }, nomsASupprimer);
      }
      let f = form ? new FormData(form) : new FormData();
      nomsASupprimer.forEach(n=>{
        f.delete(n);
      });
      return await fetch(url, {
        method: 'POST',
        body: f,
      })
        .then(async (reponse) => {
          if (!reponse.ok) {
            throw new Error(`HTTP erreur. status : ${reponse.status}`);
          }
          /*if (true) {
            //ErreurHTML = reponse.text();
            let r = reponse.body.tee();
            ErreurHTML = r[0].text();
          }
          return r[1].json();*/
          return reponse.json();
        })
        .catch((e) => {
          if (ErreurHTML) {
            document.body.append(ErreurHTML);
            ErreurHTML = '';
          }
          throw new Error('Attention : ' + e.message);
        });
    },
  },
};
</script>
