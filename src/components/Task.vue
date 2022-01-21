<template>
    <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder? 'reminder' : '', 'task']">
        <h3>
            {{task.text}}
            <i @click="onDelete(task.id)" class="fas fa-times"></i>
        </h3>
        <p>{{task.day}}</p>
    </div>
</template>

<script>
export default {
    name: 'Task',
    props: {
        task: Object
    },
    methods: {
        onDelete(id) {
            // ! emit để gửi sự kiện từ components con lên component bố
            // * sự kiện ở đây là 'delete-task' với tham số là 'id'
            this.$emit('delete-task', id)
        }
    },
    emits: ['toggle-reminder']
}
</script>

<style scoped>
.task {
    background: #f4f4f4;
    margin: 5px;
    padding: 10px 20px;
    cursor: pointer;
}

.task.reminder {
    border-left: 5px solid green;
}

.task h3 {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.fas {
    color: red;
}
</style>