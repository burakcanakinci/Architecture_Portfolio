<template>
  <main class="container mt-5" style="border-top: 1px solid #d9dbe9;">
      <div class="row">
        <div class="col-12 text-right mb-4">
          <div class="justify-content-between">
            <NuxtLink to="/AddGalleryPage"><a class="button-primary w-button">Add Gallery</a></NuxtLink>
          </div>
        </div>
        <div v-if="!images.length">Nothing is loading, try again.</div>
        <div style="
          width: 70%;
          display: flex;
          flex-wrap: wrap;">
          <div v-for="image of images" :key="image"
            class="col-lg-3 col-md-4 col-sm-6 mb-4">
            <div class="card recipe-card box-shadow width">
              
              <img :src="image.imageUrl[0]" class="card-img-top"/>
              <div class="card-body">
                <p class="card-text">{{ image.type }}</p>
                <div class="action-buttons">
                  <button @click="onDelete(image._id)" class="btn btn-sm button-color-delete">Delete</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
</template>

<script>
import axios from 'axios'
  export default {
    data() {
      return {
        images:[]
      }
    },
     created() {
      this.images = axios.get(
        'https://thesis-project-beta.herokuapp.com/api/v1/gallery'
      ).then(res => {
        console.log(res.data);
        this.images = res.data.images
      })
    },
    methods: {
    async onDelete(imageId) {
      try {
        axios.delete(`https://thesis-project-beta.herokuapp.com/api/v1/gallery/hardDelete/${imageId}`,{
          headers:{
            'auth-token':localStorage.getItem('token')
          }
        }).then((res) => {
          if(res.data.success){
            location.reload()
          }
        })
      } catch (e) {
        console.log(e);
      }}}
  }
</script>

<style scoped>
    .card .content span:first-child {
  margin-bottom: 10px;
  font-weight: 700;
  text-align: center;
  color: var(--darkGreen);
  font-size: 16px;
  }
  .card:hover .image img {
  transform: scale(1) !important;
  cursor: default;
  }
  .card img {
  transform: scale(1) !important;
  }
  .card-text {
    font-size: 0.8em;
    font-weight: 500;
  }
  .card-text2 {
    font-size: 0.85em;
    font-weight: 500;
  }
 .row {
  justify-content: center;
  margin-top: 2em;
 }
 .width28{ 
  width: 28%;
 }
  @media screen and (max-width: 768px) {
  .card {
  width: 100%;
  }
  .card .content {
  bottom: 0;
  }
  }
  @media screen and (max-width: 480px) {
  .card {
  width: 85%;
  }
  .width {
    width: 100%;
  }
  }
</style>