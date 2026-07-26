<script setup>
import { computed, onMounted, onUnmounted, ref, watch } from 'vue'

const copy = {
  zh: {
    language: '语言选择',
    theme: '切换深浅主题',
    home: '下载',
    guide: '使用方法',
    eyebrow: '原生 macOS 胶片负片工作站',
    headline: 'XerFilmLab',
    subline: '把负片，变回照片。',
    download: '下载 macOS 版',
    version: '0.1.0 · macOS 14+ · 2.6 MB',
    scrollHint: 'DMG 安装包',
    guideEyebrow: '快速开始',
    guideTitle: '从负片到正片，六步完成。',
    guideIntro: 'XerFilmLab 的所有调整都建立在当前正片预览上，原始文件始终保持不变。',
    steps: [
      ['下载安装', '下载 DMG，打开后将 XerFilmLab 拖入 Applications 文件夹。首次启动如果 macOS 提示安全确认，请在“系统设置 → 隐私与安全性”中允许打开。'],
      ['导入底片', '点击“打开”，或直接把照片、整卷文件夹拖到空画布。RAW、TIFF、JPEG、PNG 和 HEIC 均可导入。'],
      ['完成负片转换', '彩色负片先使用“自动片基”；如果没有未曝光边框，点击“选取片基”，在画面上框选一块均匀区域。黑白负片请先打开“黑白片”。'],
      ['调整颜色与明暗', '自动结果会映射到颜色、明暗和 RGB 抽屉中。继续微调曝光、色温、通道和曲线，不会修改最底层的原始文件。'],
      ['裁切与除尘', '在画面上直接裁切、拉直和旋转。打开除尘后，用圆形修复画笔点击或拖过灰尘；每张照片的修复记录相互独立。'],
      ['导出正片', '点击右上角“导出”，选择格式、色彩空间与保存位置。胶片、相机和镜头信息会随图片元数据一起写出。'],
    ],
    tipsTitle: '三个小提示',
    tips: [
      '自动片基不可靠时，优先手动选择均匀的未曝光胶片区域。',
      '对整卷应用设置时，默认只同步颜色转换，不同步裁切、旋转和除尘。',
      '随时使用顶部“对比”检查当前结果与原始负片、自动结果或除尘前后的差异。',
    ],
    back: '返回下载',
    releaseNotes: '0.1.0 发布说明',
  },
  ja: {
    language: '言語を選択',
    theme: 'テーマを切り替える',
    home: 'ダウンロード',
    guide: '使い方',
    eyebrow: 'macOS ネイティブのフィルムワークステーション',
    headline: 'XerFilmLab',
    subline: 'ネガを、写真へ戻す。',
    download: 'macOS 版をダウンロード',
    version: '0.1.0 · macOS 14+ · 2.6 MB',
    scrollHint: 'DMG インストーラー',
    guideEyebrow: 'クイックスタート',
    guideTitle: 'ネガからポジまで、6ステップ。',
    guideIntro: 'すべての調整は現在のポジプレビューに適用され、元のファイルは変更されません。',
    steps: [
      ['インストール', 'DMG を開き、XerFilmLab を Applications フォルダへドラッグします。初回起動時に確認が表示された場合は、「システム設定 → プライバシーとセキュリティ」で許可してください。'],
      ['フィルムを読み込む', '「開く」をクリックするか、画像やロールのフォルダを空のキャンバスへドロップします。RAW、TIFF、JPEG、PNG、HEIC に対応しています。'],
      ['ネガ変換', 'カラーネガは「自動フィルムベース」から始めます。未露光の縁がない場合は「フィルムベースを選択」で均一な領域を囲みます。白黒ネガは先に「白黒フィルム」を有効にします。'],
      ['カラーとトーン', '自動結果はカラー、トーン、RGB の各パネルに反映されます。露光、色温度、チャンネル、カーブを調整しても元ファイルは変わりません。'],
      ['クロップとゴミ取り', '画面上でクロップ、傾き補正、回転を行います。ゴミ取りでは円形ブラシでクリックまたはドラッグします。修復内容はコマごとに独立しています。'],
      ['ポジを書き出す', '右上の「書き出し」から形式、色空間、保存先を選びます。フィルム、カメラ、レンズ情報もメタデータとして保存されます。'],
    ],
    tipsTitle: '3つのヒント',
    tips: [
      '自動検出が不安定な場合は、均一な未露光部分を手動で選択してください。',
      'ロール全体への適用はカラー変換のみ。クロップ、回転、ゴミ取りは自動で同期されません。',
      '上部の「比較」で、元ネガ、自動結果、ゴミ取り前後をいつでも確認できます。',
    ],
    back: 'ダウンロードへ戻る',
    releaseNotes: '0.1.0 リリースノート',
  },
  en: {
    language: 'Choose language',
    theme: 'Toggle color theme',
    home: 'Download',
    guide: 'How to use',
    eyebrow: 'A native macOS film workstation',
    headline: 'XerFilmLab',
    subline: 'Turn negatives back into photographs.',
    download: 'Download for macOS',
    version: '0.1.0 · macOS 14+ · 2.6 MB',
    scrollHint: 'DMG installer',
    guideEyebrow: 'Quick start',
    guideTitle: 'Negative to positive in six steps.',
    guideIntro: 'Every edit is applied to the current positive preview. Your original file always remains untouched.',
    steps: [
      ['Install', 'Open the DMG and drag XerFilmLab into Applications. If macOS asks for confirmation on first launch, allow the app in System Settings → Privacy & Security.'],
      ['Import film', 'Click Open, or drop images and roll folders onto the empty canvas. RAW, TIFF, JPEG, PNG, and HEIC are supported.'],
      ['Convert the negative', 'For color negative, start with Auto Film Base. If there is no unexposed border, use Pick Film Base and select an even area. Enable B&W Film first for black-and-white negatives.'],
      ['Refine color and tone', 'The automatic result is mapped into the Color, Tone, and RGB drawers. Fine-tune exposure, temperature, channels, and curves without changing the source file.'],
      ['Crop and heal dust', 'Crop, straighten, and rotate directly on the canvas. In Dust Removal, click or drag the circular healing brush over dust. Healing stays independent for every frame.'],
      ['Export the positive', 'Choose Export in the upper-right, then select format, color space, and destination. Film, camera, and lens details are written into the image metadata.'],
    ],
    tipsTitle: 'Three useful tips',
    tips: [
      'If automatic film-base detection is unreliable, manually select a uniform unexposed area.',
      'Apply to Roll synchronizes color conversion by default—not crop, rotation, or dust healing.',
      'Use Compare at any time to inspect the original negative, automatic result, or before/after dust removal.',
    ],
    back: 'Back to download',
    releaseNotes: '0.1.0 release notes',
  },
}

