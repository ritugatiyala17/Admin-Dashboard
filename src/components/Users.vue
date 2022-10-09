<template>
  <div>
    <table
      class="
        table table-borderless table-hover table-responsive{-sm|-md|-lg|-xl}
      "
    >
      <thead class="thead-light">
        <tr :class="users.length === 0 ? 'no-users' : ''">
          <th class="cursor-pointer border-rounded-left" scope="col">
            <input
              type="checkbox"
              @change="selectAll"
              v-model="isSelectAll"
              class="cursor-pointer"
              id="select-all-users"
            />
          </th>
          <th
            class="text-left"
            v-for="(header, index) in listHeaders"
            :key="index"
            scope="col"
          >
            {{ toCapitalise(header) }}
          </th>
          <th class="border-rounded-right" scope="col">Actions</th>
        </tr>
      </thead>
      <Empty
        v-show="!users.length"
        :isSearchString="isSearchString"
        @search-user="onSearchUser"
      />
      <tbody v-if="users.length" class="tbody-light">
        <tr
          v-for="user in users"
          v-bind:key="user.id"
          @click="rowSelected(user)"
          class="cursor-pointer"
        >
          <td>
            <input
              type="checkbox"
              :checked="user.checked"
              @change="rowSelected(user)"
              :name="user.id"
              :id="user.id"
            />
          </td>
          <td class="text-left">{{ user.name }}</td>
          <td class="text-left">{{ user.email }}</td>
          <td class="text-left">{{ toCapitalise(user.role) }}</td>
          <td>
            <DeleteUserAction @delete-user="onDeleteUser(user.id)" />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import DeleteUserAction from "./DeleteUserAction.vue";
import Empty from "./states/Empty.vue";

export default {
  name: "Users-List",
  props: {
    usersList: {
      type: Array,
      default: () => [],
    },
    listHeaders: {
      type: Array,
      default: () => [],
    },
    selectedUsers: {
      type: Array,
      default: () => [],
    },
    isSearchString: {
      type: Boolean,
      default: false,
    },
  },
  components: { DeleteUserAction, Empty },
  data() {
    return {
      selected: false,
    };
  },
  computed: {
    users() {
      let currentUsers = this.usersList;
      if (this.selectedUsers.length) {
        for (let user of this.selectedUsers) {
          let index = currentUsers.findIndex((u) => u.id == user.id);
          if (index > -1) {
            currentUsers[index].checked = true;
          } else {
            delete currentUsers[index].checked;
          }
        }
      }
      return currentUsers;
    },
    isSelectAll: {
      get() {
        if (this.selectedUsers.length === 0) {
          return false;
        }
        return this.selected;
      },
      set(val) {
        this.selected = val;
      },
    },
  },
  methods: {
    toCapitalise(str) {
      return str.slice(0, 1).toUpperCase() + str.slice(1);
    },
    rowSelected(row) {
      if (row.checked) {
        let index = this.users.findIndex((u) => u.id == row.id);
        if (index > -1) {
          this.users[index].checked = false;
        }
        this.$emit("row-unselected", row.id);
      } else {
        this.$emit("row-selected", row.id);
      }
    },
    onDeleteUser(rowId) {
      this.$emit("row-deleted", rowId);
    },
    selectAll(event) {
      if (event.target.checked) {
        this.$emit("select-all-users");
      } else {
        this.isSelectAll = false;
        this.$emit("unselect-all-users");
      }
    },
    onSearchUser(searchedStr) {
      console.log("here in users", searchedStr);
      this.$emit("search-user", searchedStr);
    },
  },
  watch: {
    isSelectAll() {
      console.log("isSelectAll has changed", this.isSelectAll);
    },
  },
};
</script>
<style scoped>
.text-left {
  align-items: start;
}
.table {
  overflow-y: scroll;
}
th {
  font-weight: 500;
}
.border-rounded-left {
  max-width: 100px;
  border-top-left-radius: 4px !important;
  border-bottom-left-radius: 4px !important;
}
.border-rounded-right {
  border-top-right-radius: 4px !important;
  border-bottom-right-radius: 4px !important;
}
.no-users {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  background-color: #e9ecef;
  border-color: #dee2e6;
  margin-right: 10rem;
}
</style>