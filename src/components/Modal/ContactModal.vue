<template>
  <div class="modal" :class="{'modal-expandedHeight': warning}">
    <div class="modal-header">
      <h4 class="modal-header-title">Добавление пользователя</h4>
      <input
        type="button"
        class="modal-header-btn"
        value="x"
        v-on:click="closeModal"
      />
    </div>
    <contact-modal-field label="Имя" forId="name">
      <input
        class="modal-field-content"
        name="name"
        id="name"
        :value="name"
        v-on:change="changeFieldValue"
      />
      <div class="modal-field-warning" v-if="warning && !name">
        Поле обязательно для заполнения
      </div>
    </contact-modal-field>
    <contact-modal-field label="Телефон" forId="phone">
      <input
        class="modal-field-content"
        name="phone"
        id="phone"
        :value="phone"
        v-on:change="changeFieldValue"
      />
      <div class="modal-field-warning" v-if="warning && !phone">
        Поле обязательно для заполнения
      </div>
    </contact-modal-field>
    <contact-modal-field label="Начальник" forId="chief">
      <select
        class="modal-field-content"
        name="chiefId"
        id="chief"
        :value="chiefId"
        v-on:change="changeFieldValue"
      >
        <option></option>
        <option
          v-for="contact in flatted"
          :key="contact.id"
          :value="contact.id"
        >
          {{ contact.name }}
        </option>
      </select>
    </contact-modal-field>
    <input
      type="button"
      class="modal-saveBtn"
      value="Сохранить"
      v-on:click="save"
    />
  </div>
</template>

<script>
import { nanoid } from 'nanoid'
import { uuid } from '../../utils/index'
import ContactModalField from './ContactModalField.vue'

export default {
  components: { ContactModalField },
  name: 'ContactModal',
  props: ['contacts'],
  data () {
    return {
      id: '',
      name: '',
      phone: '',
      chiefId: '',
      warning: false
    }
  },
  computed: {
    flatted () {
      return this.flat(this.contacts).map(contact => {
        return {
          id: contact.id,
          name: contact.name
        }
      })
    }
  },
  methods: {
    closeModal () {
      this.$emit('closeModal')
    },

    clearAllFields () {
      this.name = ''
      this.phone = ''
      this.chiefId = ''
      this.id = ''
    },

    changeFieldValue (event) {
      this[event.target.name] = event.target.value
    },

    flat (items) {
      let final = []
      items.forEach(item => {
        final.push(item)

        if (typeof item._children !== 'undefined') {
          final = final.concat(this.flat(item._children))
        }
      })

      return final
    },

    putContactToList () {
      const contacts = this.contacts
      const newContact = {
        id: nanoid(8),
        name: this.name,
        phone: this.phone,
        _children: [],
        _meta: {
          groupColumn: null,
          groupParent: 0,
          index: 0,
          loading: false,
          parent: 0,
          selected: false,
          uniqueIndex: uuid(),
          visibleChildren: []
        },
        _selectable: false,
        _showChildren: false
      }

      if (!this.chiefId) {
        contacts.push(newContact)
      } else {
        const searchedChief = this.flat(contacts).find(contact => contact.id === this.chiefId)
        newContact._meta.parent = searchedChief._meta.parent + 1
        newContact._meta.index = searchedChief._children.length > 0 ? searchedChief._children.length + 1 : 0

        searchedChief._meta.visibleChildren.push(newContact)
        searchedChief._children.push(newContact)
      }
    },

    save () {
      if (!this.name || !this.phone) {
        this.warning = true
        return
      }

      this.putContactToList(this.contacts)

      this.clearAllFields()

      localStorage.setItem('contacts', JSON.stringify(this.contacts))
    }
  }
}
</script>

<style scoped>
.modal {
  width: 300px;
  height: 230px;
  padding: 5px 10px;
  border: 2px solid #e2e8f0;
  box-sizing: border-box;
}

.modal-expandedHeight {
  height: 260px;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-header-title {
  margin: 15px 0;
}

.modal-header-btn {
  background-color: transparent;
  cursor: pointer;
  border: none;
  font-size: 15px;
  width: 30px;
  height: 30px;
}

.modal-header-btn:focus {
  outline: none;
}

.modal-field-content {
  max-width: 170px;
  width: 170px;
  height: 25px;
  box-sizing: border-box;
  border: 1px solid  #e2e8f0;
}

.modal-field-warning {
  margin-top: 3px;
  font-size: 10px;
  color: red;
}

.modal-saveBtn {
  display: flex;
  justify-content: center;
  width: 100px;
  height: 30px;
  border: none;
  border-radius: 8px;
  background-color: #337ab7;
  color: #fff;
  margin-top: 20px;
  cursor: pointer;
}

.modal-saveBtn:focus {
  outline: none;
}
</style>
