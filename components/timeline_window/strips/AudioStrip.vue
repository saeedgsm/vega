<template>
  <div class="audio-strip">
    <div ref="wave" class="wave" />
    <span v-if="strip.asset">
      {{ strip.asset.name }}
    </span>
    <div v-if="!strip.loaded" class="error">
      [Error] Audio Resouce Not Found
    </div>
  </div>
</template>

<style scoped>
.audio-strip {
  width: 100%;
  height: 100%;
  background: var(--teal);
  font-size: 12px;
}

.wave {
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

/* Override wavesurfer overflow because scroll bar is appeared */
.wave >>> wave {
  overflow: hidden !important;
  z-index: 0;
}
</style>

<script lang="ts">
import Vue from "vue";
import WaveSufer from "wavesurfer.js";
import { Component, Prop, Watch } from "vue-property-decorator";
import { AudioStrip } from "~/models";

@Component({})
export default class AudioStripComp extends Vue {
  @Prop({ default: () => null })
  strip!: AudioStrip;

  @Prop({ default: 10 })
  scale!: number;

  wave: any = null as WaveSufer | null;

  @Watch("scale")
  watchScale(n: number) {
    this.wave?.zoom(n);
  }

  mounted() {
    this.wave = WaveSufer.create({
      container: this.$refs.wave as HTMLElement,
      height: 26,
      waveColor: "#ff9800",
      interact: false,
    });
    if (this.strip.audio.src) this.wave.load(this.strip.audio);
  }
}
</script>
