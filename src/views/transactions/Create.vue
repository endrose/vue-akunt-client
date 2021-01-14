<template>
    <div class="container my-5">

           <div class="row justify-content-center">
               <div class="col-8">
                   <router-link :to="{name: 'transaction.index'}" class="btn btn-outline-primary btn-sm mb-3">Back</router-link>
                    <div class="card rounded shadow">
                        <div class="card-header">
                            Transaction Create
                        </div>
                        <div class="card-body">
                           <form @submit.prevent="store" >
                               <div class="form-group mb-2">
                                   <label for="title">Title</label>
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
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios';

export default {
    name:"Create",
    setup(){
        // data  binding
        const transaction = reactive({
            title:"",
            amount:"",
            time: "",
            type:"",
        });

        const validation = ref([]);

        const router = useRouter();

        function store(){
            axios.post("http://127.0.0.1:8000/api/transaction", transaction)
            .then(()=>{
                router.push({name:'transaction.index'})
            }).catch((err)=>{
                validation.value = err.response.data
            })
        }

        return {
            transaction,
            validation,
            router,
            store
        }
    }
    
}
</script>

<style scoped>

</style>