<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 roundedshadow">
                    <div class="card-body">
                        <h4>EDIT POST</h4>
                        <hr>

                        <form @submit.prevent="update">
                            <div class="form-group">
                                <label for="tittle" class="font-weight-bold">TITTLE</label>
                                <input type="text" class="form-control" v-model="post.title" placeholder="Masukan Judul Post">
                                <!-- validation -->
                                <div v-if="validation.tittle" class="mt-2 alert alert-danger">
                                    {{ validation.title[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="content" class="font-weight-bold">CONTENT</label>
                                <textarea class="form-control" rows="4" v-model="post.content" placeholder="Masukan Konten Post"></textarea>
                                <!-- validation -->
                                <div v-if="validation.content" class="mt-2 alert alert-danger">
                                    {{ validation.content[0] }}
                                </div>
                            </div>
                            <button type="submit" class="btn btn-primary">SIMPAN</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { reactive, ref, onMounted} from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";

export default {
    setup() {
        const post = reactive({
           tittle: '',
           content: '' 
        })

        const validation = ref([]);
        const router = useRouter();
        const route = useRoute();

        onMounted(() => {
            axios.get('http://novaagustina.online/backendapi/public/api/post/${route.params.id}')
            .then(response=>{
                post.tittle = response.data.data.tittle
                post.content = response.data.data.content
            }).catch(error=>{
                console.log(error.response.data)
            })
        })

        function update(){
            let tittle = post.tittle
            let content = post.content

            axios.put('http://novaagustina.online/backendapi/public/api/post/${route.params.id}',
            {
                tittle : tittle,
                content : content
            }).then(()=> {
                router.push({
                    name : 'post.index'
                })
            }).catch(error=>{
                validation.value = error.response.data
            })
        }
        return{
            post,
            validation,
            router,
            route,
            update
        }
    }
}
</script>

<style>
body {
    background: lightgray;
}
</style>