<template>
    <div>
        <div class="tasks" v-if="tasks.length>0">
            <div v-for="(task,i) in tasks" :key="task.name">
                <task class="task" :task="task" 
                    @removeTask="$emit('removeTask', i)"
                    @changeStatus="$emit('changeStatus', i)" />
            </div>
        </div>
        <div class="no-task" v-else>
            <p>Sua vida está em dia</p>
        </div>
    </div>
</template>

<script>

import Task from './Task'


export default {
    components: {
        Task
    },

    props: {
        tasks: Array,
        required: true
    }
}
</script>

<style  scoped>

.tasks {
    display:flex;
    justify-content: center;
    flex-wrap: wrap;

}

.tasks .task {
    margin: 15px;
}

.no-task {
    font-size: 1.7em;
    color: "#AAA";
}

</style>