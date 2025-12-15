<template>
    <div class="container">
        <div class="row">
            <input type="text" size="20" class="input-sm" v-model="title">
            <button type="button" class="btn-sm btn-danger" @click="find()">검색</button>
        </div>
        <div class="row">
            <div class="col-md-4" v-for="(youtube, index) in youtubes" :key="index">
                <div class="thumbnail">                    
                    <iframe style="width: 280px; height: 180px;" :src="`https://www.youtube.com/embed/${youtube.id.videoId}`"></iframe>
                    <div class="caption">
                        <p v-html="youtube.snippet.title"></p>
                    </div>                    
                </div>
            </div>
        </div>  
    </div>    
</template>
<script setup>
import {ref, onMounted} from 'vue';
const title = ref("부산여행")
console.log(title)
const youtubes = ref([])
onMounted(() => {
    find()
})
const find = async () => {
    try {
        const response = await fetch(
            `https://youtube.googleapis.com/youtube/v3/search?part=snippet&maxResults=12&q=${title.value}&type=video&key=AIzaSyBqmW0985W2aHSiegcqWOz8D9Z5Dnz9B2A`
        )
        const result = await response.json()
        console.log(result.items)
        youtubes.value = result.items
    }catch(error) {
        console.log(
            "error", error
        )
    }
}
</script>
<style scoped>
.row {
    margin: 0px auto;
    width: 960px;
}
p {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
</style>