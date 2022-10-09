<template>
  <div id="app">
    <div
      class="menu-bar"
      :class="
        selectedUsers.length > 0
          ? 'justify-content-between'
          : 'justify-content-end'
      "
    >
      <UserStatus
        v-show="selectedUsers.length === 0"
        :users="filteredUsers.length"
      />
      <ActionsBar
        v-show="selectedUsers.length > 0"
        :selectedUsers="selectedUsers.length"
        @delete-selected-users="onDeleteSelectedUsers"
        @unselect-users="onUnselectUsers"
      />
      <SearchBar :searchedKeyword="searchString" @search-user="onSearchUser" />
    </div>

    <div class="users-list">
      <!-- <Loading v-if="loading" /> -->
      <Users
        :key="renderComponentKey"
        :usersList="currentPageUsers"
        :listHeaders="listHeaders"
        :selectedUsers="selectedUsers"
        :isSearchString="searchString.length > 0"
        @row-selected="onRowSelection"
        @row-unselected="onRowUnselection"
        @row-deleted="onRowDeletion"
        @select-all-users="onSelectAll"
        @unselect-all-users="onUnselectUsers"
        @search-user="onSearchUser"
      />
    </div>

    <PaginationUsers
      v-show="currentPageUsers.length > 0"
      :totalPages="totalPages"
      :currentPage="currentPage"
      @page-change="onPageChange"
    />
  </div>
</template>

<script>
import axios from "axios";

// import Loading from "./components/states/Loading.vue";
import SearchBar from "./components/SearchBar.vue";
import ActionsBar from "./components/ActionsBar.vue";

import Users from "./components/Users.vue";
import PaginationUsers from "./components/PaginationUsers.vue";
import UserStatus from "./components/UserStatus.vue";

export default {
  name: "App",
  components: {
    // Loading,
    SearchBar,
    ActionsBar,
    Users,
    PaginationUsers,
    UserStatus,
  },
  data() {
    return {
      users: [],
      selectedUsers: [],
      currentPage: 1,
      searchString: "",
      renderComponentKey: false,
      listHeaders: [],
      // loading: false,
    };
  },
  computed: {
    currentPageUsers() {
      return this.filteredUsers.slice(
        (this.currentPage - 1) * 10,
        (this.currentPage - 1) * 10 + 10
      );
    },
    filteredUsers() {
      let searchRegex = new RegExp(this.searchString, "gi");
      return this.users.filter((user) => {
        return (
          searchRegex.test(user.name) ||
          searchRegex.test(user.email) ||
          searchRegex.test(user.role)
        );
      });
    },
    totalPages() {
      return Math.ceil(this.filteredUsers.length / 10);
    },
  },
  created() {
    // this.loading = true;
    axios
      .get(
        "https://geektrust.s3-ap-southeast-1.amazonaws.com/adminui-problem/members.json"
      )
      .then((res) => {
        this.users = res.data;
        let user = this.users[0];
        if (user) {
          this.listHeaders = Object.keys(user).filter(
            (key) => key !== "id" && key !== "checked"
          );
        }
        // this.setLoader();
      })
      .catch((err) => {
        console.log(err.message);
        throw Error(err);
      });
  },
  methods: {
    onRowSelection(id) {
      let user = this.users.find((user) => user.id == id);
      if (user) {
        // if (this.selectedUsers.findIndex((user) => user.id == id) > -1) {
        //   let userAlreadySelected = this.selectedUsers.findIndex(
        //     (user) => user.id === id
        //   );
        //   this.selectedUsers.splice(userAlreadySelected, 1);
        // } else {
        this.selectedUsers.push(user);
        // }
      }
    },
    onRowUnselection(id) {
      let user = this.users.find((user) => user.id == id);
      if (user) {
        let userAlreadySelected = this.selectedUsers.findIndex(
          (user) => user.id === id
        );
        this.selectedUsers.splice(userAlreadySelected, 1);
      }
    },
    onRowDeletion(id) {
      console.log("onRowDeletion", id);
      if (this.currentPageUsers.length == 1 && this.currentPage !== 1) {
        this.currentPage -= 1;
      }
      let userIndex = this.users.findIndex((user) => user.id == id);
      this.users.splice(userIndex, 1);
      let selectedUserIndex = this.selectedUsers.findIndex(
        (user) => user.id === id
      );
      if (selectedUserIndex > -1) {
        this.selectedUsers.splice(selectedUserIndex, 1);
      }
    },
    onSearchUser(searchString) {
      // this.setLoader();
      console.log("onSearchUser", searchString);
      this.searchString = searchString;
      this.currentPage = 1;
    },
    onDeleteSelectedUsers() {
      // this.setLoader();
      if (
        this.selectedUsers.length == this.currentPageUsers.length &&
        this.currentPage !== 1
      ) {
        this.currentPage -= 1;
      }

      for (let user of this.selectedUsers) {
        let index = this.users.findIndex((u) => u.id == user.id);
        if (index > -1) {
          this.users.splice(index, 1);
        }
      }

      this.selectedUsers = [];
    },
    onUnselectUsers() {
      this.selectedUsers = [];
      for (let user of this.users) {
        delete user.checked;
      }
      console.log("onUnselectUsers", this.selectedUsers);
    },
    onSelectAll() {
      this.selectedUsers = [...this.currentPageUsers];
      console.log("SelectUsers", this.selectedUsers);
    },
    onPageChange(pageNum) {
      this.currentPage = pageNum;
      this.onUnselectUsers();
    },
    // setLoader() {
    //   this.loading = true;
    //   setTimeout(() => {
    //     this.loading = false;
    //   }, 200);
    // },
  },
  watch: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #1667b8 !important; */
  margin: 2rem;
  box-sizing: border-box;
}

.menu-bar {
  display: flex;
  align-items: center;
}
.justify-content-between {
  justify-content: space-between;
}
.justify-content-start {
  justify-content: start;
}
.justify-content-end {
  justify-content: end;
}
.users-list {
  scroll-behavior: smooth;
  overflow-y: scroll;
}
.cursor-pointer {
  cursor: pointer;
}
.fa {
  -webkit-text-stroke: 1px white;
}
button:hover,
button:focus {
  outline: none;
}
</style>
