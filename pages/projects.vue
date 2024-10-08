<template>
  <div class="container mx-auto space-y-4 px-4 sm:px-6 lg:px-8">
    <h1 class="text-3xl sm:text-4xl font-bold mb-4 sm:mb-6">
      {{ t("projects.title") }}
    </h1>
    <TransitionGroup name="project-list" tag="div" class="grid gap-4 sm:gap-6">
      <div
        v-for="project in visibleProjects"
        :key="project.id"
        class="project-card bg-white dark:bg-gray-800 rounded-lg shadow-md overflow-hidden p-4 sm:p-6 text-left col-span-full"
      >
        <a
          :href="project.url"
          target="_blank"
          rel="noopener noreferrer"
          class="block h-full relative z-10"
        >
          <h2 class="text-xl sm:text-2xl font-semibold mb-2 sm:mb-4 pr-8">
            {{ t(project.titleKey) }}
          </h2>
          <p class="text-gray-600 dark:text-gray-400 mb-4">
            {{ t(project.descriptionKey) }}
          </p>
          <div class="flex items-center justify-between">
            <div
              class="flex items-center space-x-4 text-sm text-gray-500 dark:text-gray-400"
            >
              <a
                v-for="tool in project.tools"
                :key="tool.name"
                :href="tool.url"
                target="_blank"
                rel="noopener noreferrer"
                class="hover:text-white transition-colors duration-200"
                :title="tool.name"
                @click.stop
              >
                <UIcon :name="tool.icon" class="w-6 h-6" />
              </a>
            </div>
            <a
              :href="project.url"
              target="_blank"
              rel="noopener noreferrer"
              class="text-gray-400 hover:text-white transition-colors duration-200"
              @click.stop
            >
              <UIcon name="i-simple-icons-github" class="w-6 h-6" />
            </a>
          </div>
        </a>
      </div>
      <div
        v-if="showComingSoon"
        :key="'coming-soon'"
        class="project-card coming-soon bg-transparent dark:bg-transparent rounded-lg overflow-hidden text-left col-span-full"
      >
        <div class="border-2 border-dashed border-gray-300 dark:border-gray-700 rounded-lg p-4 sm:p-6 h-full">
          <h2 class="text-xl sm:text-2xl font-semibold mb-2 sm:mb-4 text-gray-500 dark:text-gray-400">
            {{ t("projects.comingSoon") }}
          </h2>
          <p class="text-gray-400 dark:text-gray-500">
            {{ t("projects.comingSoonMessage") }}
          </p>
        </div>
      </div>
    </TransitionGroup>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import { useI18n } from "vue-i18n";

const { t } = useI18n();
const showProjects = ref(false);
const showComingSoon = ref(false);

const projects = [
  {
    id: 1,
    titleKey: "projects.portfolioSite.title",
    descriptionKey: "projects.portfolioSite.description",
    url: "https://github.com/egemenkar/egmn.dev",
    tools: [
      { name: "Vue.js", icon: "i-simple-icons-vuedotjs", url: "https://vuejs.org/" },
      { name: "Nuxt.js", icon: "i-simple-icons-nuxtdotjs", url: "https://nuxt.com/" },
      { name: "Tailwind CSS", icon: "i-simple-icons-tailwindcss", url: "https://tailwindcss.com/" },
      { name: "Vercel", icon: "i-simple-icons-vercel", url: "https://vercel.com/" },
    ],
  },
  // Add more projects here as needed
];

const visibleProjects = computed(() => (showProjects.value ? projects : []));

onMounted(() => {
  setTimeout(() => {
    showProjects.value = true;
  }, 100);

  setTimeout(() => {
    showComingSoon.value = true;
  }, 600); // Delay the appearance of the "Coming Soon" card
});
</script>

<style scoped>
.project-list-enter-active,
.project-list-leave-active {
  transition: all 0.5s ease;
}
.project-list-enter-from,
.project-list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.project-card {
  position: relative;
  transition: all 0.3s ease;
  display: inline-block;
  width: 100%;
  break-inside: avoid;
  cursor: pointer;
}

.project-card::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(255, 255, 255, 0.05);
  opacity: 0;
  transition: opacity 0.3s ease;
  pointer-events: none;
}

.project-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.project-card:hover::after {
  opacity: 1;
}

.coming-soon {
  cursor: default;
  opacity: 0.7;
  transition: all 0.3s ease;
}

.coming-soon:hover {
  transform: none;
  box-shadow: none;
  opacity: 1;
}

.coming-soon::after {
  display: none;
}

.coming-soon > div {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
