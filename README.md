# 🥁 Барабаны сепараторов

Веб-приложение для учёта и замены барабанов на сепараторах. Данные синхронизируются с **Google Таблицей**, что позволяет работать с ними с любого устройства.

## 🚀 Возможности

- 📋 **Учёт сепараторов** — просмотр списка сепараторов с установленными барабанами (верх/низ).
- 🔄 **Замена барабанов** — быстрая замена барабана через всплывающее окно.
- 📦 **Управление складом** — добавление и удаление барабанов из общего списка.
- 📜 **История замен** — автоматическое сохранение всех действий с датой и временем.
- ☁️ **Синхронизация с Google Таблицей**:
    - **Чтение**: при открытии сайта данные загружаются из таблицы.
    - **Запись**: при нажатии кнопки «Сохранить в Google» изменения отправляются в таблицу.
- 📊 **Экспорт в Excel** — выгрузка актуальных данных в файл `.xlsx`.
- 📱 **Адаптивный дизайн** — удобная работа на компьютерах, планшетах и смартфонах.

## 🛠️ Технологии

- **Frontend**: HTML, CSS, JavaScript (чистый, без фреймворков).
- **База данных**: Google Таблицы (используются как облачное хранилище).
- **Бэкенд для записи**: Google Apps Script (обрабатывает запросы на запись в таблицу).
- **Библиотеки**:
    - [SheetJS](https://sheetjs.com/) — для работы с Excel-файлами.
    - [Google Sheets API](https://developers.google.com/sheets/api) — для чтения данных.

## 🌐 Демо-версия

Сайт доступен по адресу:  
[https://dmitryerosh.github.io/Seporator/](https://dmitryerosh.github.io/Seporator/)

## 🗄️ Структура Google Таблицы

Для работы требуется создать Google Таблицу с тремя листами:

### Лист «Сепараторы»
| Колонка A | Колонка B | Колонка C |
| :--- | :--- | :--- |
| **Сепаратор** | **Верх** | **Низ** |
| 3 | 1-1 | 2-7 |
| 4 | 1-2 | 2-2 |

### Лист «Барабаны»
| Колонка A |
| :--- |
| **Барабаны** |
| 1-2 |
| 1-3 |

### Лист «История»
| Колонка A | Колонка B |
| :--- | :--- |
| **Дата** | **Событие** |
| 2026-05-16 22:53:56 | Сепаратор 7 (Низ): — → 2-1 |

## 🚀 Настройка проекта

### 1. Создайте Google Таблицу
1. Перейдите на [Google Диск](https://drive.google.com) и создайте новую таблицу.
2. Создайте три листа с названиями: `Сепараторы`, `Барабаны`, `История`.
3. Заполните их данными по примеру выше.
4. Нажмите **«Открыть доступ»** и выберите **«Все, у кого есть ссылка»** → **«Читатель»**.

### 2. Получите ID таблицы
Скопируйте ID таблицы из URL:
https://docs.google.com/spreadsheets/d/ВАШ_ID_ТАБЛИЦЫ/edit

### 3. Включите Google Sheets API
1. Перейдите в [Google Cloud Console](https://console.cloud.google.com/).
2. Создайте проект или выберите существующий.
3. Включите **Google Sheets API**.
4. Создайте **API-ключ** и ограничьте его доступ к Google Sheets API.

### 4. Настройте Google Apps Script для записи
1. В вашей таблице перейдите в **«Расширения»** → **«Apps Script»**.
2. Вставьте код из `Код для Apps Script` (см. ниже).
3. Разверните скрипт как веб-приложение с доступом **«Все, у кого есть ссылка»**.
4. Скопируйте URL веб-приложения.

### 5. Подключите к сайту
1. В файле `index.html` найдите и заполните переменные в начале скрипта:
   ```javascript
   const SPREADSHEET_ID = 'ВАШ_ID_ТАБЛИЦЫ';
   const API_KEY = 'ВАШ_API_КЛЮЧ';
   const GOOGLE_SCRIPT_URL = 'URL_ВАШЕГО_ВЕБ_ПРИЛОЖЕНИЯ';

Сохраните изменения и загрузите файл на GitHub.

📄 Код для Google Apps Script
Скопируйте этот код в редактор Apps Script:
function doPost(e) {
  try {
    const data = JSON.parse(e.postData.contents);
    
    const sepSheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Сепараторы');
    sepSheet.clear();
    sepSheet.appendRow(['Сепаратор', 'Верх', 'Низ']);
    data.separators.forEach(row => sepSheet.appendRow([row.num, row.top, row.bottom]));
    
    const drumsSheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Барабаны');
    drumsSheet.clear();
    drumsSheet.appendRow(['Барабаны']);
    data.allDrums.forEach(d => drumsSheet.appendRow([d]));
    
    const historySheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('История');
    historySheet.clear();
    historySheet.appendRow(['Дата', 'Событие']);
    data.history.forEach(h => historySheet.appendRow([h.date, h.text]));
    
    return ContentService.createTextOutput(JSON.stringify({success: true}));
  } catch(e) {
    return ContentService.createTextOutput(JSON.stringify({success: false, error: e.toString()}));
  }
}

Seporator/
├── index.html          # Главная страница (вся логика)
├── style.css           # Стили
├── data.xlsx           # Резервный файл с данными
├── README.md           # Описание проекта
└── .github/
    └── workflows/      # GitHub Actions (для автоматизации)

    📌 Планы по развитию
Добавление авторизации и ролей

Редактирование истории замен

Визуализация статистики

📞 Контакты
Автор: DmitryErosh
Деплой: GitHub Pages

📄 Лицензия
MIT License — свободное использование и модификация.
