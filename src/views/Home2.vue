<template lang="html">
  <div class="container">
    <div v-for="(units, faction) in sounds" :key="faction">
      <h2>{{ faction }}</h2>
      <div v-for="(sounds, unit) in units" :key="unit">
        <h3>{{ unit }}</h3>
        <div v-for="(src, sound) in sounds" :key="sound">
          {{ sound }}<br />
          <audio controls>
            <source :src="src" type="audio/wav" />
          </audio>
        </div>
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
    this.importAll(require.context("@/assets/en2/", true, /^.*\.wav$/));
  },

  methods: {
    importAll(r) {
      for (const assetPath of r.keys()) {
        const factionName = this.getFactionName(assetPath);
        const unitName = this.getUnitName(assetPath);
        if (!(factionName in this.sounds)) {
          this.sounds[factionName] = {};
        }
        if (!(unitName in this.sounds[factionName])) {
          this.sounds[factionName][unitName] = {};
        }
        this.sounds[factionName][unitName][this.getSoundName(assetPath)] = r(
          assetPath
        );
      }
    },
    getFactionName(path) {
      // ./orc/wc3sfx-creeps-ogre/OgreYes4.wav -> Orc
      const regex = RegExp("/(.*?)/");
      const match = regex.exec(path)[1];
      return match.charAt(0).toUpperCase() + match.slice(1);
    },
    getUnitName(path) {
      // ./orc/wc3sfx-creeps-ogre/OgreYes4.wav -> Ogre
      const regex = RegExp("-.+-(.*)/");
      const match = regex.exec(path)[1];
      return match.charAt(0).toUpperCase() + match.slice(1);
    },
    getSoundName(path) {
      // ./orc/wc3sfx-creeps-ogre/OgreYes4.wav -> OgreYes4
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
  color: #ffffff;
  font-size: 1.75em;
  font-weight: 100;
}

h3 {
  color: #ffffff;
  font-size: 1.3em;
  font-weight: 100;
}
</style>
