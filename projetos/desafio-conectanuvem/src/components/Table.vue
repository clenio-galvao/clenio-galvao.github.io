<template>
  <div class="test">
    <v-btn @click="searchPeopleApi()">Crie uma lista com os emails dos seus contatos</v-btn>
    <v-card class="card">
      <v-data-table
        :headers="headers"
        :items="items"
        item-key="email"
        sort-by="email"
        group-by="domain"
        class="elevation-1"
        show-group-by
      >
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
// console.log(this.connections);
export default {
  name: 'Table',
  props: {
    connections: Array,
  },
  methods: {
    searchPeopleApi() {
      this.$gapi._libraryInit('client',
      { discoveryDocs: [ 'https://people.googleapis.com/$discovery/rest' ] })
      .then(client => {
          return client.people.people.connections.list({
          'resourceName': 'people/me',
          'personFields': 'names,emailAddresses',
          }).then(response => {
          this.items = response.result.connections
              .filter((connection) => connection.emailAddresses)
                .map((contact) => contact.emailAddresses[0].value)
                  .map(email => ({ domain: email.split('@', 2)[1], email }));
          })
      })
    },
  },
  watch: {
    items(novo) {
      console.log(novo)
    }
  },
  data () {
    return {
      headers: [
        {
          text: 'Domínio',
          align: 'start',
          filterable: true,
          value: 'domain',
        },
        { text: 'Emails', value: 'email' },
      ],
      items: []
    }
  }
}
</script>

<style>
.v-card {
  width: 70%;
  margin-top: 30px;
  margin-left: auto;
  margin-right: auto;
}

.test {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 30px;
}
</style>
