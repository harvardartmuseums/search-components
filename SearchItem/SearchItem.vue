<template>
    <div class="w-full flex flex-col justify-center">
      <a :href="`/collections/object/${item.objectid}?position={{position}}`" class="focus:outline-black focus:outline-dashed">
        <figure class="w-full">
            <img v-if="item.imagecount > 0" class="w-full mb-4" :src="`${item.primaryimageurl}?width=280`" :alt="item.images[0].alttext">
            <img v-else alt="No image available at this time." src="data:image/png;base64,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" />
          <!-- Need to add My Collections component! -->
          <figcaption>
            <span class="artwork-grid__item-id text-gray-500">{{item.objectnumber}}</span>
            <h4 class="artwork-grid__item-author  text-lg font-bold">
                <span v-for="(person,index) of item.people" v-bind:key="person.personid">{{person.displayname}}<span v-if="index != item.people.length - 1">, </span>
                </span>
            </h4>
            <h3 class="artwork-grid__item-title text-gray-500 font-bold text-lg">{{item.title}}</h3>
            <span class="artwork-grid__item-classification text-gray-500">{{item.classification}}</span>
          </figcaption>
        </figure>        
      </a>
    </div>
</template>

<script>
    export default {
        props:['item'],
    }
</script>