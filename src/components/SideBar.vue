<template>
  <transition name="side-bar">
    <div id="side-bar" v-if="showSideBar" v-bind:style="sideBarStyle">
      <aside>
        <section class="side-bar-header">
          <slot name="info"></slot>
        </section>
        <!-- Menu items-->
        <menu-item
          :menu-items="sideBarItems"
          @toggle-side-bar="$emit('toggle-side-bar')"
        />
        <section class="is-hidden-desktop">
          <hr />
          <ul class="menu-list">
            <li v-for="navBarItem in navBarItems" :key="navBarItem.name">
              <router-link
                :to="navBarItem.route"
                @click.native="performAction(navBarItem.action)"
              >
                <div class="is-clearfix" style="width: 100%">
                  <div class="is-pulled-left" style="width: 14%">
                    <span class="icon">
                      <i :class="navBarItem.icon" size="lg"></i>
                    </span>
                  </div>
                  <div class="is-pulled-left" style="width: 86%">
                    {{ navBarItem.name }}
                  </div>
                </div>
              </router-link>
            </li>
          </ul>
        </section>
      </aside>
    </div>
  </transition>
</template>

<script>
import MenuItem from "@/components/MenuItem.vue";

export default {
  name: "Sidebar",
  props: {
    user: Object,
    sideBarItems: Array,
    navBarItems: Array,
    showSideBar: Boolean,
    sideBarType: {
      type: String,
      default: "below"
    },
    sideBarStyle: {
      type: Object,
      default: () => {
        return {
          width: "0px",
          height: "100%",
          backgroundColor: "#000",
          position: "fixed",
          zIndex: 1000,
          overflowY: "auto",
          overflowX: "hidden",
          top: "0px",
          boxShadow: `0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12)`
        };
      }
    }
  },
  watch: {
    showSideBar(allowed) {
      if (allowed) this.sideBarStyle.width = "250px";
    }
  },
  methods: {
    setSideBarTopOffset() {
      if (this.sideBarType == "below") {
        const navBarHeight = document.getElementById("nav-bar").clientHeight;
        this.sideBarStyle.top = `${navBarHeight}px`;
      } else {
        this.sideBarStyle.top = "0px";
      }
    },
    performAction(action) {
      if (typeof action === "function") action();
    }
  },
  mounted() {
    this.setSideBarTopOffset();
  },
  components: {
    MenuItem
  }
};
</script>

<style lang="scss" scoped>
.side-bar-header {
  padding: 16px 16px 0px 16px;
}

aside {
  padding-bottom: 100px;
  max-width: 250px;
  min-width: 250px;
}

.side-bar-enter-active {
  animation: side-bar 0.5s;
}
.side-bar-leave-active {
  animation: side-bar 0.5s ease-in-out reverse;
}
@keyframes side-bar {
  from {
    width: 0px;
  }
  to {
    width: 250px;
  }
}
</style>
