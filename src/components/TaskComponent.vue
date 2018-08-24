<template>
  <div class="hello">
    <h1>{{msg}}</h1>
    <div>

      <div>
        <h2>Create Task</h2>
        <form @submit="onSubmit($event)">
          <input type="text" v-model="newTask.title" name="newTask.title" id="">
          <input type="text" v-model="newTask.description" name="newTask.description" id="">
          <input type="submit" value="Create Task">
        </form>
      </div>

      <div :class="{ hidden: !editable }">
        <h2>Edit Task</h2>
        <form @submit="onUpdate($event)">
          <input type="text" v-model="editableTask.title" name="editableTask.title" id="">
          <input type="text" v-model="editableTask.description" name="editableTask.description" id="">
          <input type="submit" value="Update Task">
        </form>
      </div>

      <div>
        <h2>Task List</h2>
        <ul>
          <li 
            v-for="(task, index) in tasks" 
            :key="index" 
            :class="{ completed: task.completed }">
            
            {{index}} - {{ task.title }} - {{ task.description }}
            <button 
              :class="{ hidden: task.completed }" 
              @click="toggleCompleted(task)">
              Mark Incompleted
            </button>
            <button 
              :class="{ hidden: !task.completed }" 
              @click="toggleCompleted(task)" >
              Mark Completed
            </button>
            <button 
              @click="editTask(index)" >
              Edit
            </button>
            <button 
              @click="deleteTask(index)" >
              Delete
            </button>
            
          </li>
        </ul>
      </div>

    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { FormatInputPathObject } from "path";
import Task from "@/classes/Task.vue";

@Component
export default class TaskComponent extends Vue {
  public tasks: Task[] = [
    { title: "first task", description: "lorem ipsum lorem ipsum lorem ipsum lorem ipsum", completed: false },
    { title: "second task", description: "lorem ipsum lorem ipsum lorem ipsum lorem ipsum", completed: true },
    { title: "third task", description: "lorem ipsum lorem ipsum lorem ipsum lorem ipsum", completed: true }
  ];
  public newTask: any = { title: "", description: "", completed: true };
  public editable: boolean = false;
  public editableTask: any = { title: "", description: "", completed: true };

  @Prop() private msg!: string;

  public onSubmit(event: Event): void {
    event.preventDefault();
    this.tasks.push(this.newTask);
    this.newTask = { title: "", description: "", completed: true };
  }

  public toggleCompleted(task: Task): void {
    task.completed = !task.completed;
  }

  public editTask(index: number): void {
    this.editable = true;
    this.editableTask = this.tasks[index];
  }
  public onUpdate(event: Event): void {
    event.preventDefault();
    this.editable = false;
    this.editableTask = { title: "", description: "", completed: true };
  }

  public deleteTask(index: number): void {
    this.tasks.splice(index, 1);
  }
}
</script>

<style scoped>
.completed {
  color: green;
}
.hidden {
  display: none;
}
</style>
