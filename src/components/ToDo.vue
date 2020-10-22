<template>
    <div>
        <div class="bg-teal-400 h-screen w-screen flex justify-center items-center font-sans">
            <div class="w-4/5 bg-gray-800 rounded-md shadow-2xl p-10">
                <nav class="flex flex-col sm:flex-row sm:justify-between">
                    <div>
                        <p class="text-white text-lg font-bold">{{ todayDate }}</p>
                        <p class="text-teal-400 text-sm capitalize">{{ incompleteTasks.length }} active tasks</p>
                    </div>
                    <div class="mt-2 sm:mt-0">
                        <a :class="{'text-gray-500':showCompleted}" @click.prevent="showCompleted = false"
                           class="text-white capitalize mr-12" href="#">Incomplete</a>
                        <a :class="{'text-gray-500':!showCompleted}" @click.prevent="showCompleted = true"
                           class="text-white capitalize" href="#">Completed</a>
                    </div>
                </nav>

                <form class="mt-6 flex flex-col sm:flex-row" @submit.prevent="addTask">
                    <input v-model="taskInput" class="mr-2 px-3 py-2 rounded text-sm w-full sm:w-1/3" type="text"
                           placeholder="Enter a task...">
                    <button class="bg-teal-400 rounded text-white text-sm px-3 py-2 w-1/2 mx-auto mt-2 sm:mt-0 sm:mx-0 sm:w-1/12">Add task</button>
                </form>

                <section v-if="tasks.length !== 0" class="divide-y divide-gray-600 mt-10">
                    <div></div>
                    <div v-for="(task, key) in tasks" :key="task.text"
                         class="flex justify-between items-center py-2">
                        <div class="flex items-center">
                            <i @click="completeTask(key)"
                               :class="{'complete-icon':showCompleted}"
                               class="material-icons text-2xl text-gray-600 mr-4 hover:text-green-500 cursor-pointer select-none">check_circle</i>
                            <p :class="{'complete-text': showCompleted}" class="text-white">{{ task.text }}</p>
                        </div>
                        <i @click="deleteTask(key)"
                           class="material-icons text-2xl text-gray-600 hover:text-red-500 cursor-pointer select-none">delete</i>
                    </div>
                    <div></div>
                </section>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "ToDo",
        data: function () {
            return {
                taskInput: '',
                todayDate: (new Date()).toLocaleString('en-US', {month: 'long', day: 'numeric', weekday: 'long'}),
                showCompleted: false,
                completedTasks: [],
                incompleteTasks: []
            }
        },
        methods: {
            completeTask: function (index) {
                // eslint-disable-next-line no-debugger
                // debugger;
                if (this.showCompleted){
                    this.incompleteTasks.push(this.completedTasks[index]);
                    this.deleteTask(index);
                } else {
                    this.completedTasks.push(this.incompleteTasks[index]);
                    this.deleteTask(index);
                }
            },

            addTask: function () {
                if (this.taskInput.length === 0) return;
                this.incompleteTasks.push({text: this.taskInput});
                this.taskInput = '';
            },

            deleteTask: function (idx) {
                if (!this.showCompleted) {
                    this.incompleteTasks.splice(idx, 1);
                } else {
                    this.completedTasks.splice(idx, 1);
                }
            },
        },
        computed: {
            tasks: function () {
                if (this.showCompleted) {
                    return this.completedTasks;
                } else {
                    return this.incompleteTasks;
                }
            },
        }
    }
</script>

<style scoped>
    .complete-text {
        @apply line-through
    }

    .complete-icon {
        @apply text-green-500;
    }
</style>