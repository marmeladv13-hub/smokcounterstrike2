# smokcounterstrike2
{ id: "d2_b", title: "B Site Smoke (Plat)", 
                  positionDesc: "T спавн, у бетонного блока слева", 
                  aimDesc: "Край окна на верхней платформе B", 
                  landDesc: "Дым ложится на B site возле платформы и ящиков" }
            ]
        },
        mirage: {
            name: "Mirage",
            icon: "fa-palmtree",
            smokes: [
                { id: "mrg_window", title: "Window Smoke (Mid)", 
                  positionDesc: "T аппартаменты, прижаться к подоконнику слева", 
                  aimDesc: "Верхний правый угол рамы окна (белая точка)", 
                  landDesc: "Дым точно в окно мида, отсекает AWP" },
                { id: "mrg_jungle", title: "Jungle Connector Smoke", 
                  positionDesc: "T спавн, за коробкой у стены", 
                  aimDesc: "Между пальмой и лампой в небе", 
                  landDesc: "Дым падает в джунгли, блокирует коннектор к A" },
                { id: "mrg_bshort", title: "B Short Smoke", 
                  positionDesc: "B аппартаменты, у скамейки", 
                  aimDesc: "В угол стены на выступе", 
                  landDesc: "Дым накрывает вход на B site со стороны шорта" }
            ]
        },
        inferno: {
            name: "Inferno",
            icon: "fa-fire",
            smokes: [
                { id: "inf_mid", title: "Mid Smoke (Top Mid)", 
                  positionDesc: "T спавн, возле фонтана, смотреть в арку", 
                  aimDesc: "В самую верхнюю точку арки (трещина)", 
                  landDesc: "Дым закрывает вид со стороны мида и второго мида" },
                { id: "inf_banana", title: "Banana Deep Smoke", 
                  positionDesc: "T аппартаменты, у бочки с правой стороны", 
                  aimDesc: "В вывеску ‘Truck’ над входом", 
                  landDesc: "Дым ложится перед входом на Banana, мешает заходу CT" },
                { id: "inf_asite", title: "A Site Pit Smoke", 
                  positionDesc: "Mid, у ящиков возле лестницы", 
                  aimDesc: "В угол дома (красная черепица)", 
                  landDesc: "Дым падает в яму на A site, закрывает обзор с питса" }
            ]
        }
    };

    // Текущее состояние
    let currentMapId = null;   // 'dust2', 'mirage', 'inferno'
    let currentSmoke = null;   // объект выбранного смока

    // DOM элементы
    const mapsGrid = document.getElementById('mapsGrid');
    const smokesPanel = document.getElementById('smokesPanel');
    const smokesGridContainer = document.getElementById('smokesGridContainer');
    const currentMapTitleSpan = document.getElementById('currentMapTitle');
    const backToMapsBtn = document.getElementById('backToMapsBtn');
    const modal = document.getElementById('smokeModal');
    const modalTitle = document.getElementById('modalTitle');
    const smokeCanvas = document.getElementById('smokeCanvas');
    const modalDetails = document.getElementById('modalDetails');
    const closeModalBtn = document.getElementById('closeModalBtn');

    // ---- Функция отрисовки canvas схемы (картинка: встать, целиться, результат) ----
    function drawSmokeDiagram(canvas, smokeData) {
        const ctx = canvas.getContext('2d');
        const w = canvas.width;
        const h = canvas.height;
        
        // Фон с текстурой
        ctx.fillStyle = "#121c26";
        ctx.fillRect(0, 0, w, h);
        // Сетка-декор
        ctx.strokeStyle = "#2e445e";
        ctx.lineWidth = 1;
        for (let i = 0; i < w; i += 40) {
            ctx.beginPath();
            ctx.moveTo(i, 0);
            ctx.lineTo(i, h);
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(0, i % h);
            ctx.lineTo(w, i % h);
            ctx.stroke();
        }
        
        // Заголовок сверху
        ctx.font = "bold 16px 'Inter', sans-serif";
        ctx.fillStyle = "#ffdd99";
        ctx.shadowBlur = 0;
        ctx.fillText(smokeData.title, w/2 - ctx.measureText(smokeData.title).width/2, 32);
        
        // Рисуем 3 зоны: (1) позиция игрока, (2) прицел, (
