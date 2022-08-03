<template>
    <nav>
        <ul>
            <li v-for="item in menuList" :key="item.index" class="py-3">
                <a :href="item.link">{{item.name}}</a>
            </li>
        </ul>
    </nav>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IMenuItem from '@/models/menuItem';
import data from '../data';

export default defineComponent({
  data() {
    return {
      menuList: data as IMenuItem[],
    };
  },
});
</script>
