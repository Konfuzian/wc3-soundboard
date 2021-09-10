<template lang="html">
  <div class="container">
    <div v-for="(sounds, unit) in sounds" :key="unit">
      <h2>{{ unit }}</h2>
      <div v-for="(src, sound) in sounds" :key="sound">
        {{ sound }}<br />
        <audio controls>
          <source :src="src" type="audio/wav" />
        </audio>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sounds: {},
    };
  },

  mounted() {
    this.importAll(require.context("@/assets/en/", true, /^.*\.wav$/));
  },

  methods: {
    importAll(r) {
      for (const assetPath of r.keys()) {
        const unitName = this.getUnitName(assetPath);
        if (!(unitName in this.sounds)) {
          this.sounds[unitName] = {};
        }
        this.sounds[unitName][this.getSoundName(assetPath)] = r(assetPath);
      }
    },
    getUnitName(path) {
      // ./wc3sfx-creeps-ogre/OgreYes4.wav -> Ogre
      const regex = RegExp("-.+-(.*)/");
      const match = regex.exec(path)[1];
      return match.charAt(0).toUpperCase() + match.slice(1);
    },
    getSoundName(path) {
      // ./wc3sfx-creeps-ogre/OgreYes4.wav -> OgreYes4
      const regex = RegExp(".+/(.*)\\.");
      const match = regex.exec(path)[1];
      return match;
    },
  },
};
</script>

<style scoped>
audio {
  width: 100px;
  height: 30px;
  margin-top: 2px;
  margin-bottom: 6px;
}

.container {
  display: grid;
  grid-template-columns: repeat(200, 200px);
}

h2 {
  font-size: 1.75em;
  font-weight: 100;
}
</style>
