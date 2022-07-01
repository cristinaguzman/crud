<template>
  <v-data-table :headers="headers" :items="users" :loading="loading">
    <template v-slot:[`item.details`]="{ item }">
      <v-btn icon color="error" @click="removeUser(item.id)">
        <v-icon>mdi-delete</v-icon>
      </v-btn>

      <v-dialog width="500">
        <template v-slot:activator="{ on }">
          <v-btn icon color="info" v-on="on">
            <v-icon>mdi-eye</v-icon>
          </v-btn>
        </template>

        <v-card>
          <v-card-title>Formulario</v-card-title>
          <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="Name"
      required
    ></v-text-field>

    <v-text-field
      v-model="email"
      :rules="emailRules"
      label="E-mail"
      required
    ></v-text-field>

    <v-select
      v-model="select"
      :items="items"
      :rules="[v => !!v || 'Item is required']"
      label="Item"
      required
    ></v-select>

    <v-checkbox
      v-model="checkbox"
      :rules="[v => !!v || 'You must agree to continue!']"
      label="Do you agree?"
      required
    ></v-checkbox>

    <v-btn
      :disabled="!valid"
      color="success"
      @click="validate"
    >
      Validate
    </v-btn>

    <v-btn
      color="error"
      @click="reset"
    >
      Reset Form
    </v-btn>

    <v-btn
      color="warning"
      @click="resetValidation"
    >
      Reset Validation
    </v-btn>
  </v-form>
        </v-card>
      </v-dialog>
    </template>

    <!--
    <template #item.name="{item}">
      {{ item.name.toUpperCase() }}
    </template>

    <template #item.pepito="{item}">
      {{ item.name.toUpperCase() }}
    </template> -->
  </v-data-table>
</template>

<script>
import { mapState, mapActions } from 'vuex';
export default {
  data: () => ({
    headers: [
      { text: '#', value: 'id' },
      { text: 'Nombre', value: 'name' },
      { text: 'Apellido', value: 'lastName' },
      { text: 'Detalle', value: 'details' },
    ],
  }),
  computed: {
    ...mapState('users', {
      users: (state) => state.list,
      loading: (state) => state.loading,
    }),
  },
  methods: {
    ...mapActions('users', {
      removeOneUserById: 'removeOneById',
      getAllUsers: 'getAll',
    }),
    async removeUser(userId) {
      await this.removeOneUserById(userId);
      await this.getAllUsers();
    },
  },
};
</script>

<style></style>
