# ๐๋ชฉ์ฐจ

1. [์ด๋ฒคํธ ์ ์ด๊ฐ ํ์ํ ์๊ฐ](#1-์ด๋ฒคํธ-์ ์ด๊ฐ-ํ์ํ-์๊ฐ)
2. [Debounce](#2-debounce)<br>
3. [Debounce ์ฌ์ฉ๋ฐฉ๋ฒ](#3-debounce-์ฌ์ฉ๋ฐฉ๋ฒ)<br>
4. [Throttle](#4-throttle)
5. [Throttle ์ฌ์ฉ๋ฐฉ๋ฒ](#5-throttle-์ฌ์ฉ๋ฐฉ๋ฒ)<br>
6. [์ ๋ฆฌ](#6-์ ๋ฆฌ)<br>

---

<br><br>

# 1 ์ด๋ฒคํธ ์ ์ด๊ฐ ํ์ํ ์๊ฐ

๋ ์ด๋ฒคํธ ๋ฐ์ ์ ์ฝ๋ฐฑ์ ์คํํ๋๋ก ํ๋ค๋ฉด ์ด๋ฒคํธ๊ฐ ํ์ ์ด์์ผ๋ก ๋ฐ์ํ  ๋ ์ ์ด๊ฐ ํ์ํฉ๋๋ค. ์๋ฅผ ๋ค์ด textarea์ keyup์ด ๋  ๋ ๋ง๋ค post ์์ฒญ์ ๋ณด๋ธ๋ค๊ณ  ๊ฐ์ ํด ๋ณด๊ฒ ์ต๋๋ค.

```js
    textarea.addEventListener('keyup', (e) => {
        postApi({
            ...this.state
            content: e.target.value
        })
    })

    const postApi = async (state) => {
      await request(url,state)
    }
```

์ด ๊ฒฝ์ฐ ํค๊ฐ ๋๋ฆด ๋๋ง๋ค ํด๋น api์ ์์ฒญ์ ๋ณด๋ด๊ณ , ์ฒ ์ ํ๋ํ๋๊ฐ ๋ฌธ์๋ก ์ ์ฅ๋๊ธฐ ๋๋ฌธ์ '์ํฌ์ํธ'๋ผ๋ ์ ๋ชฉ์ผ๋ก ์์ฒญ์ ๋ณด๋ด๊ณ  ์ถ์๋ ์ฌ๋์ ๋ค์๊ณผ ๊ฐ์ด ๋์ฐํ ๊ฒฐ๊ณผ๋ฅผ ๋ง์ดํฉ๋๋ค.

![image](https://user-images.githubusercontent.com/79133602/165584418-156984d8-9641-4208-b0f7-a759bbdfca98.png)

๊ธ๋ชฉ๋ก์ด ์๋ฏธ ์๋ ์ ๋ชฉ๋ค๋ก ๊ฐ๋์ฐจ๋ ๊ฒฐ๊ณผ์  ๋ฌธ์ ๋ ์์ง๋ง ๋ ํฐ ๋ฌธ์ ๋ค์ ๋ฐ๋ก ์์ต๋๋ค.

๋ฐ๋ก ์ฑ๋ฅ๊ณผ ๋น์ฉ ๋ฌธ์ ์๋๋ค.

๊ฒจ์ฐ 4๊ธ์๋ฅผ ์ฐ๋๋ฐ 10๋ฒ์ ์์ฒญ์ ๋ณด๋๋ค๋ฉด ์ ๋๋ก ๋ ๊ธ์ ์ธ ๋ ์ ์ ์๋ ์์ฒญ์ ๋ณด๋ด๊ฒ ๋  ๊ฒ๋๋ค. ๋น์ฐํ ์ฌ์ดํธ๋ ๊ณผ๋ํ ์ฝ๋ฐฑ์ ์ฒ๋ฆฌํ๋๋ผ ๋น ๋ฅด๊ฒ ์๋ํ์ง ๋ชปํ  ํ๊ณ  ์ด๋ ์ฌ์ฉ์ ๊ฒฝํ์ ๋จ์ด๋จ๋ฆฝ๋๋ค.

๊ฒ๋ค๊ฐ ์ ๋ฃ api๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ ๋น์ฉ์ด ๋ง์ด ๋์ค๊ฒ ๋ฉ๋๋ค.

๊ทธ๋ ๋ค๋ฉด ์ด๋ ๊ฒ ์ด๋ฒคํธ๊ฐ ๊ณผํ๊ฒ ๋ฐ์ํ๋ ๊ฒฝ์ฐ ์ด๋ค์์ผ๋ก ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๋ฉด ์ข์๊น์?

<br/><br/>

# ์ด๋ฒคํธ ์ ์ด : Debounce & Throttle

๋ ์ด๋ฒคํธ์ ํจ์์ฌ์ด์ ์ ์ด๊ณ์ธต์ ๋ง๋ค๋ฉด ํด๋น ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ  ์ ์๋๋ฐ, ์ฃผ๋ก ๋๋ฐ์ด์ค์ ์ฐ๋กํ๊ธฐ๋ฒ์ ์ฌ์ฉํฉ๋๋ค.

# 2 Debounce

> ์ฐ์ด์ด ์คํ๋๋ ํจ์ ์ค ๋ง์ง๋ง ํจ์(๋๋ ์ ์ผ ์ฒซ๋ฒ์งธ)๋ง ์คํ

๋๋ฐ์ด์ค๋ ์ด๋ฒคํธ๋ฅผ ๊ทธ๋ฃนํํด์ ํน์  ์๊ฐ์ด ์ง๋ ํ ํ๋์ ์ด๋ฒคํธ๋ง ๋ฐ์ํ๋๋ก ํ๋ ๊ธฐ๋ฒ์๋๋ค.

![image](https://user-images.githubusercontent.com/79133602/165986879-b33486bd-77cb-462d-9412-78114fb86256.png)

์๋ก ๊ทธ๋ฆผ์ ๋ณด์๋ฉด ์ Raw events over time์ ๋ง๋๋ ์ด๋ฒคํธ ๋ฐ์ ๋ด์ญ์๋๋ค. ์ฌ๊ธฐ์ 0.4์ด๋ก ๋๋ฐ์ด์ฑ์ ํ๋ฉด ์ด๋ฒคํธ๊ฐ 0.4์ด๋์ ๋ฐ์ํ์ง ์์์ ๋ ๋ง์ง๋ง ์ด๋ฒคํธ๋ก ํจ์ ํธ์ถ์ ํฉ๋๋ค.

<br/>

# 3 Debounce ์ฌ์ฉ๋ฐฉ๋ฒ

์ด๋ฒคํธ ๋ฐ์์ ํธ์ถํ๋ ํจ์์ setTimeout()์ ์ค์ ์ผ์ ์๊ฐ์ดํ์ ์ด๋ฒคํธ๋ง ์ ํจํ๊ฒ ๋ง๋ค๋ฉด ๋ฉ๋๋ค.

## 3.1 ์์: ๊ธ ์๋ ์ ์ฅ

์๊น ์ฒ์ ์ธ๊ธํ๋ keyup ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ  ๋๋ง๋ค ๊ธ์ ์ ์ฅํ๋ ํจ์ ํธ์ถ ๋ฌธ์ ๋ ๋๋ฐ์ด์ฑ์ผ๋ก ํด๊ฒฐํ  ์ ์์ต๋๋ค.

```js
textarea.addEventListener('keyup', async (e) => {
    let timer

    if(timer){
        clearTimeout(timer)
    }

    timer = setTimeout(
      postApi({
        ...this.state
        content: e.target.value
    }),์ผ์ ์๊ฐ)
})
```

'์ํฌ์ํธ'๋ฅผ ๋ค ์๋ ฅํ  ๋๊น์ง ์ฌ๋ฌ๋ฒ keyup ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ๋๋ฐ, ์ด๋ setTimeout() ํจ์๋ฅผ ์ฐ๋ฉด ์ผ์ ์๊ฐ์ด ์ง๋  ๋๊น์ง ํจ์ ํธ์ถ์ ํ์ง ์์ต๋๋ค.

์ฌ๊ธฐ์ ์ค์ํ ์ ์ setTimeout()์ด ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ  ๋๋ง๋ค ์๋ก ๋ง๋ค์ด์ง๋ค๋ ์ฌ์ค์๋๋ค.

์ ๋๋ฐ์ด์ค ๊ธฐ๋ฒ์ผ๋ก ๋ง๋  ์ฝ๋๋ฅผ ๋ณด๋ฉด , ๋จผ์  setTimeout ํจ์๋ฅผ ๋ณ์์ (์ฌ๊ธฐ์  timer) ๋ด์ ์ด๊ธฐํํ๊ณ , ํน์ ์ด์ ์ ๋ง๋  ๋ณ์๊ฐ ์กด์ฌํ๋ค๋ฉด ํด๋น ๋ณ์์ setTimeout()์ ์์ ๊ณ  ์๋ก setTimeout()์ ๋ง๋ค์ด ์ค๋๋ค.

๋ง์ฝ clearTimeout(timer)๋ฅผ ํ์ง ์๋ ๋ค๋ฉด 10๊ฐ์ ์ด๋ฒคํธ๋ง๋ค setTimeout์ด ์กด์ฌํ๊ฒ ๋๋ฏ๋ก ๊ผญ ํ์ํ ์ฝ๋์๋๋ค.
<br><br>

## โ  ์ฃผ์

> ํน์ ํท๊ฐ๋ฆฌ์  ๋ถ์ด ์์์ง ๋ชจ๋ฅด๊ฒ ์ง๋ง

์  ์ฒ์์ setTimeout()์ ๋๋ฒ์งธ ๋งค๊ฐ๋ณ์๋ก ์ฃผ๋ ์ง์ฐ์๊ฐ์ด ๊ธ์ ๋ค ์๋ ฅํ  ๋๊น์ง ํ์ํ ์๊ฐ์ด๋ผ๊ณ  ์๊ฐํ์ต๋๋ค. ๊ทธ๋์ 10000, ์ฆ 10์ด๋ฅผ ์คฌ๋๋ฐ, ํด๋น ์๊ฐ์ keyup์ ๋ง์ง๋ง์ผ๋ก ํ๊ณ  ๋ ๋ค์ ์๋ฌด๊ฒ๋ ํ์ง ์์ ์๊ฐ์ ์๋ฏธํ์ต๋๋ค. ๊ทธ๋์ 300, 0.3์ด์ ๋๋ฉด ์ถฉ๋ถํ์ต๋๋ค.

<br><br>

## 3.2 ์์: ํ์๊ฐ์ ์ ํจ์ฑ ๊ฒ์ฌ

ํ์ ๊ฐ์์ ์๋ ฅ๊ฐ์ ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ํ  ๋๋ ๋๋ฐ์ด์ฑ์ ์ฌ์ฉํ  ์ ์์ต๋๋ค.

<p class="codepen" data-height="300" data-default-tab="result" data-slug-hash="mVGVOL" data-user="dcorb" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/dcorb/pen/mVGVOL">
  Debouncing keystrokes Example</a> by Corbacho (<a href="https://codepen.io/dcorb">@dcorb</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

๊ธ์๊ฐ ์ ๋ถ ์๋ ฅ๋์ ๋ ๋ง์ง๋ง ๊ฒฐ๊ณผ๋ฌผ๋ง ๊ฒ์ฌ๋ฅผ ํ๋ฉด ํจ์จ์ฑ์ ๋์ผ ์ ์์ต๋๋ค.

<br/><br/>

## 3.3 ์์: ์ฐฝ์กฐ์ 

์ฐฝ์กฐ์  ์ด๋ฒคํธ์๋ ๋๋ฐ์ด์ค๋ฅผ ์ฌ์ฉํ๋ฉด ํจ์จ์ ์๋๋ค. ์ฐฝ ๋๋น๊ฐ 500px๋ฏธ๋ง์ด ๋๋ฉด ๋ ์กฐ์ ํจ์๋ฅผ ํธ์ถํ  ๋ ๋ง์ฝ resize๋  ๋๋ง๋ค ์กฐ๊ฑด๋ฌธ์ ์คํํ๋ฉด 500px์ดํ์์ ์ฐฝ์กฐ์ ์ ๋ถํ์ํ ํจ์ ํธ์ถ์ด ๋ฐ์ํฉ๋๋ค.

์ด์งํผ ์ต์ข์ ์ผ๋ก ๊ฒฐ์ ํ ์ฐฝํฌ๊ธฐ๋ง์ด ์ ์๋ฏธํ๊ธฐ ๋๋ฌธ์ ๋ค์๊ณผ ๊ฐ์ด ๋๋ฐ์ด์ฑ์ ํด์ ์ต์ ํ๋ฅผ ํด์ค์ผ ํฉ๋๋ค. 
์๋ ์์๋ฅผ ๋ณด๋ฉด ์์๊ฒ ์ง๋ง ๋๋ฐ์ด์ค๋ฅผ ์ฃผ๊ณ  ์ ์ฃผ๊ณ ์ ๋ฐ๋ผ ์ฐฝ์กฐ์  ์ด๋ฒคํธ์ ํ์๊ฐ ํ์ฐํ ์ฐจ์ด๊ฐ ๋ฉ๋๋ค.


<p class="codepen" data-height="300" data-default-tab="result" data-slug-hash="XXPjpd" data-user="dcorb" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/dcorb/pen/XXPjpd">
  Debounce Resize Event Example</a> by Corbacho (<a href="https://codepen.io/dcorb">@dcorb</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>


์ด๋ฒ์ ํด๋ก์ ๋ฅผ ์ด์ฉํด์ ๋ชจ๋ํ๋ ๋๋ฐ์ด์ค ํจ์๋ฅผ ์ฌ์ฉํด๋ณด๊ฒ ์ต๋๋ค.

```js
const debounce = (fn, delay) => {
  let timer = null
  return function () {
    clearTimeout(timer)

    timer = setTimeout(fn, delay)
  }
}
```

๋๋ฐ์ด์ค ํจ์์ ๋งค๊ฐ๋ณ์๋ก ์ด๋ฒคํธ ๋ฐ์์ ํธ์ถํ  ํจ์์, ๋ง์ง๋ง ์ด๋ฒคํธ๋ก๋ถํฐ ์ง์ฐ์๊ฐ์ ๋ฐ์ต๋๋ค. ์๋ฆฌ๋ ์๊น์ ๊ฐ์ง๋ง ์ฌ์ฌ์ฉ์ฑ, ๊ฐ๋์ฑ์ด ์ข๊ธฐ์ ์ถ์ฒํ๋ ๋ฐฉ๋ฒ์๋๋ค.

```js
const fn = () => {
  ๋์กฐ์
}

window.addEventListener('resize', (e) => {
  if (window.innerWidth < 500) {
    debounce(fn, 300)
  }
})
```

์ฐฝ์กฐ์  ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ  ๋ ํธ์ถํ  ํจ์๋ฅผ ๋๋ฐ์ด์ค๋ก ๊ฐ์ธ์ฃผ๋ฉด ํด๋น ์ฐฝ์กฐ์ ์ด ๋์ด์ ์ผ์ด๋์ง ์์ ๋ ์ผ์ ์๊ฐ ์ดํ ๋์กฐ์์ ํฉ๋๋ค.

ํ์ง๋ง ๋๋ฐ์ด์ค ๋ฐฉ๋ฒ์ ๋ฌธ์ ์ ์ด ์๋๋ฐ์ ์ด๋ฒคํธ ๋ฐ์์ค์๋ ํจ์๋ฅผ ํธ์ถํด ๊ฒฐ๊ณผ๋ฌผ์ ๋ณด์ฌ์ค์ผ ํ  ๋์๋๋ค. 

์๋ฅผ ๋ค์ด ๋ฌดํ ์คํฌ๋กค์ ๊ฒฝ์ฐ ์คํฌ๋กค ์ด๋ฒคํธ๋ฅผ ์ฃผ๋ ๋์์๋ ํจ์๋ฅผ ํธ์ถํด์ผ ๋๋๋ฐ, ๋๋ฐ์ด์ค๋ ์ด๋ฒคํธ๊ฐ ๋๋  ๋๋ง api๋ฅผ ํธ์ถํ๊ธฐ ๋๋ฌธ์ ์ฌ์ฉ์๋ ์คํฌ๋กค์ ๋ฉ์ถ ๋๊น์ง ์๋ฌด ๊ฒ๋ ๋ณผ ์ ์์ต๋๋ค.

<br/><br/>

# 4 Throttle

> ์ผ์  ์๊ฐ ๋์ ํ๋ฒ๋ง ์คํ๋๋๋ก ์ ํ

๊ทธ๋ด ๋ ์ฐ๋กํ์ ์ฌ์ฉํ๋ฉด ๋ฉ๋๋ค. ์ฐ๋กํ์ ์ผ์  ๊ธฐ๊ฐ์ ์ฃผ๊ธฐ๋ก ํจ์ ํธ์ถ์ ๋ณด์ฅํฉ๋๋ค. ๋๋ฌธ์ ๋๋ฐ์ด์ค์ ๋ฌ๋ฆฌ ๋ง์ง๋ง ์ด๋ฒคํธ๋ฅผ ๊ธฐ๋ค๋ฆด ํ์๊ฐ ์๊ณ  ๊ณ์ ์คํฌ๋กค ์ด๋ฒคํธ๋ฅผ ์ฃผ๋๋ผ๋ ์ค๊ฐ์ค๊ฐ ํ์ด์ง๋ฅผ ๊ฐ์ ธ์ฌ ์ ์์ต๋๋ค.

![image](https://user-images.githubusercontent.com/79133602/165988811-2ce1f4f8-8bdc-4152-8441-0bf7bb970832.png)

## 5 Throttle ์ฌ์ฉ๋ฐฉ๋ฒ

์ด๋ฒคํธ๊ฐ ๋ฐ์ํ  ๋๋ง๋ค throttle์ ๋งค๊ฐ๋ณ์์ ํธ์ถํ  ํจ์์ ์ผ์ ์๊ฐ์ ์๋ ฅํฉ๋๋ค. ๊ทธ๋ฆฌ๊ณ  timer๊ฐ false์ธ ๊ฒฝ์ฐ์๋ง ์ผ์ ์๊ฐ์ด ์ง๋ ํ ํจ์๋ฅผ ์คํํฉ๋๋ค.

```js
const throttle = (fn, delay) => {
  return function () {
    let timer

    if (!timer) {
      timer = setTimeout(() => {
        fn()
        timer = null
      }, ์ผ์ ์๊ฐ)
    }
  }
}
```

์ฌ๊ธฐ์ ๋์ฌ๊ฒจ ๋ณผ ์ ์ timer๊ฐ false์ธ ์กฐ๊ฑด๋ฌธ์๋๋ค. ํด๋น ์กฐ๊ฑด ๋์ ์ฐ๋กํ์ ๋๋ฐ์ด์ค์ ๋ฌ๋ฆฌ ์ฒ์ ์ด๋ฒคํธ ์ดํ ๊ณ์ ์ด๋ฒคํธ๊ฐ ์ถ๊ฐ๋๋ ์ฒซ ์ด๋ฒคํธ์ ํจ์๋ฅผ ์คํํฉ๋๋ค. ๊ทธ๋ฆฌ๊ณ  ๋ค์ timer๋ฅผ null๋ก ๋ง๋ค๊ธฐ ๋๋ฌธ์ ์ผ์ ์๊ฐ๋ง๋ค ๋ค์ ์ด๋ฒคํธ๋ฅผ ๋ฐ์์ฌ ์ ์์ต๋๋ค.

## 5.1 ์์: ๋ธ์ ๋ชฉ์ฐจ

๋ธ์ ํด๋ก ์ผ๋ก ๋ง๋  ์ฌ์ดํธ์ ์ ๋ชฉ์ ํ์ฌ ๋๋ฐ์ด์ค๋ก ๋ฐ๋๊ณ  ์์ต๋๋ค.

![๋ธ์ ์ ๋ชฉ ๋๋ฐ์ด์ฑ](https://user-images.githubusercontent.com/79133602/166110501-3d1c76a6-c49f-42d8-af8c-98f25c4e2583.gif)

ํ์ง๋ง ์ค์  ์ฌ์ดํธ๋ ์ฐ๋กํ๋ง ๊ธฐ๋ฒ์ ์จ์ ๋ณ๊ฒฝ์ฌํญ์ ์ฌ์ฉ์๊ฐ ์ค์๊ฐ์ผ๋ก ๋ณผ ์ ์๊ฒ ํด์ค๋๋ค.

![๋ธ์ ์ ๋ชฉ ์ฐ๋กํ๋ง](https://user-images.githubusercontent.com/79133602/166110499-523b9cab-2d03-47f3-b140-c09d5301eca1.gif)

๋น์ฐํ ์ฌ์ฉ์ ์์ฅ์์  ์ฐ๋กํ๋ง ๊ธฐ๋ฒ์ด ๋ ํธํ๊ฒ ์ฃ .

<br/><br/>

# 6 ์ ๋ฆฌ

> ์ ์ฌ์ ์์ ๊ธฐ๋ฒ์ ์ฐพ์

๋๋ฐ์ด์ค์ ์ฐ๋กํ์ ์ฐจ์ด์ ์ ์ฐ์์ ์ธ ์ด๋ฒคํธ๊ฐ ๋๋๊ธธ ๊ธฐ๋ค๋ฆฌ๋์ ์ฌ๋ถ์๋๋ค. ๋๋ฐ์ด์ค๋ ์ฐ์์ ์ธ ์ด๋ฒคํธ๊ฐ ๋ค ๋๋๊ณ  ๋์ ๋ง์ง๋ง ์ด๋ฒคํธ์ ์ฝ๋ฐฑํจ์๋ง ์คํํ๊ณ , ์ฐ๋กํ์ ๋ง์ง๋ง ์ด๋ฒคํธ๋ฅผ ๊ธฐ๋ค๋ฆฌ๋ ๋์  ์ผ์ ์๊ฐ๋ง๋ค ์ด๋ฒคํธ์ ์ฝ๋ฐฑํจ์๋ฅผ ์คํํฉ๋๋ค.

์๋ ์์์ฒ๋ผ ๋ชจ๋  ์ด๋ฒคํธ๋ฅผ ์ฒ๋ฆฌํ  ๋, ์ฐ๋กํ ๊ธฐ๋ฒ์ ์ธ ๋, ๋๋ฐ์ด์ค ๊ธฐ๋ฒ์ ์ธ ๋ ์ด๋ฒคํธ ๋ด ํจ์๋ฅผ ์คํํ๋ ํ์๋ ๋ค๋ฆ๋๋ค.

<p class="codepen" data-height="300" data-default-tab="result" data-slug-hash="jXrYQz" data-user="jaehee" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jaehee/pen/jXrYQz">
  The Difference Between Throttling, Debouncing, and Neither</a> by jaeheekim (<a href="https://codepen.io/jaehee">@jaehee</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

๊ทธ๋ฌ๋ฏ๋ก ์ต์ ํ๊ฐ ํ์ํ์ง๋ง ์ฃผ๊ธฐ์ ์ธ ์ด๋ฒคํธ ํธ์ถ์ด ํ์ํ๋ค๋ฉด ์ฐ๋กํ์ ๋ง์ง๋ง ์ด๋ฒคํธ๋ง ์ ํจํ๊ฒ ํ๊ณ  ์ถ๋ค๋ฉด ๋๋ฐ์ด์ค๋ฅผ ์ ํํด์ผ ํฉ๋๋ค.

> ๋๋ฐ์ด์ค๊ฐ ํ์ํ์ง๋ง ๋ง์ง๋ง์ ๊ธฐ๋ค๋ฆฌ๊ณ  ์ถ์ง ์๋ค๋ฉด?

๋ง์ฝ ๋๋ฐ์ด์ค๋ฅผ ์ฐ๊ณ  ์ถ์ง๋ง ํ๋ฉด์ด ๋ฐ ๋๊น์ง ๊ธฐ๋ค๋ฆฌ๋ ๊ฒ ์ซ๋ค๋ฉด ๋๊ด์  ์๋ฐ์ดํธ๋ฅผ ํ๋ฉด ๋ฉ๋๋ค.

## ๋๊ด์  ์๋ฐ์ดํธ

> ๋ฐ์ดํฐ ์ถ๊ฐ ์์ฒญ ์ฑ๊ณต์ฌ๋ถ์ ์๊ด์์ด ์๋ฐ์ดํธ

๋๊ด์  ์๋ฐ์ดํธ๋ ๋ฐ์ดํฐ ์๋ ฅ ํ ํด๋น ๋ฐ์ดํฐ๊ฐ ์๋ฒ์ ์ ๋ค์ด๊ฐ๋์ง ํ์ธํ์ง ์๊ณ  ์ง์  ๋ฐ์ดํฐ๋ฅผ ๊ธฐ์กด ๋ฐ์ดํฐ์ ์ถ๊ฐํด์ฃผ๋ ๊ธฐ๋ฒ์๋๋ค.

## ์์: ๊ธ ์๋ ์ ์ฅ

๋ง์ฝ ๊ธ์ ์๋์ ์ฅํ  ๋ post ์์ฒญ ํ get์์ฒญ์ผ๋ก ๊ฐ์ ๋ฐ์ ๋๊น์ง ํ๋ฉด ๋ณ๊ฒฝ์ด ๋์ง ์๋๋ค๋ฉด ์ฌ์ฉ์ ์์ฅ์์ ๋ถํธํ  ๊ฒ์๋๋ค. ์ด๋ ๋ค์๊ณผ ๊ฐ์ด post์์ฒญ ์  ์๋ ฅ๊ฐ์ ํ์ฌ state์ ์ถ๊ฐํ๋ setStateํจ์๋ฅผ ์ฐ๋ฉด ๋๊ด์  ์๋ฐ์ดํธ๋ฅผ ํ  ์ ์์ต๋๋ค.

```js
timer = setTimeout(
  this.setState({
    ...this.state,
    content: e.target.value
  })
  postApi({
      ...this.state
      content: e.target.value
}),์ผ์ ์๊ฐ)
```

์ด๊ฒฝ์ฐ get ์์ฒญ์ด ์คํ๋  ๋๊น์ง ๊ธฐ๋ค๋ฆด ํ์๊ฐ ์๊ณ  ์ฆ๊ฐ์ ์ผ๋ก ๋ณ๊ฒฝ์ฌํญ์ด ๋ฐ์๋๊ธฐ ๋๋ฌธ์ ์ฌ์ฉ์ ์นํ์ ์ธ ์ฌ์ดํธ๋ฅผ ๊ตฌํํ  ์ ์์ต๋๋ค.

<br/><br/><br/>

# 5 ์ฐธ๊ณ 

๐ป [๋๋ฐ์ด์ค์ ์ค๋กํ ๊ทธ๋ฆฌ๊ณ  ์ฐจ์ด์ ](https://webclub.tistory.com/607)

๐ป [Throttle, Debounce ๊ฐ๋ ์ก๊ธฐ](https://medium.com/@progjh/throttle-debounce-%EA%B0%9C%EB%85%90-%EC%9E%A1%EA%B8%B0-19cea2e85a9f)

๐ป [์ฐ๋กํ๋ง๊ณผ ๋๋ฐ์ด์ฑ](https://www.zerocho.com/category/JavaScript/post/59a8e9cb15ac0000182794fa)

๐ป [CodePen .debounce vs \*.throttle](https://codepen.io/sumolari/pen/MWKzwgb)

๐ป [CodePen Debouncing keystrokes](https://codepen.io/dcorb/pen/mVGVOL)

๐ป [CodePen_Debounce Resize](https://codepen.io/dcorb/pen/XXPjpd)

๐ป [CodePen The Difference Between Throttling, Debouncing, and Neither](https://codepen.io/jaehee/pen/jXrYQz)
