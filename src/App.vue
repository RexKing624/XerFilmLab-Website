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
    subline: '全新高效的胶片去色罩全流程，',
    download: 'macOS',
    version: '1.1 · macOS 14+ · 2.6 MB',
    repository: 'GitHub',
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
    adviceTitle: '使用建议',
    adviceIntro: 'XerFilmLab 使用可重复的数学模型完成去色罩与校色，不依赖 AI 猜色。不同胶片、冲洗方式、扫描仪或翻拍流程都会改变片基与通道分布，因此自动结果也可能不同。',
    advice: [
      '自动结果不理想时，请重新框选一块均匀的片基，再使用颜色、明暗、RGB 与曲线手动校正。',
      '完成一套适合当前冲洗与数字化流程的校色后，可以保存为预设，并应用到同一批胶片。',
      '需要保留完整胶片外观时可以启用齿孔；填写胶片、相机与镜头信息后，导出时会写入图片元数据。',
      'XerFilmLab 的优势不是一次性自动猜色，而是提供从片基、校色、除尘、裁切到元数据导出的完整流程。',
    ],
    back: '返回下载',
    releaseNotes: '1.1 发布说明',
    historyTitle: '历史版本',
    historyVersion: 'XerFilmLab 1.0',
    historyDownload: '下载 DMG',
    historyNotes: '发布说明',
  },
  ja: {
    language: '言語を選択',
    theme: 'テーマを切り替える',
    home: 'ダウンロード',
    guide: '使い方',
    eyebrow: 'macOS ネイティブのフィルムワークステーション',
    headline: 'XerFilmLab',
    subline: '新しく効率的なフィルムベース除去ワークフロー。',
    download: 'macOS',
    version: '1.1 · macOS 14+ · 2.6 MB',
    repository: 'GitHub',
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
    adviceTitle: '使い方のアドバイス',
    adviceIntro: 'XerFilmLab のフィルムベース除去と色補正は、AI による推測ではなく再現可能な数理モデルで処理されます。フィルム、現像、スキャナー、カメラ複写の工程によってベース色とチャンネル分布が変わるため、自動結果にも差が生じます。',
    advice: [
      '自動結果が適切でない場合は、均一なフィルムベースをもう一度囲み、カラー、トーン、RGB、カーブで手動調整してください。',
      '現像・デジタル化工程に合う補正ができたらプリセットとして保存し、同じロールや同条件の画像に再利用できます。',
      'フィルムらしい外観を残す場合はパーフォレーションを有効にできます。フィルム、カメラ、レンズ情報は書き出し時にメタデータへ保存されます。',
      '一度の自動推測ではなく、フィルムベース、色補正、ゴミ取り、クロップ、メタデータまでを一つの流れで扱えることが XerFilmLab の強みです。',
    ],
    back: 'ダウンロードへ戻る',
    releaseNotes: '1.1 リリースノート',
    historyTitle: '過去のバージョン',
    historyVersion: 'XerFilmLab 1.0',
    historyDownload: 'DMG をダウンロード',
    historyNotes: 'リリースノート',
  },
  en: {
    language: 'Choose language',
    theme: 'Toggle color theme',
    home: 'Download',
    guide: 'How to use',
    eyebrow: 'A native macOS film workstation',
    headline: 'XerFilmLab',
    subline: 'A new, efficient end-to-end film-base removal workflow.',
    download: 'macOS',
    version: '1.1 · macOS 14+ · 2.6 MB',
    repository: 'View GitHub',
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
    adviceTitle: 'Workflow advice',
    adviceIntro: 'XerFilmLab removes the film base and corrects color with reproducible mathematical models—not AI color guessing. Film stock, development, scanner, and camera-scanning workflows all change the base and channel distribution, so automatic results can vary.',
    advice: [
      'If the automatic result is not right, select a uniform film-base area again, then refine Color, Tone, RGB, and curves manually.',
      'Once a correction matches your development and digitization workflow, save it as a preset and reuse it across the same roll or setup.',
      'Enable sprockets when you want to preserve the complete film look. Film, camera, and lens details can be embedded as metadata during export.',
      'XerFilmLab is designed around a complete workflow—from film base and color correction through dust healing, crop, presets, and metadata—not a one-click AI guess.',
    ],
    back: 'Back to download',
    releaseNotes: '1.1 release notes',
    historyTitle: 'Previous versions',
    historyVersion: 'XerFilmLab 1.0',
    historyDownload: 'Download DMG',
    historyNotes: 'Release notes',
  },
}

