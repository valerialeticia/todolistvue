<template>
    <div id="task">
        <form @submit.prevent="addTask">
            <input 
              v-model="task"
              type="text"
              placeholder="Tarefa de hoje?"
             />
            <button type="submit">Adicionar</button>
        </form>

        <Item :list="tasks"  :delete="deleteTask" />

        <span v-show="tasks.length">
            Você tem <strong :class="{'strong': tasks.length > 4}">
            {{ tasks.length }}</strong> tarefas pendentes
        </span>
    </div>
</template>

<script>
import Item from './Item'
export default {
    name: 'Task',
    components: {
        Item
    },

    data() {
        return {
            task: '',
            tasks: []
        }
    },

    watch: {
        tasks: {
            deep: true,
            handler() {
                localStorage.setItem('tasks', JSON.stringify(this.tasks))
            }
        } 
    },

    created(){
        const items = localStorage.getItem('tasks')
        this.tasks = JSON.parse(items) || []
    },

    methods: {
        addTask() {
            if(this.task !== '') {
                this.tasks.push({
                    text: this.task,
                    key: Date.now()
                })
            }else {
                alert('Digite uma tarefa...')
            }
            this.task = ''
        },

        deleteTask(key) {
            let filter = this.tasks.filter((item) => {
                return (item.key !== key)
            })

            return this.tasks = filter
        }
    }
}
</script>

<style scoped>
#task {
    max-width: 700px;
    background: #ffffff;
    border-radius: 4px;
    padding: 20px;
    margin: 20px auto;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
}

form {
    margin-top: 30px;
    display: flex;
    flex-direction: row;
}

form button {
    cursor: pointer;
    background: #0f5959;
    border: 0;
    border-radius: 4px;
    margin-left: 10px;
    padding: 0 15px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
}

input {
    flex: 1;
    border: 1px solid #eee;
    padding: 6px 10px;
    border-radius: 4px;
    font-size: 14px;
    outline: none;
}

.strong {
    color: #ff0400;
}

</style>