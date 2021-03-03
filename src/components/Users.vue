<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-row>
        <v-col cols="4" md="4">
          <v-text-field
          dense
          placeholder="Busque por nomes ou e-mails"
          solo
          v-model="searchUsers"
          append-icon="mdi-magnify"
          ></v-text-field>
        </v-col>
        <v-spacer></v-spacer>
        <v-col cols="2" md="4">
          <v-row>
            <v-col cols="3" md="2">
              <div class="pt-2 text-center">
                <strong>Filtros:</strong>
              </div>
            </v-col>
            <v-col cols="9" md="10">
              <v-select v-model="selectFilter" :items="filterOptions" item-text="filter" item-value="filter" placeholder="Selecionar" dense solo></v-select>
            </v-col>
          </v-row>
        </v-col>
        <v-col cols="2" md="2">
          <v-btn @click="userCreateDialog = !userCreateDialog" color="red" block medium dark>
            <v-icon left>mdi-account-plus</v-icon>
            novo aluno
          </v-btn>
        </v-col>
      </v-row>
      </v-col>
      
      <v-col cols="12" md="12" lg="12">
        <v-row>
          <v-col cols="12" v-if="checkItemsList() && searchUsers.length == ''">
            <div class="text-center">
              Não há usuários cadastrados
            </div>
          </v-col>
          <v-col cols="12" v-if="searchUsers.length != '' && checkItemsList()">
            <div class="text-center">
              Não há usuários com os critérios de busca utilizado
            </div>
          </v-col>
          <v-col v-else cols="2" md="4" lg="3" xl="2" v-for="(user, index) in users" :key="index">
            <v-card :color="!user.active ? 'grey lighten-1' : ''" :elevation="!user.active ? 0 : ''">
              <v-card-text>
                <v-row>
                  <v-col cols="3">
                    <v-avatar size="50">
                      <v-img src="https://www.kindpng.com/picc/m/495-4952535_create-digital-profile-icon-blue-user-profile-icon.png"></v-img>
                    </v-avatar>
                  </v-col>
                  <v-col cols="9">
                    <v-row>
                      <v-col cols="12">
                        <strong>{{user.name}}</strong><br>
                        <div class="caption">
                          Última avaliação: {{user.date}}
                        </div>
                      </v-col>
                    </v-row>
                  </v-col>
                </v-row>
              </v-card-text>
              <v-divider></v-divider>
              <v-card-text>
                <v-row>
                  <v-col cols="8">
                    <div class="caption">
                      {{user.email}} <br>
                      {{user.age}} anos {{user.phone}}
                    </div>
                  </v-col>
                  <v-col cols="4">
                    <v-btn small :disabled="!user.active" elevation="2" color="indigo darken-4" icon>
                      <v-icon>edit</v-icon>
                    </v-btn>
                    <v-menu offset-y>
                      <template v-slot:activator="{ on, attrs }">
                        <v-btn v-bind="attrs" v-on="on" :disabled="!user.active" icon>
                          <v-icon>mdi-dots-horizontal</v-icon>
                        </v-btn>
                      </template>
                      <v-list>
                        <v-list-item>
                          <v-list-item-title @click="activeUserStatus(index)">Desativar usuário</v-list-item-title>
                        </v-list-item>
                      </v-list>
                    </v-menu>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>

      <v-dialog width="50%" scrollable v-model="userCreateDialog">
        <v-card>
          <v-toolbar dark class="indigo darken-4">
            <v-toolbar-title>Cadastrar Aluno</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn @click="userCreateDialog = !userCreateDialog" icon>
              <v-icon>close</v-icon>
            </v-btn>
          </v-toolbar>
          <v-card-text>
            <v-text-field v-model="userData.name" placeholder="Nome"></v-text-field>
            <v-text-field v-model="userData.email" type="email" placeholder="E-mail"></v-text-field>
            <v-text-field v-model="userData.age" type="number" placeholder="Idade"></v-text-field>
            <v-text-field v-model="userData.phone" v-mask="'(##) #####-####'" placeholder="Telefone"></v-text-field>
          </v-card-text>
          <v-card-actions>
            <v-btn text color="indigo darken-4" @click="userCreateDialog = !userCreateDialog">
              cancelar
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn @click="postNewUser" dark color="red">
              salvar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'Users',
    data: () => ({
      storage: localStorage,
      userCreateDialog: false,
      filterOptions: [
        {filter: "Todos"}
      ],
      selectFilter: "Todos",
      searchUsers: "",
      userData: {
        name: null,
        email: null,
        date: '03/03/2020',
        age: null,
        phone: null,
        active: true
      },
      users: []
    }),
    methods:{
      checkItemsList(){
        return this.users.length > 0 ? false : true
      },
      postNewUser(){
        this.users.push(this.userData)
        this.storage.setItem('users', JSON.stringify(this.users))
        this.reserUserData()
        this.userCreateDialog = false
      },
      activeUserStatus(index){
        this.users[index].active = false
        this.storage.setItem('users', JSON.stringify(this.users))
      },
      reserUserData(){
        this.userData = {
          name: null,
          email: null,
          date: '03/03/2020',
          age: null,
          phone: null,
          active: true
        }
      }
    },
    watch:{
      async searchUsers () {
        let userTemp = this.users
  
        if (this.searchUsers != '' && this.searchUsers) {
          userTemp = userTemp.filter((user) => {
            return user.name
              .toUpperCase()
              .includes(this.searchUsers.toUpperCase())
          })
          this.users = userTemp
        }else{
          this.users = await JSON.parse(this.storage.getItem('users'))
        }
      }
    },
    async mounted () {
      if (this.storage.users){
        this.users = await JSON.parse(this.storage.getItem('users'))
      }
    }
  }
</script>
