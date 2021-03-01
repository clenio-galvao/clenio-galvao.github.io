<template>
    <div>

        <v-banner  class="vBanner" single-line v-if="!loggedIn">
            <v-icon
                slot="icon"
                color="warning"
                size="36"
            >
                mdi-wifi-strength-alert-outline
            </v-icon>
                Por favor faça o login!
        </v-banner>
        <div v-else >
            <v-banner>
                <v-avatar slot="icon" color="deep-purple accent-4" size="40">
                    <v-icon icon="mdi-lock" color="white">mdi-lock</v-icon>
                </v-avatar>
                Obrigado {{userProfile.name}} por acessar nosso app!<span v-if="user">Você autorizou o acesso às informações dos contados!</span>
                <google-user v-model="user" v-if="user">></google-user>
            </v-banner>
            <v-banner v-if="!user">
                <p>Precisamos da sua permissão para acessar seus contatos do google. Ao clicar no botão de autorização aparecerá um erro com o título "O Google não verificou este app" clique em avançado e clique em "Acessar project-355107773529 (não seguro)" depois siga o passo a passo. </p> 
            </v-banner>
        </div>
        <div v-if="loggedIn">
            <google-user v-model="user" v-if="!user">></google-user>
            <Table :connections="connections" :loggedIn="loggedIn"> </Table>
        </div>
  </div>
</template>


<script>
  import { mapGetters } from 'vuex';
  import GoogleUser from '@/components/GoogleUser'
  import Table from '../components/Table.vue';

  export default {
    name: 'home',
    computed: {
        ...mapGetters('user', {
        loggedIn: 'loggedIn',
        userProfile: 'userProfile'
      }),
    },
    components: { GoogleUser, Table },
    data: () => ({
        user: undefined,
    }),
}
</script>

<style >
.v-banner__text {
    height: 50px;
}
.v-banner__text span{
    margin-left: 6px;
}
</style>