<!-- eslint-disable max-len -->
<template>
  <div class="q-pa-md">
    <q-form @submit="handleFilter(form.field, form.query)" class="row q-col-gutter-sm justify-center align-center q-mb-md">
      <q-select outlined v-model="form.field" :options="selectOptions" label="Field" class="col-lg-5 col-xs-12" type="text" />
      <q-input filled v-model="form.query" label="Your query" class="col-lg-5 col-xs-12" />
      <div class="row align-center  col-lg-2 col-xs-12">
        <q-btn label="Filter" type="submit" color="primary" style="height: 40px;" />
      </div>
    </q-form>
    <q-table title="Users" :rows="users" :columns="columns" row-key="name">
      <template v-slot:top>
        <q-space />
        <q-btn color="primary" label="Add user" :to="{ name: 'formUser' }" />
      </template>
      <template v-slot:body-cell-actions="props">
        <q-td :props="props" class="q-gutter-sm">
          <q-btn icon="edit" color="info" dense size="sm" @click="handleEditUser(props.row.id)" />
          <q-btn icon="delete" color="negative" dense size="sm" @click="handleDeleteUser(props.row.id)" />
        </q-td>
      </template>
    </q-table>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue';
import { api } from 'boot/axios';
import { useQuasar } from 'quasar';
import { useRouter } from 'vue-router';

export default defineComponent({
  name: 'IndexPage',
  setup() {
    const users = ref([]);
    const form = ref({
      field: '',
      query: '',
    });
    const columns = [
      {
        name: 'id', field: 'id', label: 'Id', sortable: true, align: 'left',
      }, {
        name: 'name', field: 'name', label: 'Name', sortable: true, align: 'left',
      }, {
        name: 'email', field: 'email', label: 'E-mail', sortable: true, align: 'left',
      }, {
        name: 'gender', field: 'gender', label: 'Gender', sortable: true, align: 'left',
      }, {
        name: 'status', field: 'status', label: 'Status', sortable: true, align: 'left',
      }, {
        name: 'actions', field: 'actions', label: 'Actions', align: 'right',
      },
    ];
    const selectOptions = [
      'id', 'name', 'email', 'gender', 'status',
    ];
    const $q = useQuasar();
    const router = useRouter();

    const getUsers = async () => {
      try {
        const { data } = await api.get('users');
        users.value = data.data;
      } catch (error) {
        throw new Error(error);
      }
    };
    // eslint-disable-next-line consistent-return
    const remove = async (id) => {
      try {
        const data = await api.delete(`users/${id}`);
        return data.data;
      } catch (error) {
        throw new Error(error);
      }
    };

    const handleEditUser = async (id) => {
      router.push({ name: 'formUser', params: { id } });
    };

    const handleFilter = (model, query) => {
      router.push({ name: 'formFilter', query: { model, query } });
    };

    const handleDeleteUser = async (id) => {
      try {
        $q.dialog({
          dark: true,
          title: 'Confirm',
          message: 'Would you like to delete user?',
          cancel: true,
          persistent: true,
        }).onOk(async () => {
          await remove(id);
          $q.notify({ message: 'User deleted', icon: 'check', color: 'positive' });
          await getUsers();
        });
      } catch (error) {
        $q.notify({ message: 'Error User delete', icon: 'times', color: 'negative' });
      }
    };

    onMounted(() => {
      getUsers();
    });

    return {
      columns,
      users,
      form,
      selectOptions,
      handleDeleteUser,
      handleEditUser,
      handleFilter,
    };
  },
});

</script>
