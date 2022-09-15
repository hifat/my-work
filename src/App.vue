<script setup>
import { ref } from 'vue'
import contentWorks from './content/works'

const works = ref(contentWorks)

function changeSlide(workID, n) {
   const work = works.value.filter((work) => work.id === workID)[0]

   let img_index = 0
   work.images.filter((image, index) => {
      if (image.active === true) {
         // Prevent when index -1
         if (index + n >= 0 && index + n < work.images.length) {
            img_index = (index + n) % work.images.length
         }
      }
   })

   activeSlide(workID, work.images[img_index].id)
}

function activeSlide(workID, imgID) {
   // Find work by workID
   const work = works.value.filter((work) => work.id === workID)[0]

   // Change image active in work
   work.images.map((image, index) => {
      if (image.id == imgID) {
         work.images[index].active = true
      } else {
         work.images[index].active = false
      }
   })
}
</script>

<template>
   <section class="card mx-auto max-w-3xl">
      <h2 class="text-4xl text-center tracking-wide font-extrabold leading-loose mb-2">My Work</h2>
      <p class="my-10 indent-10">Hi My name is Noei. I am a new graduate. Who is passionate about studying web technologies and spends his free time practicing web development skills</p>
      <div v-for="work in works" :key="work.id" class="mb-20">
         <h3 class="text-2xl font-semibold mb-6">{{ work.title }}</h3>
         <div class="shadow-md relative">
            <!-- large image on slides -->
            <div v-for="image in work.images" :class="`mySlides hidden`" :style="{ display: `${image.active ? 'block' : 'none'}` }">
               <div class="image1 w-full object-cover" :style="{ content: `url('${image.link}')` }"></div>
            </div>

            <!-- butttons -->
            <a class="absolute left-0 inset-y-0 flex items-center -mt-32 px-4 text-white hover:text-gray-800 cursor-pointer text-3xl font-extrabold" @click="changeSlide(work.id, -1)">❮</a>
            <a class="absolute right-0 inset-y-0 flex items-center -mt-32 px-4 text-white hover:text-gray-800 cursor-pointer text-3xl font-extrabold" @click="changeSlide(work.id, 1)">❯</a>

            <!-- smaller images under description -->
            <div class="flex">
               <div v-for="(image, index_img) in work.images" :key="`wd_${index_img}`">
                  <img
                     :class="`image1 description h-24 hover:opacity-100 cursor-pointer object-cover ${image.active ? 'opacity-100' : 'opacity-50'}`"
                     :src="image.link"
                     @click="activeSlide(work.id, image.id)"
                     alt="Dog's Nose"
                  />
               </div>
            </div>
         </div>
         <p class="text-lg mt-10 text-justify indent-10">{{ work.detail }}</p>
         <p class="text-lg text-gray-400">Technologies: {{ work.technologies }}</p>
         <div class="text-lg mt-4">
            Repository:
            <a v-if="work.repo" :href="work.repo" target="_blank"> {{ work.repo }}</a>
            <span v-else>private</span>
         </div>
      </div>
   </section>
</template>
