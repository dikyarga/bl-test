<template>
  <v-app light>
    <v-navigation-drawer
      persistent
      :mini-variant="miniVariant"
      :clipped="clipped"
      v-model="drawer"
    >
      <v-list>
        <v-list-tile
          v-for="(item, i) in items"
          :key="i"
          value="true"
        >
          <v-list-tile-action>
            <v-icon light v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar fixed>
      <v-toolbar-side-icon @click.native.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-btn
        icon
        @click.native.stop="miniVariant = !miniVariant"
      >
        <v-icon v-html="miniVariant ? 'chevron_right' : 'chevron_left'"></v-icon>
      </v-btn>
      <v-btn
        icon
        @click.native.stop="clipped = !clipped"
      >
        <v-icon>web</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.native.stop="fixed = !fixed"
      >
        <v-icon>remove</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        icon
        @click.native.stop="rightDrawer = !rightDrawer"
      >
        <v-icon>menu</v-icon>
      </v-btn>
    </v-toolbar>
    <main>
      <v-container fluid>
        <v-slide-y-transition mode="out-in">
          <template>
            <v-layout row>
              <v-flex xs12 sm6 offset-sm3>
                <v-card>
                  <v-toolbar class="white--text pink" dark>

                    <v-toolbar-title>  <img class="text-xs-center" src="./assets/images/logo.png" alt="Logo Bukalapak"height="20px" /></v-toolbar-title>
                    <v-spacer></v-spacer>
                    <v-btn icon>
                      <v-icon>refresh</v-icon>
                    </v-btn>

                  </v-toolbar>
                  <div class="text-xs-right" @click="backToParrent()">

                    <span class="text-xs-left">Pilih Kategori : {{ parrent }}</span>
                    <v-btn dark small >
                      <v-icon dark>close</v-icon>
                    </v-btn>
                  </div>
                  <v-list two-line>
                    <v-list-tile avatar ripple v-for="(menu, index) in existingMenu" v-bind:key="menu.name">
                      <v-list-tile-content @click="goToSubMenu(menu.sub, menu.name)">

                        <v-list-tile-title >{{ menu.name }} - <span class="h6">{{ menu.count }}</span></v-list-tile-title>

                      </v-list-tile-content>
                      <v-list-tile-action>
                        <v-list-tile-action-text>{{ menu.action }}</v-list-tile-action-text>
                        <span v-if="menu.sub != null">+</span>
                        <span v-if="menu.sub == null">-</span>
                      </v-list-tile-action>
                      <v-divider v-if="index + 1 < existingMenu.length"></v-divider>
                    </v-list-tile>
                  </v-list>
                </v-card>
              </v-flex>
            </v-layout>
          </template>
          <v-layout column align-center>

            <img src="/public/v.png" alt="Vuetify.js" class="mb-5" />
            <blockquote>
              &#8220;First, solve the problem. Then, write the code.&#8221;
              <footer>
                <small>
                  <em>&mdash;Diky Arga</em>
                </small>
              </footer>
            </blockquote>
          </v-layout>
        </v-slide-y-transition>
      </v-container>
    </main>
    <v-navigation-drawer
      temporary
      :right="right"
      v-model="rightDrawer"
    >
      <v-list>
        <v-list-tile @click.native="right = !right">
          <v-list-tile-action>
            <v-icon light>compare_arrows</v-icon>
          </v-list-tile-action>
          <v-list-tile-title>Switch drawer (click me)</v-list-tile-title>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-footer :fixed="fixed">
      <span>&copy; 2017</span>
    </v-footer>
  </v-app>
</template>

<script>
import axios from 'axios'
let apiHost = 'http://127.0.0.1:8887/'
  export default {
    created () {
      console.log('created');
      this.getRootMenu()
    },
    data () {
      return {
        clipped: false,
        drawer: true,
        fixed: false,
        items: [
          { icon: 'bubble_chart', title: 'Inspire' }
        ],
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Test Bukalapak',
        existingMenu: [],
        parrent: ''
      }

    },
    methods: {
      getRootMenu() {
        axios({
          method: 'get',
          url: apiHost + 'main.json'
        }).then((rootMenu) => {
          // console.log('dapet rootmenu : ', rootMenu.data.categories);
          this.existingMenu = rootMenu.data.categories
          this.parrent = ''
        })
      },
      goToSubMenu(sub, parrent) {
        // console.log('sub menu jalan');
        axios({
          method: 'get',
          url: apiHost + sub
        }).then((rootMenu) => {
          // console.log('dapet sub menu : ', rootMenu.data.categories);
          this.existingMenu = rootMenu.data.categories
          this.parrent = parrent
        })
      },
      backToParrent() {
        // console.log('backToParrent running', this.parrent);
        this.getRootMenu()
      },
      refreshMenu() {
        console.log('refreshMenu runing');
      }
    }
  }
</script>

<style lang="stylus">
  @import './stylus/main'
</style>
