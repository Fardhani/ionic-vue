<template>
  <ion-page>
    <!-- <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1</ion-title>
      </ion-toolbar>
    </ion-header> -->
    <!-- Default Refresher -->
    <!-- <ion-content>
      <ion-refresher slot="fixed" @ionRefresh="doRefresh($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
    </ion-content> -->

    <!-- Custom Refresher Properties -->
    <!-- <ion-content>
      <ion-refresher slot="fixed" pull-factor="0.5" pull-min="100" pull-max="200">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
    </ion-content> -->

    <!-- Custom Refresher Content -->
    <ion-content :fullscreen="true">
      <ion-refresher slot="fixed" @ionRefresh="doRefresh($event)">
        <ion-refresher-content
          :pulling-icon="chevronDownCircleOutline"
          pulling-text="Pull to refresh"
          refreshing-spinner="circles"
          refreshing-text="Refreshing...">
        </ion-refresher-content>
      </ion-refresher>

      <ion-list>
        <ion-card v-for="v in list" :key="v.id">
          <img v-bind:src="v.images[0].name" />
          <ion-card-header>
            <ion-card-subtitle>Card Subtitle</ion-card-subtitle>
            <ion-card-title>{{ v.subj }}</ion-card-title>
          </ion-card-header>

          <ion-card-content>
            <div v-html="v.body"></div>
          </ion-card-content>
        </ion-card>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script>
import { defineComponent } from 'vue';
import { IonPage, IonContent, IonRefresher, IonRefresherContent, IonList, IonCard, IonCardContent, IonCardHeader, IonCardSubtitle, IonCardTitle } from '@ionic/vue';
import { chevronDownCircleOutline } from 'ionicons/icons';
import axios from 'axios'

export default  defineComponent({
  name: 'Tab1Page',
  components: { IonContent, IonPage, IonRefresher, IonRefresherContent, IonList, IonCard, IonCardContent, IonCardHeader, IonCardSubtitle, IonCardTitle },
  data() {
    return {
      list: {},
    }
  },
  setup() {
    return { chevronDownCircleOutline }
  },
  methods: {
    doRefresh: function(event) {
      console.log('Begin async operation');
      axios.get('http://192.168.25.50/auto/api/es/news_ess?userid=0067892&page=1')
        .then(response => {
          // JSON responses are automatically parsed.
          this.list = response.data.result.data
          console.log(response.data.result.data)
          if(event != null) {
            event.target.complete();
          }
        })
        .catch(e => {
          console.log(e)
          if(event != null) {
            event.target.complete();
          }
        })
    }
  },
  mounted(){
      this.doRefresh()
  },
});
</script>