const browserLanguage = (navigator.language || '').toLowerCase()
const detectedLanguage = browserLanguage.startsWith('zh') ? 'zh' : browserLanguage.startsWith('ja') ? 'ja' : 'en'
const language = ref(detectedLanguage)

const theme = ref('dark')
const page = ref(window.location.hash === '#guide' ? 'guide' : 'home')
const activeImage = ref('')
const t = computed(() => copy[language.value])
const guideImages = computed(() => {
  const localizedLabel = language.value === 'zh' ? '中文' : language.value === 'ja' ? '日本語' : 'English'
  return [
    { src: '/assets/guide/install.png', label: 'DMG' },
    { src: `/assets/guide/import-${language.value}.png`, label: localizedLabel },
    { src: `/assets/guide/convert-${language.value}.png`, label: localizedLabel },
    { src: `/assets/guide/color-${language.value}.png`, label: localizedLabel },
    { src: `/assets/guide/crop-dust-${language.value}.png`, label: localizedLabel },
    { src: '/assets/guide/export.png', label: 'Export' },
  ]
})

function syncPage() {
  page.value = window.location.hash === '#guide' ? 'guide' : 'home'
  activeImage.value = ''
  window.scrollTo({ top: 0, behavior: 'auto' })
}

function handleKeydown(event) {
  if (event.key === 'Escape') activeImage.value = ''
}

watch(language, (value) => {
  document.documentElement.lang = value === 'zh' ? 'zh-CN' : value
}, { immediate: true })

watch(theme, (value) => {
  document.documentElement.dataset.theme = value
  document.querySelector('meta[name="theme-color"]')?.setAttribute('content', value === 'dark' ? '#111310' : '#f3f1e9')
}, { immediate: true })

onMounted(() => {
  window.addEventListener('hashchange', syncPage)
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('hashchange', syncPage)
  window.removeEventListener('keydown', handleKeydown)
})
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
        <a class="download-button" href="/downloads/XerFilmLab-1.1.dmg">
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
          <figure class="step-media">
            <button type="button" @click="activeImage = guideImages[index].src">
              <img :src="guideImages[index].src" :alt="`${step[0]} · ${guideImages[index].label}`" loading="lazy" />
            </button>
          </figure>
          <div class="step-body">
            <h2>{{ step[0] }}</h2>
            <p>{{ step[1] }}</p>
          </div>
        </li>
      </ol>

      <section class="advice">
        <div>
          <h2>{{ t.adviceTitle }}</h2>
          <p>{{ t.adviceIntro }}</p>
        </div>
        <ul>
          <li v-for="item in t.advice" :key="item">{{ item }}</li>
        </ul>
      </section>

      <section class="release-history">
        <p>{{ t.historyTitle }}</p>
        <div>
          <strong>{{ t.historyVersion }}</strong>
          <a href="/downloads/XerFilmLab-1.0.dmg">{{ t.historyDownload }} ↓</a>
          <a href="/XerFilmLab-1.0.html">{{ t.historyNotes }} ↗</a>
        </div>
      </section>

      <div class="guide-actions">
        <a class="back-link" href="#home">← {{ t.back }}</a>
        <a href="/XerFilmLab-1.1.html">{{ t.releaseNotes }} ↗</a>
      </div>

      <footer class="site-meta">
        <a class="copyright-link" href="https://xergnik.com">© 2026 RexKing624</a>
        <p>Tokyo · Japan</p>
        <a href="https://github.com/RexKing624/XerFilmLab-Website" target="_blank" rel="noreferrer">
          {{ t.repository }} <span aria-hidden="true">↗</span>
        </a>
      </footer>
    </main>

    <Transition name="lightbox">
      <div v-if="activeImage" class="lightbox" role="dialog" aria-modal="true" @click.self="activeImage = ''">
        <button type="button" aria-label="Close image" @click="activeImage = ''">×</button>
        <img :src="activeImage" alt="" />
      </div>
    </Transition>
  </div>
</template>
