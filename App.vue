<template>
  <div>
    <section>
      <h3>Img</h3>
      <div class="spacer" />

      <Illustration src="https://picsum.photos/id/96/200" height="200" />

      <div class="spacer" />

      <Illustration src="https://picsum.photos/id/102/200" />

      <div class="spacer" />

      <Illustration src="https://picsum.photos/id/98/200">
        <template #placeholder>
          <div
            :style="{ background: 'red', height: '200px', width: '200px' }"
          />
        </template>
      </Illustration>

      <div class="spacer" @click="expanded = !expanded">Expand?</div>

      <Illustration src="https://picsum.photos/id/102/200" v-if="expanded" />

      <div class="spacer" />
    </section>

    <section>
      <h3>Picture</h3>
      <div class="spacer" />

      <Illustration src="https://picsum.photos/id/100/200" loading="eager">
        <template #sources>
          <source
            media="(max-width: 480px)"
            srcset="https://picsum.photos/id/1/200"
          />
          <source
            media="(max-width: 920px)"
            srcset="https://picsum.photos/id/2/200"
          />
        </template>
      </Illustration>

      <div class="spacer" />
    </section>
  </div>
</template>

<script>
import Illustration from './Illustration'

export default {
  components: {
    Illustration,
  },

  data() {
    return {
      expanded: false,
    }
  },
}
</script>

<style scoped>
.spacer {
  margin: 20px 0;
  height: 500px;
  background: grey;
}
</style>
