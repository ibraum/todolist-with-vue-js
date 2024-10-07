<script setup>
  import {computed, ref} from 'vue'

  const mask = ref(false)
  let tasks = ref([])
  const taskName = ref("")
  const message = "Aucune tâche n'est disponible ..."
  let showForm = ref(false)
  
  const addTask = () => {
    const now = new Date()
    const task = {
        title: taskName.value,
        completed: false,
        date: now.getTime()
    }
    tasks.value.push(task)
    taskName.value = ""
  }

  const sortTask =
      () => {
      return tasks.value.toSorted((a, b) => 
        a.completed > b.completed ? 1 : -1
      )
    }

  const sortUncheckedTask = () => {
    return tasks.value.filter(task => task.completed === false)
  }

  const sortCheckedTask = () => {
    return tasks.value.filter(task => task.completed === true)
  }

  const deleteTask = (date) => {
    console.log(tasks.value.length)
    tasks = ref(tasks.value.filter((task) => task.date !== date))
    sortTask()
  }

  // const isVisibleForm = () => {
  //   return showForm = showForm ? false : true
  // }

    console.log(computed(sortTask()))

</script>

<template>
  <div class="container">
    <!-- <div v-if="showForm === true"> -->
      <form  @submit.prevent="addTask()" :id="['form']" >
        <div>
          <input type="text" v-model="taskName" :placeholder="['Entrer une tâche']" >
          <input type="submit" value="add">
        </div>
      </form>
    <!-- </div> -->

  <div :class="['list-task']">
    <ul v-if="tasks.length > 0">
      <div v-if="mask === false">
        <li v-for="task in sortTask()"  :class="{checked: task.completed === true}" :key="task.date">
            <span>
              <input type="checkbox" v-model="task.completed" id="">
              {{ task.title }}
            </span>
            <span>
              <button @click="deleteTask(task.date)">x</button>
            </span>
        </li>
      </div>

      <div v-else>
        <li v-for="task in sortUncheckedTask()"  :class="{checked: task.completed === true}" :key="task.date">
          <input type="checkbox" v-model="task.completed" id="">
          {{ task.title }}
        </li>
      </div>
    </ul>

    <span :class="['message']" v-else>
      {{ message }}
    </span>
  </div>

  <div :class="['container-mask']">
    <span>Mask checked tasks ?<input type="checkbox" v-model="mask"></span>
    <ul v-if="tasks.length > 0">
      <div :class="['ul-div']" v-if="mask === true">
        <li  v-for="task in sortCheckedTask()"  :class="[task.completed ? 'checked' : '', 'li-mask']" :key="task.date">
          {{ task.title }}
          <input type="checkbox" v-model="task.completed" id="">
        </li>
      </div>
    </ul>
  </div>

  <!-- <input type="button" v-on:click="isVisibleForm()" :class="['add-circle']" value="+"> -->
  </div>
</template>

<style scoped>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    /* outline: 1px solid red; */
  }
  .container{
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: start;
    align-items: center;
  }
  #form{
    width: 90%;
    box-shadow: 0 0 10px 1px rgba(0, 0, 0, 0.2);
    padding: 30px;
    border-radius: 4px;
  }
  #form div{
    width: 100%;
    height: 100%;
  }
  #form div input[type="text"]{
    padding: 20px 10px;
    width: 100%;
    display: block;
    border: 1px solid black;
    border-radius: 8px 8px 0 0;
    font-size: 1.1rem;
  }
  #form div input[type="text"]:focus{
    border: 1px solid black;
    outline: none;
  }
  #form div input[type="submit"]{
    padding: 15px;
    width: 100%;
    border: 1px solid black;
    border-radius: 0 0 8px 8px;
    background-color: black;
    color: white;
    cursor: pointer;
    font-size: 1.1rem;
font-weight: bold;
  }
  .list-task{
    padding: 30px 0;
    width: 90%;
    margin-top: 5px;
  }
  .checked{
    text-decoration: line-through;
  }
  .message{
    width: 100%;
    padding: 10px;
    text-align: center;
    color: rgb(139, 137, 137);
    display: block;
  }
  ul{
    list-style: none;
    width: 100%;
  }
  li{
    width: 100%;
    padding: 20px;
    margin: 10px 0;
    border-radius: 4px;
    /* border: 1px solid black; */
    display: flex;
    justify-content: space-between;
    box-shadow: 5px 2px 10px 1px rgba(0, 0, 0, 0.2);
  }
  .container-mask{
    width: 90%;
  }
  .ul-div{
    width: 100%;
  }
  .li-mask{
    background-color: rgb(216, 213, 213);
    color: rgb(110, 110, 110);
  }
  .add-circle{
    text-align: center;
    align-content: center;
    width: 60px;
    height: 60px;
    font-size: 2rem;
    background-color: black;
    border: none;
    color: white;
    border-radius: 50%;
    position: fixed;
    bottom: 10px;
    margin: 0 auto;
    cursor: pointer;
  }
</style>
