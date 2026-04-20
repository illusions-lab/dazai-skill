<script setup lang="ts">
import { computed } from 'vue'
import { marked } from 'marked'
import readmeRaw from '@root/README.md?raw'

function stripHeaderImage(md: string): string {
  return md.replace(/!\[og\]\([^)]*\)\s*/g, '')
}

function stripGithubAlerts(md: string): string {
  return md.replace(/^>\s*\[!(NOTE|TIP|IMPORTANT|WARNING|CAUTION)\]\s*\n/gim, '')
}

const html = computed(() => {
  marked.setOptions({ gfm: true, breaks: false })
  const cleaned = stripGithubAlerts(stripHeaderImage(readmeRaw))
  return marked.parse(cleaned) as string
})
</script>

<template>
  <section class="readme">
    <div class="readme__inner markdown-body" v-html="html" />
  </section>
</template>

<style scoped>
.readme {
  padding: 40px 24px 80px;
  max-width: 860px;
  margin: 0 auto;
}
.readme__inner {
  font-family: var(--font-body);
  color: #cfcfcf;
  font-size: 16px;
  line-height: 1.85;
}
.readme__inner :deep(h1),
.readme__inner :deep(h2),
.readme__inner :deep(h3) {
  font-family: var(--font-title);
  color: #f5f5f5;
  margin: 56px 0 20px;
  line-height: 1.3;
}
.readme__inner :deep(h1) { display: none; }
.readme__inner :deep(h2) {
  font-size: 28px;
  padding-bottom: 8px;
  border-bottom: 1px solid #1f1f1f;
}
.readme__inner :deep(h3) {
  font-size: 20px;
  color: var(--accent);
}
.readme__inner :deep(p) { margin: 16px 0; }
.readme__inner :deep(a) {
  color: var(--accent);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  transition: border-color 0.15s;
}
.readme__inner :deep(a:hover) { border-bottom-color: var(--accent); }
.readme__inner :deep(strong) { color: #f5f5f5; font-weight: 600; }
.readme__inner :deep(em) { color: #bbb; }
.readme__inner :deep(blockquote) {
  border-left: 3px solid var(--accent);
  padding: 4px 20px;
  margin: 20px 0;
  color: #b8b8b8;
  background: rgba(139, 0, 0, 0.04);
  border-radius: 0 4px 4px 0;
}
.readme__inner :deep(blockquote p) { margin: 8px 0; }
.readme__inner :deep(code) {
  font-family: var(--font-mono);
  font-size: 0.9em;
  background: #141414;
  padding: 2px 6px;
  border-radius: 3px;
  color: #e5a0a0;
}
.readme__inner :deep(pre) {
  background: #0a0a0a;
  border: 1px solid #1f1f1f;
  border-radius: 8px;
  padding: 16px 20px;
  overflow-x: auto;
  margin: 20px 0;
}
.readme__inner :deep(pre code) {
  background: transparent;
  padding: 0;
  color: #e5e5e5;
  font-size: 14px;
}
.readme__inner :deep(table) {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
  font-size: 15px;
}
.readme__inner :deep(th),
.readme__inner :deep(td) {
  padding: 10px 14px;
  border: 1px solid #1f1f1f;
  text-align: left;
}
.readme__inner :deep(th) {
  background: #141414;
  color: #f5f5f5;
  font-family: var(--font-title);
  font-weight: 600;
}
.readme__inner :deep(td) {
  background: #0f0f0f;
}
.readme__inner :deep(ul),
.readme__inner :deep(ol) {
  padding-left: 22px;
  margin: 16px 0;
}
.readme__inner :deep(li) { margin: 6px 0; }
.readme__inner :deep(hr) {
  border: 0;
  border-top: 1px solid #1f1f1f;
  margin: 48px 0;
}
.readme__inner :deep(img) {
  max-width: 100%;
  height: auto;
  border-radius: 6px;
}
.readme__inner :deep(summary) {
  cursor: pointer;
  font-family: var(--font-title);
  color: var(--accent);
  margin: 16px 0 8px;
}
.readme__inner :deep(div[align='center']) {
  text-align: center;
}
@media (max-width: 768px) {
  .readme { padding: 24px 20px 60px; }
  .readme__inner { font-size: 15px; }
  .readme__inner :deep(h2) { font-size: 24px; margin-top: 40px; }
  .readme__inner :deep(h3) { font-size: 18px; }
}
</style>
