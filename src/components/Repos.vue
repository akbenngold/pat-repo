<script setup>
import nextIcon from './icons/nextIcon.vue';
import previousIcon from './icons/previousIcon.vue';
import Skeleton from './Skeleton.vue';
import { onMounted, ref, computed, reactive } from 'vue';
import { useRouter} from 'vue-router';

const github = reactive({
  repoData: [],
  page: 1,
  view: "",
  currentPage: 1,
  loading: false,
  perPage: 6,
  skeleton: [...new Array(6)],
});

const username = ref('Patiee-Lyop92'); 

const userSearch = ref("");
const filteredLanguage = ref('')
const router = useRouter();

const fetchRepos = () => {
  github.loading = true;
  fetch(`https://api.github.com/users/${username.value}/repos`, {
    headers: {
      Accept: "application/json"
    },
  })
  .then((res) => res.json())
  .then((data) => {
    github.repoData = data;
    github.loading = false;
  })
  .catch((error) => {
    console.error('Error fetching repositories:', error);
    github.loading = false;
  });
};

// pagination
const prevPage = () => {
  if (github.currentPage > 1) {
       github.currentPage--;
  }
};

const nextPage = () => {
  if (github.currentPage < lastPage.value) {
       github.currentPage++
  }
};

const showMore = computed(() => {
  const start = (github.currentPage - 1) * github.perPage;
  const end = start + github.perPage;
  // github.loading = false;
  return filteredUser.value.slice(start, end);
});

const lastPage = computed(() => {
  const length = filteredUser.value.length;
  return Math.ceil(length / github.perPage);
});

// for search feature
const handleSearch = () => {
  if (userSearch.value && filteredUser.value.length === 0) {
    router.push('/404');
  }
};

// for filter feature
const handleFilter = (event) => {
  filteredLanguage.value = event.target.value;
};

const filteredUser = computed(() => {
  return github.repoData.filter(repo => {
    return (
      repo.name.toLowerCase().includes(userSearch.value.toLowerCase()) &&
      (filteredLanguage.value === '' || repo.language === filteredLanguage.value)
    );
  });
});

onMounted(() => {
  fetchRepos();
});
</script>

<template>
  <div>
    <section class="select-input">
      <div class="input-container">
        <input
          type="text"
          placeholder="Search by repository name"
          v-model="userSearch"
          @input="handleSearch"
        />
        <select v-model="filteredLanguage" @change="handleFilter">
          <option value="">All Languages</option>
          <option value="JavaScript">JavaScript</option>
          <option value="CSS">CSS</option>
          <option value="HTML">HTML</option>
        </select>
      </div>
    </section>
    <div class="repo-container">
      <template v-if="github.loading">
        <Skeleton v-for="(n, index) in github.skeleton" :key="index">{{ github.skeleton }}</Skeleton>
      </template>
      <div v-else v-for="repo in showMore" class="repo-card" :key="repo.id">
        <router-link :to="`/details/${repo.name}`"><h2 class="repo-name">{{ repo.name }}</h2></router-link>
        <p class="language">Language: {{ repo.language }}</p>
        <p class="date">Start date & time: {{ repo.created_at }}</p>
        <p class="visibility">Visibility: {{ repo.visibility }}</p>
      </div>
    </div>
    <div class="pagination">
      <!-- Adjusted class binding for disabled state -->
      <button class="view-more" :class="{ 'disabled': github.currentPage === 1 }" @click="prevPage"><previousIcon class="pagination-icon" /></button>
      <p class="current-page">{{ github.currentPage }}</p>
      <!-- Adjusted class binding for disabled state -->
      <button class="view-more" :class="{ 'disabled': github.currentPage === lastPage.value }" @click="nextPage"><nextIcon class="pagination-icon" /></button>
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
    min-height: 100vh;
}

.repo-card,
.repodetail-card {
    border: 1px solid #ac2d2d;
    padding: 13px;
    border-radius: 5px;
}


.repo-name {
    color: #ac2d2d;
    font-size: 2rem;
    word-break: break-word;
}

p {
    padding: 5px 0;
}

.pagination {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    margin: 3rem 0;
}

button {
    background: none;
    border: none;
    cursor: pointer;
}

.select-input {
  display: flex;
  justify-content: center;
  align-items: center;
}

.input-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 80%;
  max-width: 400px;
  margin-bottom: 15px;
}

.input-container input,
.input-container select {
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  /* width: 100%; */
}

select {
    border: none;
    border-radius: 5px;
    padding: 0 10px;
    color: #ac2d2d;
}

@media (min-width: 600px) {
  .input-container {
    flex-direction: row;
  }

  input {
    width: fit-content
  }
}

</style>