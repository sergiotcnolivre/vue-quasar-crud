<!-- eslint-disable max-len -->
<template>
  <q-page padding>
    <q-form @submit="handleSubmit()" class="row q-col-gutter-sm">
      <q-input filled v-model="form.name" label="Your Name *" lazy-rules :rules="[val => val && val.length > 0 || 'Please type min 1 and max 191 characters']" class="col-lg-6 col-xs-12" type="text" />
      <q-input filled v-model="form.email" label="Your email *" lazy-rules class="col-lg-6 col-xs-12" type="email" :rules="[(val, rules) => rules.email(val) || 'Please enter a valid email address']" />
      <q-select outlined v-model="form.gender" :options="selectOptions" label="Gender" class="col-lg-6 col-xs-12" type="text" :rules="[val => !!val || 'Field is required']" />
      <q-select outlined v-model="form.status" :options="statusOptions" label="Status" class="col-lg-6 col-xs-12" type="text" :rules="[val => !!val || 'Field is required']" />
      <q-select v-model="form.id" class="col-lg-6 col-xs-12" style="display: none;" />

      <div class="col-12 q-gutter-sm">
        <q-btn label="Save" type="submit" color="primary" class="float-right" icon="save" @submit="handleSubmit()" />
        <q-btn label="Cancelar" color="white" class="float-right" text-color="primary" :to="{ name: 'home' }" />
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue';
import { api } from 'boot/axios';
import { useQuasar } from 'quasar';
import { useRouter, useRoute } from 'vue-router';

export default defineComponent({
  name: 'FormUser',
  setup() {
    const form = ref({
      name: '',
      id: '',
      email: '',
      gender: '',
      status: '',
    });
    const route = useRoute();
    const getUsersById = async (id) => {
      try {
        const { data } = await api.get(`users/${id}`);
        return data.data;
      } catch (error) {
        throw new Error(error);
      }
    };
    onMounted(async () => {
      if (route.params.id) {
        const response = await getUsersById(route.params.id);
        form.value = response;
      }
    });
    const $q = useQuasar();
    const router = useRouter();
    const selectOptions = [
      'male', 'female',
    ];
    const statusOptions = [
      'active', 'inactve',
    ];

    // eslint-disable-next-line @typescript-eslint/no-shadow
    const post = async (form) => {
      try {
        const { data } = await api.post('users', form.value);
        return data.data;
      } catch (error) {
        throw new Error(error);
      }
    };
    // eslint-disable-next-line @typescript-eslint/no-shadow
    const update = async (form) => {
      try {
        const { data } = await api.put(`users/${route.params.id}`, form.value);
        $q.notify({ message: 'User updated', icon: 'check', color: 'positive' });
        router.push({ name: 'home' });
        return data.data;
      } catch (error) {
        throw new Error(error);
      }
    };

    const handleSubmit = async () => {
      if (route.params.id) {
        try {
          await update(form);
        } catch (error) {
          $q.notify({ message: 'Error User not updated', icon: 'times', color: 'negative' });
        }
      } else {
        post(form);
        $q.notify({ message: 'User created', icon: 'check', color: 'positive' });
        router.push({ name: 'home' });
      }
    };

    return {
      form,
      selectOptions,
      statusOptions,
      handleSubmit,
    };
  },
});
</script>

<style></style>
