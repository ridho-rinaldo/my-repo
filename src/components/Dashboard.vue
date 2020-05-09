<template>
  <div class="page-container">
    <md-app style="max-height: 100vh">
      <md-app-toolbar class="md-primary" style="flex-direction: row-reverse">
        <md-menu>
          <md-button md-menu-trigger>
            <md-icon>person</md-icon>
          </md-button>

          <md-menu-content>
            <md-menu-item class="btn-drawer" @click="logout()">Logout</md-menu-item>
          </md-menu-content>
        </md-menu>
      </md-app-toolbar>

      <md-app-drawer md-permanent="full">
        <md-toolbar class="md-transparent" md-elevation="0">Navigation</md-toolbar>

        <md-list>
          <md-list-item class="btn-drawer" @click="type = 'home'">
            <md-icon>home</md-icon>
            <span class="md-list-item-text">Home</span>
          </md-list-item>

          <md-list-item class="btn-drawer" @click="type = 'search user'">
            <md-icon>person</md-icon>
            <span class="md-list-item-text">Search User</span>
          </md-list-item>

          <md-list-item class="btn-drawer" @click="type = 'all user'">
            <md-icon>people</md-icon>
            <span class="md-list-item-text">All User</span>
          </md-list-item>

          <md-list-item class="btn-drawer" @click="type = 'action user'">
            <md-icon>edit</md-icon>
            <span class="md-list-item-text">Action User</span>
          </md-list-item>

          <md-list-item class="btn-drawer" @click="type = 'add information'">
            <md-icon>info</md-icon>
            <span class="md-list-item-text">Add Information</span>
          </md-list-item>
        </md-list>
      </md-app-drawer>

      <md-app-content v-if="type === 'home'">
        <p class="text-user" style="font-weight: bold; color: blueviolet">Welcome back Administrator</p>
        <p
          class="text-user"
        >Thanks for signing up, we have some information for you. Let's check it out.</p>
        <div class="flex">
          <div style="height: 100%; width: 50%; background-color: #d1d1d1; padding: 0% 2%">
            <h1 style="text-align: left">Information</h1>
            <div
              v-for="(value, name, index) in store"
              :key="index"
              class="announcement"
              @click="announce(value)"
            >
              <p class="announcement-text ann-title">{{value.title}}</p>
              <p class="announcement-text ann-date">{{value.date}}</p>
            </div>
          </div>
          <div
            style="height: auto; width: 50%; background-color: #bcbcbc; padding: 0% 2%"
            v-if="detailAnnouncement !== {}"
          >
            <div>
              <h1 style="text-align: left">Detail Info</h1>
              <p
                class="announcement-text ann-title"
                style="font-weight: bold; text-align: center"
              >{{detailAnnouncement.title}}</p>
              <p
                class="announcement-text ann-title"
                style="margin: 2% 0px"
              >{{detailAnnouncement.detail}}</p>
            </div>
          </div>
        </div>
      </md-app-content>

      <md-app-content v-if="type === 'search user'">
        <h1 class="md-title">Search User</h1>
        <div class="flex" style="display: inline-flex">
          <md-field class="btn-searchUser">
            <md-icon>person</md-icon>
            <label>Id User</label>
            <md-input v-model="searchUser.id" type="text"></md-input>
          </md-field>

          <md-button class="md-raised md-primary" v-on:click="btnSearchUser()">Search</md-button>
        </div>

        <div class="detail-user" v-if="resp !== {}">
          <div style="width: 50%">
            <div v-for="(value, name, index) in resp.data" :key="index">
              <div style="display: flex">
                <p class="text-user text-left">{{name}}</p>
                <p class="text-user text-right">{{`: ${value}`}}</p>
              </div>
            </div>
          </div>

          <div style="width: 50%">
            <div v-for="(value, name, index) in resp.ad" :key="index">
              <div style="display: flex">
                <p class="text-user text-left">{{name}}</p>
                <p class="text-user text-right">{{`: ${value}`}}</p>
              </div>
            </div>
          </div>
        </div>
      </md-app-content>

      <md-app-content v-if="type === 'all user'">
        <div v-if="listUser !== {}">
          <md-table v-model="listUser.data" md-card>
            <md-table-toolbar>
              <h1 class="md-title">All Users</h1>
            </md-table-toolbar>

            <md-table-row slot="md-table-row" slot-scope="{ item }">
              <md-table-cell style="text-align: left" md-label="ID" md-sort-by="id">{{ item.id }}</md-table-cell>
              <md-table-cell
                style="text-align: left"
                md-label="Email"
                md-sort-by="email"
              >{{ item.email }}</md-table-cell>
              <md-table-cell
                style="text-align: left"
                md-label="First Name"
                md-sort-by="firstname"
              >{{ item.first_name }}</md-table-cell>
              <md-table-cell
                style="text-align: left"
                md-label="Last Name"
                md-sort-by="lastname"
              >{{ item.last_name }}</md-table-cell>
              <md-table-cell
                style="text-align: left"
                md-label="Avatar"
                md-sort-by="avatar"
              >{{ item.avatar }}</md-table-cell>
            </md-table-row>
          </md-table>

          <div class="flex" style="margin-top: 1%">
            <md-button class="md-icon-button md-primary" v-on:click="allUser(pageListUser - 1)">
              <md-icon>navigate_before</md-icon>
            </md-button>
            <p class="text-user text-center">{{pageListUser}}</p>
            <md-button class="md-icon-button md-primary" v-on:click="allUser(pageListUser + 1)">
              <md-icon>navigate_next</md-icon>
            </md-button>

            <p class="text-user text-center">{{`Total Page: ${listUser.total_pages}`}}</p>
          </div>
        </div>
      </md-app-content>

      <md-app-content v-if="type === 'action user'">
        <h1 class="md-title">Action User</h1>

        <md-field class="create-user">
          <md-icon>person</md-icon>
          <label>Name</label>
          <md-input v-model="actionUser.name" type="text"></md-input>
        </md-field>

        <md-field class="create-user">
          <md-icon>work</md-icon>
          <label>Job</label>
          <md-input v-model="actionUser.job" type="text"></md-input>
        </md-field>

        <div class="button-group">
          <md-button
            class="md-raised md-primary btn-create-user"
            v-on:click="btnCreateUser()"
          >Create</md-button>
          <md-button
            class="md-raised md-primary btn-create-user"
            v-on:click="btnUpdateUser()"
          >Update</md-button>
          <md-button
            class="md-raised md-primary btn-create-user"
            v-on:click="btnDeleteUser()"
          >Delete</md-button>
        </div>
      </md-app-content>

      <md-app-content v-if="type === 'add information'">
        <h1 class="md-title">Add Information</h1>

        <md-field class="create-user">
          <md-icon>title</md-icon>
          <label>Title</label>
          <md-input v-model="addInfo.title" type="text"></md-input>
        </md-field>

        <md-datepicker class="create-user" v-model="addInfo.date" />

        <md-field class="create-user">
          <label>Detail</label>
          <md-textarea v-model="addInfo.detail"></md-textarea>
        </md-field>

        <md-field class="create-user">
          <md-select v-model="addInfo.type" name="country" id="country" placeholder="Type">
            <md-option value="Information">Information</md-option>
            <md-option value="Workshop">Workshop</md-option>
          </md-select>
        </md-field>

        <md-button class="md-raised md-primary btn-create-user" v-on:click="btnAddInfo()">Submit</md-button>
      </md-app-content>
    </md-app>

    <md-snackbar
      :md-position="'center'"
      :md-duration="5000"
      :md-active.sync="showSnackbar"
      md-persistent
    >
      <span>{{ errorResp.text }}</span>
      <md-button class="md-raised md-primary" @click="showSnackbar = false">Done</md-button>
    </md-snackbar>

    <modal name="action-user-summary" height="auto">
      <p class="text-summary">{{`${modal} Successfull.`}}</p>
      <div style="padding: 3% 8% 0% 8%" v-for="(value, name, index) in actionUserResp" :key="index">
        <div style="display: flex">
          <p class="text-user text-left">{{name}}</p>
          <p class="text-user text-right">{{`: ${value}`}}</p>
        </div>
      </div>

      <md-button class="md-raised md-primary" v-on:click="closeModal(modal)">Done</md-button>
    </modal>

    <modal
      name="loading"
      width="80px"
      height="auto"
      style="background: rgba(0, 0, 0, 0.4); padding: 10px"
    >
      <md-progress-spinner md-mode="indeterminate"></md-progress-spinner>
    </modal>
  </div>
