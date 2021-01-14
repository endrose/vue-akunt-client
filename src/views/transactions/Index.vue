<template>
    <div class="container my-5">

           <div class="row justify-content-center">
               <div class="col-8">
                   <router-link :to="{name: 'transaction.create'}" class="btn btn-outline-success btn-sm mb-3">Add</router-link>
                    <div class="card rounded shadow">
                        <div class="card-header">
                            Transaction List
                        </div>
                        <div class="card-body">
                            <div class="table table-responsive">
                                <table class="table">
                                    <thead>
                                        <tr>
                                            <th>Title</th>
                                            <th>Amount</th>
                                            <th>Type</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(transaction, index) in transactions.data" :key="index">
                                            <td>{{transaction.title}}</td>
                                            <td>{{transaction.amount}}</td>
                                            <td>{{transaction.type}}</td>
                                            <td><router-link :to="{name: 'transaction.edit', params:{id:transaction.id}}" class="btn btn-sm btn-outline-primary mx-2">Edit</router-link> 
                                                <button class="btn btn-sm btn-outline-danger" @click.prevent="destroy(transaction.id, index)" >Delete</button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
               </div>

           </div>
       </div>
</template>
<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'

export default {
    name:"Index",
    setup(){
        // reactive state
        let transactions = ref([]);


        onMounted(()=> {
            axios.get("http://127.0.0.1:8000/api/transaction")
            .then((result=> {

                transactions.value = result.data
            })).catch((error)=>{
                console.log(error)
            });
        });

        function destroy(id, index){
            axios.delete(`http://127.0.0.1:8000/api/transaction/${id}`)
            .then((response)=> {
                transactions.value.data.splice(index, 1)
            }).catch((err)=>{

            }) 
            console.log(transactions.value)
        }

        return {
            transactions,
            destroy
        }
    }
}
</script>

<style scoped>

</style>