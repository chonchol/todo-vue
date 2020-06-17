<template>
    <div>
        <div class="container">
            <div class="row">
                <div class="col-6 offset-3">
                    <div class="form-area pb-4 pt-4">
                        <form v-on:submit.prevent>
                            <div class="form-row align-items-center">
                              <div class="col-12">
                                <label class="sr-only" for="task-name">Task Name</label>
                                <input type="text" class="form-control" placeholder="what needs to be done!" v-model="newTodo" @keyup.enter="addTodo">
                                <!-- <button @click="addTodo">Add</button> -->
                              </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-6 offset-3">
                    <div class="card">
                        <div class="card-header">
                          TODO - LIST
                        </div>
                        <div class="card-body">                    
                            <div class="pills-area">
                                <div class="extra-option1">
                                    <label for=""><input type="checkbox" name="" id=""> Check All</label>
                                    <span> {{ remaining }} Item left </span>
                                </div>
                                <ul class="nav nav-pills">
                                    <li class="nav-item">
                                        <a class="nav-link" href="#" @click="filter = 'all'" :class="{active : filter == 'all'}">All</a>
                                    </li>
                                    <li class="nav-item">
                                        <a class="nav-link" href="#" @click="filter = 'completed'" :class="{active : filter == 'completed'}">Completed</a>
                                    </li>                                    
                                    <li class="nav-item">
                                        <a class="nav-link" href="#" @click="filter = 'active'" :class="{active : filter == 'active'}">Active</a>
                                    </li>
                                    <li class="nav-item">
                                        Clear Completed
                                    </li>
                                </ul>
                                
                            </div>
                            <ul>
                                <li v-for="(todo, index) in todosFiltered" :key="todo.id">

                                    <!-- <div v-if="!todo.editing" class="custom-control custom-checkbox" @change="completedTodo(todo)">
                                        <input type="checkbox" name="" class="custom-control-input" :id="'customCheck' + index" v-model="todo.completed">
                                        <label class="custom-control-label" :for="'customCheck' + index" @dblclick="editTodo(todo)">{{ todo.title }}</label>
                                    </div> -->

                                    <input type="checkbox" name="" id="" v-model="todo.completed">
                                    <span v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-title" :class="{ completed : todo.completed }">{{ todo.title }}</span>

                                    <input v-else type="text" class="form-control" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus>
                                    <span class="remove-item" @click="removeTodo(index)">&times;</span>
                                </li>
                            </ul>
                        </div>
                    </div>
                    <small id="passwordHelpBlock" class="form-text text-muted">
                    To insert task, type and enter and for editing task, double click on specific task!
                    </small>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'todo-list',
    data () {
        return {
            newTodo: '',
            idFortodo: 3,
            filter: 'all',
            todos: [
            { id: 1, title: 'My first Vue App', completed: false, editing: false},  
            { id: 2, title: 'Please enter your vue task', completed: false, editing: false},  
            ]
        }
    },

    computed: {
        remaining(){
            return this.todos.filter(todo => !todo.completed).length
        },

        todosFiltered(){
            if(this.filter == 'all'){
                return this.todos
            } else if(this.filter == 'completed'){
                return this.todos.filter(todo => todo.completed)
            }else if(this.filter == 'active'){
                return this.todos.filter(todo => !todo.completed)
            }
            return this.todos
        }
    },

    directives: {
        focus: {
            inserted: function (el) {
                el.focus()
            }
        }
    },

    methods: {
        addTodo(){
            if(this.newTodo.trim().length == 0){
                return
            }

            this.todos.push({
                id: this.idFortodo,
                title: this.newTodo,
                completed: false,
                editing: false
            })

            this.newTodo = ''
            this.idFortodo++
        },

        editTodo(todo){
            todo.editing = true
        },

        doneEdit(todo){
            todo.editing = false
        },

        completedTodo(todo){
            //alert(todo.completed)
            todo.completed = true
        },

        removeTodo(index){
            this.todos.splice(index, 1)
        }
    }
}
</script>

<style>
.card-header{
    background-color: #9c27b0;
    color: white;
    text-align: center;
    box-shadow: 0 4px 20px 0 rgba(0,0,0,.14), 0 7px 12px -5px rgba(156,39,176,.46);
    padding: .4rem 1.25rem;
}
.form-control:focus{
    border-color: #9c27b0;
    box-shadow: 0 4px 20px 0 rgba(0,0,0,.14), 0 7px 12px -5px rgba(156,39,176,.46);
}
.card-body ul{
    margin-bottom: 0px;
    padding-left: 5px;
}
.card-body ul li{
    padding: 5px 0px;
    list-style: none;
    text-align: left;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-right: 5px;
}
.todo-title{
    margin-right: auto;
    margin-left: 8px;
    font-size: 13px;
}
.remove-item{
    cursor: pointer;
    padding-left: 10px;
}
.nav-link{
    padding: 2px 10px;
}
.nav-pills .nav-link.active, .nav-pills .show>.nav-link{
    background-color: #9c27b0;
}
.pills-area .nav-pills{
    margin-bottom: 20px;
    border-bottom: 1px solid #9c27b0;
    padding: 2px 0px;
}
.nav-item .nav-link{
    color: #9c27b0;
}
.pills-area .nav-item{
    font-size: 12px;
}
.custom-control{
    font-size: 14px;
}
small{
    font-size: 70%;
}
.completed{
    text-decoration: line-through;
    color: grey;
}
.extra-option1{
    display: flex;
    justify-content: space-between;
    font-size: 12px;
}
</style>
