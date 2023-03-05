
<template>
  <section>
    <div
      class="relative pt-16 pb-32 flex content-center items-center justify-center"
      style="min-height: 75vh"
    >
      <div
        class="absolute top-0 w-full h-full bg-center bg-cover"
        style="background-color: #bbd0ff"
      >
        <span
          
          class="w-full h-full absolute opacity-75"
        ></span>
      </div>
      <div class="container relative mx-auto">
        <div class="items-center flex flex-wrap">
          <div class="w-full lg:w-6/12 px-4 ml-auto mr-auto text-center">
            <div>
              <h1 class="text-dark font-semibold text-5xl">
                Portfolio de Lorenzo
              </h1>
              <p class="mt-4 text-lg text-dark-300">
                Voici un portfolio réalisé avec Strapi et Nuxt.js pour présenter
                mes projets.
              </p>
            </div>
          </div>
        </div>
      </div>

    </div>
    <section class="background-color">
      <div v-if="types" class=" mb-10 flex justify-center flex-wrap">
        <button
          @click="filterProjects('all')"
          class="button-filter"
        >
          all
        </button>
        <button
          v-for="type in types"
          :key="type"
          @click="filterProjects(type)"
          class="button-filter"
        >
          {{ type }}
        </button>
      </div>
        <div v-if="projets" class="flex flex-wrap justify-center container m-auto">
          <nuxt-link
            :to="`/projets/${projet.slug}`"
            v-for="projet in filteredProjects"
            :key="projet.slug"
          >
            <div
              class="max-w-xs h-96 flex flex-col items-center m-6 p-6 drop-shadow-2xl bg-[#ffd6ff] transform hover:scale-105 transition duration-300 ease-in-out"
            >
            <div class="font-bold text-xl mb-2">{{ projet.name }}</div>
              <p class="text-gray-700 text-base">
                {{ projet.description }}
              </p>
              <div class="px-6 pt-4 pb-2 flex flex-row">
                <div v-for="(technologie, index) in projet.technologies" :key="index" class="pt-4 pb-2">
                  <span class="inline-block bg-[#b8c0ff] rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">#{{technologie.name}}</span>
                </div>
              </div>
            </div>
          </nuxt-link>
        </div>

      
    </section>
  </section>
</template>



<script setup>
const { find } = useStrapi();
const projets = ref();
const types = ref([]);
const activeFilter = ref("all");

const filterProjects = (type) => {
  activeFilter.value = type;
};

const filteredProjects = computed(() => {
  if (activeFilter.value === "all") {
    return projets.value.data;
  }
  return projets.value.data.filter(
    (projet) => projet.type === activeFilter.value
  );
});

onMounted(async () => {
  projets.value = await find("projets", {
    populate: "deep",
    sort: "id:asc",
  });
  types.value = new Set(projets.value.data.map((projet) => projet.type));
});
</script>


<style scoped>

.background-color {
  background-color:#bbd0ff;
}

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
