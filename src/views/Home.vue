<template>
  <div v-if="showAddTask">
    <AddTask @add-task="addTask"/>
  </div>
  <Tasks 
    @toggle-reminder="toggleReminder" 
    @delete-task="deleteTask" 
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks.vue'
import AddTask from '../components/AddTask.vue'

export default {
    name: 'Home',
    components: {
        Tasks, AddTask
    },
    props: {
        showAddTask: Boolean
    },
    data() {
        return {
            tasks: []
        }
    },
    methods: {
        
        async deleteTask(id) {
            const res = await fetch(`http://localhost:5000/tasks/${id}`, {
                method: 'DELETE'
            })

            if(res.ok)
                this.tasks = this.tasks.filter((task) => task.id != id)
            else alert('Error deleting task!')
        },

        async addTask(newTask) {
            const res = await fetch('http://localhost:5000/tasks', {
                method: 'POST',
                headers: {
                'Content-Type': 'application/json'
                },
                body: JSON.stringify(newTask)
            })
            const task = await res.json()

            this.tasks = [...this.tasks, task]
        },

        async toggleReminder(id) {
            let taskToToggle = await this.fetchTask(id)
            taskToToggle.reminder = !taskToToggle.reminder
            const res = await fetch(`http://localhost:5000/tasks/${id}`, {
                method: 'PUT',
                headers: {
                'Content-type': 'application/json'
                },
                body: JSON.stringify(taskToToggle)
            })
            const data = await res.json()

            this.tasks = this.tasks.map(task => 
                task.id == id ? { ...task, reminder: data.reminder} : task 
            )
        },

        async fetchTasks() {
            const res = await fetch('http://localhost:5000/tasks')
            const data = await res.json()
            return data
        },

        async fetchTask(id) {
            const res = await fetch(`http://localhost:5000/tasks/${id}`)
            const task = await res.json()
            return task
        }
    },
    async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>