<script setup>
  import { ref, watchEffect , reactive} from 'vue';
  import { useRoute } from 'vue-router';
  import BackIcon from './icons/backIcon.vue';
  import BranchIcon from './icons/branchIcon.vue';
  import ForkIcon from './icons/forkIcon.vue';
  import StarIcon from './icons/starIcon.vue';
  import WatchIcon from './icons/watchIcon.vue';
  import DetailsSkeleton from './DetailsSkeleton.vue';

  const route = useRoute();

  const details = reactive({
  name: '',
  stargazers_count: 0,
  watchers: 0,
  forks: 0,
  language: null,
  full_name: '',
});

  const username = ref('patiee-lyop92')
  const branches = ref([]);
  const deployments = ref([]);
  const loading = ref(false);

  const fetchData = () => {
  loading.value = true;
    // fetch repo details
  fetch(`https://api.github.com/repos/${username.value}/${route.params.id}`, {
    headers: {
        Accept: "application/json",
  
    },
  })
    .then((res) => {
        return res.json()
    })
    .then((data) => {
        details.name = data.name;
        details.stargazers_count = data.stargazers_count;
        details.watchers = data.watchers;
        details.forks = data.forks;
        details.language = data.language;
        details.full_name = data.full_name;
        loading.value = false;
    })
    .catch((error) => {
        console.error('Error fetching repo details:', error);
        loading.value = false;
    });
    // fetch branches
   fetch(`https://api.github.com/repos/${username.value}/${route.params.id}/branches`, {
    headers: {
        Accept: "application/json",
  
    },
   })
    .then((res) => res.json())
    .then((data) => (branches.value = data));
   // fetch deployments
   fetch(`https://api.github.com/repos/${username.value}/${route.params.id}/deployments`, {
    headers: {
        Accept: "application/json",
  
    },
   })
    .then((res) => res.json())
    .then((data) => (deployments.value = data)); 
};

  watchEffect(() => {
    fetchData();
  });
 </script>

 
<template>
    <div id="repodetail">
        <router-link :to="`/`"><button class="back"><BackIcon/> back</button></router-link>
        <DetailsSkeleton v-if="loading"/>
        <div v-else class="repodetail-card">
            <h2 class="repo-name">{{ details.name }}</h2>
            <div class="repo-mini-details">
                <p><StarIcon /> Stars: {{ details.stargazers_count }}</p>
                <p><WatchIcon /> Watch: {{ details.watchers }}</p>
                <p><ForkIcon /> Forks: {{ details.forks }}</p>
                <p><BranchIcon /> Branches: {{ branches.length }}</p>
            </div>
            <p v-if="details.language === null">Main Language: Not Specified</p>
            <p v-else>Main Language: {{ details.language }}</p>
            <p v-if="deployments.length === 0">Live site: none</p>
            <p v-else>Live site: <a :href="`https://akbenngold.github.io/${details.name}`">akbenngold.github.io/{{ details.name }}</a></p>
            <p><a :href="`https://github.com/${details.full_name}`">View on Github</a></p>
        </div>
    </div>
  
</template>

<style>

.repo-container {
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(300px,1fr));
    grid-gap: 30px;
    width: 90%;
    margin: 0 auto;
    margin-bottom: 5rem;
}


.repodetail-card {
    border: 1px solid #ac2d2d;
    padding: 13px;
    border-radius: 5px;
}


.repo-name {
    color: #0f1136;
    font-size: 2rem;
    word-break: break-word;
}

#repodetail {
    width: 90%;
    max-width: 620px;
    margin: 0 auto;
    margin-bottom: 50px;
}

#repodetail p {
    padding: 15px 0;
    word-break: break-all;
}

#repodetail a {
    color: #ac2d2d;
}

.repo-mini-details {
    gap: 10px 50px;
    margin: 0 0 15px;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
}

.repo-mini-details  p {
    display: flex;
    align-items: center;
    gap: 0.3rem;
}

.icons {
    color: #ac2d2d;
}

.back {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin-bottom: 2rem;
    color: #ac2d2d;
}
</style>
