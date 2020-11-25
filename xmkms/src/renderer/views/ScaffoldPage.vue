<template>
  <v-app dark>
    <v-navigation-drawer app :mini-variant="isMiniVariant">
      <v-container display-1 font-weight-black text-uppercase v-show="!isMiniVariant">
        <v-layout>
          <v-flex layout justify-center align-center>
            <div>{{ title }}</div>
          </v-flex>
        </v-layout>
    </v-container>

      <v-list>
        <template v-for="(item, i) in items">
          <v-list-tile :to="item.to" :key="i"
            active-class="orange"
          >
            <v-list-tile-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>
                {{ item.label }}
              </v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </template>
        <v-list-tile
          class="nav-item-logout"
          @click="logout"
          active-class="orange"
        >
          <v-list-tile-action>
            <v-icon>exit_to_app</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title> Logout </v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar app style="-webkit-user-select: none;-webkit-app-region: drag">
      <v-toolbar-side-icon @click="isMiniVariant = !isMiniVariant"/>
      <v-toolbar-title>
        欢迎，XMK
      </v-toolbar-title>
      <v-spacer/>
      <v-btn icon @click.native="winControl('minimize')">
        <v-icon>remove</v-icon>
      </v-btn>
      <v-btn icon @click.native="winControl('maximize')" v-show="isMaximized">
        <v-icon>filter_none</v-icon>
      </v-btn>
      <v-btn icon @click.native="winControl('maximize')" v-show="!isMaximized">
        <v-icon>crop_square</v-icon>
      </v-btn>
      <v-btn icon @click.native="winControl('close')">
        <v-icon color="red">close</v-icon>
      </v-btn>
    </v-toolbar>
    <v-content>
      <v-container>
        <router-view></router-view>
      </v-container>
    </v-content>
    <v-footer>
    </v-footer>
  </v-app>
</template>

<script>
const electron = require("electron");
const remote = electron.remote;

import { mapActions } from "vuex";
import { ipcRenderer } from "electron";

export default {
  data: () => ({
    title: "XMKMS",
    items: [{ icon: "home", label: "首页", to: "/home", isShowLabel: true }],
    isMiniVariant: false,
    isMaximized: false,
  }),
  methods: {
    winControl(action) {
      const browserWindow = remote.getCurrentWindow();
      switch (action) {
        case "minimize":
          browserWindow.minimize();
          break;
        case "maximize":
          if (browserWindow.isMaximized()) {
            browserWindow.unmaximize();
          } else {
            if (this.isMaximized) {
              browserWindow.unmaximize();
            } else {
              browserWindow.maximize();
            }
          }
          this.isMaximized = !this.isMaximized;

          break;
        case "close":
          browserWindow.close();
          break;
        default:
          break;
      }
    },
    logout() {
      ipcRenderer.send("openLoginWindow");
    },
  },
};
</script>

<style scoped lang="scss">
.primary--text {
  color: orange !important;
  caret-color: orange !important;
}

.nav-item-logout {
  position: fixed;
  bottom: 0;
  width: 100%;
}
</style>