</template>

<script>
import axios from "axios";
import store from "../store/index";
import router from "../router/index";

export default {
  name: "Normal",
  data() {
    return {
      store: store.announcement,
      detailAnnouncement: {},
      addInfo: {
        title: "",
        date: "",
        detail: "",
        type: ""
      },
      type: "home",
      pageListUser: 1,
      showSnackbar: false,
      searchUser: {
        id: ""
      },
      resp: {},
      listUser: {},
      actionUser: {
        name: "",
        job: ""
      },
      actionUserResp: {},
      modal: "",
      errorResp: {}
    };
  },
  mounted() {
    this.allUser(this.pageListUser);
  },
  methods: {
    btnSearchUser() {
      this.$modal.show("loading");
      let url = "https://reqres.in/api/users/" + this.searchUser.id;
      axios({
        method: "GET",
        url: url,
        headers: {
          "content-type": "application/json"
        }
      }).then(
        result => {
          this.$modal.hide("loading");
          this.resp = result.data;
        },
        error => {
          this.$modal.hide("loading");
          this.errorResp = {
            error: error.response.status,
            text: `Request failed with status code ${error.response.status}`
          };
          this.showSnackbar = true;
        }
      );
    },
    allUser(page) {
      this.$modal.show("loading");
      if (page < 1 || page > this.listUser.total_pages) {
        return false;
      }
      let url = "https://reqres.in/api/users?page=" + page;
      axios({
        method: "GET",
        url: url,
        headers: {
          "content-type": "application/json"
        }
      }).then(
        result => {
          this.$modal.hide("loading");
          this.listUser = result.data;
          this.pageListUser = result.data.page;
        },
        error => {
          this.$modal.hide("loading");
          this.errorResp = {
            error: error.response.status,
            text: `Request failed with status code ${error.response.status}`
          };
          this.showSnackbar = true;
        }
      );
    },
    btnCreateUser() {
      this.$modal.show("loading");
      axios({
        method: "POST",
        url: "https://reqres.in/api/user",
        data: this.actionUser,
        headers: {
          "content-type": "application/json"
        }
      })
        .then(result => {
          this.$modal.hide("loading");
          this.actionUserResp = result.data;
          this.modal = "Create User";
          this.$modal.show("action-user-summary");
        })
        .catch(err => {
          this.$modal.hide("loading");
          this.errorResp = {
            error: error.response.status,
            text: err
          };
          this.showSnackbar = true;
        });
    },
    btnUpdateUser() {
      this.$modal.show("loading");
      axios({
        method: "PUT",
        url: "https://reqres.in/api/user/2",
        data: this.actionUser,
        headers: {
          "content-type": "application/json"
        }
      })
        .then(result => {
          this.$modal.hide("loading");
          this.actionUserResp = result.data;
          this.modal = "Update User";
          this.$modal.show("action-user-summary");
        })
        .catch(err => {
          this.$modal.hide("loading");
          this.errorResp = {
            error: error.response.status,
            text: err
          };
          this.showSnackbar = true;
        });
    },
    btnDeleteUser() {
      this.$modal.show("loading");
      axios({
        method: "DELETE",
        url: "https://reqres.in/api/user/2",
        data: this.actionUser,
        headers: {
          "content-type": "application/json"
        }
      })
        .then(result => {
          this.$modal.hide("loading");
          this.actionUserResp = result.data;
          this.modal = "Delete User";
          this.$modal.show("action-user-summary");
        })
        .catch(err => {
          this.$modal.hide("loading");
          this.errorResp = {
            error: error.response.status,
            text: err
          };
          this.showSnackbar = true;
        });
    },
    closeModal(param) {
      this.$modal.hide("action-user-summary");
      if (param === "Add Information") {
        this.addInfo = {
          title: "",
          date: "",
          detail: "",
          type: ""
        };
      } else {
        this.actionUser = {
          name: "",
          job: ""
        };
        this.actionUserResp = {};
      }
    },
    announce(param) {
      this.detailAnnouncement = param;
    },
    btnAddInfo() {
      if (
        this.addInfo.title === "" ||
        this.addInfo.date === "" ||
        this.addInfo.detail === "" ||
        this.addInfo.type === ""
      ) {
        this.errorResp = {
          error: 500,
          text: "Please fill the info."
        };
        this.showSnackbar = true;
        return false;
      }
      store.announcement = [...store.announcement, this.addInfo];
      this.store = store.announcement;
      this.modal = "Add Information";
      this.$modal.show("action-user-summary");
    },
    logout() {
      router.push({ name: "Login" });
    }
  }
};
</script>

