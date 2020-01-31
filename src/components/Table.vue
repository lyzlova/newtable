<template>
    <div class="container">
      <h3 class="text-center mb-3">Users</h3>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Id</th>
            <th scope="col">Name</th>
            <th scope="col">Email</th>
            <th scope="col">Website</th>
            <th scope="col">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in users" v-bind:key="item.id">
            <th scope="row">{{item.id}}</th>
            <td>{{item.name}}</td>
            <td>{{item.email}}</td>
            <td>{{item.website}}</td>
            <td>
              <button type="button" class="btn btn-primary" @click="editItem(item, item.id)">Edit</button>
            </td>
          </tr>
        </tbody>
        <Modal :dialog="dialog"
          v-bind:editedItem="editedItem"
          @save="saveItem"
          :editedIndex="editedIndex"
          @close="closeModal" />
      </table>
    </div>
  </template>

<script>

import axios from 'axios';
axios.defaults.baseURL = 'https://jsonplaceholder.typicode.com';
import Modal from './Modal.vue';

export default {
  name: 'Table',
  components: {
    Modal,
  },

  data: () => ({
    dialog: false,
    users: [],
    editedIndex: -1,
    editedItem: {
      name: '',
      emael: '',
      website: '',
    },
  }),

  created() {
    return axios.get('/users').then(res => {
      this.users = res.data;
    });
  },

  methods: {
    editItem(item, index) {
      this.editedIndex = index;
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    closeModal() {
      this.dialog = false;
    },

    saveItem(item) {
      this.editedIndex = item[1] - 1;
      this.editedItem = item[0];
      if (this.editedIndex > -1) {
        Object.assign(this.users[this.editedIndex], this.editedItem);
      } else {
        this.users.push(this.editedItem);
      }
      this.closeModal();
    },
  },
};
</script>

<style></style>