const savedLanguage = localStorage.getItem('xerfilmlab-language')
const browserLanguage = (navigator.language || '').toLowerCase()
const detectedLanguage = browserLanguage.startsWith('zh') ? 'zh' : browserLanguage.startsWith('ja') ? 'ja' : 'en'
const language = ref(['zh', 'ja', 'en'].includes(savedLanguage) ? savedLanguage : detectedLanguage)

const savedTheme = localStorage.getItem('xerfilmlab-theme')
const theme = ref(savedTheme || (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'))
const page = ref(window.location.hash === '#guide' ? 'guide' : 'home')
const t = computed(() => copy[language.value])

function syncPage() {
  page.value = window.location.hash === '#guide' ? 'guide' : 'home'
  window.scrollTo({ top: 0, behavior: 'auto' })
}

watch(language, (value) => {
  localStorage.setItem('xerfilmlab-language', value)
  document.documentElement.lang = value === 'zh' ? 'zh-CN' : value
}, { immediate: true })

watch(theme, (value) => {
  localStorage.setItem('xerfilmlab-theme', value)
  document.documentElement.dataset.theme = value
  document.querySelector('meta[name="theme-color"]')?.setAttribute('content', value === 'dark' ? '#111310' : '#f3f1e9')
}, { immediate: true })

onMounted(() => window.addEventListener('hashchange', syncPage))
onUnmounted(() => window.removeEventListener('hashchange', syncPage))
</script>

<template>
  <div class="site">
    <header>
      <a class="brand" href="#home" aria-label="XerFilmLab home">
        <img :src="theme === 'dark' ? '/assets/icon-dark.png' : '/assets/icon-light.png'" alt="" />
        <span>XerFilmLab</span>
      </a>

      <div class="header-actions">
        <nav aria-label="Primary navigation">
          <a href="#home" :class="{ active: page === 'home' }">{{ t.home }}</a>
          <a href="#guide" :class="{ active: page === 'guide' }">{{ t.guide }}</a>
        </nav>

        <div class="language-switcher" role="group" :aria-label="t.language">
          <button
            v-for="item in [{ id: 'en', label: 'EN' }, { id: 'ja', label: '日' }, { id: 'zh', label: '中' }]"
            :key="item.id"
            type="button"
            :class="{ active: language === item.id }"
            :aria-pressed="language === item.id"
            @click="language = item.id"
          >{{ item.label }}</button>
        </div>

        <button class="theme-switcher" type="button" :aria-label="t.theme" @click="theme = theme === 'light' ? 'dark' : 'light'">
          <span aria-hidden="true" :class="{ dark: theme === 'dark' }"></span>
        </button>
      </div>
    </header>

    <main v-if="page === 'home'" class="landing">
      <div class="landing-copy">
        <p class="eyebrow">{{ t.eyebrow }}</p>
        <h1>{{ t.headline }}</h1>
        <p class="subline">{{ t.subline }}</p>
        <a class="download-button" href="/downloads/XerFilmLab-0.1.0.dmg">
          <span>{{ t.download }}</span>
          <b aria-hidden="true">↓</b>
        </a>
        <p class="version">{{ t.version }}</p>
      </div>

      <div class="landing-mark" aria-hidden="true">
        <span class="mark-negative"></span>
        <span class="mark-positive"></span>
        <span class="mark-divider"></span>
      </div>

      <p class="installer-label">{{ t.scrollHint }}</p>
    </main>

    <main v-else class="guide">
      <section class="guide-intro">
        <p class="eyebrow">{{ t.guideEyebrow }}</p>
        <h1>{{ t.guideTitle }}</h1>
        <p>{{ t.guideIntro }}</p>
      </section>

      <ol class="steps">
        <li v-for="(step, index) in t.steps" :key="step[0]">
          <span>{{ String(index + 1).padStart(2, '0') }}</span>
          <div>
            <h2>{{ step[0] }}</h2>
            <p>{{ step[1] }}</p>
          </div>
        </li>
      </ol>

      <section class="tips">
        <h2>{{ t.tipsTitle }}</h2>
        <ul>
          <li v-for="tip in t.tips" :key="tip">{{ tip }}</li>
        </ul>
      </section>

      <footer>
        <a class="back-link" href="#home">← {{ t.back }}</a>
        <a href="/XerFilmLab-0.1.0.html">{{ t.releaseNotes }} ↗</a>
      </footer>
    </main>
  </div>
</template>
