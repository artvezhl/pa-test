<template>
  <header
    @mouseenter="isMenuOpen = true"
    @focus="() => menuHoverHandler(true)"
    @mouseleave="isMenuOpen = false"
    @blur="() => menuHoverHandler(false)"
    class="p-4 fixed top-0 left-0 right-0 bg-white"
  >
    <div class="flex justify-between items-center pt-2 pb-6 md:justify-start">
      <h1>ОЛИМПИК КЛИНИК</h1>
      <app-input class="hidden md:block md:mx-auto lg:w-2/5 md:w-2/6" />
      <app-buttons-menu class="hidden md:block md:ml-auto py-0" />
      <app-menu-button @handle-menu="onHandleMenu" />
    </div>
    <div v-show="isMenuOpen || (innerWidth >=768 && positionTop < 40)">
      <app-input class="block md:hidden" />
      <app-nav-list />
      <app-buttons-menu class="block md:hidden" />
    </div>
  </header>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import MenuButton from '@/components/UI/MenuButton.vue';
import SearchInput from './UI/SearchInput.vue';
import NavList from './NavList.vue';
import ButtonsMenu from './ButtonsMenu.vue';

export default defineComponent({
  components: {
    'app-menu-button': MenuButton,
    'app-input': SearchInput,
    'app-nav-list': NavList,
    'app-buttons-menu': ButtonsMenu,
  },
  data() {
    return {
      isMenuOpen: false,
      isMobile: false,
      innerWidth: 0 as number,
      pageHeight: 0,
      positionTop: 0,
    };
  },
  methods: {
    onHandleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
    menuHoverHandler(isOpen: boolean) {
      if (this.isMenuOpen !== isOpen) this.isMenuOpen = isOpen;
    },
    updateWidth() {
      this.innerWidth = window.innerWidth;
    },
    updatePosition() {
      this.positionTop = (window.pageYOffset !== undefined)
        ? window.pageYOffset
        : (document.documentElement || document.body.parentNode || document.body).scrollTop;
    },
  },
  created() {
    this.positionTop = (window.pageYOffset !== undefined)
      ? window.pageYOffset
      : (document.documentElement || document.body.parentNode || document.body).scrollTop;
    window.addEventListener('resize', this.updateWidth);
    window.addEventListener('scroll', this.updatePosition);
    this.pageHeight = document.documentElement.scrollHeight;
  },
  mounted() {
    this.innerWidth = window.innerWidth;
    this.isMobile = this.innerWidth < 768;
  },
});
</script>
