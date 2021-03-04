<template>
  <div class="nav-menu"
       :class="active ? 'isActive' : ''"
  >
    <div class="nav-menu__container">
      <div class="nav-menu__header"
           :class="initClass ? 'initialized' : ''"
           @click="closeSubmenu"
      >
        <div class="nav-menu__parentBlock">
          <div v-if="menuBlocks.length > 0"
               class="nav-menu__backSign"
          >
            <div class="nav-menu__backSign__icon" />
          </div>
          <div v-if="menuBlocks.length > 0"
               class="nav-menu__parentText"
          >
            {{ currentBlock.name }}
          </div>
        </div>
        <div class="nav-menu__closeButton"
             @click="setvisibility"
        >
          <div class="nav-menu__closeButton__icon" />
        </div>
      </div>

      <menu-block :currentblock="currentBlock"
                  :showsubmenu="showSubmenu"
                  :initclass="initClass"
      />
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex';
import MenuBlock from './menuBlock.vue';

export default {
  components: {
    'menu-block': MenuBlock,
  },
  props: {
    active: {
      type: Boolean,
    },
    setvisibility: {
      type: Function,
      default: () => {},
    },
  },
  data() {
    return {
      currentBlock: {},
      menuBlocks: [],
      initClass: false,
    };
  },
  computed: mapState({
    menu: (state) => state.the_menu.menu,
  }),
  created() {
    this.currentBlock = {
      url: '/catalog/',
      name: '',
      items: this.menu,
    };
    setTimeout(this.removeInitClass, 100);
  },
  methods: {
    removeInitClass() {
      this.initClass = true;
    },
    showSubmenu(item) {
      const parentUrl = `${this.currentBlock.url}${item.slug}/`;
      this.initClass = false;
      this.menuBlocks.push(this.currentBlock);
      this.currentBlock = {
        url: parentUrl,
        name: item.name,
        items: item.submenu,
      };
      setTimeout(this.removeInitClass, 100);
    },
    closeSubmenu() {
      if (this.menuBlocks.length === 0) return;
      this.initClass = false;
      this.currentBlock = this.menuBlocks.pop();
      setTimeout(this.removeInitClass, 100);
    },
  },
};
</script>

<style lang="scss" scoped>
.nav-menu {
  background-color: transparent;
  color: #fff;
  position: absolute;
  width: 100%;
  height: 100vh;
  top: 85px;
  z-index: 1;
  border-top: 1px solid #ccc;
  transition: background-color .3s;
}

.nav-menu__container {
  margin-left: 100%;
  width: 100%;
  background-color: #8200ff;
  transition: margin .3s;
  padding-bottom: 20px;
}

.nav-menu__header {
  padding: 20px 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-weight: bold;
  background-color: #8200ff;

  a {
    color: inherit;
  }
}

.nav-menu__header.initialized {
  .nav-menu__parentText {
    margin-left: 10px;
    opacity: 1;
    transition: margin .3s;
  }
}

.nav-menu__parentBlock {
  display: flex;
  align-items: center;
}

.nav-menu__parentText {
  margin-left: 5px;
}

.nav-menu__closeButton {
  width: 20px;
  height: 20px;
  position: relative;

  &__icon {
    position: absolute;
    top: 0;
    left: 0;
  }

  &__icon:before {
    content: '';
    position: absolute;
    top: 0;
    left: 5px;
    width: 2px;
    height: 20px;
    background-color: #fff;
    transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
  }

  &__icon:after {
    content: '';
    position: absolute;
    top: 0;
    left: 5px;
    width: 2px;
    height: 20px;
    background-color: #fff;
    transform: rotate(-45deg);
    -webkit-transform: rotate(-45deg);
  }
}

.nav-menu__backSign {
  width: 20px;
  height: 20px;
  position: relative;

  &__icon {
    position: absolute;
    top: 3px;
    border: solid #fff;
    border-width: 0 0 2px 2px;
    display: inline-block;
    padding: 5px;
    transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
  }
}

.nav-menu.isActive {
  background-color: rgba(145,145,145,0.5);

  .nav-menu__container {
    margin-left: 0;
  }
}

@media (min-width: 768px) {
  .nav-menu {
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;
  }

  .nav-menu__container {
    width: 45%;
    max-width: 400px;
    height: 100%;
    margin-left: -400px;
  }

  .nav-menu__header {
    padding: 30px;
  }
}
</style>
