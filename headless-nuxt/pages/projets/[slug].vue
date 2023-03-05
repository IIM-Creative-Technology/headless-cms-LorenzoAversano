<template>
<section v-if="projet" class="relative py-20 bg-[#bbd0ff]">
  <div class="container mx-auto px-4">
    <div class="items-center flex flex-wrap flex-col">
      <div class="w-full md:w-4/12 ml-auto mr-auto px-4">
        <img alt="ecommerce" class="max-w-full rounded-lg shadow-lg" v-if="projet.image[0].url" :src="(projet.image[0].url)">
      </div>
      <div class="w-full md:w-5/12 ml-auto mr-auto px-4">
        <div class="mt-10">
          <h3 class="text-3xl font-semibold">{{ projet.name }}</h3>
          <p class="mt-4 text-lg leading-relaxed text-gray-600">
            {{ projet.description }}
          </p>
          <ul class="list-none mt-6">
            <li class="py-2">
              <div class="flex items-center justify-center">
                <div v-for="(technologie, index) in projet.technologies" :key="index" class="flex flex-col items-center">
                  <img alt="ecommerce" class="w-12 h-12 inline-block py-1 px-2 " v-if="technologie.image[0].url" :src="(technologie.image[0].url)">
                  <h4 class="text-gray-600 text-xs">
                    {{technologie.name}}
                  </h4>
                </div>
              </div>
            </li>
          </ul>
          <div class="flex items-center justify-between" style="display: flex; justify-content:center;">
            <a :href="('/')" class="button-filter" style="margin-top: 20px">
                Revenir aux projets
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
</template>


<script setup>
const { findOne } = useStrapi();
const route = useRoute();
const projet = ref();

onMounted(async () => {
    projet.value = await findOne(
        `projets?filters[slug]=${route.params.slug}&populate=deep`
    );
    projet.value = projet.value.data[0];
    console.log({val : projet.value })
});
</script>

<style scoped>

.button-filter {
  font-size: 18px;
  padding: 10px 20px;
  background: #b8c0ff;
  text-transform: capitalize;
  margin-right: 10px;
  border-radius: 10px;
}
.button-filter:hover {
  background: #ffd6ffc4;
  transition: 0.3s;
  color: white;
}


</style>
