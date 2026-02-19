<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Estudio Pro - Academia de Matemáticas</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Tipografía clara y moderna */
        body {
            font-family: 'Calibri', 'Segoe UI', 'Candara', 'Open Sans', sans-serif;
            background-color: #f1f5f9;
            color: #1e293b;
            overflow-x: hidden;
        }

        /* Pizarra Verde Académica */
        .chalkboard {
            background-color: #1a3a2a; 
            border: 18px solid #4e342e; 
            border-radius: 1rem;
            box-shadow: inset 0 0 100px rgba(0,0,0,0.8), 0 25px 50px rgba(0,0,0,0.4);
            position: relative;
            color: #ffffff;
            background-image: url('https://www.transparenttextures.com/patterns/black-chalkboard.png');
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 620px; 
            padding: 35px;
        }

        .minimal-card {
            background: #ffffff;
            border-radius: 1.5rem;
            border: 2px solid #e2e8f0;
            transition: all 0.3s ease;
            position: relative;
        }

        .minimal-card:hover {
            border-color: #6366f1;
            transform: translateY(-4px);
        }

        .btn-action {
            border-radius: 1rem;
            font-weight: 800;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 3px solid #1e293b;
            background: white;
            box-shadow: 5px 5px 0px 0px #1e293b;
            text-transform: uppercase;
        }

        .btn-action:active {
            transform: translate(2px, 2px);
            box-shadow: 0px 0px 0px 0px #1e293b;
        }

        .btn-primary { background: #6366f1; color: white; border-color: #1e293b; }
        .btn-danger { background: #f43f5e; color: white; border-color: #1e293b; }
        .btn-success { background: #10b981; color: white; border-color: #1e293b; }

        .progress-container {
            height: 16px;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 8px;
            overflow: hidden;
            width: 180px;
        }

        .progress-fill {
            height: 100%;
            background: #facc15;
            transition: width 0.6s ease;
        }

        .fraction {
            display: inline-flex;
            flex-direction: column;
            vertical-align: middle;
            text-align: center;
            line-height: 1.1;
        }
        .fraction .top { border-bottom: 5px solid white; padding-bottom: 6px; margin-bottom: 6px; }
        .fraction .bottom { padding-top: 6px; }

        .problem-text {
            width: 100%;
            max-width: 800px;
            text-align: center;
            line-height: 1.6;
            font-size: 1.3rem; 
            font-weight: 500;
            color: rgba(255,255,255,0.95);
        }

        .math-big {
            font-size: 3.5rem; 
            font-weight: 800;
            letter-spacing: -1px;
            white-space: nowrap;
        }

        /* Diploma */
        .diploma-bg {
            background: #fffdf5;
            border: 12px double #92400e;
            padding: 60px;
            position: relative;
            box-shadow: 0 20px 50px rgba(0,0,0,0.1);
            border-radius: 4px;
        }
        .diploma-seal {
            width: 110px;
            height: 110px;
            background: #fbbf24;
            border: 4px solid #92400e;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            position: absolute;
            bottom: 40px;
            right: 40px;
            font-weight: 900;
            color: #92400e;
            transform: rotate(-10deg);
        }

        .hidden-view { display: none !important; }
        
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.85);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 100;
        }

        .expert-badge {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 1.5rem;
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.1));
        }
    </style>
