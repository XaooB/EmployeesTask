<template>
  <div class="page employees-list">
    <h1 class="employees-list__header">Employees</h1>
    <div v-if="loading" class="employees-list__loading">Loading...</div>
    <table v-else class="employees-list__list">
      <tr class="employees-list__list-header">
        <th>id</th>
        <th>Name</th>
        <th>Address</th>
        <th>Phone</th>
        <th>Email</th>
        <th>Akcja</th>
      </tr>
      <tr v-for="employee in employees" class="employees-list__list-row">
        <ListItem :employee="employee" />
      </tr>
    </table>
  </div>
</template>
<script>
import axios from "axios";
import EditButton from "../components/EditButton";
import ListItem from "../pages/EmployeesListItem";

export default {
  data() {
    return {
      loading: false,
      editing: false,
      selectedID: 0,
      employees: []
    };
  },
  created() {
    this.fetchData();
  },
  watch: {
    $route: "fetchData"
  },
  methods: {
    fetchData() {
      this.loading = true;
      axios
        .get("https://jsonplaceholder.typicode.com/users")
        .then(({ data }) => {
          this.employees = data;
        })
        .finally(() => {
          this.loading = false;
        });
    }
  },
  components: {
    ListItem
  }
};
</script>
<style lang="scss" scoped>
input {
  width: 100%;
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