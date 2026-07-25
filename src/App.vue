<script setup>
import { computed, onMounted, ref, watch } from 'vue'

const translations = {
  zh: {
    language: '语言选择',
    theme: '切换深浅主题',
    nav: ['功能', '工作流', '下载'],
    eyebrow: '原生 macOS 胶片工作站',
    headline: ['让每一格负片，', '回到它应有的颜色。'],
    intro: '从 RAW 与高位深扫描开始，完成负片转换、片基校正、调色、除尘、裁切和元数据导出。所有步骤都保留在同一套非破坏性工作流里。',
    download: '下载 XerFilmLab',
    releaseNotes: '查看 0.1.0 发布说明',
    system: 'macOS 14 或更高版本 · Apple Silicon / Intel',
    previewLabel: 'XerFilmLab 编辑工作区',
    ticker: ['负片转换', '片基校正', 'RAW 解码', '除尘修复', '批量导出'],
    philosophyKicker: '为胶片而生',
    philosophyTitle: '不是反相滤镜，而是一间数字暗房。',
    philosophyBody: 'XerFilmLab 把胶片数字化中最重要的判断留给摄影师：片基从哪里取、色彩如何回归、画面如何裁切、灰尘如何修复。自动工具负责加速，最终结果仍由你掌控。',
    featuresTitle: '一套完整的负片处理链',
    features: [
      ['01', '片基与负片转换', '自动检测或手动框选未曝光片基，分别处理彩色与黑白负片，并保留可继续调整的转换参数。'],
      ['02', 'RAW 与高位深输入', '支持相机翻拍 RAW，以及 JPEG、PNG、HEIC 和 8/16-bit TIFF，在完整精度下进入处理链。'],
      ['03', '颜色与明暗', '曝光、对比、高光、阴影、色温、色调、饱和度、RGB 通道和曲线都建立在转换后的正片基础上。'],
      ['04', '除尘与灰点修复', '使用可视化修复画笔处理胶片灰尘，笔触属于当前画面，不会污染整卷中的其他照片。'],
      ['05', '几何、裁切与齿孔', '旋转、镜像、拉直与胶片比例裁切保持预览坐标一致，也能检测并生成纯白齿孔效果。'],
      ['06', '项目、元数据与导出', '保存 .xfl 项目和预设，将胶片、相机与镜头信息写入导出文件，并支持整卷同步与批量导出。'],
    ],
    workflowKicker: '简单而可控',
    workflowTitle: '从扫描到成片，四个步骤。',
    workflow: [
      ['导入', '打开单张、整卷或文件夹。RAW 会自动进入对应的解码流程。'],
      ['转换', '自动检测片基，或在画面上选取一块均匀的未曝光区域。'],
      ['调整', '先完成颜色与明暗，再处理几何、裁切、齿孔和局部除尘。'],
      ['导出', '选择格式、色彩空间和文件名，写入胶片元数据并输出正片。'],
    ],
    localTitle: '你的底片，留在你的 Mac。',
    localBody: 'XerFilmLab 是原生本地应用。照片处理不依赖云端上传；项目、预设和修复记录都由你自己保管。',
    downloadKicker: '首个公开预览版',
    downloadTitle: 'XerFilmLab 0.1.0',
    downloadBody: '下载 DMG，将 XerFilmLab 拖入 Applications 即可。应用内通过 Sparkle 检查 EdDSA 签名的后续更新。',
    downloadDmg: '下载 DMG',
    downloadZip: '下载 ZIP',
    notes: '发布说明',
    version: '0.1.0 · Build 1',
    requirements: '需要 macOS 14 Sonoma 或更高版本',
    footerText: 'Made for film photographers in Tokyo.',
    personal: '访问 XerGnik.com',
  },
  ja: {
    language: '言語を選択',
    theme: 'テーマを切り替える',
    nav: ['機能', 'ワークフロー', 'ダウンロード'],
    eyebrow: 'macOS ネイティブのフィルムワークスペース',
    headline: ['一コマごとのネガを、', '本来の色へ。'],
    intro: 'RAW・高ビット深度スキャンから、ネガ変換、フィルムベース補正、カラー調整、ゴミ取り、クロップ、メタデータ書き出しまで。すべてを一つの非破壊ワークフローで扱います。',
    download: 'XerFilmLab をダウンロード',
    releaseNotes: '0.1.0 リリースノート',
    system: 'macOS 14 以降 · Apple Silicon / Intel',
    previewLabel: 'XerFilmLab 編集ワークスペース',
    ticker: ['ネガ変換', 'フィルムベース補正', 'RAW 現像', 'ゴミ取り', '一括書き出し'],
    philosophyKicker: 'フィルムのために',
    philosophyTitle: '反転フィルターではなく、デジタル暗室。',
    philosophyBody: 'フィルムベースの採取、色の戻し方、クロップ、ゴミ取り。XerFilmLab は大切な判断を写真家に残します。自動処理は作業を速め、最終結果は常に自分でコントロールできます。',
    featuresTitle: 'ネガのための完全な処理チェーン',
    features: [
      ['01', 'フィルムベースとネガ変換', '未露光部分を自動検出、または手動で選択。カラーネガと白黒ネガを別々に処理し、変換後も細かく調整できます。'],
      ['02', 'RAW・高ビット深度入力', 'デジタルカメラ複写の RAW、JPEG、PNG、HEIC、8/16-bit TIFF を高精度のまま処理します。'],
      ['03', 'カラーとトーン', '露光、コントラスト、ハイライト、シャドウ、色温度、色かぶり、彩度、RGB、カーブをポジ変換後に適用します。'],
      ['04', 'ゴミ・ホコリ修復', '画面上の修復ブラシでフィルムのホコリを除去。ストロークは現在のコマだけに保存されます。'],
      ['05', 'ジオメトリ、クロップ、パーフォレーション', '回転、反転、傾き補正、フィルム比率クロップを表示座標のまま操作し、白いパーフォレーションも生成できます。'],
      ['06', 'プロジェクト、メタデータ、書き出し', '.xfl プロジェクトとプリセットを保存し、フィルム・カメラ・レンズ情報を埋め込んで一括書き出しできます。'],
    ],
    workflowKicker: 'シンプルで確実',
    workflowTitle: 'スキャンから完成まで、4ステップ。',
    workflow: [
      ['読み込み', '一枚、ロール全体、フォルダを開きます。RAW は適切なデコード処理に進みます。'],
      ['変換', 'フィルムベースを自動検出するか、均一な未露光部分を画面上で選択します。'],
      ['調整', 'カラーとトーンを整え、ジオメトリ、クロップ、パーフォレーション、ゴミ取りを行います。'],
      ['書き出し', '形式、色空間、ファイル名を選び、フィルム情報を埋め込んでポジ画像を書き出します。'],
    ],
    localTitle: 'ネガは、あなたの Mac の中に。',
    localBody: 'XerFilmLab はネイティブのローカルアプリです。画像処理にクラウドへのアップロードは不要。プロジェクト、プリセット、修復履歴は自分で管理できます。',
    downloadKicker: '最初のパブリックプレビュー',
    downloadTitle: 'XerFilmLab 0.1.0',
    downloadBody: 'DMG をダウンロードし、XerFilmLab を Applications にドラッグしてください。今後のアップデートは Sparkle と EdDSA 署名で安全に配信されます。',
    downloadDmg: 'DMG をダウンロード',
    downloadZip: 'ZIP をダウンロード',
    notes: 'リリースノート',
    version: '0.1.0 · Build 1',
    requirements: 'macOS 14 Sonoma 以降',
    footerText: 'Made for film photographers in Tokyo.',
    personal: 'XerGnik.com へ',
  },
  en: {
    language: 'Choose language',
    theme: 'Toggle color theme',
    nav: ['Features', 'Workflow', 'Download'],
    eyebrow: 'A native macOS film workspace',
    headline: ['Bring every negative', 'back to its true color.'],
    intro: 'Start with RAW or high-bit-depth scans, then convert, correct the film base, grade, heal dust, crop, and export metadata—all in one non-destructive workflow.',
    download: 'Download XerFilmLab',
    releaseNotes: 'Read the 0.1.0 release notes',
    system: 'macOS 14 or later · Apple Silicon / Intel',
    previewLabel: 'XerFilmLab editing workspace',
    ticker: ['Negative conversion', 'Film-base correction', 'RAW decoding', 'Dust healing', 'Batch export'],
    philosophyKicker: 'Made for film',
    philosophyTitle: 'Not an invert filter. A digital darkroom.',
    philosophyBody: 'XerFilmLab keeps the important decisions with the photographer: where to sample the film base, how color should return, how the frame should be cropped, and where dust should be healed. Automation makes the work faster; the final image remains yours.',
    featuresTitle: 'A complete negative-processing chain',
    features: [
      ['01', 'Film base & conversion', 'Automatically detect or manually sample unexposed film base, with dedicated color and black-and-white conversion paths that remain fully adjustable.'],
      ['02', 'RAW & high-bit-depth input', 'Process camera-scanned RAW alongside JPEG, PNG, HEIC, and 8/16-bit TIFF while preserving precision through the pipeline.'],
      ['03', 'Color & tone', 'Exposure, contrast, highlights, shadows, temperature, tint, saturation, RGB channels, and curves all operate after positive conversion.'],
      ['04', 'Dust & spot healing', 'Use a visible healing brush to remove film dust. Retouching belongs to the current frame and never leaks across the roll.'],
      ['05', 'Geometry, crop & sprockets', 'Rotate, mirror, straighten, and crop to film ratios in displayed coordinates, with optional detection and pure-white sprocket rendering.'],
      ['06', 'Projects, metadata & export', 'Save .xfl projects and presets, embed film, camera, and lens details, synchronize a roll, and export in batches.'],
    ],
    workflowKicker: 'Simple and controlled',
    workflowTitle: 'From scan to positive in four steps.',
    workflow: [
      ['Import', 'Open one image, a complete roll, or a folder. RAW files enter the appropriate decoding path.'],
      ['Convert', 'Detect the film base automatically or select an even, unexposed region directly on the image.'],
      ['Refine', 'Finish color and tone, then handle geometry, crop, sprockets, and local dust healing.'],
      ['Export', 'Choose format, color space, and filename, embed film metadata, and render the positive.'],
    ],
    localTitle: 'Your negatives stay on your Mac.',
    localBody: 'XerFilmLab is a native local application. Image processing requires no cloud upload, and your projects, presets, and healing records remain under your control.',
    downloadKicker: 'First public preview',
    downloadTitle: 'XerFilmLab 0.1.0',
    downloadBody: 'Download the DMG and drag XerFilmLab into Applications. Future releases are delivered through Sparkle with EdDSA-signed updates.',
    downloadDmg: 'Download DMG',
    downloadZip: 'Download ZIP',
    notes: 'Release notes',
    version: '0.1.0 · Build 1',
    requirements: 'Requires macOS 14 Sonoma or later',
    footerText: 'Made for film photographers in Tokyo.',
    personal: 'Visit XerGnik.com',
  },
}

