<script setup lang="ts">
import { computed } from 'vue'
import type { SkillMeta } from '../composables/useSkillMeta'
import { usePortraitPath } from '../composables/useSkillMeta'

const props = defineProps<{ meta: SkillMeta }>()
const portraitPath = computed(() => usePortraitPath(props.meta))
const taglineLines = computed(() => {
  const t = props.meta.tier_1.tagline ?? ''
  const idx = t.indexOf('、')
  return idx > 0 ? [t.slice(0, idx + 1), t.slice(idx + 1)] : [t]
})
</script>

<template>
  <section class="hero">
    <figure class="hero__portrait" :style="{ backgroundImage: `url(${portraitPath})` }" />

    <div class="hero__content">
      <h1 class="hero__title">{{ meta.tier_1.name_ja }}</h1>
      <p class="hero__skill">.skill</p>
      <div class="hero__tagline">
        <p v-for="(line, i) in taglineLines" :key="i">{{ line }}</p>
      </div>
      <p v-if="meta.tier_2?.description" class="hero__description">
        {{ meta.tier_2.description }}
      </p>
      <div v-if="meta.tier_2?.badges" class="hero__badges">
        <span v-for="badge in meta.tier_2.badges" :key="badge" class="badge">
          {{ badge }}
        </span>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  display: grid;
  grid-template-columns: minmax(280px, 38%) 1fr;
  align-items: stretch;
  min-height: 640px;
  border-bottom: 1px solid #1f1f1f;
  background: #0d0d0d;
}
.hero__portrait {
  margin: 0;
  background-size: cover;
  background-position: center top;
  background-repeat: no-repeat;
  background-color: #111;
}
.hero__content {
  padding: 96px 48px 80px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: right;
  max-width: 760px;
  margin-left: auto;
  width: 100%;
}
.hero__title {
  font-family: var(--font-title);
  font-size: clamp(48px, 7vw, 112px);
  line-height: 1;
  margin: 0;
  color: #f5f5f5;
}
.hero__skill {
  font-family: var(--font-mono);
  font-size: clamp(28px, 4.5vw, 60px);
  color: var(--accent);
  margin: 8px 0 32px;
  line-height: 1;
}
.hero__tagline {
  font-family: var(--font-body);
  font-size: clamp(16px, 1.6vw, 26px);
  color: #cccccc;
  line-height: 1.5;
  margin: 0 0 24px;
}
.hero__tagline p { margin: 0; }
.hero__description {
  color: #aaa;
  font-size: 15px;
  line-height: 1.7;
  margin: 0 0 24px;
  max-width: 540px;
  margin-left: auto;
}
.hero__badges {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  justify-content: flex-end;
}
.badge {
  border: 1px solid var(--accent);
  border-radius: 999px;
  padding: 4px 14px;
  font-family: var(--font-mono);
  font-size: 13px;
  color: var(--accent);
}

@media (max-width: 768px) {
  .hero {
    grid-template-columns: 1fr;
    min-height: 0;
  }
  .hero__portrait {
    height: 56vh;
    min-height: 320px;
    max-height: 480px;
    background-position: center 20%;
  }
  .hero__content {
    padding: 40px 22px 56px;
    text-align: left;
    max-width: none;
    margin-left: 0;
  }
  .hero__title {
    font-size: clamp(44px, 12vw, 64px);
  }
  .hero__skill {
    font-size: clamp(24px, 7vw, 36px);
    margin: 6px 0 24px;
  }
  .hero__tagline {
    font-size: 16px;
    margin-bottom: 20px;
  }
  .hero__description {
    max-width: 100%;
    margin-left: 0;
    font-size: 14px;
  }
  .hero__badges {
    justify-content: flex-start;
  }
  .badge {
    font-size: 12px;
    padding: 3px 12px;
  }
}
</style>