<style lang="scss" scoped>
.md-app {
  max-height: 400px;
  border: 1px solid rgba(#000, 0.12);
}

// Demo purposes only
.md-drawer {
  width: 230px;
  max-width: calc(100vw - 125px);
}

.btn-drawer:hover {
  background-color: #448aff;
  cursor: pointer;
}

.btn-searchUser {
  margin: 0px;
  width: 70%;
}
.detail-user {
  display: flex;
  margin-top: 2%;
}
.text-user {
  text-align: left;
  font-size: 16px;
  font-family: sans-serif;
  margin: 1% 0px;
  text-transform: capitalize;
}
.text-left {
  width: 30%;
}
.text-right {
  width: 65%;
}
.create-user {
  margin: 0px;
  width: 50%;
  display: inline-flex;
}
.btn-create-user {
  margin-top: 1%;
  width: 50%;
  display: inline-flex;
}
.text-summary {
  font-family: sans-serif;
  font-size: 30px;
  font-weight: 600;
  margin: 5% 0px 0px 0px;
}
.button-group {
  width: 50%;
  display: inline-flex;
}
.announcement {
  margin-bottom: 1%;
}
.announcement:hover {
  background-color: #448aff;
  cursor: pointer;
}
.announcement-text {
  text-align: left;
  text-transform: capitalize;
  font-family: sans-serif;
  margin: 0px;
}
.ann-title {
  font-size: 16px;
}
.ann-date {
  font-size: 13px;
  font-style: italic;
}

.flex {
  display: flex;
}
.text-center {
  text-align: center;
}
</style>