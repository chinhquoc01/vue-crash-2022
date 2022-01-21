<template>
    <header>
        <h1>{{title}}</h1>
        <Button 
                v-show="homePage"
                @btn-click="$emit('toggle-add-task')" 
                :text="showAddTask ? 'Hide' : 'Add Task'" 
                :color="showAddTask ? 'Gray' : 'Green'" 
                :showAddTask="showAddTask"/>
    </header>
</template>

<script>
import Button from './Button.vue'
export default {
    name: 'Header',
    components:{
        Button
    },
    props: {
        title: {
            type: String,
            default: 'Hello World'
        },
        showAddTask: Boolean
    },
    emits: ['toggle-add-task'],
    computed: {
        homePage() {
            if(this.$route.path == '/') {
                return true
            } else {
                return false
            }
        }
    }
}
</script>

<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>