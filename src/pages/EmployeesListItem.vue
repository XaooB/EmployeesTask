<template>
  <fragment>
    <td v-text="id">{{employee.id}}</td>

    <td v-if="editing && id === employee.id">
      <input type="text" v-model="name" />
    </td>
    <td v-else v-text="name">{{employee.name}}</td>

    <td v-if="editing && id === employee.id">
      <input type="text" v-text="address.street" v-model="address.street" />
      <input type="text" v-text="address.suite" v-model="address.suite" />
      <input type="text" v-text="address.city" v-model="address.city" />
    </td>
    <td
      v-else
      v-text="address.street + ' ' + address.suite + ' ' + address.city"
    >{{employee.address.street}} {{employee.address.suite}} {{employee.address.city}}</td>

    <td v-if="editing && id === employee.id">
      <input type="text" v-model="phone" />
    </td>
    <td v-else v-text="phone">{{employee.phone}}</td>

    <td v-if="editing && id === employee.id">
      <input type="text" v-model="email" />
    </td>
    <td v-else>
      <a v-text="email" :href="`mailto:${ employee.email }`">{{employee.email}}</a>
    </td>

    <td>
      <EditButton
        :toggleEdit="() => toggleEdit(employee)"
        :update="updateEmployee"
        :status="editing"
        :updating="updating"
      />
    </td>
  </fragment>
</template>
<script>
import axios from "axios";
import Vue from "vue";
import { Plugin } from "vue-fragment";
Vue.use(Plugin);
import EditButton from "../components/EditButton";

export default {
  props: {
    employee: Object
  },
  data() {
    return {
      updating: false,
      id: null,
      name: "",
      address: {
        street: "",
        suite: "",
        city: ""
      },
      phone: "",
      email: "",
      editing: false
    };
  },
  methods: {
    toggleEdit({ id, name, address, phone, email }) {
      if (!this.editing) {
        this.editing = true;
      } else {
        this.editing = false;
      }
    },
    updateEmployee() {
      const { name, address, phone, email } = this;

      this.updating = true;
      axios
        .put(`https://jsonplaceholder.typicode.com/users/${this.id}`, {
          name,
          phone,
          address,
          email
        })
        .then(response => {
          if (response.status === 200) {
            this.name = response.data.name;
            this.phone = response.data.phone;
            this.email = response.data.email;
            this.address.street = response.data.address.street;
            this.address.suite = response.data.address.suite;
            this.address.city = response.data.address.city;
          } else {
            alert("Błąd");
          }
          this.editing = false;
        })
        .finally(() => {
          this.updating = false;
        });
    }
  },
  mounted: function() {
    this.$nextTick(function(e) {
      this.id = this.employee.id;
      this.name = this.employee.name;
      this.phone = this.employee.phone;
      this.email = this.employee.email;
      this.address.street = this.employee.address.street;
      this.address.suite = this.employee.address.suite;
      this.address.city = this.employee.address.city;
    });
  },
  components: {
    EditButton
  }
};
</script>
<style lang="scss" scoped>
input {
  width: 100%;
  padding: 6px;
}
.employees-list {
  &__header {
    font-size: 20px;
    padding: 0 0 10px;
  }

  &__loading {
    color: #999d9d;
    text-align: center;
  }

  &__list {
    font-size: 14px;
    width: 100%;
    border-collapse: collapse;
  }

  &__list-header {
    position: absolute;
    left: -9999px;
    background: #f7f8f9;
    border-bottom: 1px solid #999d9d;
    th {
      padding: 8px;
    }
    @media (min-width: 736px) {
      position: initial;
    }
  }

  &__list-row {
    background: #fff;
    position: relative;
    td {
      position: relative;
      display: block;
      padding: 8px 8px 8px 70px;
      &:before {
        position: absolute;
        top: 8px;
        font-weight: bold;
        left: 0;
      }
      &:nth-of-type(1):before {
        content: "ID:";
      }
      &:nth-of-type(2):before {
        content: "Name:";
      }
      &:nth-of-type(3):before {
        content: "Address:";
      }
      &:nth-of-type(4):before {
        content: "Phone:";
      }
      &:nth-of-type(5):before {
        content: "Email:";
      }
      &:nth-of-type(6):before {
        content: "Akcja:";
      }
      @media (min-width: 736px) {
        padding: 8px;
        display: table-cell;
        &:before {
          content: "" !important;
        }
      }
    }
    &:not(:last-child) {
      border-bottom: 1px solid black;
    }
    @media (min-width: 736px) {
      &:not(:last-child) {
        border: none;
      }
    }
  }
}
</style>