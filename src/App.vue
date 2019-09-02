<template>
    <div class="container">
        <div class="toDo-app">
            <app-progress v-bind:val="toDoArr.length" v-bind:max="maxNumber"></app-progress>
            <span>{{toDoArr.length}}/{{maxNumber}}</span>
            <input type="text" class="form-control" v-model="val">
 
            <button class="btn btn-success" v-on:click="addNewToDo" v-bind:disabled="done">Add</button>
            
            <ul class="toDo-nav list-group">
               <transition-group
                name="custom-classes-transition"
                enter-active-class="animated tada"
                leave-active-class="animated bounceOutRight"
              >
                <li  v-bind:key="item" v-for="(item,index) in toDoArr" class="list-group-item">
                    <span>{{item}}</span>
                    <template v-if="!status">
                        <div class="delete" v-on:click="removeToDo(index)">
                            <i class="fas fa-times-circle"></i>
                        </div>
                        <div class="edit" v-on:click="editToDo(item,index)">
                            <i class="fas fa-edit"></i>
                        </div>                         
                    </template>
                    <template v-else>
                        <div class="save" v-on:click="saveEditedToDo(item,index)">
                            <i class="fas fa-save"></i>
                        </div>
                    </template>
                </li>
              </transition-group>
            </ul>
        </div>
    </div>
</template>
<script>
    import Progress from './Progress.vue'
    export default {
        data(){
            return{
                toDoArr: [],
                val: '55',
                maxNumber: 10,
                status: false
            }
        },
        created: function(){
            console.log(this.toDoArr)
        },
        methods: {
            addNewToDo(){
                if( !this.done ) {
                    this.toDoArr.push(this.val);
                    this.val = '';
                }
            },
            removeToDo(index){
                this.toDoArr.splice(index,1)
            },
            editToDo(item,index){
                this.status = true;
                this.val = item;
            },
            saveEditedToDo(item,index){
                this.status = false;
                this.toDoArr.splice(index,1,this.val)
                this.val = '';
            }
        },
        computed: {
            done(){
                return this.toDoArr.length >= this.maxNumber;
            }
        },
        components: {
            AppProgress: Progress
        }
    }
    
</script>

<style scoped>
    .toDo-app{
        width: 360px;
        margin: 0 auto;
    }

    input[type="text"]{
        width: 300px;
        display: inline-block;
    }

    .delete, .edit, .save{
        position: absolute;
        top: 5px;
        right: 10px;
        cursor: pointer;
    }
    .edit{
        right: 40px;
    }
    .delete i, .edit i, .save i{
        font-size: 25px;
    }

    .list-group-item{
        position: relative;
    }

    span{
        display: block;
    }
</style>