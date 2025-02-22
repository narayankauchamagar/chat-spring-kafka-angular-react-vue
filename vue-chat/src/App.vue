<template>
  <div class="application-component">
    <div class="application-container">
      <div v-if="!user" class="userForm">
        <h1 class="marginBottom15">
          Welcome to the chat
        </h1>
        <div class="marginBottom15">
          What is your name ?
        </div>
        <form @submit="activeUser">
          <input v-model="userName" class="marginBottom15" type="text">
          <button v-if="userName !== ''" type="submit">
            Let's go!
          </button>
        </form>
      </div>
      <messages v-else :user="user"></messages>
    </div>
  </div>
</template>

<script lang="ts">
import {User} from './user.interface';
import axios, {AxiosResponse} from 'axios';
import Messages from './components/Messages.vue';
import {Options, Vue} from "vue-class-component";

const server = 'http://localhost:8000';

@Options({
  props: {},
  components: {
    messages: Messages,
  },
})
export default class App extends Vue {

  public userName = '';
  public user: User | null = null;

  public activeUser(e: Event) {
    e.preventDefault();
    if (this.userName) {
      return axios.post(`${server}/users`, {name: this.userName}).then((response: AxiosResponse) => {
        this.user = response ? response.data : null;
        this.userName = '';
      });
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Roboto');
@import url('https://fonts.googleapis.com/icon?family=Material+Icons');

html, body, #app {
  height: 100%;
  margin: 0;
}

body {
  margin: 0;
  font-family: Roboto, "Helvetica Neue", sans-serif;
  background-color: #D9DBD6;
}

.application-component {
  width: 100%;
  height: 100%;
  min-width: 100%;
  min-height: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  align-content: center;

  &::before {
    content: '';
    width: 100%;
    height: 200px;
    position: absolute;
    background-color: #3798D4;
    top: 0;
    z-index: -1;
  }

  .application-container {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    align-content: center;
    border-radius: 4px;
    box-shadow: 0 1px 1px 0 rgba(0, 0, 0, 0.06), 0 2px 5px 0 rgba(0, 0, 0, 0.2);
    background-color: #ffffff;
    font-size: 24px;

    @media (min-width: 1919px) {
      width: 60%;
      height: 90%;
    }

    .userForm {
      padding: 24px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      align-content: center;

      .marginBottom15 {
        margin-bottom: 15px;
      }


      h1 {
        @media (max-width: 1200px) {
          font-size: 30px;
        }
      }

      form {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        align-content: center;

        input {
          font-size: 22px;
          width: 300px;
          border: 2px solid #aaa;
          border-radius: 4px;
          margin: 8px 0;
          outline: none;
          padding: 15px;
          box-sizing: border-box;
          transition: .3s;
        }

        input[type=text]:focus {
          border-color: dodgerBlue;
          box-shadow: 0 0 8px 0 dodgerBlue;
        }

        button {
          vertical-align: middle;
          padding: 10px 20px;
          font-size: 22px;
          color: #ffffff;
          text-decoration: none;
          background-color: #3798D4;
          text-align: center;
          letter-spacing: .5px;
          height: 50px;
          line-height: 36px;
          text-transform: uppercase;
          border: none;
          border-radius: 2px;
          outline: 0;
          position: relative;
          cursor: pointer;
          display: inline-block;
          overflow: hidden;
        }

      }
    }
  }
}
</style>
