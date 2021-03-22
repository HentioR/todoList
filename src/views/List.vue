<template>

<div>
  <label>Choisir une liste</label>
  <br />
  <select v-model="selected" v-on:change="selectList($event)">
    <option v-for="list in lists" :value="list.id" v-bind:key="list.id"> {{list.name}} </option>
  </select>
  <br />

  <ul class="list">
    <li class="task" v-for="task in selected.tasks" v-bind:key="task.id" :class="{completed: task.completed}">
      <div>
        <input type="checkbox" v-model="task.completed" v-on:change="updateTask(task)">
        <label> {{task.name}} </label> 
        <button type="destroy" @click="deleteTask(task)">❌</button>
      </div>
    </li>
  </ul>
  <form v-if="selected" v-on:submit.prevent="addNewTask()">
    <label for="new-task">Ajouter une tâche</label>
    <input v-model="newTaskName"  id="new-task" placeholder="Ex. nourrir le chat">
    <button>Ajouter</button>
  </form>
  
</div> 
</template>

<script>
export default{
      name: 'App',
      components: {
      },
      data() {
        return {
          lists:[
            {
              id: "1",
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
                completed: false
              }],
              filter: 'all',
              complete: false,
              checkAll: false
            },
            {
              id: "2",
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
                completed: false
              },
              {
                id: 3,
                name: 'Jouer au bowling',
                completed: false
              },
              {
                id: 4,
                name: 'Aller au cinéma',
                completed:false
              }],
              filter: 'all',
              complete: false,
              checkAll: false
            }
          ],
          selected : ''
        }
      },
      methods: { 
        addNewTask() {
          this.selected.tasks.push({
            id: this.nextTodoId++,
            name: this.newTaskName
          })
          this.newTodoText = ''
        },
    
        deleteTask(task){
          let nb = this.selected.tasks.indexOf(task);
          this.selected.tasks.splice(nb,1);
        },
        updateTask(task){
          if (task.completed)
            this.completed = true;
          else this.completed = this.selected.tasks.filter(task => task.completed);
        },
        selectList(e){
          let selectedList = this.lists.find(l => l.id == e.target.value);
          this.selected = selectedList;
        }
      },
  }
</script>

<style>
li{
    list-style-type: none;
}
</style>