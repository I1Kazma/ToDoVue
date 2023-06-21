
<template>
    <div>
        <h1>ToDo-List</h1>
        <TodoList location="home" />
    </div>
</template>

<script>
import TodoList from '@/components/TodoList.vue';

export default {
    name: "HomePage",
    components: {
        TodoList
    }
}
</script>