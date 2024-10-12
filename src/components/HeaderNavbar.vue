<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import MobileDetect from 'mobile-detect';
import axios from 'axios';

const isMobile = ref(false); // 用來判斷是否為移動裝置
const langOpen = ref(false);
const searchOpen = ref(false);
const isOpen = ref(false); // 行動裝置選單是否開起
const toggleNav = () => {
  isOpen.value = !isOpen.value;
  if (isOpen.value) {
    searchOpen.value = false;
    langOpen.value = false;
  }
};

const baseUrl = import.meta.env.VITE_API_BASE_URL;
const menuItems = ref([]);
const showSubMenu = ref(null);

const toggleSubMenu = (menuTitle) => {
  // 在移動裝置模式下才使用 click 來控制次選單
  if (isMobile.value && menuTitle) {
    showSubMenu.value = showSubMenu.value === menuTitle ? null : menuTitle;
  }
};

// 檢查設備和視窗大小的函數
const checkDeviceAndWindowSize = () => {
  const UA = window.navigator.userAgent;
  const md = new MobileDetect(UA);
  const { body } = document;

  // 判斷是否為移動設備
  if (md.mobile() || window.innerWidth < 991) {
    body.classList.remove('pc');
    body.classList.add('mb');
    isMobile.value = true;
  } else {
    body.classList.remove('mb');
    body.classList.add('pc');
    isMobile.value = false;
  }
};

onMounted(() => {
  axios.get(`${baseUrl}/header.json`)
    .then((response) => {
      menuItems.value = response.data.menu;
    });

  // 初始檢查
  checkDeviceAndWindowSize();

  // 監聽視窗大小變化
  const resizeObserver = new ResizeObserver(() => {
    checkDeviceAndWindowSize();
  });

  resizeObserver.observe(document.body);

  // 在組件卸載時停止觀察
  onUnmounted(() => {
    resizeObserver.disconnect(); // 使用 onUnmounted 來停止觀察
  });
});

</script>

<template>
  <header class="header">
    <div class="w-container" :class="{'is-open-nav': isOpen}">
      <a href="#" class="nav-brand">
        <img src="../assets/images/logo.svg" />
        <span>Your Company</span>
      </a>

      <!-- Navigation -->
      <nav class="navbar">
        <ul class="menu">
          <li
            v-for="(item, index) in menuItems"
            :key="index"
            :class="{'has-child': item.subMenu && item.subMenu.length > 0}"
          >
            <a
              :href="item.link"
              :title="item.title"
              @click.prevent=
              "item.subMenu && item.subMenu.length > 0 ? toggleSubMenu(item.title) : null"
            >
              {{ item.title }}
            </a>
            <ul v-if=
              "item.subMenu && item.subMenu.length > 0 && (showSubMenu === item.title || !isMobile)"
            >
              <li
                v-for="(subItem, subIndex) in item.subMenu"
                :key="subIndex"
              >
                <a :href="subItem.link" :title="subItem.title">{{ subItem.title }}</a>
              </li>
            </ul>
          </li>
        </ul>
      </nav>

      <!-- nav links -->
      <ul class="nav-links">
        <li>
          <a href="#" class="search-switch"
          :class="{'is-open': searchOpen}" @click.prevent="searchOpen = !searchOpen">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round" class="icon icon-tabler icons-tabler-outline icon-tabler-search" aria-hidden="true"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M10 10m-7 0a7 7 0 1 0 14 0a7 7 0 1 0 -14 0" /><path d="M21 21l-6 -6" /></svg>
          </a>
        </li>
        <li class="lang-switch"
          :class="{'is-open': langOpen}">
          <a href="#" @click.prevent="langOpen = !langOpen">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"  fill="none" stroke="currentColor" stroke-width="1" stroke-linecap="round"  stroke-linejoin="round" class="icon icon-tabler icons-tabler-outline icon-tabler-world"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M3 12a9 9 0 1 0 18 0a9 9 0 0 0 -18 0" /><path d="M3.6 9h16.8" /><path d="M3.6 15h16.8" /><path d="M11.5 3a17 17 0 0 0 0 18" /><path d="M12.5 3a17 17 0 0 1 0 18" /></svg>
          </a>
          <ul class="reset">
            <li><a href="javascript:void(0);" title="繁中">繁中</a></li>
            <li><a href="javascript:void(0);" title="EN">EN</a></li>
          </ul>
        </li>
      </ul>

      <!-- Navigation Switch -->
      <a href="#" class="nav-switch" title="點擊前往主導覽"
      :class="{'is-open': isOpen}" @click.prevent="toggleNav">
        <span></span>
        <span></span>
        <span></span>
      </a>
    </div>

    <!-- search popup -->
    <div class="search-popup" :class="{'is-open': searchOpen}">
      <form action="SearchResult.html" name="form_search" class="search-group">
        <label for="search_keyword" class="search-label">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24"  viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round" class="icon icon-tabler icons-tabler-outline icon-tabler-search" aria-hidden="true"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M10 10m-7 0a7 7 0 1 0 14 0a7 7 0 1 0 -14 0" /><path d="M21 21l-6 -6" /></svg>
          <input type="search" name="search_keyword" class="search-input"
          autocomplete="off" placeholder="搜尋關鍵字..." />
        </label>

        <button type="submit" class="search-send" title="開始搜尋">
          Search
        </button>
      </form>
      <a class="search-close" href="#" title="關閉搜尋"
      @click.prevent="searchOpen = !searchOpen">
        <span class="sr-only">關閉搜尋</span>
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#999999" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round" class="icon icon-tabler icons-tabler-outline icon-tabler-x"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M18 6l-12 12" /><path d="M6 6l12 12" /></svg>
      </a>
    </div>

  </header>
</template>
