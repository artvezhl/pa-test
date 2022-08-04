<template>
  <header
    @mouseenter="() => onHoverMenu(true)"
    @focus="() => menuHoverHandler(true)"
    @mouseleave="() => onHoverMenu(false)"
    @blur="() => menuHoverHandler(false)"
    class="p-4 md:py-1 fixed top-0 left-0 right-0 bg-white"
  >
    <div
      class="flex justify-between items-center pt-2 pb-6 md:pb-1 md:justify-start"
      :style="{
        height: isMenuOpen ? '120px' : '',
      }"
    >
      <h1>ОЛИМПИК КЛИНИК</h1>
      <app-input class="hidden md:block md:mx-auto lg:w-2/5 md:w-2/6" />
      <app-buttons-menu class="hidden md:block md:ml-auto py-0" />
      <app-menu-button @handle-menu="onHandleMenu" />
    </div>
    <div class="menu__list" v-show="isMenuOpen">
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
      pageHeight: 0 as number,
      positionTop: 0 as number,
    };
  },
  methods: {
    onHandleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
    onHoverMenu(isOpen: boolean) {
      if (this.innerWidth >= 768) this.isMenuOpen = isOpen;
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
  watch: {
    // (innerWidth >=768 && positionTop < 40)
    positionTop() {
      if (this.innerWidth >= 768 && this.positionTop < 40) {
        this.isMenuOpen = true;
      } else {
        this.isMenuOpen = false;
      }
    },
  },
});
</script>

<style>
.menu__list {
  transform: translateY(-180%);
  animation: ani .3s forwards;
}
@keyframes ani {
  0% {transform: translateY(-150%);}
  100% {transform: translateY(0);}
}
</style>