const savedLanguage = localStorage.getItem('xerfilmlab-language')
const browserLanguage = navigator.language || ''
const detectedLanguage = browserLanguage.toLowerCase().startsWith('zh')
  ? 'zh'
  : browserLanguage.toLowerCase().startsWith('ja') ? 'ja' : 'en'
const language = ref(['zh', 'ja', 'en'].includes(savedLanguage) ? savedLanguage : detectedLanguage)

const savedTheme = localStorage.getItem('xerfilmlab-theme')
const preferredDark = window.matchMedia('(prefers-color-scheme: dark)').matches
const theme = ref(savedTheme || (preferredDark ? 'dark' : 'light'))
const t = computed(() => translations[language.value])

watch(language, (value) => {
  localStorage.setItem('xerfilmlab-language', value)
  document.documentElement.lang = value === 'zh' ? 'zh-CN' : value
}, { immediate: true })

watch(theme, (value) => {
  localStorage.setItem('xerfilmlab-theme', value)
  document.documentElement.dataset.theme = value
  document.querySelector('meta[name="theme-color"]')?.setAttribute('content', value === 'dark' ? '#151614' : '#f2f0e9')
}, { immediate: true })

onMounted(() => {
  const elements = document.querySelectorAll('[data-reveal]')
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    elements.forEach((element) => element.classList.add('is-visible'))
    return
  }
  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (!entry.isIntersecting) return
      entry.target.classList.add('is-visible')
      observer.unobserve(entry.target)
    })
  }, { threshold: 0.12, rootMargin: '0px 0px -7% 0px' })
  elements.forEach((element) => observer.observe(element))
})
</script>

