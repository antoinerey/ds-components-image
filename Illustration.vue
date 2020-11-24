<template>
  <div>
    <!-- The template we show when the request fails for whatever reason. -->
    <!-- It may be overriden using the `error` slot from the parent. -->
    <slot name="error" v-if="error">
      <!-- Note that there is no need to make sure that `height` and `width` -->
      <!-- are properly defined. Vue compiler will get rid of wrong values. -->
      <div
        :class="$style.error"
        :style="{ height: `${height}px`, width: `${width}px` }"
      >
        <!-- TODO: Render an icon instead. -->
        Oops!
      </div>
    </slot>

    <!-- The template we show when the image is being downloaded. -->
    <!-- It may be overriden using the `placeholder` slot from the parent. -->
    <slot name="placeholder" v-else-if="downloading">
      <!-- TODO: Render a skeleton instead. -->
      <!-- Note that there is no need to make sure that `height` and `width` -->
      <!-- are properly defined. Vue compiler will get rid of wrong values. -->
      <div
        :class="$style.skeleton"
        :style="{ height: `${height}px`, width: `${width}px` }"
      />
    </slot>

    <!-- Using the `sources` slot, we may pass source elements to -->
    <!-- request different files based on media queries. -->
    <picture v-if="hasSources && error === false">
      <slot name="sources" />
      <img
        v-bind="$attrs"
        :class="$style.image"
        :height="height"
        :width="width"
        :loading="loading"
        v-on="$listeners"
        @load="onLoad"
        @error="onError"
      />
    </picture>

    <!-- The most simple case. -->
    <img
      v-else-if="error === false"
      v-bind="$attrs"
      :class="$style.image"
      :height="height"
      :width="width"
      :loading="loading"
      v-on="$listeners"
      @load="onLoad"
      @error="onError"
    />
  </div>
</template>

<script>
const Loading = {
  Lazy: 'lazy',
  Eager: 'eager',
}

export default {
  inheritAttrs: false,

  props: {
    height: {
      type: [Number, String],
      required: false,
    },

    width: {
      type: [Number, String],
      required: false,
    },

    loading: {
      type: String, // See "Loading" enum above.
      default: Loading.Lazy,
    },
  },

  data() {
    return {
      error: false,
      downloading: this.loading === Loading.Lazy,
    }
  },

  computed: {
    hasSources() {
      return Boolean(this.$slots.sources)
    },
  },

  methods: {
    onLoad() {
      this.downloading = false
    },

    onError() {
      this.error = true
    },
  },
}
</script>

<style module>
.image {
  max-width: 100%;
}

.error {
  display: flex;
  align-items: center;
  justify-content: center;
  /* TODO: Switch to DS color (grey-200). */
  background: #f2f2f2;
}

/* TODO: Remove this. It should be managed by the skeleton component. */
.skeleton {
  background: #f2f2f2;
}
</style>
