<template>
  <v-app>
    <v-app-bar
      app
      color="red"
      dark
      flat
    >
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn color="red lighten-1">
          <v-row>
            <v-col>
              <v-avatar>
                <v-img src="https://www.kindpng.com/picc/m/495-4952535_create-digital-profile-icon-blue-user-profile-icon.png"></v-img>
              </v-avatar>
            </v-col>
            <v-col>
              <v-row>
                <v-col>
                  <div class="text-left caption text-capitalize">
                    Jonas Doe
                  </div>
                  <div class="text-left caption text-capitalize">
                    Último login: 03/03/2020
                  </div>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-btn>
        <v-btn color="red lighten-1">
          <v-icon>mdi-menu-down</v-icon>
        </v-btn>
      </v-toolbar-items>
    </v-app-bar>

    <v-main>
      <Users/>
    </v-main>
  </v-app>
</template>

<script>
import Users from './components/Users';

export default {
  name: 'App',

  components: {
    Users,
  },

  data: () => ({
    //
  }),
};
</script>