<template>
  <div class="page-shell">
    <header class="nav">
      <a class="brand" href="#top" aria-label="XerFilmLab">
        <img :src="theme === 'dark' ? '/assets/icon-dark.png' : '/assets/icon-light.png'" alt="" />
        <span>XerFilmLab</span>
      </a>
      <div class="nav-right">
        <nav aria-label="Primary navigation">
          <a href="#features">{{ t.nav[0] }}</a>
          <a href="#workflow">{{ t.nav[1] }}</a>
          <a href="#download">{{ t.nav[2] }}</a>
        </nav>
        <div class="language-switcher" role="group" :aria-label="t.language">
          <button v-for="item in [{ id: 'en', label: 'EN' }, { id: 'ja', label: '日' }, { id: 'zh', label: '中' }]" :key="item.id" type="button" :class="{ active: language === item.id }" :aria-pressed="language === item.id" @click="language = item.id">{{ item.label }}</button>
        </div>
        <button class="theme-switcher" type="button" :aria-label="t.theme" @click="theme = theme === 'light' ? 'dark' : 'light'">
          <span class="theme-track" aria-hidden="true"><span :class="{ dark: theme === 'dark' }"></span></span>
        </button>
      </div>
    </header>

    <main id="top">
      <section class="hero">
        <p class="eyebrow"><span></span>{{ t.eyebrow }}</p>
        <h1><span>{{ t.headline[0] }}</span><span>{{ t.headline[1] }}</span></h1>
        <div class="hero-meta">
          <p>{{ t.intro }}</p>
          <div class="hero-actions">
            <a class="primary-action" href="/downloads/XerFilmLab-0.1.0.dmg">{{ t.download }} <span>↓</span></a>
            <a class="text-action" href="/XerFilmLab-0.1.0.html">{{ t.releaseNotes }} <span>↗</span></a>
          </div>
          <small>{{ t.system }}</small>
        </div>
      </section>

      <figure class="app-stage" data-reveal>
        <div class="stage-glow"></div>
        <img src="/assets/xerfilmlab-app.png" :alt="t.previewLabel" />
        <figcaption>{{ t.previewLabel }} <span>0.1.0</span></figcaption>
      </figure>

      <div class="ticker" aria-hidden="true">
        <div>
          <template v-for="round in 2" :key="round">
            <span v-for="item in t.ticker" :key="`${round}-${item}`">{{ item }} <b>✦</b></span>
          </template>
        </div>
      </div>

      <section class="philosophy" data-reveal>
        <p class="section-label">{{ t.philosophyKicker }}</p>
        <div>
          <h2>{{ t.philosophyTitle }}</h2>
          <p>{{ t.philosophyBody }}</p>
        </div>
      </section>

      <section id="features" class="features">
        <div class="section-heading">
          <p>{{ t.nav[0] }}</p>
          <h2>{{ t.featuresTitle }}</h2>
        </div>
        <div class="feature-grid">
          <article v-for="feature in t.features" :key="feature[0]" data-reveal>
            <span>{{ feature[0] }}</span>
            <h3>{{ feature[1] }}</h3>
            <p>{{ feature[2] }}</p>
          </article>
        </div>
      </section>

      <section id="workflow" class="workflow">
        <div class="workflow-heading" data-reveal>
          <p class="section-label">{{ t.workflowKicker }}</p>
          <h2>{{ t.workflowTitle }}</h2>
        </div>
        <ol>
          <li v-for="(step, index) in t.workflow" :key="step[0]" data-reveal>
            <span>{{ String(index + 1).padStart(2, '0') }}</span>
            <h3>{{ step[0] }}</h3>
            <p>{{ step[1] }}</p>
          </li>
        </ol>
      </section>

      <section class="local-note" data-reveal>
        <div class="privacy-mark" aria-hidden="true"><span></span><span></span><span></span></div>
        <div>
          <h2>{{ t.localTitle }}</h2>
          <p>{{ t.localBody }}</p>
        </div>
      </section>

      <section id="download" class="download">
        <div class="download-copy">
          <p class="section-label">{{ t.downloadKicker }}</p>
          <h2>{{ t.downloadTitle }}</h2>
          <p>{{ t.downloadBody }}</p>
          <div class="release-details">
            <span>{{ t.version }}</span>
            <span>{{ t.requirements }}</span>
          </div>
        </div>
        <div class="download-actions">
          <a class="download-main" href="/downloads/XerFilmLab-0.1.0.dmg">
            <span class="download-icon" aria-hidden="true">↓</span>
            <span><b>{{ t.downloadDmg }}</b><small>2.6 MB</small></span>
          </a>
          <div>
            <a href="/XerFilmLab-0.1.0.zip">{{ t.downloadZip }} <span>↗</span></a>
            <a href="/XerFilmLab-0.1.0.html">{{ t.notes }} <span>↗</span></a>
          </div>
        </div>
      </section>
    </main>

    <footer>
      <p>© 2026 XerFilmLab</p>
      <p>{{ t.footerText }}</p>
      <div>
        <a href="https://xergnik.com" target="_blank" rel="noreferrer">{{ t.personal }} <span>↗</span></a>
        <a href="https://github.com/RexKing624/XerFilmLab-Website" target="_blank" rel="noreferrer">GitHub <span>↗</span></a>
      </div>
    </footer>
  </div>
</template>
