<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Khan Academy</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Khan Academy</ion-title>
        </ion-toolbar>
      </ion-header>

      <div id="container">
        <div v-if="!isWebViewOpen">
          <strong>¡Bienvenido a Khan Academy!</strong>
          <p>Presiona el botón para comenzar tu aprendizaje</p>
          <ion-button 
            expand="block" 
            size="large" 
            @click="openKhanAcademy"
            :disabled="isLoading"
          >
            <ion-icon :icon="playCircle" slot="start"></ion-icon>
            {{ isLoading ? 'Cargando...' : 'Iniciar Khan' }}
          </ion-button>
        </div>
        
        <div v-if="resultMessage" id="result" v-html="resultMessage"></div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButton, IonIcon } from '@ionic/vue';
import { playCircle } from 'ionicons/icons';
// @ts-ignore
import { custominappbrowser } from 'custominappbrowser';

const isWebViewOpen = ref(false);
const isLoading = ref(false);
const resultMessage = ref('');

const openKhanAcademy = async () => {
  const formattedUrl = 'https://es.khanacademy.org/';
  
  try {
    isLoading.value = true;
    resultMessage.value = "<strong>Opening:</strong> " + formattedUrl;
    
    const result = await custominappbrowser.openUrl({ url: formattedUrl });
    
    if (result.success) {
      resultMessage.value = `<strong>Success:</strong> Fullscreen WebView opened for ${formattedUrl}`;
      isWebViewOpen.value = true;
    }
  } catch (error) {
    resultMessage.value = `<strong>Error:</strong> ${error}`;
  } finally {
    isLoading.value = false;
  }
};
</script>

<style scoped>
#container {
  text-align: center;
  padding: 20px;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 24px;
  line-height: 30px;
  color: #2d3748;
  display: block;
  margin-bottom: 10px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0 0 30px 0;
}

ion-button {
  margin: 20px 0;
  --border-radius: 12px;
  height: 56px;
  font-weight: 600;
}

#result {
  margin-top: 20px;
  padding: 15px;
  border-radius: 8px;
  background-color: #f7fafc;
  border: 1px solid #e2e8f0;
  font-size: 14px;
}

#result strong {
  font-size: 14px;
  margin-bottom: 5px;
}
</style>
