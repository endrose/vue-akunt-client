<template>
    <div class="container my-5">

           <div class="row justify-content-center">
               <div class="col-8">
                   <router-link :to="{name: 'transaction.index'}" class="btn btn-outline-primary btn-sm mb-3">Back</router-link>
                    <div class="card rounded shadow">
                        <div class="card-header">
                            Transaction Edit
                        </div>
                        <div class="card-body">
                           <form @submit.prevent="update" >
                               <div class="form-group mb-2">
                                   <label for="amount">Title</label>
                                   <input type="text" name="title" id="title" v-model="transaction.title" class="form-control outline-none">
                                   <div v-if="validation.title" class="text-danger mx-3">
                                       {{ validation.title[0]}}
                                   </div>
                               </div>
                               <div class="form-group mb-2">
                                   <label for="amount">Amount</label>
                                   <input type="text" name="amount" id="amount" v-model="transaction.amount" class="form-control outline-none">
                                   <div v-if="validation.amount" class="text-danger mx-3">
                                       {{ validation.amount[0]}}
                                   </div>
                               </div>
                                <div class="form-group mb-2">
                                   <label for="amount">Time</label>
                                   <input type="text" name="time" id="time" v-model="transaction.time" placeholder="yyyy-mm-dd hh:mm:ss" class="form-control outline-none">
                                   <div v-if="validation.time" class="text-danger mx-3">
                                       {{ validation.time[0]}}
                                   </div>
                               </div>
                                <div class="form-group mb-2">
                                   <label for="type">Type</label>
                                  
                                   <select name="type" class="form-select" v-model="transaction.type">

                                       <option value="expense">Expense</option>
                                       <option value="revenue">Revenue</option>
                                   </select>
                                   <div v-if="validation.type" class="text-danger mx-3">
                                       {{ validation.type[0]}}
                                   </div>
                               </div>
                               <button type="submit" class="btn btn-outline-success btn-sm">Submit</button>
                           </form>
                        </div>
                    </div>
               </div>

           </div>
       </div>
</template>
<script>
import { onMounted, reactive, ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import axios from 'axios';

export default {
    name:"Edit",
    setup(){
        // data  binding
        let transaction = reactive({
            title:"",
            amount:"",
            time: "",
            type:"",
        });

        const validation = ref([]);

        const router = useRouter();

        const route = useRoute();
        console.log(route);
        
        onMounted(()=>{
            axios.get(`http://127.0.0.1:8000/api/transaction/${route.params.id}`)
            .then((response)=> {

                transaction.title = response.data.data.title
                transaction.amount = response.data.data.amount
                transaction.time = response.data.data.time
                transaction.type = response.data.data.type

              console.log(transaction)
                
            }).catch((err)=>{
                validation.value = err.response.data
            })
        });

        function update(){
            axios.put(`http://127.0.0.1:8000/api/transaction/${route.params.id}`, transaction)
            .then(()=>{
                router.push({name:'transaction.index'})
            }).catch((err)=>{
                validation.value = err.response.data
            })
        }

        return {
            transaction,
            validation,
            route,
            router,
            update
        }
    }
    
}
</script>

<style scoped>

</style>