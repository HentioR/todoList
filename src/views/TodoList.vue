<template>

<div>
  <h1 class="m-3 text-white text-2xl font-semibold uppercase md:text-3xl">TODO LIST</h1>


<label class="content-center mt-4">
  <span class="text-gray-700">Liste de tâches</span>
  <br>

  <sidebar :lists="lists" v-model="selected" v-on:change="selectList($event)"></sidebar>
  <!--- <sidebar-new-list :lists="lists" v-model="newListName" v-on:submit.prevent="addNewList()"></sidebar-new-list> --->
</label>

<p class="text-blue-300">
  <button class="m-3 text-blue-300" @click="checkAllTasks">Tout cocher</button>
</p>


<table class="rounded-t-lg m-5 w-2/5 mx-auto bg-gray-800 text-gray-200">
  <tr class="border-b border-gray-300">
    <th class="px-4 py-3"></th>
    <th class="px-4 py-3">Tâches</th>
    <th class="px-4 py-3">Status</th>
    <th class="px-4 py-3">Editer</th>
    <th class="px-4 py-3">Supprimer</th>
  </tr>
 
  <tr class="bg-gray-700 border-b border-gray-600" v-for="task in selected.tasks" v-bind:key="task.id" :class="{completed: task.completed}">
    <td class="px-4 py-3"><input type="checkbox" v-model="task.completed" v-on:change="updateTask(task)"></td>
    <td class="px-4 py-3">{{task.name}}</td>
    <td class="px-4 py-3" v-if="task.completed == true"><span class="bg-green-200 text-green-600 py-1 px-3 rounded-full">Fait</span></td>
    <td class="px-4 py-3" v-if="task.completed == false"><span class="bg-red-200 text-red-800 py-1 px-3 rounded-full">A faire</span></td>
    <td class="px-4 py-3"><button v-on:click="edit = !edit">✏️</button></td>
    <td class="px-4 py-3"><button type="destroy" @click="deleteTask(task)">❌</button></td>
  </tr>    
</table>

<div>
    <form class="m-3 h-10 content-center" v-if="selected" v-on:submit.prevent="addNewTask()">
      <input class="rounded-l-lg p-4 border-t mr-0 border-b border-l text-gray-700 border-gray-200 bg-white" v-model="newTask"  id="new-task" placeholder="Ex. faire les courses" />
    <button class="px-8 rounded-r-lg bg-gray-700  text-white font-bold p-4 uppercase border-gray-700 border-t border-b border-r">Ajouter</button>
  </form>
</div>

<p>
  <button class="m-5" @click="clearLocalStorage">Supprimer le stockage local</button>
</p>
  
</div> 
</template>

<script>
import Sidebar from '../components/Sidebar.vue';
//import SidebarNewList from '../components/SidebarNewList.vue';

export default{
      name: 'App',
      components: {
            Sidebar
      },
      data() {
        return {
          lists:[
            {
              id: 1,
              name: 'Une première liste',
              tasks: [
              {
                id: 1,
                name: 'Faire la vaiselle',
                completed: false
              },
              {
                id: 2,
                name: 'Se promener',
                completed: false
              },
              {
                id: 3,
                name: 'Travailler',
                completed: true
              }],
              complete: false,
              checkAll: false
            },
            {
              id: 2,
              name: 'Une seconde liste',
              tasks: [
              {
                id: 1,
                name: 'Regarder une série',
                completed: false
              },
              {
                id: 2,
                name: 'Aller a la piscine',
                completed: true
              },
              {
                id: 3,
                name: 'Jouer au bowling',
                completed: true
              },
              {
                id: 4,
                name: 'Aller au cinéma',
                completed:false
              }],
              complete: false,
              checkAll: false
            }
          ],
          selected : '',
          newTask : null,
          newListName : null
        }
      },

      methods: { 
        addNewList(){
          if (!this.newListName) {
            return;
          }

          this.lists.push({
            id: this.lists.length+1,
            name: this.newListName,
            tasks: [],
            complete : false,
            checkAll : false
          })

          this.newListName = '';
          this.saveTask();
        },

        addNewTask() {
          if (!this.newTask){
            return;
          }

          this.selected.tasks.push({
          id: this.selected.tasks[this.selected.tasks.length],
          name: this.newTask,
          completed : false
          })

          this.newTask = '';
          this.saveTask();
        },

        checkAllTasks(){

          if (this.checkAll == false) {
            for (let index = 0; index < this.selected.tasks.length; index++) {
              this.selected.tasks[index].completed = true;
            }
            this.checkAll = true;
          }
          else{
            for (let index = 0; index < this.selected.tasks.length; index++) {
              this.selected.tasks[index].completed = false;
            }
            this.checkAll = false;
          }

          this.saveTask();
        },

        editTask(task){ // Non fonctionnel
          let id = this.selected.tasks.indexOf(task);
          this.selected.tasks.splice(id, 1);
          this.selected.tasks.push({
            id : id,
            name : this.newTask,
            completed : false
          })
        },

        deleteTask(task){
          let id = this.selected.tasks.indexOf(task);
          this.selected.tasks.splice(id,1);
          this.saveTask();
        },
        updateTask(task){
          if (task.completed)
            this.completed = true;
          else this.completed = this.selected.tasks.filter(task => task.completed);
          this.saveTask();
        },
        selectList(e){
          let selectedList = this.lists.find(l => l.id == e.target.value);
          this.selected = selectedList;
        },

        clearLocalStorage(){
          localStorage.clear();
          alert("Les données locales ont été supprimé.");
          this.$forceUpdate();
        },

        saveTask() {
          const parsed = JSON.stringify(this.lists);
          localStorage.setItem('lists', parsed);
        }
      },

      mounted() {
        if (localStorage.getItem('lists')) {
          try {
            this.lists = JSON.parse(  localStorage.getItem('lists'));
          } catch(e) {
            localStorage.removeItem('lists');
          }
        }
      },
  }
</script>

<style>

th{
  text-align: center;
}
</style>