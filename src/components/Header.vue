<script setup>
import { ref } from 'vue';
import { useUserStore } from '../stores/UserStore';
import { useRouter } from 'vue-router';
import SearchComponent from './Search.vue';

const router = useRouter();
const userStore = useUserStore();

const darkModeVal = ref(false)
function darkModeSwitch() {
  darkModeVal.value = !darkModeVal.value
  let html = document.documentElement;
  html.setAttribute('data-bs-theme', html.getAttribute('data-bs-theme') == 'dark' ? '' : 'dark');
}

function logout() {
  helpers.setCookieY('api_token', 'value', -1, '/');
  userStore.$reset();
  sessionStorage.toastMsg = "You have successfully loged out!"
  userStore.recentTriggerToast = Date.now()
  router.push('/');
}
function settings() {
  router.push('/settings')
}
</script>

<template>
  <nav class="border-bottom nav-bar zindex-fixed" id="myNav">
    <div class="ps-3 pe-3 position-relative">
      <div class="row">
        <div class="col-12 col-md-3 d-flex align-items-center">
          <router-link to="/" class="text-decoration-none">
            <h1 class="m-0">NVN</h1>
          </router-link>
        </div>
        <div class="col-12 col-md-6 d-flex align-items-center">
          <SearchComponent />
        </div>
        <div class="col-12 col-md-3 d-flex justify-content-end align-items-center">
          <div class="d-flex flex-column justify-content-center" v-if="userStore.isLoggedIn" id="header-user-info">
            <div class="dropdown">
              <img :src="userStore.avatar" alt="avatar" width="40" height="40"
                role="button" id="accountDropdownMenuButton" data-bs-toggle="dropdown" aria-expanded="false" data-bs-auto-close="outside"
                class="me-2 rounded-circle" ref="avatarRef">
              <ul class="dropdown-menu" id="accountDropdownMenu" aria-labelledby="accountDropdownMenuButton">
                <li class="dropdown-item d-flex align-items-center" @click.stop="darkModeSwitch" role="button">
                  <i class="bi bi-moon fs-4 me-3"></i>
                  <div class="d-flex justify-content-between align-items-center flex-grow-1">
                    <div class="text-capitalize">dark mode</div>
                    <div class="form-check form-switch">
                      <input class="form-check-input switch-btn" type="checkbox" id="flexSwitchCheckDefault"
                        v-model="darkModeVal" role="button">
                    </div>
                  </div>
                </li>
                <li class="dropdown-item text-capitalize d-flex align-items-center" role="button" @click="settings">
                  <i class="bi bi-gear fs-4 me-3"></i>
                  <span>settings</span>
                </li>
                <li class="dropdown-item text-capitalize d-flex align-items-center" role="button" @click="logout">
                  <i class="bi bi-box-arrow-right fs-4 me-3"></i>
                  <span>logout</span>
                </li>
              </ul>
            </div>
          </div>
          <div v-if="!userStore.isLoggedIn" class="d-flex align-items-center">
            <router-link to="/login" class="btn btn-primary rounded-pill text-capitalize me-2">login</router-link>
            <div class="dropdown">
              <i class="bi bi-three-dots-vertical setting-btn" id="settingsDropdownMenuButton" role="button"
                title="settings" data-bs-toggle="dropdown" aria-expanded="false"
                data-bs-auto-close="outside"></i>
              <ul class="dropdown-menu" id="settingsDropdownMenu" aria-labelledby="settingsDropdownMenuButton">
                <li class="dropdown-item" @click.stop="darkModeSwitch" role="button">
                  <div class="d-flex justify-content-between align-items-center">
                    <div class="text-capitalize">dark mode</div>
                    <div class="form-check form-switch">
                      <input class="form-check-input switch-btn" type="checkbox" id="flexSwitchCheckDefault"
                        v-model="darkModeVal" role="button">
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.nav-bar {
  position: fixed;
  left: 0;
  width: 100%;
  background-color: white;
  padding-top: 5px;
  padding-bottom: 5px;
}

.search-bar-ctn {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.setting-btn {
  font-size: 1.25rem;
}

.form-check {
  margin-bottom: 0
}

.switch-btn {
  width: 40px;
  height: 24px;
  margin: 0;
}

#settingsDropdownMenu,
#accountDropdownMenu {
  width: 250px;
}

@media only screen and (min-width: 768px) {
  .search-input {
    width: 50%;
  }
}

[data-bs-theme=dark] .nav-bar {
  background-color: var(--bs-body-bg) !important;
}
</style>