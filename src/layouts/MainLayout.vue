<template>
  <q-layout view="lhh lpr fFf">
    <q-header elevated class="text-white">
      <q-toolbar class="bg-dark">
        <q-btn
          flat
          dense
          round
          :icon="menuBtn"
          aria-label="Menu"
          @click="toggleLeftDrawer"
          v-show="$q.screen.lt.md"
          class="menu-btn"
        />

        <div
          class="flex genders-head"
          v-show="$q.screen.md || $q.screen.lg || $q.screen.xl"
        >
          <q-btn :to="{ name: 'donna' }">DONNA</q-btn>
          <q-separator vertical inset color="white" />
          <q-btn :to="{ name: 'uomo' }">UOMO</q-btn>
        </div>

        <q-toolbar-title align="center" class="title-head ellipsis-2-lines">
          <router-link :to="{ name: 'index' }" class="rout-link">
            {{ title }}
          </router-link>
        </q-toolbar-title>

        <!--ICONS-->
        <div class="icons-wrapper">
          <q-icon :name="searchIcon" class="cursor-pointer icon"></q-icon>
          <q-icon :name="heartIcon" class="cursor-pointer icon"></q-icon>
          <q-icon :name="personIcon" class="cursor-pointer icon"></q-icon>
          <q-icon :name="shopIcon" class="cursor-pointer icon"></q-icon>
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" overlay behavior="mobile" elevated>
      <q-btn-group spread flat class="genders-drawer">
        <q-btn>DONNA</q-btn>
        <q-btn>UOMO</q-btn>
      </q-btn-group>

      <q-list>
        <EssentialLink
          v-for="link in linksList"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>

  <q-resize-observer @resize="onResize" />
</template>

<script setup lang="ts">
//const and components
import {
  matSearch,
  matFavorite,
  matPerson,
  matShoppingBag,
  matMenu,
} from '@quasar/extras/material-icons';
import { defineComponent, ref } from 'vue';
import EssentialLink from 'components/EssentialLink.vue';
import { useQuasar } from 'quasar';

const linksList = [
  {
    title: 'Abbigliamento',
    link: '/Abbigliamento',
  },
  {
    title: 'Scarpe',
    link: '/Scarpe',
  },
  {
    title: 'Accessori',
    link: '/Accessori',
  },
];

const leftDrawerOpen = ref(false);
let title = ref('MONKEY WEAR');
const $q = useQuasar();

//icons
const menuBtn = matMenu;
const searchIcon = matSearch;
const heartIcon = matFavorite;
const personIcon = matPerson;
const shopIcon = matShoppingBag;

defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink,
  },
});

//functions
function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}

if ($q.platform.is.mobile) {
  title.value = 'MW';
}

function onResize(size: { height: number; width: number }) {
  if (size.width < 500) {
    title.value = 'MW';
  } else {
    title.value = 'MONKEYWEAR';
  }
}
</script>

<style scoped>
.title-head {
  user-select: none;
  font-size: 250%;
  font-weight: bold;
}

.genders-drawer {
  border-bottom: 1px solid rgb(0, 0, 0, 0.5);
  box-shadow: 0 2px 5px rgb(0, 0, 0, 0.2);
}
.menu-btn {
  margin-left: 0;
}

.icon {
  padding: 10px;
  height: 25px;
  width: 25px;
}

.rout-link {
  text-decoration: none;
  color: inherit;
  border: none;
}

@media only screen and (max-width: 700px) {
  .icon {
    padding: 5px;
    height: 20px;
    width: 20px;
  }
}
</style>
