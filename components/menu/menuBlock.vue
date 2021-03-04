<template>
  <ul class="nav-menu__list">
    <li v-for="(item, index) in currentblock.items"
        :key="index"
        class="nav-menu__item"
        :class="initclass ? 'initialized' : ''"
        @click="updateMenuBlock(item)"
    >
      <a :href="currentblock.url + item.slug + '/'"
         class="nav-menu__text"
      >
        {{ item.name }}
      </a>
      <div v-if="item.submenu"
           class="nav-menu__expandSign"
      >
        <div class="nav-menu__expandSign__icon" />
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    currentblock: {
      type: Object,
      default: () => {},
    },
    showsubmenu: {
      type: Function,
      default: () => {},
    },
    initclass: {
      type: Boolean,
    },
  },
  methods: {

    updateMenuBlock(item) {
      if (!item.submenu) return;
      this.showsubmenu(item);
    },
  },
};
</script>

<style lang="scss" scoped>
$items: 13;
$transition-delay: .02s;

.nav-menu__list {
  padding: 0;
}

.nav-menu__item {
  padding: 12px 30px;
  padding-left: 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  opacity: 0;
  position: relative;

  a {
    color: inherit;
  }

  @media (max-width: 767px) {
    &:hover {
      background-color: #fff;
      color: #8200ff;

      .nav-menu__expandSign__icon {
        border: solid #8200ff;
        border-width: 0 2px 2px 0;
      }
    }
  }
}

.nav-menu__item.initialized {
  padding-left: 30px;
  opacity: 1;
  transition: padding .3s, opacity .3s, background-color .3s, color .3s;

  @for $i from 1 through $items {
    &:nth-child(#{$i}) {
      $delay: ($i - 1) * $transition-delay;
      transition-delay: $delay;
    }
  }
}

.nav-menu__expandSign {
  width: 20px;
  height: 20px;
  margin-left: 20px;
  position: relative;

  &__icon {
    position: absolute;
    top: 2px;
    border: solid #fff;
    border-width: 0 2px 2px 0;
    display: inline-block;
    padding: 5px;
    transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
  }
}

@media (min-width: 768px) {
  .nav-menu__item {
    &:after {
      content: '';
      width: 0;
      height: 2px;
      background-color: #fff;
      transition: width .3s;
      position: absolute;
      bottom: 0;
      left: 30px;
    }

    &:hover {
      &:after {
        width: 60%;
      }
    }
  }
}
</style>
