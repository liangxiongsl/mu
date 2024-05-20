<script setup>
let key = ref('')
let res = ref([])
let url ='https://netease-cloud-music-api-git-master-liangxiongsl.vercel.app'
// let url ='http://localhost:4000'
watchEffect(async ()=>{
  res.value = (await useFetch(`${url}/search?keywords=${key.value}`)).data._rawValue?.result?.songs
  if (!res.value) return
  console.log(Object.keys(res.value[0]))
  console.log(res.value[0])
})

// let sid = ref(localStorage.getItem('sid'))
let sid = ref('')
let on_play = (sid_)=>{
  // localStorage.setItem('sid', sid_)
  sid.value = sid_
}

</script>

<template>
  <el-input v-model="key"></el-input>
<!--  {{res}}-->
<!--  <li v-for="(v) in res">{{v}}</li>-->
<!--  <el-scrollbar max-height="500" scrollbar-always-on>-->
<!--    -->
<!--  </el-scrollbar>-->
  
  <el-table :data="res" scrollbar-always-on>
    <el-table-column label="歌曲">
      <template #default="{row: song}">
        <el-link :href="`https://music.163.com/#/song?id=${song.id}`" target="_blank">
          {{song.name}}
        </el-link>
      </template>
    </el-table-column>
    <el-table-column label="歌手">
      <template #default="{row: song}">
        <el-tag v-for="(v) in song.artists">
          <el-link :href="`https://music.163.com/#/artist?id=${v.id}`" target="_blank">
            {{v.name}}
          </el-link>
        </el-tag>
      </template>
    </el-table-column>
    <el-table-column label="专辑">
      <template #default="{row: song}">
        <el-tag>
          <el-link :href="`https://music.163.com/#/album?id=${song.album.id}`" target="_blank">
            {{song.album.name}}
          </el-link>
        </el-tag>
      </template>
    </el-table-column>
    <el-table-column label="练习时长" prop="duration" sortable>
      <template #default="{row: song}">
        {{`${new Date(song.duration).getMinutes()}:${new Date(song.duration).getSeconds()<=9?0:''}${new Date(song.duration).getSeconds()}`}}
      </template>
    </el-table-column>
    <el-table-column label="">
      <template #default="{row: song}">
        <el-button @click="on_play(song.id)">播放</el-button>
<!--        <el-button @click="">复制</el-button>-->
      </template>
    </el-table-column>
  </el-table>

  <el-affix position="bottom">
    <iframe class="w-full h-full" :src="`//music.163.com/outchain/player?type=2&id=${sid}`"></iframe>
  </el-affix>

</template>

<style scoped>

</style>
