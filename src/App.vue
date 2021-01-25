<template>
  <div id="app" class="m-6">
    <input type='button' value="Добавить" class="addBtn" v-on:click="toggleModalVisibility"/>
    <div class="contacts-container mb-6">
      <contacts-table :contacts="contacts"></contacts-table>
      <contact-modal :contacts="contacts" v-if="isModalVisible" @closeModal="toggleModalVisibility"></contact-modal>
    </div>
  </div>
</template>

<script>
import ContactModal from './components/Modal/ContactModal.vue'
import ContactsTable from './components/Table/ContactsTable.vue'

export default {
  name: 'App',
  data () {
    return {
      isModalVisible: false,
      contacts: []
    }
  },
  components: { ContactsTable, ContactModal },
  created () {
    const contactsFromStorage = JSON.parse(localStorage.getItem('contacts'))

    if (contactsFromStorage) {
      this.contacts = contactsFromStorage
    }
  },
  methods: {
    toggleModalVisibility () {
      this.isModalVisible = !this.isModalVisible
    }
  }
}
</script>

<style>
#app {
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 5px;
}

.addBtn {
  width: 100px;
  height: 30px;
  border: none;
  border-radius: 8px;
  background-color: #337ab7;
  color: #fff;
  cursor: pointer;
}

.addBtn:focus {
  outline: none;
}

.contacts-container {
  display: inline-flex;
  flex-wrap: wrap;
  margin-top: 8px;
  width: 100%;
}
</style>
