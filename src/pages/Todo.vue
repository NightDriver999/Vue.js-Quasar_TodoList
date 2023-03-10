<template>
  <q-page class="bg-red-2 column">

    <div class="row q-pa-sm bg-primary">
      <q-input 
        v-model="newTask" 
        @keyup.enter="addTask"
        class="col"
        square
        filled 
        bg-color="white" 
        placeholder="Add task" 
        dense>
        <template v-slot:append>
          <q-btn 
            @click="addTask"
            round 
            dense 
            flat 
            icon="add" />
        </template>
      </q-input>
    </div>

    <q-list separator bordered>
      <q-item
        @click="task.done = !task.done" 
        clickable
        :class="{ 'done bg-green-2': task.done }"
        v-for="(task, index) in tasks" 
        :key="task.title" 
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox class="no-pointer-events" v-model="task.done" val="teal" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label> {{task.title}} </q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" dense flat round color="primary" icon="delete" />
        </q-item-section>
      </q-item>

      <div v-if="!tasks.length" class="no-tasks absolute-center">
        <q-icon
          name="check"
          size="100px"
          color="primary" />
        <div class="text-h5 text-primary text-center">
          No tasks
        </div>
      </div>

    </q-list>
  </q-page>
</template>

<script>

export default {
  data() {
    return {
      newTask: '',
      tasks: [
        {
          title: "hello",
          done: true,
        },
        {
          title: "hello2",
          done: false,
        },
        {
          title: "hello3",
          done: false,
        },
      ]
    }
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify('Task deleted')
      })
    },
    addTask() {
      if (this.newTask !== '') {
        this.tasks.push({
        title: this.newTask,
        done: false
        })
        this.newTask = ''
      }
    }
  }
}

</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: red;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
