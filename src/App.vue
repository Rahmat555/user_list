<template>
  <div id="app" class="container mt-2">
    <h1>User List</h1>
    <div class="container d-inline-flex">
      <input type="text" class="form-control rounded m-2" placeholder="User name" v-model="nameis" v-on:keyup.enter="PostApi()">
      <input type="text" class="form-control rounded m-2" placeholder="User description" v-model="descriptionis" v-on:keyup.enter="PostApi()">
      <button type="button" class="btn btn-primary rounded m-2" @click="PostApi()">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-search" viewBox="0 0 16 16">
          <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/>
        </svg>
      </button>
    </div>
    <table class="container">
      <tr>
        <td>ID</td>
        <td>NAME</td>
        <td>Description</td>
        <td>Edit</td>
        <td>Delete</td>
      </tr>
      <tr v-for="todo of todos" v-bind:key="todo.id">
        <td>{{ todo.id }}</td>
        <td><input class="rounded form-control" type="text" v-model="todo.name" />{{ todo.name }}</td>
        <td><input class="rounded form-control" type="text" v-model="todo.description" />{{ todo.description }}</td>
        <td><button class="btn btn-outline-warning rounded button_escape" v-on:click="PutApi(todo.id, todo.name, todo.description)">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil-square" viewBox="0 0 16 16">
            <path d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"/>
            <path fill-rule="evenodd" d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"/>
          </svg>
        </button></td>
        <td><button class="btn btn-outline-danger rounded button_escape" v-on:click="DeleteApi(todo.id)">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
            <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5Zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5Zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6Z"/>
            <path d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1ZM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118ZM2.5 3h11V2h-11v1Z"/>
          </svg>
        </button></td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
const baseUrl = 'http://localhost:3000/todos/'

export default {
    name: 'App',
    data() {
        return {
            todos: [],
            nameis:'',
            descriptionis:''
        }
    },
    methods: {
        async GetApi(){
          await axios
          .get(baseUrl)
          .then((resp)=>{this.todos = resp.data;})
          .catch((err)=>{console.log(err);});
        },
        async PostApi(){
          await axios
          .post(baseUrl, {name:this.nameis, description:this.descriptionis})
          .then((resp)=>{console.log(resp);this.nameis='';this.descriptionis=''; this.GetApi();})
          .catch((err)=>{console.log(err);});
        },
        async DeleteApi(id){
          await axios
          .delete(baseUrl + id)
          .then((resp)=>{console.log(resp);this.GetApi();})
          .catch((err)=>{console.log(err);});
        },
        async PutApi(id, name, description){
          await axios
          .put(baseUrl+id, {name:name, description:description})
          .then((resp)=>{console.log(resp); this.GetApi();})
          .catch((err)=>{console.log(err);});
        }
    },
    mounted(){
      this.GetApi();
    }
}
</script>

<style>
td{
  padding: 5px;
}
.button_escape{
  margin-bottom: 24px;
}
</style>