</head>
<body class="min-h-screen">

    <header class="max-w-7xl mx-auto p-6 flex flex-col md:flex-row items-center justify-between gap-6 relative z-10">
        <div class="flex items-center gap-4 cursor-pointer" onclick="goHome()">
            <div class="w-14 h-14 bg-indigo-600 rounded-2xl flex items-center justify-center text-white shadow-xl rotate-2 border-2 border-indigo-900">
                <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><path d="M12 2v20M2 12h20"/></svg>
            </div>
            <div>
                <h1 class="text-3xl font-black text-slate-800 tracking-tighter">ESTUDIO PRO</h1>
                <p class="text-[10px] font-bold text-indigo-500 uppercase tracking-widest">Matemáticas Avanzadas</p>
            </div>
        </div>

        <div class="flex items-center gap-8 bg-white p-4 px-8 rounded-full border-2 border-slate-100 shadow-sm">
            <div class="flex flex-col items-end">
                <span class="text-[9px] font-black uppercase text-slate-400 tracking-wider">Maestría Global</span>
                <div class="progress-container mt-1">
                    <div id="mastery-fill" class="progress-fill" style="width: 0%"></div>
                </div>
            </div>
            <div class="flex flex-col items-center">
                <span class="text-[9px] font-black uppercase text-slate-400">Racha</span>
                <span id="streak-count" class="text-2xl font-black text-indigo-600">0</span>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-6 py-4 relative z-10">
        
        <!-- INICIO -->
        <div id="view-home">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 mb-12">
                <div class="lg:col-span-2 minimal-card p-10 bg-white flex flex-col justify-center border-b-8 border-indigo-50">
                    <h2 class="text-4xl font-black text-slate-800 mb-2 tracking-tight">Módulos de Práctica</h2>
                    <p class="text-slate-500 font-semibold text-lg leading-relaxed">Consigue 10 puntos en cada módulo. En los niveles avanzados, debes ganar al menos 2 puntos en modo Experto.</p>
                    <button id="btn-reset" onclick="resetProgress()" class="hidden w-fit mt-6 text-rose-500 font-bold text-xs uppercase hover:underline">Reiniciar Academia</button>
                </div>
                <div id="card-rank" class="minimal-card p-10 flex flex-col items-center justify-center text-center bg-slate-800 text-white border-none shadow-xl transition-all">
                    <div id="rank-icon" class="text-7xl mb-4">🌱</div>
                    <span class="text-[10px] font-black uppercase opacity-60 tracking-widest">Tu Estatus</span>
                    <h3 id="rank-name" class="text-xl font-black mt-1 uppercase">Novato</h3>
                    <button id="btn-claim-diploma" onclick="openNameModal()" class="hidden-view mt-4 bg-amber-400 text-slate-900 px-6 py-2 rounded-xl font-black text-xs uppercase shadow-lg animate-bounce">¡Canjear Diploma!</button>
                </div>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8" id="grid-categories"></div>
        </div>

        <!-- NIVEL -->
        <div id="view-diff" class="hidden-view text-center max-w-4xl mx-auto py-12">
            <h2 class="text-5xl font-black mb-16 text-slate-800 tracking-tight">Selecciona tu Nivel</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-10 px-6">
                <button onclick="startExercise('aprendiz')" class="minimal-card p-14 hover:border-indigo-600 group bg-white shadow-lg">
                    <div class="text-7xl mb-8 transition-transform group-hover:scale-110">📗</div>
                    <h3 class="text-3xl font-black mb-2 uppercase">Aprendiz</h3>
                    <p class="text-slate-400 font-bold text-lg">Gana 1 punto por acierto.</p>
                </button>
                <button onclick="startExercise('experto')" class="minimal-card p-14 hover:border-indigo-600 group bg-white shadow-lg">
                    <div class="text-7xl mb-8 transition-transform group-hover:scale-110">📕</div>
                    <h3 class="text-3xl font-black mb-2 uppercase">Experto</h3>
                    <p class="text-slate-400 font-bold text-lg">Gana 2 puntos por acierto.</p>
                </button>
            </div>
            <button onclick="goHome()" class="mt-16 text-slate-400 font-black hover:text-indigo-600 transition-all uppercase text-sm tracking-[0.2em]">← Volver al Aula</button>
        </div>

        <!-- PIZARRA -->
        <div id="view-exercise" class="hidden-view w-full mx-auto">
            <div class="flex justify-between items-center mb-6">
                <button onclick="goHome()" class="btn-action px-6 py-3 text-[10px]">← Menú</button>
                <div id="badge-diff" class="bg-slate-900 text-white px-5 py-2 rounded-xl text-[10px] font-black uppercase tracking-[0.2em]">MODO</div>
            </div>
            <div class="chalkboard">
                <div class="w-full flex justify-between items-start opacity-20 mb-10 px-4">
                    <span id="txt-module" class="text-sm font-black uppercase tracking-[0.4em]">PIZARRA</span>
                    <span class="text-[10px] font-black uppercase">Academia Estudio Pro</span>
                </div>
                <h3 id="txt-instruction" class="text-2xl md:text-3xl font-bold mb-10 text-center max-w-4xl text-white/80">...</h3>
                <div id="display-area" class="flex-grow flex items-center justify-center w-full my-6">
                    <div id="math-problem" class="text-center w-full text-white"></div>
                </div>
                <div class="w-full max-w-2xl relative z-10">
                    <div id="input-container"></div>
                    <div id="feedback-box" class="hidden mt-8 p-6 rounded-[2rem] font-black text-center border-4 border-white/10"></div>
                    <div class="mt-12 flex gap-6 w-full">
                        <button id="btn-check" onclick="check()" class="flex-1 btn-action btn-success py-6 text-sm">Comprobar</button>
                        <button id="btn-next" onclick="nextExercise()" class="hidden flex-1 btn-action btn-primary py-6 text-sm">Siguiente</button>
                        <button id="btn-skip" onclick="nextExercise()" class="btn-action btn-danger px-10 py-6 text-sm">Saltar</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- MODAL NOMBRE -->
        <div id="modal-name" class="modal-overlay hidden-view">
            <div class="bg-white p-12 rounded-[2.5rem] text-center max-w-md w-full shadow-2xl border-4 border-amber-400">
                <h3 class="text-3xl font-black mb-4">¡Maestría Total!</h3>
                <p class="text-slate-500 font-bold mb-8 italic">Introduce tu nombre completo para emitir el diploma:</p>
                <input type="text" id="user-name-input" class="w-full text-center p-5 border-2 border-slate-200 rounded-2xl mb-8 font-black uppercase text-xl text-slate-800 placeholder-slate-300 outline-none focus:border-indigo-500" placeholder="NOMBRE COMPLETO">
                <button onclick="generateDiploma()" class="btn-action btn-primary w-full py-5 text-base">Generar Diploma</button>
                <button onclick="document.getElementById('modal-name').classList.add('hidden-view')" class="mt-4 text-slate-400 font-bold text-[10px] uppercase hover:text-rose-500 transition-all">Cancelar</button>
            </div>
        </div>

        <!-- DIPLOMA -->
        <div id="view-diploma" class="hidden-view w-full max-w-5xl mx-auto py-12">
            <div class="diploma-bg text-center">
                <h2 class="text-6xl font-black text-slate-800 mb-4 tracking-tighter">CERTIFICADO DE EXCELENCIA</h2>
                <p class="text-xl font-bold text-amber-700 uppercase tracking-[0.4em] mb-12">Academia de Matemáticas Estudio Pro</p>
                <p class="text-2xl italic text-slate-500 mb-2">Se otorga con distinción de honor al alumno</p>
                <p class="text-5xl font-black text-slate-900 mb-6 border-b-8 border-amber-200 inline-block px-12 pb-4 tracking-tight" id="diploma-name-text">NOMBRE</p>
                
                <div class="bg-amber-50 p-6 rounded-3xl border-2 border-amber-100 mb-10 max-w-2xl mx-auto">
                    <p id="diploma-special-message" class="text-xl font-bold text-amber-900 leading-relaxed italic"></p>
                </div>

                <p class="text-base text-slate-500 leading-relaxed max-w-3xl mx-auto mb-16 px-10">
                    Por haber superado con éxito todos los desafíos avanzados de números decimales, operaciones combinadas complejas, 
                    clasificación fraccionaria y resolución de problemas de nivel 1º ESO con precisión absoluta.
                </p>
                <div class="flex justify-around items-end">
                    <div class="text-center"><p class="text-2xl font-black text-slate-800">VALIDADO</p><p class="text-[10px] font-black text-slate-400 uppercase">Control Académico</p></div>
                    <div class="diploma-seal text-[11px] leading-tight">SUMMA CUM<br>LAUDE</div>
                    <div class="text-center"><p class="text-2xl font-black text-slate-800">2026</p><p class="text-[10px] font-black text-slate-400 uppercase">Año Lectivo</p></div>
                </div>
            </div>
            <div class="text-center mt-12 flex flex-col items-center gap-4">
                <button onclick="goHome()" class="btn-action btn-primary px-16 py-6 text-lg">Seguir Practicando</button>
                <p class="text-slate-400 text-xs font-bold uppercase">¡Tu maestría se mantiene guardada!</p>
            </div>
        </div>

    </main>

    <script>
        function getGCD(a, b) { return b ? getGCD(b, a % b) : a; }

        function getFractionType(n, d) {
            const common = getGCD(n, d);
            let simplifiedD = d / common;
            if (simplifiedD === 1) return 'Exacto';
            let tempD = simplifiedD;
            while (tempD % 2 === 0) tempD /= 2;
            while (tempD % 5 === 0) tempD /= 5;
            if (tempD === 1) return 'Exacto';
            if (tempD === simplifiedD) return 'P. Puro';
            return 'P. Mixto';
        }

        const categories = [
            { id: 'ident', title: 'Clasificación', icon: '🏷️', needsDiff: false, color: 'bg-indigo-50' },
            { id: 'repr', title: 'Representación', icon: '📏', needsDiff: false, color: 'bg-blue-50' },
            { id: 'aprox', title: 'Aproximación', icon: '🎯', needsDiff: false, color: 'bg-emerald-50' },
            { id: 'sum', title: 'Suma y Resta', icon: '➕', needsDiff: true, color: 'bg-amber-50' },
            { id: 'mult', title: 'Multiplicación', icon: '✖️', needsDiff: true, color: 'bg-orange-50' },
            { id: 'div', title: 'División', icon: '➗', needsDiff: true, color: 'bg-rose-50' },
            { id: 'pot10', title: 'Potencias de 10', icon: '🚀', needsDiff: true, color: 'bg-cyan-50' },
            { id: 'comb', title: 'Combinadas', icon: '🧩', needsDiff: true, color: 'bg-violet-50' },
            { id: 'prob', title: 'Problemas', icon: '🏫', needsDiff: true, color: 'bg-slate-50' }
        ];

        const POINTS_GOAL = 10;
        const EXPERT_GOAL = 2;

        const state = { 
            streak: 0, category: null, difficulty: 'aprendiz', currentAns: null, currentHint: "", answered: false, 
            mastery: {} 
        };
        categories.forEach(c => state.mastery[c.id] = { total: 0, expert: 0 });

        function goHome() {
            document.querySelectorAll('main > div').forEach(el => el.classList.add('hidden-view'));
            document.getElementById('view-home').classList.remove('hidden-view');
            renderCategories();
            updateHUD();
        }

        function clickCategory(id) {
            state.category = id;
            const cat = categories.find(c => c.id === id);
            if(cat.needsDiff) {
                document.getElementById('view-home').classList.add('hidden-view');
                document.getElementById('view-diff').classList.remove('hidden-view');
            } else {
                startExercise('normal');
            }
        }

        function startExercise(diff) {
            state.difficulty = diff;
            document.querySelectorAll('main > div').forEach(el => el.classList.add('hidden-view'));
            document.getElementById('view-exercise').classList.remove('hidden-view');
            document.getElementById('badge-diff').innerText = diff === 'normal' ? 'ESO' : diff.toUpperCase();
            gen();
        }

        function gen() {
            const display = document.getElementById('math-problem');
            const instruct = document.getElementById('txt-instruction');
            const inputCont = document.getElementById('input-container');
            const isExp = state.difficulty === 'experto';
            state.answered = false;
            document.getElementById('feedback-box').classList.add('hidden');
            document.getElementById('btn-check').classList.remove('hidden');
            document.getElementById('btn-next').classList.add('hidden');
            document.getElementById('btn-skip').classList.remove('hidden');
            display.innerHTML = ''; inputCont.innerHTML = '';
            document.getElementById('txt-module').innerText = categories.find(c => c.id === state.category).title;
            display.className = "text-center w-full math-big text-white";

            switch(state.category) {
                case 'ident':
                    const types = ['Exacto', 'P. Puro', 'P. Mixto'];
                    const pick = types[Math.floor(Math.random()*3)];
                    state.currentAns = pick;
                    if(Math.random() > 0.4) {
                        instruct.innerText = "¿Qué tipo de decimal genera esta fracción?";
                        let n, d;
                        const dExacto = [2, 4, 5, 8, 10, 16, 20, 25, 40, 50, 80, 100];
                        const dPuro = [3, 7, 9, 11, 13, 27, 33, 37, 99];
                        const dMixto = [6, 12, 14, 15, 18, 21, 22, 24, 26, 28, 30, 44, 45, 60];
                        let valid = false;
                        while(!valid) {
                            if(pick === 'Exacto') d = dExacto[Math.floor(Math.random()*dExacto.length)];
                            else if(pick === 'P. Puro') d = dPuro[Math.floor(Math.random()*dPuro.length)];
                            else d = dMixto[Math.floor(Math.random()*dMixto.length)];
                            n = Math.floor(Math.random()*30)+1;
                            if (getFractionType(n, d) === pick) valid = true;
                        }
                        display.innerHTML = `<div class="fraction"><span class="top">${n}</span><span class="bottom">${d}</span></div>`;
                        state.currentHint = `Divide el numerador entre el denominador.`;
                    } else {
                        instruct.innerText = "¿Cómo clasificarías este número?";
                        let tx = "";
                        if(pick === 'Exacto') tx = (Math.random()*150).toFixed(Math.floor(Math.random()*3)+1).replace('.',',');
                        else if(pick === 'P. Puro') {
                            const ent = Math.floor(Math.random()*40), per = (Math.floor(Math.random()*899)+11).toString();
                            tx = `${ent},${per}${per}${per}...`;
                        } else {
                            const ent = Math.floor(Math.random()*20), ante = (Math.floor(Math.random()*89)+10).toString(), per = (Math.floor(Math.random()*8)+1).toString();
                            tx = `${ent},${ante}${per}${per}${per}${per}...`;
                        }
                        display.innerHTML = tx;
                        state.currentHint = pick === 'Exacto' ? "Cifras finitas." : pick === 'P. Puro' ? "Repite tras la coma." : "Anteperíodo.";
                    }
                    inputCont.className = "grid grid-cols-1 gap-4";
                    inputCont.innerHTML = types.map(t => `<button onclick="check('${t}')" class="bg-white border-4 border-slate-800 py-4 rounded-2xl font-bold hover:bg-slate-100 transition-all text-xl text-slate-800 uppercase">${t}</button>`).join('');
                    break;

                case 'sum':
                    instruct.innerText = isExp ? "Suma y resta combinada (Sin negativos):" : "Suma y resta exacta:";
                    if(isExp) {
                        const count = Math.floor(Math.random() * 2) + 3; 
                        let problemStr = ""; let resultVal = 0;
                        for(let i=0; i<count; i++){
                            let val = parseFloat((Math.random()*40).toFixed(Math.floor(Math.random()*3)+3));
                            if(i === 0) resultVal = val;
                            else {
                                let sign = (resultVal - val >= 0) ? (Math.random() > 0.4 ? 1 : -1) : 1;
                                if(sign === 1) resultVal += val; else resultVal -= val;
                                problemStr += sign === 1 ? " + " : " - ";
                            }
                            problemStr += val.toString().replace('.',',');
                        }
                        state.currentAns = resultVal.toFixed(5).replace(/\.?0+$/, "");
                        display.innerHTML = `<span class="text-2xl">${problemStr}</span>`;
                        state.currentHint = "Cuidado con la posición de las comas.";
                    } else {
                        let d1 = Math.floor(Math.random()*3)+1, d2 = Math.floor(Math.random()*3)+1;
                        let as = (Math.random()*50).toFixed(d1), bs = (Math.random()*20).toFixed(d2);
                        let op = parseFloat(as) >= parseFloat(bs) ? (Math.random() > 0.5) : true;
                        state.currentAns = (op ? parseFloat(as)+parseFloat(bs) : parseFloat(as)-parseFloat(bs)).toFixed(Math.max(d1, d2)).replace(/\.?0+$/, "");
                        display.innerHTML = `${as.replace('.',',')} ${op?'+':'-'} ${bs.replace('.',',')}`;
                        state.currentHint = "Alinea las comas.";
                    }
                    renderTextInput();
                    break;

                case 'mult':
                    instruct.innerText = isExp ? "Producto de 3 factores avanzados:" : "Halla el producto:";
                    if(isExp) {
                        const d1 = Math.floor(Math.random()*2)+2, d2 = Math.floor(Math.random()*2)+2, d3 = Math.floor(Math.random()*2)+1;
                        const f1 = parseFloat((Math.random()*10).toFixed(d1)), f2 = parseFloat((Math.random()*5).toFixed(d2)), f3 = parseFloat((Math.random()*4).toFixed(d3));
                        state.currentAns = (f1 * f2 * f3).toFixed(d1+d2+d3).replace(/\.?0+$/, "");
                        display.innerHTML = `<span class="text-3xl">${f1.toString().replace('.',',')} × ${f2.toString().replace('.',',')} × ${f3.toString().replace('.',',')}</span>`;
                        state.currentHint = "Suma los decimales de todos los factores.";
                    } else {
                        const md1 = Math.floor(Math.random()*3)+1, md2 = Math.floor(Math.random()*2)+1;
                        const am = (Math.random()*40).toFixed(md1), bm = (Math.random()*10).toFixed(md2);
                        state.currentAns = (parseFloat(am)*parseFloat(bm)).toFixed(md1+md2).replace(/\.?0+$/, "");
                        display.innerHTML = `${am.replace('.',',')} × ${bm.replace('.',',')}`;
                        state.currentHint = "Multiplica y cuenta decimales.";
                    }
                    renderTextInput();
                    break;

                case 'comb':
                    instruct.innerText = isExp ? "Jerarquía ESO Experto (Paréntesis complejos):" : "Jerarquía de Operaciones:";
                    if(isExp) {
                        const structures = [
                            { s: "(a + b) × (c - d)", fn: (a,b,c,d) => (a+b)*(c-d) },
                            { s: "a + (b × c) - d", fn: (a,b,c,d) => a+(b*c)-d },
                            { s: "(a ÷ 0,5) + (b × c)", fn: (a,b,c,d) => (a/0.5)+(b*c) }
                        ];
                        const struct = structures[Math.floor(Math.random()*structures.length)];
                        const c = parseFloat((Math.random()*10+5).toFixed(1)), d = parseFloat((Math.random()*4+1).toFixed(1));
                        const a = parseFloat((Math.random()*10+5).toFixed(2)), b = parseFloat((Math.random()*5+1).toFixed(2));
                        state.currentAns = struct.fn(a,b,c,d).toFixed(5).replace(/\.?0+$/, "");
                        let formula = struct.s.replace('a', a.toString().replace('.',',')).replace('b', b.toString().replace('.',',')).replace('c', c.toString().replace('.',',')).replace('d', d.toString().replace('.',','));
                        display.innerHTML = `<span class="text-3xl">${formula}</span>`;
                        state.currentHint = "Paréntesis > Multiplicación > Suma.";
                    } else {
                        const ja = parseFloat((Math.random()*50 + 10).toFixed(1)), jb = parseFloat((Math.random()*10).toFixed(1));
                        state.currentAns = (ja + jb * 2).toFixed(1).replace(/\.?0+$/, "");
                        display.innerHTML = `${ja.toString().replace('.',',')} + ${jb.toString().replace('.',',')} × 2`;
                    }
                    renderTextInput();
                    break;

                case 'pot10':
                    instruct.innerText = isExp ? "Cálculo rápido (incluye 0,001):" : "Cálculo rápido:";
                    const baseV = (Math.random()*8000).toFixed(Math.floor(Math.random()*3)+1);
                    const isMult = Math.random() > 0.5;
                    let factor = isExp ? [10, 100, 1000, 0.1, 0.01, 0.001][Math.floor(Math.random()*6)] : [10, 100, 1000][Math.floor(Math.random()*3)];
                    state.currentAns = isMult ? (parseFloat(baseV)*factor).toFixed(7).replace(/\.?0+$/, "") : (parseFloat(baseV)/factor).toFixed(7).replace(/\.?0+$/, "");
                    display.innerHTML = `${baseV.replace('.',',')} ${isMult?'×':'÷'} ${factor.toString().replace('.',',')}`;
                    renderTextInput();
                    break;

                case 'prob':
                    display.className = "problem-text";
                    instruct.innerText = isExp ? "RETO EXPERTO (Varias Operaciones):" : "Problema de Aprendiz:";
                    
                    const pAprendiz = [
                        { q: "Un kilo de filetes cuesta 11,45€. Si compro 1,5 kg, ¿cuánto pago?", a: "17.175", h: "Multiplica el precio por los kilos." },
                        { q: "Tenía 20€. He comprado un libro de 12,45€ y un estuche de 3,50€. ¿Cuánto me queda?", a: "4.05", h: "Suma los gastos y réstalo a 20." },
                        { q: "Repartimos 15,3 litros de limonada en 6 jarras iguales. ¿Cuánto hay en cada una?", a: "2.55", h: "Divide el total entre 6." },
                        { q: "Una cuerda mide 12,45 metros. Cortamos 3 trozos de 1,2 metros cada uno. ¿Qué queda?", a: "8.85", h: "Resta los tres trozos (3.6m) al total." }
                    ];

                    const pExperto = [
                        { 
                          q: "<b>PLAN DE AHORRO:</b> <br> Juan tiene 150,50€. <br> - Compra 2 juegos de 19,95€ cada uno. <br> - Recibe un regalo de 15,30€. <br> <b>¿Cuánto dinero tiene Juan ahora?</b>", 
                          a: "125.9", 
                          h: "Primero resta el coste de los juegos (2 x 19,95) y luego suma el regalo." 
                        },
                        { 
                          q: "<b>REBAJAS DE TECNOLOGÍA:</b> <br> Un móvil cuesta 245,50€. <br> - Le aplican un descuento de 35,25€. <br> - El seguro opcional cuesta 12,40€. <br> <b>¿Cuál es el precio final si compras el móvil rebajado y el seguro?</b>", 
                          a: "222.65", 
                          h: "Resta el descuento al precio inicial y luego suma el coste del seguro." 
                        },
                        { 
                          q: "<b>CARRERA DE ATLETISMO:</b> <br> Un circuito mide 4,25 km. <br> - Un atleta da 5 vueltas completas. <br> - Otro atleta da 3 vueltas completas. <br> <b>¿Cuántos km de diferencia hay entre los dos atletas?</b>", 
                          a: "8.5", 
                          h: "Calcula los km de cada uno (vueltas x 4,25) y luego resta los resultados." 
                        },
                        { 
                          q: "<b>FACTURA DE SUSCRIPCIÓN:</b> <br> Una plataforma de cine cuesta 9,99€ al mes. <br> - Los 3 primeros meses son a mitad de precio. <br> - El resto del año (9 meses) es a precio normal. <br> <b>¿Cuánto pagas en total por un año completo?</b>", 
                          a: "104.895", 
                          h: "Calcula (3 x 4,995) + (9 x 9,99). No redondees hasta el final." 
                        },
                        { 
                          q: "<b>PRESUPUESTO DE PINTURA:</b> <br> Pintar una pared de 25,5 m² cuesta 5,20€ por cada m². <br> - Además, hay que pagar 15,50€ de transporte. <br> <b>¿Cuánto cuesta en total pintar la pared?</b>", 
                          a: "148.1", 
                          h: "Multiplica los metros por el precio y suma el transporte." 
                        },
                        { 
                          q: "<b>DESCARGA DE DATOS:</b> <br> Tienes un archivo de 500,5 MB. <br> - Descargas 12,25 MB cada minuto. <br> - Llevas descargando 20 minutos. <br> <b>¿Cuántos MB faltan por descargar?</b>", 
                          a: "255.5", 
                          h: "Calcula cuánto llevas (20 x 12,25) y réstalo al total del archivo." 
                        }
                    ];

                    const pool = isExp ? pExperto : pAprendiz;
                    const prob = pool[Math.floor(Math.random()*pool.length)];
                    state.currentAns = prob.a;
                    display.innerHTML = `<div class="problem-text">${prob.q}</div>`;
                    state.currentHint = prob.h;
                    renderTextInput();
                    break;

                case 'aprox':
                    const trgts = ['unidades', 'décimas', 'centésimas', 'milésimas'];
                    const target = trgts[Math.floor(Math.random()*4)], val = (Math.random()*1500).toFixed(5), isRed = Math.random() > 0.5;
                    instruct.innerText = `${isRed?'Redondea':'Trunca'} a las ${target}:`;
                    display.innerHTML = val.replace('.',',');
                    let fct = target === 'unidades' ? 1 : target === 'décimas' ? 10 : target === 'centésimas' ? 100 : 1000;
                    if(isRed) state.currentAns = (Math.round(parseFloat(val) * fct) / fct).toFixed(target==='unidades'?0:target==='décimas'?1:target==='centésimas'?2:3);
                    else {
                        const pts = val.split('.');
                        if(target === 'unidades') state.currentAns = pts[0];
                        else if(target === 'décimas') state.currentAns = pts[0] + '.' + pts[1][0];
                        else if(target === 'centésimas') state.currentAns = pts[0] + '.' + pts[1].substring(0,2);
                        else state.currentAns = pts[0] + '.' + pts[1].substring(0,3);
                    }
                    renderTextInput();
                    break;

                case 'div':
                    instruct.innerText = "Cociente exacto:";
                    if(!isExp) {
                        const dvor = Math.floor(Math.random()*8)+2;
                        let res;
                        do {
                            res = parseFloat(((Math.random() * 20) + 1.1).toFixed(1));
                        } while (Number.isInteger(res));
                        state.currentAns = res.toString();
                        display.innerHTML = `${(res * dvor).toFixed(1).replace('.',',')} ÷ ${dvor}`;
                    } else {
                        const dcs = Math.floor(Math.random()*2)+1, dvor = (Math.random()*20 + 1.2).toFixed(dcs), res = Math.floor(Math.random()*400)+150;
                        state.currentAns = res.toString();
                        display.innerHTML = `${(res * parseFloat(dvor)).toFixed(dcs).replace('.',',')} ÷ ${dvor.replace('.',',')}`;
                    }
                    renderTextInput();
                    break;

                case 'repr':
                    const s_sc = [{n:'Décimas', s:0.1, d:1}, {n:'Centésimas', s:0.01, d:2}, {n:'Milésimas', s:0.001, d:3}][Math.floor(Math.random()*3)];
                    const s_sv = parseFloat((Math.random()*15).toFixed(s_sc.d-1)), s_pIdx = Math.floor(Math.random()*9)+1;
                    state.currentAns = (s_sv + s_pIdx*s_sc.s).toFixed(s_sc.d);
                    instruct.innerText = `Escala: ${s_sc.n}. Indica el valor del punto:`;
                    let svgContent = `<svg viewBox="0 0 500 120" class="w-full overflow-visible"><line x1="20" y1="60" x2="480" y2="60" stroke="#fff" stroke-width="8" stroke-linecap="round" />`;
                    for(let i=0; i<=10; i++) {
                        const x = 20 + i*46, isM = i === 0 || i === 10;
                        let lb = isM ? (i===0?s_sv:(s_sv + 10*s_sc.s).toFixed(s_sc.d)).toString().replace('.',',') : "";
                        svgContent += `<line x1="${x}" y1="60" x2="${x}" y2="${isM?20:50}" stroke="#fff" stroke-width="${isM?8:4}" opacity="${isM?1:0.7}" />${lb?`<text x="${x}" y="105" text-anchor="middle" class="text-[14px] font-black fill-white">${lb}</text>`:''}${i===s_pIdx?`<circle cx="${x}" cy="10" r="16" fill="#facc15" stroke="#ca8a04" stroke-width="4" />`:''}`;
                    }
                    svgContent += `</svg>`;
                    display.innerHTML = svgContent;
                    renderTextInput();
                    break;
            }
        }

        function renderTextInput() {
            document.getElementById('input-container').innerHTML = `<input type="text" id="main-input" placeholder="Tu respuesta..." autocomplete="off" class="w-full text-center text-3xl md:text-5xl font-black py-8 bg-white/5 border-b-4 border-white outline-none focus:bg-white/10 transition-all text-white placeholder-white/20">`;
            setTimeout(() => { const mi = document.getElementById('main-input'); if(mi) mi.focus(); }, 100);
        }

        function check(manual = null) {
            if(state.answered) return;
            const inputEl = document.getElementById('main-input');
            const val = (manual !== null) ? manual : (inputEl ? inputEl.value : null);
            if(!val) return;
            const isOk = (parseFloat(val.toString().replace(',','.')) === parseFloat(state.currentAns.toString().replace(',','.'))) || (val.toLowerCase().trim() === state.currentAns.toString().toLowerCase().trim());
            state.answered = true;
            document.getElementById('btn-check').classList.add('hidden');
            document.getElementById('btn-skip').classList.add('hidden');
            const fb = document.getElementById('feedback-box');
            fb.classList.remove('hidden');
            
            if(isOk) {
                fb.innerHTML = `<div class="font-black text-xl mb-1 uppercase tracking-widest text-emerald-300">¡CORRECTO!</div>`;
                fb.className = "mt-8 p-6 border-8 border-emerald-400 bg-emerald-950/40 text-emerald-100 rounded-[2rem] text-center";
                
                let pointsToAdd;
                if (['ident', 'repr', 'aprox'].includes(state.category)) pointsToAdd = 0.5;
                else pointsToAdd = state.difficulty === 'experto' ? 2 : 1;
                
                state.mastery[state.category].total = Math.min(state.mastery[state.category].total + pointsToAdd, POINTS_GOAL);
                if(state.difficulty === 'experto') {
                    state.mastery[state.category].expert += pointsToAdd;
                }
                
                state.streak++;
                setTimeout(nextExercise, 1200);
            } else {
                fb.innerHTML = `<div class="font-black text-xl mb-1 uppercase tracking-widest text-rose-300">ERROR</div><div class="text-sm font-bold mb-3 uppercase">Era: ${state.currentAns.toString().replace('.',',')}</div><div class="text-[10px] font-bold bg-white/10 p-2 rounded-lg italic">${state.currentHint || ""}</div>`;
                fb.className = "mt-8 p-6 border-8 border-rose-400 bg-rose-950/40 text-rose-100 rounded-[2rem] text-center";
                state.streak = 0;
                document.getElementById('btn-next').classList.remove('hidden');
            }
            updateHUD();
        }

        function nextExercise() { gen(); }
        function resetProgress() { categories.forEach(c => state.mastery[c.id] = { total: 0, expert: 0 }); state.streak = 0; goHome(); }

        function updateHUD() {
            const values = Object.values(state.mastery);
            const totalScore = values.reduce((a,b) => a + b.total, 0);
            const totalPossible = categories.length * POINTS_GOAL;
            document.getElementById('mastery-fill').style.width = `${(totalScore / totalPossible) * 100}%`;
            document.getElementById('streak-count').innerText = state.streak;
            
            const rNames = ["Novato", "Aventajado", "Genio", "Maestro", "Leyenda"];
            document.getElementById('rank-name').innerText = rNames[Math.min(Math.floor(totalScore / 18), 4)];
            
            const allComplete = categories.every(c => {
                const m = state.mastery[c.id];
                return m.total >= POINTS_GOAL && (!c.needsDiff || m.expert >= EXPERT_GOAL);
            });

            if(allComplete) {
                document.getElementById('rank-icon').innerText = "🏅";
                document.getElementById('btn-claim-diploma').classList.remove('hidden-view');
                document.getElementById('btn-reset').classList.remove('hidden');
            } else {
                document.getElementById('rank-icon').innerText = "🌱";
                document.getElementById('btn-claim-diploma').classList.add('hidden-view');
            }
        }

        function renderCategories() {
            document.getElementById('grid-categories').innerHTML = categories.map(c => {
                const m = state.mastery[c.id];
                const perc = Math.min((m.total / POINTS_GOAL) * 100, 100);
                const hasExpertGoal = !c.needsDiff || m.expert >= EXPERT_GOAL;
                const expertBadge = c.needsDiff ? (m.expert >= EXPERT_GOAL ? '🥇' : (m.expert > 0 ? '🥈' : '')) : '';
                
                return `
                <div onclick="clickCategory('${c.id}')" class="minimal-card p-10 cursor-pointer flex flex-col items-center group ${c.color} shadow-sm">
                    <div class="expert-badge">${expertBadge}</div>
                    <div class="text-7xl mb-4 group-hover:rotate-12 transition-transform duration-300">
                        ${(m.total >= POINTS_GOAL && hasExpertGoal) ? '✅' : (c.id === 'ident' ? '🏷️' : c.id === 'repr' ? '📏' : c.id === 'aprox' ? '🎯' : c.id === 'sum' ? '➕' : c.id === 'mult' ? '✖️' : c.id === 'div' ? '➗' : c.id === 'pot10' ? '🚀' : c.id === 'comb' ? '🧩' : '🏫')}
                    </div>
                    <h3 class="font-black text-[10px] uppercase tracking-widest text-slate-800 mb-2">${c.title}</h3>
                    <div class="w-full bg-slate-200 h-2 rounded-full mt-2 overflow-hidden">
                        <div class="bg-indigo-500 h-full transition-all duration-500" style="width: ${perc}%"></div>
                    </div>
                    <span class="text-[9px] font-bold text-slate-400 mt-2 uppercase">${m.total} / ${POINTS_GOAL} PTOS ${c.needsDiff ? `(${m.expert}/${EXPERT_GOAL} EXP)` : ''}</span>
                </div>`}).join('');
        }

        function openNameModal() { document.getElementById('modal-name').classList.remove('hidden-view'); }
        function generateDiploma() {
            const name = document.getElementById('user-name-input').value.trim();
            if(!name) return;
            document.getElementById('diploma-name-text').innerText = name.toUpperCase();
            document.getElementById('diploma-special-message').innerText = `¡Enhorabuena, ${name}! Has demostrado un talento excepcional. Ya estás totalmente preparado/a para el examen y te has convertido en un auténtico supermatemático/a.`;
            document.getElementById('modal-name').classList.add('hidden-view');
            document.querySelectorAll('main > div').forEach(el => el.classList.add('hidden-view'));
            document.getElementById('view-diploma').classList.remove('hidden-view');
        }

        document.addEventListener('keypress', (e) => {
            if(e.key === 'Enter') {
                if(!state.answered) { 
                    if(document.getElementById('main-input')) check(); 
                    if(document.getElementById('user-name-input').offsetParent) generateDiploma(); 
                }
                else if(state.streak === 0) nextExercise();
            }
        });
        window.onload = goHome;
    </script>
</body>
</html>
