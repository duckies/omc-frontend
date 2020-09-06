<template>
  <v-app dark>
    <v-navigation-drawer v-model="drawer" fixed temporary app>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar flat app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

      <v-toolbar-title v-text="title" />

      <v-spacer />

      <template v-if="$auth.loggedIn">
        <v-menu offset-y>
          <template #activator="{ on, attrs }">
            <v-btn text v-bind="attrs" v-on="on">
              <v-icon class="mr-2">mdi-account-circle</v-icon>
              {{ $auth.user.name }}
            </v-btn>
          </template>

          <v-list>
            <v-list-item @click="$auth.logout()">
              <v-list-item-title>Logout</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </template>

      <template v-else>
        <v-btn v-if="!$auth.loggedIn" text nuxt to="/login">Login</v-btn>
      </template>

      <v-btn icon @click="isDark = !isDark">
        <v-icon>mdi-theme-light-dark</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'Default',
  data() {
    return {
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
        {
          icon: 'mdi-calendar',
          title: 'Event Calendar',
          to: '/events',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Orlando Math Circle',
    }
  },
  computed: {
    isDark: {
      get() {
        return this.$vuetify.theme.isDark
      },
      set() {
        this.$vuetify.theme.isDark = !this.$vuetify.theme.isDark
      },
    },
  },
}
</script>
