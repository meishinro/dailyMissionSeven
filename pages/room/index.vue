<script setup>
const route = useRoute();
const router = useRouter();
const roomsList = ref([]);

const apiUrl = 'https://nuxr3.zeabur.app/api/v1/rooms';

fetch(apiUrl)
// 當fetch成功時，將回傳的資料轉換成json格式
    .then((res)=>{
    if(!res.ok){
        throw new Error("取得資料失敗");
    }
    return res.json();
})
// 當轉換成功時，將資料寫入roomsList變數
    .then((data)=>{
    const {result} = data;
    roomsList.value = result;
})
// 當發生錯誤時，印出錯誤訊息
.catch((error)=>{
    console.error("錯誤：",error);
})
</script>

<template>
  <h2>房型 index {{ route.fullPath }}</h2>
  <div class="container mt-4">
    <div class="row justify-content-center gy-3">
      <div class="col-8 col-md-6 col-lg-3" v-for="room in roomsList">
        <div class="card h-100 shadow-sm" @click="router.push('/room/_id')">
          <img :src="room.imageUrl" class="card-img-top" alt="Room Image" />
          <div class="card-body d-flex flex-column">
            <h3 class="card-title">{{ room.name }}</h3>
            <p class="card-text flex-grow-1">{{ room.description }}</p>
            <ul class="list-unstyled">
              <li><strong>面積:</strong> {{ room.areaInfo }}</li>
              <li><strong>床型:</strong> {{ room.bedInfo }}</li>
              <li><strong>最大容納人數:</strong> {{ room.maxPeople }}</li>
              <li><strong>價格:</strong> {{ room.price }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card-img-top {
  object-fit: cover;
  height: 200px;
  max-width: 100%;
}
</style>
