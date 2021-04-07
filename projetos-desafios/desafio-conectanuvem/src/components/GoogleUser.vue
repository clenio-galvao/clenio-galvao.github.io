<template>
  <span>
    <span v-if="value">
      <span class="g-signout"
        @click.prevent.stop="signout"
        @keypress.enter.prevent.stop="signout">
        <span class="icon"></span>
        <span class="text">desautorizar</span>
      </span>
    </span>
    <span class="user">
      <google-signin-btn v-if="!value" @click="signin"></google-signin-btn>
    </span>
  </span>
</template>

<script>
export default {
  name: 'GoogleUser',
  components: {},
  props: [ 'value' ],
  methods: {
    signin () {
      this.$gapi.signIn()
        .then(user => {
          this.$emit('input', user)
        })
    },
    signout () {
      this.$gapi.signOut()
        .then(() => {
          this.$emit('input', undefined)
        })
    }
  }
}
</script>

<style scoped>
.user {
  display: table;
  margin: 32px auto;
}
.user .card {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.25);
  border-radius: 4px;
  padding: 32px;
  background-color: white;
}
.user .card .infos {
  text-align: right;
  padding-left: 32px;
}
.user .card .infos .name {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 6px;
}
.user .card .infos .email {
  font-size: 16px;
  font-weight: bold;
  font-style: italic;
  margin-bottom: 16px;
}
@import url("https://fonts.googleapis.com/css?family=Roboto");
.g-signout {
  margin-left: 10px;
  display: inline-block;
  background: #fff;
  color: #555;
  border-radius: 5px;
  box-shadow: 1px 1px 4px rgba(0,0,0,0.25);
  white-space: nowrap;
}
.g-signout:hover {
  cursor: pointer;
  box-shadow: 2px 2px 8px rgba(0,0,0,0.5);
  color: #000;
}
.g-signout span.icon {
  background-image: url("https://developers.google.com/identity/images/g-logo.png");
  background-size: 26px 26px;
  background-repeat: no-repeat;
  background-position: center center;
  display: inline-block;
  vertical-align: middle;
  padding-left: 8px;
  width: 42px;
  height: 42px;
}
.g-signout span.text {
  display: inline-block;
  vertical-align: middle;
  padding-left: 8px;
  padding-right: 8px;
  font-size: 14px;
  font-weight: bold;
  font-family: 'Roboto', sans-serif;
}
</style>
