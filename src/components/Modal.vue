<template>
  <div class="modal" v-if="dialog">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <h5 class="modal-title text-center">Edit user</h5>
        <div class="modal-body">
          <form>
            <div class="form-group">
              <label for="name" class="col-form-label">Name:</label>
              <input class="form-control" id="name" v-model="editedItem.name" value="name" type="text" >
            </div>
            <div class="form-group">
              <label for="email" class="col-form-label">Email:</label>
              <input v-model="editedItem.email" type="text" class="form-control" value="email" id="email">
            </div>
            <div class="form-group">
              <label for="city" class="col-form-label">Website:</label>
              <input v-model="editedItem.website" type="text" class="form-control" value="city" id="city">
            </div>
          </form>
        </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary mr-3" @click="emitClose">Close</button>
            <button type="button" class="btn btn-primary" @click="saveItem">Save</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
axios.defaults.baseURL = "https://jsonplaceholder.typicode.com";

export default {
  name: "Modal",
  props: {
    dialog: false,
    editedItem: {
      name: "",
      email: "",
      website: ""
    },
    editedIndex: ""
  },

  data() {
    return {
      newUser: {}
    };
  },

  methods: {
    emitClose() {
      this.$emit("close");
    },

    saveItem() {
      const index = this.editedIndex;
      const item = this.editedItem;
      return axios.put(`/users/${index}`, item).then(res => {
        this.newUser = res.data;
        this.$emit("save", [this.newUser, this.editedIndex]);
      });
      this.emitClose();
    }
  }
};
</script>

<style></style>
