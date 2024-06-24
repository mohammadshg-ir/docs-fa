# سینتکس کامپوننت های تک فایلی | SFC {#sfc-syntax-specification}

## نمای کلی {#overview}

کامپوننت تک فایلی (SFC) در Vue که معمولاً از پسوند فایل &lrm;`*.vue` استفاده می‌کند، یک تمپلیت فایل سفارشی است که از سینتکسی شبیه به HTML برای توصیف یک کامپوننت Vue استفاده می‌کند. کامپوننت تک فایلی (SFC)  از نظر سینتکس با HTML سازگار است.

هر فایل &lrm;`*.vue` از سه نوع بلوک زبان سطح بالا تشکیل شده است: `<template>` و `<script>` و `<style>` و همچنین می‌توان به صورت اختیاری بلوک‌های سفارشی اضافه کرد:

```vue
<template>
  <div class="example">{{ msg }}</div>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>

<style>
.example {
  color: red;
}
</style>

<custom1>
  This could be e.g. documentation for the component.
</custom1>
```

## بلوک‌های زبان {#language-blocks}

### `<template>` {#template}

- هر فایل &lrm;`*.vue` می‌تواند حداکثر یک بلوک `<template>` در سطح بالا داشته باشد.

- محتویات این بلوک استخراج و به `vue/compiler-dom@` منتقل می‌شوند، از قبل در توابع رندر جاوا اسکریپت کامپایل می‌شوند و به‌عنوان آپشن `render` به کامپوننت صادر شده پیوست می‌شوند.

### `<script>` {#script}

- هر فایل &lrm;`*.vue` می‌تواند حداکثر یک بلوک `<script>` داشته باشد (به استثنای  [`<script setup>`](/api/sfc-script-setup)).

- اسکریپت به عنوان یک ماژول ES اجرا می‌شود.

- در این بلوک، **default export** باید یک آبجکت از آپشن‌های کامپوننت Vue باشد، چه به‌عنوان یک آبجکت ساده یا به‌عنوان مقدار بازگشتی از [defineComponent](/api/general#definecomponent).

### `<script setup>` {#script-setup}

- هر فایل &lrm;`*.vue` می‌تواند حداکثر یک بلوک `<script setup>` داشته باشد (به استثنای `<script>` معمولی).

- اسکریپت از قبل پردازش شده و به عنوان تابع کامپوننت `()setup` استفاده می‌شود، به این معنی که **برای هر نمونه (Instance) از کامپوننت** اجرا می‌شود. پیوندهای سطح بالا در `<script setup>` به طور خودکار در template قرار می‌گیرند. برای جزئیات بیشتر، [اسناد اختصاصی در `<script setup>`](/api/sfc-script-setup) را ببینید.

### `<style>` {#style}

- یک فایل &lrm;`*.vue` می‌تواند حاوی چندین تگ `<style>` باشد.

- یک تگ `<style>` می‌تواند دارای ویژگی‌های `scoped` یا `module` باشد (برای جزئیات بیشتر به [SFC Style Features](/api/sfc-css-features) مراجعه کنید) تا به کپسوله کردن استایل‌ها در کامپوننت فعلی کمک کند. چندین تگ `<style>` با حالت‌های کپسوله‌سازی متفاوت را می‌توان در یک کامپوننت یکسان ترکیب کرد.

### بلوک‌های سفارشی {#custom-blocks}

بلوک‌های سفارشی اضافی را می‌توان در فایل &lrm;`*.vue` برای هر نیاز خاص در پروژه گنجاند، برای مثال بلوک `<docs>`. برخی از نمونه های دنیای واقعی از بلوک‌های سفارشی عبارتند از:

- [استفاده از Gridsome با بلوک سفارشی: `<page-query>`](https://gridsome.org/docs/querying-data/)
- [استفاده از vite-plugin-vue-gql با بلوک سفارشی: `<gql>`](https://github.com/wheatjs/vite-plugin-vue-gql)
- [استفاده از vue-i18n با بلوک سفارشی: `<i18n>`](https://github.com/intlify/bundle-tools/tree/main/packages/unplugin-vue-i18n#i18n-custom-block)

## پیش‌بینی خودکار نام {#automatic-name-inference}

یک SFC به طور خودکار نام مؤلفه را از **نام فایل** آن در موارد زیر پیش‌بینی می‌کند:

- قالب‌بندی هشدار توسعه (Dev warning formatting)
- بازرسی DevTools یا (DevTools inspection)
- ارجاع بازگشتی به خود، به عنوان مثال فایلی به نام `FooBar.vue` می‌تواند در تمپلیت خود به `<FooBar/>` اشاره کند. این اولویت کمتری نسبت به کامپوننت‌های صریحا ثبت‌شده/ایمپورت شده دارد.

## پیش پردازشگرها {#pre-processors}

بلوک‌ها می‌توانند زبان‌های پیش‌پردازنده را با استفاده از ویژگی `lang` اعلام کنند. رایج ترین مورد، استفاده از TypeScript برای بلوک `<script>` است:


```vue-html
<script lang="ts">
  // use TypeScript
</script>
```
`lang` را می‌توان برای هر بلوکی اعمال کرد - برای مثال می‌توانیم از [Sass](https://sass-lang.com/) داخل بلوک `<style>` و از [Pug](https://pugjs.org/api/getting-started.html) داخل `<template>` استفاده کنیم:

```vue-html
<template lang="pug">
p {{ msg }}
</template>

<style lang="scss">
  $primary-color: #333;
  body {
    color: $primary-color;
  }
</style>
```

توجه داشته باشید که ادغام با پیش پردازنده‌های مختلف ممکن است بسته به زنجیره ابزار متفاوت باشد. برای مثال اسناد مربوطه را بررسی کنید:

- [Vite](https://vitejs.dev/guide/features.html#css-pre-processors)
- [Vue CLI](https://cli.vuejs.org/guide/css.html#pre-processors)
- [webpack + vue-loader](https://vue-loader.vuejs.org/guide/pre-processors.html#using-pre-processors)

## ایمپورت‌های `src` {#src-imports}

اگر ترجیح می‌دهید کامپوننت‌های &lrm;`*.vue` خود را به چندین فایل تقسیم کنید، می‌توانید از ویژگی `src` برای ایمپورت کردن یک فایل خارجی برای یک بلوک زبان استفاده کنید:


```vue
<template src="./template.html"></template>
<style src="./style.css"></style>
<script src="./script.js"></script>
```

مراقب باشید که ایمپورت‌های `src` از قوانین وضوح مسیر، مشابه درخواست‌های ماژول webpack پیروی می‌کند، به این معنی که:

- مسیرهای نسبی باید با `/.` شروع شوند
- می‌توانید منابع را از وابستگی‌های npm ایمپورت کنید:

```vue
<!-- import a file from the installed "todomvc-app-css" npm package -->
<style src="todomvc-app-css/index.css" />
```

ایمپورت‌های `src` با بلوک‌های سفارشی نیز کار می‌کند، به عنوان مثال:

```vue
<unit-test src="./unit-test.js">
</unit-test>
```

## کامنت‌ها {#comments}

در داخل هر بلوک باید از سینتکس کامنت‌گذاری زبان مورد استفاده (HTML، CSS، جاوا اسکریپت، Pug و غیره) پیروی کنید. برای کامنت گذاری سطح بالا، از سینتکس کامنت گذاری HTML استفاده کنید: `<!-- مطالب را در اینجا کامنت کنید -->`

