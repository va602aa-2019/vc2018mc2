<template>
  <div class="container">
    <div class="header">
      <h1>Inquinamento idrico nella riserva di Boonsong Lekagul</h1>

      <ul class="nav nav-tabs">
        <li class="nav-item"></li>
        <li>
          <a class="nav-link" href="/">Home</a>
        </li>
        <li>
          <a class="nav-link" href="#/outliers_table">Tabella degli outliers</a>
        </li>
        <li>
          <a class="nav-link" href="#/dot_plot">Calendario dei rilevamenti</a>
        </li>
        <li>
          <a class="nav-link" href="#/conclusioni">Conclusioni</a>
        </li>
      </ul>
    </div>

    <div class="row justify-content-center">
      <div class="col-md-6">
        <img src="~@/assets/inquinamento_acqua.jpg" class="img-fluid" alt="Responsive image">
      </div>
    </div>

    <div class="row justify-content-center no-gutters">
      <div class="col-md-6 text-justify">
        <h2>L'evento</h2>
        <p>
          L'anno scorso, Kasios Furniture Company è stata coinvolta in danni ambientali alla Riserva della fauna selvatica di Boonsong Lekagul sia per lo scarico di rifiuti tossici che per l'inquinamento dell'aria con prodotti chimici dal suo processo di produzione.
          La Kasios Company non sta prendendo seriamente queste accuse e nega ogni accusa di scarico dei rifiuti industriali! I portavoce di Kasios affermano che non vi è alcuna contaminazione del terreno vicino alla stazione remota dei ranger suggerita dai partecipanti
          alla mini-sfida 1 e 3 dello scorso anno della VastChallenge,
          e ispezionando l'area è stata dichiarata incontaminata come il resto della riserva.
          Con una prova regina contro Kasios ormai scomparso, gli investigatori dovranno adottare un altro approccio. I professori del dipartimento di idrologia del Mistford College si sono fatti avanti con diversi anni di letture del sensore dell'acqua da fiumi e torrenti nella riserva.
          Questi campioni sono stati prelevati da diverse località sparse in tutta l'area e contengono misure di diverse sostanze chimiche di possibile interesse, ma non sono mai state analizzate a causa della mancanza di fondi.
        </p>
        <p>
          <b>L'analisi visiva può aiutare a rivelare qualcosa in questi dati che potrebbe compensare la prova del suolo che è stata distrutta?</b>
          Dalle analisi statistiche effettuate sui dati e attraverso la visualizzazione di essi, con varie tipologie di grafici, sono emersi i risultati che certificano l'inquinamento idrico della Riserva.
        </p>
      </div>
      <div class="col-md-4">
        <stationsMap @stations-active="stationsActive"/>
      </div>
    </div>

    <div class="container-fluid">
      <calendarView :stations="activeStations"/>
    </div>
  </div>
</template>

<script>
import stationsMap from './StationsMap'
import calendarView from './CalendarView'

export default {
  name: 'HomePage',
  data () {
    return {
      stats: [],
      activeStations: []
    }
  },
  mounted () {},
  methods: {
    stationsActive: function (n) {
      console.log('Stazioni attive', n)
      this.activeStations = n
    }
  },
  watch: {},
  components: {
    stationsMap: stationsMap,
    calendarView: calendarView
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
  .nav-tabs .nav-link{
    border:1px solid gray;
  }
  .nav-tabs .nav-item{
    margin-left: 16em;
  }
  a.nav-link{
    background-color: beige;
  }
</style>
