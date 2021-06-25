<template>
    <div class="w-full flex flex-col justify-center">
        <figure class="w-full">
          <a v-if="item.imagecount > 0" :href="`/collections/object/${item.objectid}?position={{position}}`">
            <img class="w-full mb-4" :src="`${item.primaryimageurl}?width=280`" :alt="item.images[0].alttext">
          </a>
          <a v-else :href="`/collections/object/${item.objectid}?position={{position}}`">
            <img src="/assets/no_image_thumb.png" alt="No image available at this time.">
          </a>
          

          <!-- Need to add My Collections component! -->
        </figure>
          <a :href="`/collections/object/${item.objectid}?position={{position}}`">
        <span class="artwork-grid__item-id text-gray-500">{{item.objectnumber}}</span>
        
        <h4 class="artwork-grid__item-author  text-lg font-bold">
            <span v-for="(person,index) of item.people" v-bind:key="person.personid">{{person.displayname}}<span v-if="index != item.people.length - 1">, </span>
            </span>
        </h4>
        
        <h3 class="artwork-grid__item-title text-gray-500 font-bold text-lg">{{item.title}}</h3>
        
        <span class="artwork-grid__item-classification text-gray-500">{{item.classification}}</span>
      </a>
    </div>
</template>

<script>
    export default {
        props:['item'],

    }
</script>