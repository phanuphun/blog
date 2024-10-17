---
title: Tailwind CSS
date: '2024-09-27'
description: บันทึกการใช้งาน Tailwind CSS
tag: Note
tags:
   - Tailwind
   - CSS Framework
img: /img/tailwind/cover.png
path: basicTailwind
draft: false
---

---

## เบื้องต้น

Tailwind CSS เป็น Framework CSS เพื่อใช้ในการออกแบบเว็บไซต์โดยใช้แนวทาง Utility-first ที่เน้นใช้ class ขนาดเล็กเพื่อกำหนดสไตล์โดยตรงให้กับ html

### โครงสร้างไฟล์สำคัญ

หลงจากการลง tailwind เสร็จแล้ว(ยกเว้นการใช้งานแบบ CDN) จะมีโครงสร้างไฟล์เพิ่มขึ้นมาเพื่อใช้ในการใช้งานตัว tailwind ดังนี้

-  **📁 tailwind.conf.js** : เป็นไฟล์สำคัญที่จำเป็นต้องมีทุกครั้งในการใช้งาน และที่สำคัญคือไฟล์นี้จะใช้ในการปรับแต่งตัว tailwind ของเรา เช่นการเพิ่ม plugin ไฟล์นี้จะถูกสร้างขึ้นมาเมื่อเรารันคำสั่ง `npx tailwindcss init`
-  **📁 input.css** : ไฟล์นี้เปรียบเสมือนไฟล์ต้นแบบที่เราจะนำเข้า directive ต่างๆดังนี้ **@tailwind base** , **@tailwind components** และ **@tailwind utilities** ในไฟล์นี้เมื่อเราทำการ build จะถูกนำไปเป็นต้นทางสำหรับการ build ไฟล์ css ออกมาตามคำสั่งด้านล่างในส่วนของ output เลย ทั้งนี้เรายังสามารถปรับแต่ง class ได้ในนี้อีกด้วย
-  **📁 src/output.css** : เป้นไฟล์ css ที่จะถูก build ออกมาในโฟลเดอร์ **src/** ในไฟล์ tailwind.css นี้จะเป็น css ที่ถูก build มาจาก utility classes ที่เราเลือกใช้ โดยเราสามารถกำหนด directory หรือชื่อใหม่ได้เช่น **dist/tailwind.css** โดยไฟล์นี้จะได้จากการที่เรารันคำสั่ง `npx tailwindcss -i ./src/input.css -o ./dist/tailwind.css --watch`
-  **📁 postcss.config.js** : หากตัวโปรเจ็คมีการใช้งาน PostCSS รวมกับ tailwind CSS เพื่อปรับปรุงการ build ไฟล์นี้จะถูกสร้างขึ้นมาเพื่อกำหนด plugin ต่างๆ

<ref-box>
   <b>เพิ่มเติมเกี่ยวกับ directive ของ tailwind</b>
   @tailwind base : สำหรับการตั้งค่าพื้นฐานและ CSS reset
   @tailwind components : สำหรับการจัดการกับ component-based CSS
   @tailwind utilities : สำหรับ utility classes ที่ใช้จัดการ styling เช่น margin, padding, ขนาดฟอนต์ และสีต่าง ๆ
</ref-box>

---

## เพิ่มต้น

### การใช้งาน Dark Mode

ใน tailwind css มีระบบ Dark Mode เพื่อรองรับการทำเว็บไซต์ที่มีทั้งโมหดมืดและสว่างได้อย่างง่ายดาย โดยใช้ utility class ที่จะรองรับการเปิดปิดโหมดมืดและสว่าง โดยจะมี 2 วิธีหลักๆ ดังนี้

**วิธีที่ 1 :** การใช้งานการตั้งค่าตามระบบปฏิบัติการของเราตามค่า **prefers-color-scheme** (เป็นหนึ่งใน CSS Media Query ที่ถูกออกแบบมาเพื่อรองรับการพัฒนาเว็บไซต์ที่ต้องการปรับเปลี่ยนธีมโหมดโดยเฉพาะ) ในโหมดนี้จะเป็นค่า Default ที่ถูกตั้งเอาไว้ การที่ธีมของเราจะเปลี่ยนได้จำเป็นต้องมีการระบุ class `dark:` ใน Element ที่เราต้องการเปลี่ยนโหมดก่อน จากโค้ดด้านล่างจะเป็นการกำหนดให้ตัวอักษรมีสีฟ้าซึ่งเป็นค่าเริ่มต้นหรือ **Light Mode** ที่เป็นค่าตรงข้ามจาก **Dark Mode** นั่นเอง กรณีนี้เราต้องการให้เปลี่ยนสีตัวอักษรเป็นสีแดงเมื่อเราเปลี่ยนธีมเป็น Dark Mode ก็ต่อเมื่อเราเปลี่ยนธรมในระบบปฏิบัติการของเรา

```html
<div class="text-blue-500 dark:text-red-500">Text</div>
```

**วิธีที่ 2 :** การเปิดใช้งานเอง จะเป็นการที่เราสามารถควบคุมการเปลี่ยนธีมได้ด้วยตัวเราเอง โดยอันดับแรกให้เราไปที่ไฟล์ **tailwind.conf.ts** ให้เราเพิ่ม key ที่ชื่อว่า `darkMode` ขึ้นมาโดยปกติค่าเริ่มต้นของ darkMode จะอยู่ที่ `media` ซึ่งก็คือวิธีการแรกที่เลือกการตามตั้งค่าของระบบปฏิบัติการของเรา แต่ในกรณีที่เราต้องการปรับเปลี่ยนเองให้เราเปลี่ยนเป็น `class` ดังนี้

```js
module.exports = {
   darkMode: "class",
};
```

จากนั้น ให้เราเพิ่ม class `dark` ไปที่แท็ก **html** หรือ **body** เพื่อบ่งบอกว่า Dark Mode ถูกเปิดใช้งานแล้ว เมื่อมีคลาส dark:class ถูกเพิ่มเข้าไป นั่นหมายความว่าโหมดนี้จะถูกเปิดทันที ซึ่งหมายความว่าหากเราต้องการจะทำให้ Light Mode เป็นค่าเริ่มต้นจำเป็นต้องมีส่วนที่เราสามารถควบคุมการเพิ่มและลบ class ในส่วนของคลาส dark ออกไป และการที่เราจะทำแบบนั้นได้เราจำเป็นต้องใช้ javascript เข้าช่วยอีกทีนั่นเอง

```html
<html class="dark">
   <div class="text-blue-500 dark:text-red-500">Text</div>
</html>
```

```js
// JavaScript สำหรับสลับโหมดมืด
const toggleDarkMode = () => {
   const bodyElement = document.body;
   bodyElement.classList.toggle("dark");
};
```

### การทำ Responsive

ในการทำ Responsive เพื่อรองรับอุปกรณ์หลายๆขนาด ทาง tailwind ก็มีการรองรับไว้อย่างดีพร้อมด้วย **Breakpoint** ที่เตรียมเอาไว้ให้เรา โดยการทำ Reponsive ด้วย Breakpoint ของทาง tailwind จะอยู่ในคอรเซ็ป **Mobile First** ซึ่งก็คือให้คำนึงถึงการใช้มือถือก่อนนั่นเอง จากนั้นก็ใช้ Breakpoint ในการเปลี่ยนแปลงตามแต่ละขนาด
<image-box
detail="https://blog.yunusemre.dev/responsive-design-with-tailwind/"
:src="https://blog.yunusemre.dev/_astro/mobile-first.bafe34e3_22KpYS.webp">
</image-box>

นอกจากนี้เรายังสามารถกำหนดขนาดของ Breakpoint ได้ด้วยตัวเราเองอีกด้วยโดยสามารถกำหนดได้ในไฟล์ **tailwind.conf.js** โดยที่ในส่วนของการตั้งค่าด้านล่างหากเราต้องการใช้ **prefix** เดิมของทาง tailwind เราก็สามารถ Override ได้เลยโดยการเปลี่ยนขนาดในส่วนที่เป็น **theme** → **screen**

หรือหากเราไม่ต้องการเปลี่ยนขนาดแต่ต้องการเปลี่ยน prefix ก็สามารถทำได้เช่นกัน และในกรณีที่เราต้องการสร้าง **prefix** ใหม่ด้วยขนาดของเราเองสามารถทำได้ใน **theme** → **extend** → **screen** จากนั้นก็เพิ่ม Breakpoint ที่เราต้องการได้เลย

```js
module.exports = {
   theme: {
      screens: {
         sm: "640px",
         md: "768px",
         lg: "1024px",
         xl: "1280px",
         "2xl": "1536px",
      },
      extend: {
         screens: {
            nDevice: "2000px",
         },
      },
   },
};
```

**ตัวอย่างการใช้งาน :**
ในตัวอย่างจะเป็นการเปลี่ยนสีข้อความตามขนาดต่างๆของหน้าจอของเรา โดยที่อย่างที่บอกเราจะคำนึงถึงขนาดหน้าจอโทรศัพท์ก่อนเป็นอันดับแรก จะเห็นได้ว่าเราไม่ได้ใส่ prefix อะไรให้มันซึ่งก็คือสีเริ่มต้นของขนาดหน้าจอโทรศัพท์ ส่วนเมื่อมาถึงขนาดหน้าจอ 768px หรือก็คือค่า Breakpoint ที่ตั้งไว้ของขนาด md ตัวอักษรก็จะเปลี่ยนเป็นสีน้ำเงินนั่นเอง และเมื่อขนาดหน้าจอของเราขยายไปอีกจนถึง Breakpoint ของขนาด lg ก็จะเปลี่ยนเป็นสีชมพู

```html
<div class="text-red-500 md:text-blue-500 lg:text-pink-500">text</div>
```
---

## การจัดวางเนื้อหาและเค้าโครงหน้า
### จัดเรียงเนื้อหาด้วย Column
เป็น utilities สำหรับการจัดเรียงเนื้อหาเป็นคอลัมน์หรือก็คือในแนวตั้ง โดยปกติแล้วคอลัมน์จะนิยมนำมาใช้งานกับการแสดงผลจำพวก ข้อความ รูปภาพ ซึ่งไม่ได้มีความซับซ้อน ง่ายต่อการใช้งาน และไม่นิยมนำมาจัดวางเค้าโครงหน้าเว็บแต่อย่างใด

- **รูปแบบการใช้งาน** เราสามารถกำหนด class `column-{จำนวนแถว}` ไดเลย เช่น `column-2` , `column-3` หรือ `column-xl` เป็นต้น
- **ตัวอย่างการใช้งาน** : ▶️[Masonry Gallery](https://www.youtube.com/watch?v=C8U86OIAJPE&ab_channel=FrontendOnly)

### จัดวางเค้าโครงด้วย Flexbox
### จัดวางเค้าโครงด้วย Grid



---
## ปลั๊กอินเสริม

### การใช้ Typography เพื่อแสดงผล Html
`tailwind typography` เป็นปลั๊กอินที่จะเพิ่มสไตล์มาให้กับเรา เพราะโดยปกติแล้วเมื่อเราใช้งาน tailwind การแสดงผลของ html จะยังไม่มีการเปลี่ยนแปลงอะไรจนกว่าเราจะเพิ่มคลาสให้กับแท็กนั้นๆ และเมื่อติดตั้งเสร็จแล้ว เราสามารถนำมาใช้กับการแสดงผลของ Markdown ที่ผ่านการแปลงมาเป็นแท็ก Html ได้โดยหากเราต้องการใช้งานก็เพียงเพิ่ม class `prose` ในส่วนของเนื้อหาได้เลย <br>

[📄เอกสารและการติดตั้ง ](https://github.com/tailwindlabs/tailwindcss-typography)<br>
[⚙️การปรับแต่งคลาส ](/blogs/basicnuxtcontent)

### การใช้ Line clamp เพื่อตัดบรรทัดข้อความ

`tailwind line clamp` เป็นปลั๊กอินส่วนเสริมที่จะช่วยเราในการตัดข้อความตามบรรทัดที่เราต้องการ โดยใช้คลาส `line-clamp-3` ตัวเลขด้านหลังคือจำนวนบรรทัดที่เราต้องการจะข้อความ <br>

 [📄เอกสารและการติดตั้ง ](https://github.com/tailwindlabs/tailwindcss-line-clamp)


---

## อ้างอิง

<ref-box>
   https://blog.yunusemre.dev/responsive-design-with-tailwind/
   https://github.com/tailwindlabs/tailwindcss-typography
   https://github.com/tailwindlabs/tailwindcss-line-clamp
   https://www.youtube.com/watch?v=C8U86OIAJPE&ab_channel=FrontendOnly
</ref-box>


