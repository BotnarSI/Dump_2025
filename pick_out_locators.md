# Dump_2025
Привет

Если инфы будет мало, жду в личку в телеге @sibotnar
Поехали


Начальный запрос в нейронку

Запрос - как скачать dom дерево сайта, но чтобы там остались только обьекты с которыми может взаимодействовать пользователь

Результат

Запрос - дополни, чтобы скрипт убирал в dom дереве все параметры style

Результат

Запрос - убери все родительские ноды, оставь только те внутри которых есть текст, кнопки и svg элементы

При необходимости повторяем итерации по корректировке вашего DOM и доводим примерно до такого результата(ниже). Скрипт ниже уже можно вставлять в консоль

===============================
// Функция для проверки, является ли узел значимым элементом
function isSignificant(node) {
    if (!node || !node.parentNode) return false;
    
    // Значимыми считаем: текстовые узлы, кнопки, ссылки, input'ы, img
    return node.nodeType === Node.TEXT_NODE ||
           ['BUTTON', 'A', 'INPUT', 'IMG'].includes(node.tagName.toUpperCase()) ||
           (node.hasChildNodes() && Array.from(node.childNodes).some(isSignificant));
}

// Создаем новый документ для фильтрованного DOM дерева
const filteredDoc = document.implementation.createHTMLDocument();

// Формируем новую структуру документа
const body = filteredDoc.body;
let nodesToKeep = [];

// Собираем список значимых элементов
Array.from(document.body.childNodes).forEach((child) => {
    if (isSignificant(child)) {
        nodesToKeep.push(child.cloneNode(true)); // Глубокая копия узла
    }
});

// Переносим отобранные элементы в новое тело документа
nodesToKeep.forEach((el) => {
    body.appendChild(el);
});

// Удаляем ненужные элементы (скрипты, стили, комментарии)
['script', 'style'].forEach(tag => {
    const elementsToRemove = filteredDoc.querySelectorAll(tag);
    elementsToRemove.forEach(el => el.remove());
});

// Удаляем все inline-стили (атрибуты style)
filteredDoc.querySelectorAll('[style]').forEach(el => {
    el.removeAttribute('style'); // удаляет атрибут style
});

// Дополнительная обработка: удаление путей path[d]
filteredDoc.querySelectorAll('path[d]').forEach(pathEl => {
    pathEl.remove(); // Удаляем путь
});

// Удаляем все блоки linearGradient
filteredDoc.querySelectorAll('linearGradient').forEach(gradientEl => {
    gradientEl.remove(); // Удаляем блок линейного градиента
});

// Наконец, удаляем все SVG-элементы
filteredDoc.querySelectorAll('svg').forEach(svgEl => {
    svgEl.remove(); // Удаляем весь SVG
});

// Удаляем классы у всех элементов
filteredDoc.querySelectorAll('*').forEach(el => {
    el.classList.length > 0 && el.removeAttribute('class'); // Удаляем класс
});

// Удаляем пустые DIV-теги
filteredDoc.querySelectorAll('div').forEach(divEl => {
    if (!divEl.textContent.trim() && !divEl.hasChildNodes()) {
        divEl.remove(); // Удаляем пустой div
    }
});

// Сохранение файла
const htmlContent = new Blob([filteredDoc.documentElement.outerHTML], { type: 'text/html' });
const link = document.createElement('a');
link.href = window.URL.createObjectURL(htmlContent);
link.download = 'filtered-dom-final.html';
document.body.appendChild(link); // Нужно добавить ссылку в DOM перед кликом
link.click(); // Автоматически инициируем загрузку файла
document.body.removeChild(link); // Убираем временную ссылку

console.log("Файл сохранён!");

===============================

Можем пересохранить в txt к примеру и отправляем обратно нейронке

Запрос - оформи абсолютно все локаторы из данного файла используя pytest selenium xpath  на выходе получаем долгожданный список с локаторами
