<template>
  <ion-page>
    <ion-content :fullscreen="true">
      <div id="container">
        <form>
          <h1>habits</h1>

          <Input
            type="text"
            v-model="formData.email"
            placeholder="Endereço de e-mail"
          /> 

          <Button :is-loading="isLoading" @click="submitForm">
            Continuar
          </Button>
        </form>

        <router-link to="/signin">
          Voltar ao login
        </router-link>
      </div>

      <Toast ref="toastRef" />
    </ion-content>
  </ion-page>
</template>

<script setup>
import { ref, reactive, computed } from 'vue';
import { useRouter } from 'vue-router';
import { IonPage, IonContent } from '@ionic/vue';
import { useVuelidate } from '@vuelidate/core';
import { required, email } from '@vuelidate/validators';
import axios from '@/api/axios';
import Input from '@/components/Input.vue';
import Button from '@/components/Button.vue';
import Toast from '@/components/Toast.vue';

const router = useRouter();
const isLoading = ref(false);
const toastRef = ref(undefined);
const formData = reactive({
  email: ''
});

const handleContinue = async () => {
  isLoading.value = true;

  const response = await axios.post('/forgot', formData);

  isLoading.value = false;

  if (response.status === 'error') {
    toastRef.value?.setOpen(true, response.status, response.data);
    return;
  }

  toastRef.value?.setOpen(true, response.status, response.data);

  router.push('/forgot/token'); 
};

const rules = computed(() => {
  return {
    email: { required, email }
  };
});

const v$ = useVuelidate(rules, formData);

const submitForm = async () => {
  const isFormCorrect = await v$.value.$validate();

  if (!isFormCorrect) {
    toastRef.value?.setOpen(true, 'error', 'Informe um e-mail válido');
    return;
  } 
  
  handleContinue();
};
</script>

<style scoped>
#container {
  display: flex;
  flex-direction: column;
}
form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin-top: 5rem;
  margin-bottom: 3rem;
  padding: 0 .5rem;
}

form h1 {
  font-size: 3rem;
  text-align: center;
  color: var(--font);
  font-weight: 800;
}

form a {
  display: block;
  text-align: right;
}

a {
  font-size: .85rem;
  text-align: center;
  text-decoration: none;
  letter-spacing: .25px;
  color: var(--success);
}
</style>