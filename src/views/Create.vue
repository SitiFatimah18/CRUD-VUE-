<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                    <h4>TAMBAH POST</h4>
                    <hr>

                    <form @submit.prevent="store">
                        <div class="form-group">
                            <label for="title" class="font-wight-bold">TITTLE</label>
                            <input type="text" class="form-control" v-model="post.title" placeholder="Masukan Judul Post">
                            <!-- validation -->
                            <div v-if="validation.title" class="mt-2 alert alert-danger">
                                {{ validation.title[0] }}
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="content" class="font-weight-bold">CONTENT</label>
                            <textarea class="form-control" rows="4" v-model="post.content" placeholder="Masukan Konten Post"></textarea>
                            <!-- validation -->
                            <div v-if="validation.content" class="mt-2 alert alert-dager">
                                {{ validation.content }}
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
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";

export default {
    setup() {
        //state posts
        const post = reactive({
            tittle: "",
            content: "",
        });

        //stete validation
        const validation = ref([]);

        //vue router
        const router = useRouter();


        //method store
        function store() {
            let tittle = post.tittle;
            let content = post.content;

            axios
            .post("http://novaagustina.online/backendapi/public/api/post",{
                tittle: tittle,
                content: content,
            })
            .then(() => {
                //redirect ke post index
                router.push({
                    name: "post.index",
                });
            })
            .catch((error) =>{
                //assign state validation with error
                validation.value = error.response.data;
            });
        }
        
        //return
        return {
            post,
            validation,
            router,
            store,
        }
    }
    
}
</script>
