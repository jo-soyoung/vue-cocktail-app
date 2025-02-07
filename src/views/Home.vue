<template>
  <main class="home">
    <Search v-if="userId" msg="Search" @submit="onSearch" />
    <Login v-else msg="Sign in" @login="onLogin" />
    <section v-if="userId && cocktailList">
      <SearchList :cocktail-list="cocktailList" />
    </section>
  </main>
</template>

<script>
import { defineComponent, ref } from '@vue/composition-api'

import authService from '@/service/auth_service'
import cocktail from '@/service/cocktail_server'

export default defineComponent({
  name: 'Home',
  components: {
    Login: () => import('@/components/Login.vue'),
    Search: () => import('@/components/Search.vue'),
    SearchList: () => import('@/components/SearchList.vue')
  },
  setup() {
    const userId = ref(localStorage.getItem('userId'))
    const cocktailList = ref(JSON.parse(localStorage.getItem('cocktailList')))

    function saveUserId(userId) {
      localStorage.setItem('userId', JSON.stringify(userId))
    }

    function saveCocktailList(cocktailList) {
      localStorage.setItem('cocktailList', JSON.stringify(cocktailList))
    }

    function onLogin(title) {
      authService
        .login(title)
        .then((data) => {
          userId.value = data.user.uid
          saveUserId(userId.value)
        })
    }

    function onSearch(query) {
      cocktail
        .searchByName(query)
        .then(cocktails => {
          cocktailList.value = cocktails
          saveCocktailList(cocktailList.value)
        })
    }

    return {
      userId,
      cocktailList,
      onLogin,
      onSearch
    }
  }
})
</script>

<style lang="scss">
@import "@/scss/mixins";
main {
  width: 100%;
  height: calc(100vh - 120px);
}

.home {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  overflow-y: auto;
}

@include tablet {
  .home {
    position: relative;
    display: flex;
    justify-content: flex-end;
    padding: 2rem 0;
  }
}

@include desktop {
  .home {
    position: relative;
    justify-content: flex-end;
    padding: 2rem 0;
  }
}
</style>
