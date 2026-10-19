# 🦊 NFox Online Tuner

<p align="center">
  <img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64' width='96' height='96'%3E%3Cdefs%3E%3ClinearGradient id='foxG' x1='0%25' y1='0%25' x2='100%25' y2='100%25'%3E%3Cstop offset='0%25' stop-color='%23fb923c'/%3E%3Cstop offset='100%25' stop-color='%23ea580c'/%3E%3C/linearGradient%3E%3C/defs%3E%3Cpath fill='url(%23foxG)' d='M32 54 L10 14 L24 38 L32 30 L40 38 L54 14 Z'/%3E%3Ccircle cx='24' cy='36' r='3' fill='%23ffffff'/%3E%3Ccircle cx='40' cy='36' r='3' fill='%23ffffff'/%3E%3Ccircle cx='32' cy='47' r='3.5' fill='%2310b981'/%3E%3C/svg%3E" alt="NFox Online Tuner Logo" />
</p>

<p align="center">
  <strong>Современный высокоточный веб-тюнер для гитар и струнных инструментов</strong><br>
  Работает прямо в браузере через микрофон с нулевой задержкой и без отправки аудио на сервер.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white" alt="HTML5">
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white" alt="CSS3">
  <img src="https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black" alt="JavaScript">
  <img src="https://img.shields.io/badge/Web_Audio_API-DSP-43B02A?style=flat-square" alt="Web Audio API">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square" alt="License MIT">
</p>

---

## 🌟 Возможности

- 🎯 **Субгерцевая точность**: Алгоритм автокорреляции (YIN / Autocorrelation) с параболической интерполяцией пика определяет высоту тона даже в шумной обстановке.
- 🎨 **Две темы оформления**:
  - **Dark Studio** — классическая тёмная студийная тема с неоновыми акцентами.
  - **Soft Sakura** — нежная пастельно-розовая светлая тема.
  - Сохранение выбранной темы в `localStorage`.
- 🎸 **Широкая поддержка инструментов и строев**:
  - **6-струнная гитара**: Standard (EADGBE), Drop D, Eb (Half-Step Down), Drop C, Open D, Open G, DADGAD.
  - **7-струнная гитара**: Standard 7-string, Drop A.
  - **Бас-гитара**: 4-струнный Standard, Drop D, 5-струнный Standard (Low B).
  - **Укулеле**: Сопрано/Концерт (GCEA), D-строй.
- 🔊 **Встроенный акустический синтезатор**: Воспроизведение натурального щипка для каждой струны или кнопка «Проиграть все» для настройки инструмента на слух.
- 🎚️ **Калибровка и шумоподавление**: Регулировка эталона камертона $A_4$ (430–450 Гц) и настраиваемый Noise Gate.
- 📱 **Полная адаптивность**: Удобно пользоваться как со смартфона, так и с планшета или компьютера.

---

## 📁 Структура проекта

Репозиторий имеет чистую архитектуру без сборщиков и зависимостей (Zero Dependencies):

```text
nfox-online-tuner/
├── index.html       # Структура, разметка, Favicon и логика Web Audio API
├── style.css        # Таблицы стилей, темы (Dark / Sakura) и адаптивность
└── README.md        # Документация проекта
```

---

## 🚀 Быстрый запуск

### Вариант 1: Запуск локально
Так как проект использует микрофон через Web Audio API, браузеры требуют безопасный контекст (`localhost` или `HTTPS`):

1. Склонируйте репозиторий:
   ```bash
   git clone https://github.com/your-username/nfox-online-tuner.git
   cd nfox-online-tuner
   ```
2. Откройте локальный сервер через любую удобную утилиту:
   - **VS Code**: установите плагин **Live Server** и нажмите `Go Live`.
   - **Node.js**:
     ```bash
     npx serve .
     ```
   - **Python 3**:
     ```bash
     python -m http.server 8000
     ```
3. Откройте адрес `http://localhost:8000` в браузере.

---

## 🌐 Размещение на GitHub Pages (Бесплатный хостинг)

Сайт полностью статичен и готов к моментальной публикации на GitHub Pages:

1. Загрузите файлы `index.html` и `style.css` в корень репозитория (`main` ветка).
2. Перейдите во вкладку **Settings** вашего репозитория на GitHub.
3. В левом меню выберите раздел **Pages**.
4. В блоке **Build and deployment**:
   - **Source**: `Deploy from a branch`
   - **Branch**: выберите ветку `main` и папку `/(root)`
   - Нажмите кнопку **Save**.
5. Через 1–2 минуты страница станет доступна по адресу:
   ```text
   https://<ваш-логин>.github.io/<имя-репозитория>/
   ```

---

## 🛠️ Как это работает (Технические детали)

1. **Захват аудиопотока**: `navigator.mediaDevices.getUserMedia` с отключённым автоматическим шумоподавлением и эхоподавлением для сохранения естественных гармоник струн.
2. **Аудиофильтрация**: `BiquadFilterNode` (Low-pass) срезает высокие гармоники медиатора и дребезг ладов выше 1000 Гц.
3. **Анализ шага частоты**: Массив данных временной области анализируется методом автокорреляции. Функция находит смещение (lag), при котором автокорреляция максимальна, а параболическая интерполяция трёх соседних точек восстанавливает дробное значение периода колебания:
   $$\Delta = \frac{x_3 - x_1}{2(2x_2 - x_1 - x_3)}$$
4. **Сглаживание стрелки**: Применяется экспоненциальное сглаживание показаний для устранения дрожания шкалы в реальном времени.

---

## 📋 Требования к окружению

- Современный браузер с поддержкой HTML5 и Web Audio API (Google Chrome, Firefox, Safari, Edge, Opera, мобильные браузеры на iOS и Android).
- Подключение по протоколу **HTTPS** (или `localhost`) для разрешения доступа к микрофону.

---

## 📄 Лицензия

Проект распространяется под свободной лицензией [MIT](LICENSE). Вы можете свободно использовать, модифицировать и публиковать его код.