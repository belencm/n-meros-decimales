<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Estudio Pro - Master en Sistemas</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&family=Fira+Code:wght@400;500&display=swap');
        
        body { font-family: 'Inter', sans-serif; background-color: #f1f5f9; overflow-x: hidden; }
        .math-font { font-family: 'Fira Code', monospace; }
        
        ::-webkit-scrollbar { width: 4px; height: 4px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #475569; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #6366f1; }

        .sqrt { display: inline-flex; align-items: flex-start; vertical-align: middle; }
        .sqrt-symbol { font-family: 'Inter', sans-serif; font-size: 1.2em; line-height: 1; margin-right: 1px; font-weight: 300; }
        .sqrt-content { padding-top: 2px; line-height: 1; }

        .mini-system { 
            display: inline-flex; 
            align-items: center; 
            background: rgba(15, 23, 42, 0.4); 
            padding: 0.5rem 1rem; 
            border-radius: 14px; 
            margin: 8px 0;
            border: 1px solid rgba(214, 188, 250, 0.3);
            max-width: 100%;
        }
        .mini-bracket { 
            color: #D6BCFA; 
            font-size: 2.2rem; 
            font-weight: 300;
            line-height: 1; 
            margin-right: 0.5rem; 
            user-select: none;
            display: flex;
            align-items: center;
        }
        .mini-equations { 
            display: flex; 
            flex-direction: column; 
            gap: 0.2rem;
            font-size: 0.85rem; 
            text-align: left;
            white-space: nowrap;
        }

        .blackboard {
            background-color: #0f172a;
            background-image: radial-gradient(circle at 1.5px 1.5px, rgba(51, 65, 85, 0.3) 1px, transparent 0);
            background-size: 20px 20px;
            color: #e2e8f0;
            border: 10px solid #334155;
            box-shadow: inset 0 8px 32px rgba(0,0,0,0.8);
            display: flex;
            align-items: stretch;
            min-height: 580px; 
            position: relative;
        }

        .system-display-area {
            flex: 0 0 35%; 
            padding: 1.5rem;
            display: flex;
            align-items: center;
            justify-content: center;
            border-right: 2px solid rgba(51, 65, 85, 0.5);
            background: rgba(0,0,0,0.1);
        }

        .log-area {
            flex: 1;
            padding: 1.5rem;
            overflow-y: auto;
            max-height: 580px;
        }

        .system-container {
            display: inline-flex;
            align-items: center;
            padding: 1.5rem 2rem;
            background: rgba(255, 255, 255, 0.03);
            border-radius: 30px;
            box-shadow: 0 15px 40px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.05);
        }

        .lavender-bracket {
            color: #D6BCFA;
            font-size: 5rem; 
            font-weight: 100;
            line-height: 0.7;
            margin-right: 1rem;
            display: flex;
            align-items: center;
        }

        .equation-stack {
            display: flex; 
            flex-direction: column; 
            gap: 1.2rem; 
            white-space: nowrap;
        }

        .step-entry {
            border-left: 3px solid #D6BCFA;
            padding: 0.8rem 1.2rem;
            margin-bottom: 1.5rem;
            animation: fadeIn 0.4s ease-out;
            color: #94a3b8;
            background: rgba(30, 41, 59, 0.5);
            border-radius: 0 15px 15px 0;
            font-size: 0.85rem;
        }

        .step-entry b {
            display: block;
            margin-top: 10px;
            color: #f8fafc;
            font-family: 'Fira Code', monospace;
            background: #1e293b;
            padding: 10px 14px;
            border-radius: 12px;
            font-weight: 500;
            overflow-x: auto;
            border: 1px solid rgba(214, 188, 250, 0.1);
            box-shadow: 0 3px 8px rgba(0,0,0,0.2);
            font-size: 0.85rem;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

        .btn-correct { background-color: #dcfce7 !important; border-color: #22c55e !important; color: #166534 !important; }
        .btn-wrong { background-color: #fee2e2 !important; border-color: #ef4444 !important; color: #991b1b !important; }

        .glass-card {
            background: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(10px);
            border: 1px solid #e2e8f0;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .congrats-overlay {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(15, 23, 42, 0.98);
            z-index: 100;
            display: flex; align-items: center; justify-content: center;
            border-radius: 2.2rem;
        }
    </style>
</head>
<body class="min-h-screen">

    <header class="bg-slate-900 text-white shadow-xl sticky top-0 z-50">
        <div class="container mx-auto px-6 py-3 flex justify-between items-center">
            <h1 class="text-xl font-black text-indigo-400 tracking-tight">Estudio PRO <span class="text-slate-500 font-light text-[9px] ml-2 uppercase tracking-[0.2em]">Master Algebra</span></h1>
            <div class="flex gap-6 items-center">
                <div class="text-right">
                    <span class="text-[8px] uppercase text-slate-500 font-black block tracking-widest">DOMINIO</span>
                    <span class="text-lg font-mono font-bold text-indigo-300" id="ui-points">0</span>
                </div>
                <div id="ui-streak" class="bg-indigo-600/20 text-indigo-400 border border-indigo-500/30 px-3 py-1 rounded-full text-xs font-black">0 🔥</div>
            </div>
        </div>
    </header>

    <main class="container mx-auto px-4 py-6">
        
        <div id="screen-categories" class="max-w-6xl mx-auto animate-in fade-in duration-700">
            <div class="mb-10 text-center">
                <h2 class="text-3xl font-black text-slate-900 tracking-tighter mb-3">Laboratorio de Álgebra Rigurosa</h2>
                <div class="h-1 w-16 bg-indigo-500 mx-auto rounded-full"></div>
                <p class="text-slate-500 mt-3 text-sm italic">Resolución de sistemas de ecuaciones no lineales y radicales.</p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <button onclick="showDifficulty('simples')" class="glass-card p-8 rounded-[2.5rem] text-left group">
                    <div class="w-12 h-12 bg-emerald-50 text-emerald-500 rounded-2xl flex items-center justify-center text-xl mb-4 group-hover:scale-110 transition-transform">XY</div>
                    <h3 class="font-extrabold text-slate-800 text-xl">Sistemas No Lineales</h3>
                    <p class="text-xs text-slate-500 mt-2 leading-relaxed">Ejercicios estrictamente Cuadráticos o de Producto.</p>
                </button>
                <button onclick="showDifficulty('radicales')" class="glass-card p-8 rounded-[2.5rem] text-left group">
                    <div class="w-12 h-12 bg-purple-50 text-purple-500 rounded-2xl flex items-center justify-center text-xl mb-4 group-hover:scale-110 transition-transform">√x</div>
                    <h3 class="font-extrabold text-slate-800 text-xl">Sistemas Radicales</h3>
                    <p class="text-xs text-slate-500 mt-2 leading-relaxed">Eliminación de raíces y comprobación obligatoria.</p>
                </button>
                <button onclick="showDifficulty('racionales')" class="glass-card p-8 rounded-[2.5rem] text-left group">
                    <div class="w-12 h-12 bg-orange-50 text-orange-500 rounded-2xl flex items-center justify-center text-xl mb-4 group-hover:scale-110 transition-transform">1/x</div>
                    <h3 class="font-extrabold text-slate-800 text-xl">Sistemas Racionales</h3>
                    <p class="text-xs text-slate-500 mt-2 leading-relaxed">Uso de m.c.m. algebraico y simplificación.</p>
                </button>
            </div>
        </div>

        <div id="screen-difficulty" class="hidden max-w-2xl mx-auto py-16 text-center">
            <h2 class="text-3xl font-black text-slate-900 mb-8 tracking-tighter uppercase">Dificultad</h2>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
                <button onclick="initGame('basico')" class="p-8 border-2 border-slate-200 rounded-[2rem] bg-white hover:border-indigo-500 hover:shadow-xl transition-all">
                    <span class="text-[9px] font-black uppercase text-blue-500 bg-blue-50 px-3 py-1 rounded-full tracking-widest">Aprendiz</span>
                    <h3 class="text-xl font-black mt-4 text-slate-800">Nivel 1</h3>
                </button>
                <button onclick="initGame('experto')" class="p-8 border-2 border-slate-200 rounded-[2rem] bg-white hover:border-indigo-600 hover:shadow-xl transition-all">
                    <span class="text-[9px] font-black uppercase text-red-500 bg-red-50 px-3 py-1 rounded-full tracking-widest">Matemático</span>
                    <h3 class="text-xl font-black mt-4 text-slate-800">Nivel 2</h3>
                </button>
            </div>
            <button onclick="showHome()" class="mt-10 text-slate-400 hover:text-slate-600 font-bold uppercase text-[9px] tracking-widest flex items-center justify-center gap-2 mx-auto">
                VOLVER AL INICIO
            </button>
        </div>

        <div id="screen-game" class="hidden space-y-6 max-w-7xl mx-auto">
            <div class="blackboard rounded-[2.5rem]">
                <div id="congrats-modal" class="hidden congrats-overlay">
                    <div class="bg-white p-12 rounded-[3rem] text-center border-[8px] border-indigo-600 shadow-2xl">
                        <h2 class="text-4xl font-black text-indigo-600 mb-4 tracking-tighter">¡LOGRADO!</h2>
                        <button onclick="processNext(true)" class="bg-indigo-600 text-white px-10 py-4 rounded-[1.5rem] font-black text-lg hover:bg-indigo-700 transition-all shadow-xl">Siguiente Ejercicio</button>
                    </div>
                </div>

                <div class="system-display-area">
                    <div class="system-container">
                        <span class="lavender-bracket">{</span>
                        <div id="equation-lines" class="equation-stack text-lg md:text-xl math-font"></div>
                    </div>
                </div>
                <div class="log-area" id="log-container">
                    <h4 class="text-[8px] text-slate-500 uppercase tracking-widest mb-4 font-black border-b border-slate-800 pb-2 flex justify-between">
                        <span>Bitácora de Resolución</span>
                        <span class="text-indigo-400 opacity-50 font-mono">Algebra Engine</span>
                    </h4>
                    <div id="blackboard-history" class="math-font"></div>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-4 gap-6">
                <div class="lg:col-span-3">
                    <div class="glass-card p-8 rounded-[2rem]">
                        <div class="flex flex-wrap justify-between items-center mb-6 gap-4">
                            <h2 id="instruction-text" class="text-lg font-black text-slate-800 tracking-tight"></h2>
                            <span id="game-info-label" class="text-[8px] px-4 py-1.5 bg-indigo-50 text-indigo-600 rounded-full font-black uppercase tracking-widest"></span>
                        </div>
                        <div id="game-options" class="grid grid-cols-1 md:grid-cols-2 gap-3"></div>
                        
                        <div id="feedback-ui" class="hidden mt-6 p-6 rounded-[1.5rem] border-l-[8px] animate-in slide-in-from-bottom-3">
                            <div class="flex justify-between items-center gap-6">
                                <p id="feedback-msg" class="text-sm font-medium flex-1"></p>
                                <button id="btn-next-step" onclick="processNext()" class="hidden shrink-0 bg-slate-900 text-white px-6 py-3 rounded-xl font-black hover:bg-slate-800 transition-all text-xs">CONTINUAR →</button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="space-y-4">
                    <div class="glass-card p-6 rounded-[2rem] text-center">
                        <canvas id="progressCanvas" height="120" class="w-full"></canvas>
                    </div>
                    <button onclick="skipExercise()" class="w-full py-4 bg-white border-2 border-slate-100 text-indigo-600 font-black rounded-2xl hover:bg-indigo-50 transition-all text-[10px] tracking-widest">SALTAR</button>
                </div>
            </div>
        </div>
    </main>

    <script>
        const DATABASE = {
            simples: {
                basico: [
                    { system: ["x + y = 2", "x · y = -3"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "Aislar x en la lineal es ideal." },
                        { q: "Paso 1: Despejar x en la lineal:", opt: ["x = 2 - y", "x = y - 2"], ans: "x = 2 - y", show: "Despeje", label: "x = 2 - y", exp: "Trasposición estándar." },
                        { q: "Paso 2: Sustituir x en la segunda ecuación:", opt: ["(2 - y) · y = -3", "x · y = -3"], ans: "(2 - y) · y = -3", show: "Sustitución", label: "(2 - y) · y = -3", exp: "Cambiamos x por la expresión aislada." },
                        { q: "Paso 3: Resolver para y:", opt: ["y = 3, y = -1", "y = 1"], ans: "y = 3, y = -1", show: "y", label: "y = 3, y = -1", exp: "Raíces por fórmula general." },
                        { q: "Paso 4: Calcular x:", opt: ["x = -1, x = 3", "x = 0"], ans: "x = -1, x = 3", show: "x", label: "x = -1, x = 3", exp: "x = 2 - y." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(-1, 3) y (3, -1)", "(1, 1)"], ans: "(-1, 3) y (3, -1)", show: "Final", label: "(-1, 3), (3, -1)", exp: "Coordenadas finales.", isLast: true }
                    ]},
                    { system: ["x² + y = 5", "x + y = 3"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "Despejar y es directo." },
                        { q: "Paso 1: Despejar y:", opt: ["y = 3 - x", "y = x - 3"], ans: "y = 3 - x", show: "Despeje", label: "y = 3 - x", exp: "Correcto." },
                        { q: "Paso 2: Resolver para x:", opt: ["x = 2, x = -1", "x = 0"], ans: "x = 2, x = -1", show: "x", label: "x = 2, x = -1", exp: "x² - x - 2 = 0." },
                        { q: "Paso 3: Obtener y:", opt: ["y = 1, y = 4", "y = 2"], ans: "y = 1, y = 4", show: "y", label: "y = 1, y = 4", exp: "y = 3 - x." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(2, 1) y (-1, 4)", "(2, 1)"], ans: "(2, 1) y (-1, 4)", show: "Final", label: "(2, 1), (-1, 4)", exp: "Hecho.", isLast: true }
                    ]},
                    { system: ["x² + y² = 5", "x² - y² = 3"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Reducción", show: "Método", label: "Reducción", exp: "Sumar anula y²." },
                        { q: "Paso 1: Sumar ecuaciones:", opt: ["2x² = 8", "x² = 4"], ans: "2x² = 8", show: "Suma", label: "2x² = 8", exp: "Eliminamos y²." },
                        { q: "Paso 2: Obtener x:", opt: ["x = 2, x = -2", "x = 4"], ans: "x = 2, x = -2", show: "x", label: "x = 2, x = -2", exp: "x² = 4." },
                        { q: "Paso 3: Obtener y:", opt: ["y = 1, y = -1", "y = 3"], ans: "y = 1, y = -1", show: "y", label: "y = 1, y = -1", exp: "4 + y² = 5." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(2, 1), (2, -1), (-2, 1) y (-2, -1)", "(2, 1)"], ans: "(2, 1), (2, -1), (-2, 1) y (-2, -1)", show: "Final", label: "4 puntos", exp: "Listo.", isLast: true }
                    ]}
                ],
                experto: [
                    { system: ["x² + y² = 13", "y + 2x = 1"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "Despejamos y." },
                        { q: "Paso 1: Despejar y:", opt: ["y = 1 - 2x", "y = 2x - 1"], ans: "y = 1 - 2x", show: "Despeje", label: "y = 1 - 2x", exp: "Correcto." },
                        { q: "Paso 2: Desarrollo Identidad Notable (1-2x)²:", opt: ["1 - 4x + 4x²", "1 + 4x + 4x²"], ans: "1 - 4x + 4x²", show: "Identidad", label: "1 - 4x + 4x²", exp: "1² - 2·1·2x + (2x)²." },
                        { q: "Paso 3: Resolver para x:", opt: ["x = 2, x = -6/5", "x = 0"], ans: "x = 2, x = -6/5", show: "x", label: "x = 2, x = -6/5", exp: "Fórmula general." },
                        { q: "Paso 4: Obtener y:", opt: ["y = -3, y = 17/5", "y = 1"], ans: "y = -3, y = 17/5", show: "y", label: "y = -3, y = 17/5", exp: "y = 1 - 2x." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(2, -3) y (-6/5, 17/5)", "(2, 3)"], ans: "(2, -3) y (-6/5, 17/5)", show: "Final", label: "2 puntos", exp: "Listo.", isLast: true }
                    ]},
                    { system: ["x² + y² = 5", "x + 2y = 4"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "Aislamos x." },
                        { q: "Paso 1: Despejar x:", opt: ["x = 4 - 2y", "x = 2y - 4"], ans: "x = 4 - 2y", show: "Despeje", label: "x = 4 - 2y", exp: "Trasposición." },
                        { q: "Paso 2: Desarrollo Identidad Notable (4-2y)²:", opt: ["16 - 16y + 4y²", "16 + 4y²"], ans: "16 - 16y + 4y²", show: "Identidad", label: "16 - 16y + 4y²", exp: "Cuadrado del binomio." },
                        { q: "Paso 3: Resolver para y en fracciones:", opt: ["y = 11/5, y = 1", "y = 0"], ans: "y = 11/5, y = 1", show: "y", label: "y = 11/5, y = 1", exp: "5y² - 16y + 11 = 0." },
                        { q: "Paso 4: Obtener x:", opt: ["x = -2/5, x = 2", "x = 1"], ans: "x = -2/5, x = 2", show: "x", label: "x = -2/5, x = 2", exp: "x = 4 - 2y." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(-2/5, 11/5) y (2, 1)", "(2, 1)"], ans: "(-2/5, 11/5) y (2, 1)", show: "Final", label: "2 puntos", exp: "Hecho.", isLast: true }
                    ]},
                    { system: ["2x² - y² = 1", "x + y = 2"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "Aislamos y." },
                        { q: "Paso 1: Despejar y:", opt: ["y = 2 - x", "y = x - 2"], ans: "y = 2 - x", show: "Despeje", label: "y = 2 - x", exp: "Correcto." },
                        { q: "Paso 2: Desarrollo Identidad Notable (2-x)²:", opt: ["4 - 4x + x²", "4 + x²"], ans: "4 - 4x + x²", show: "Identidad", label: "4 - 4x + x²", exp: "Correcto." },
                        { q: "Paso 3: Resolver para x:", opt: ["x = 1, x = -5", "x = 0"], ans: "x = 1, x = -5", show: "x", label: "x = 1, x = -5", exp: "x² + 4x - 5 = 0." },
                        { q: "Paso 4: Obtener y:", opt: ["y = 1, y = 7", "y = 2"], ans: "y = 1, y = 7", show: "y", label: "y = 1, y = 7", exp: "y = 2 - x." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(1, 1) y (-5, 7)", "(1, 1)"], ans: "(1, 1) y (-5, 7)", show: "Final", label: "2 puntos", exp: "Correcto.", isLast: true }
                    ]}
                ]
            },
            radicales: {
                basico: [
                    { system: ["√x + y = 5", "x - y = 1"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "Sustituimos x = y + 1." },
                        { q: "Paso 1: Obtener la ecuación radical:", opt: ["√(y + 1) + y = 5", "√(y - 1) + y = 5"], ans: "√(y + 1) + y = 5", show: "Ec. Radical", label: "√(y+1) + y = 5", exp: "Reemplazo de x." },
                        { q: "Paso 2: Aislar la raíz cuadrada:", opt: ["√(y + 1) = 5 - y", "√(y + 1) = y - 5"], ans: "√(y + 1) = 5 - y", show: "Aislar", label: "√(y+1) = 5 - y", exp: "Trasposición." },
                        { q: "Paso 3: Elevar al cuadrado y desarrollar identidad notable:", opt: ["y + 1 = 25 - 10y + y²", "y + 1 = 25 + y²"], ans: "y + 1 = 25 - 10y + y²", show: "Identidad", label: "y+1 = 25-10y+y²", exp: "(5-y)² = 25 - 10y + y²." },
                        { q: "Paso 4: Resolver la ecuación:", opt: ["y = 3, y = 8", "y = 0"], ans: "y = 3, y = 8", show: "y", label: "y = 3, y = 8", exp: "y² - 11y + 24 = 0." },
                        { q: "Paso 5: Comprobar soluciones:", opt: ["Solo y = 3 es válida", "Ambas"], ans: "Solo y = 3 es válida", show: "Comprobar", label: "y = 3 OK", exp: "√(4) + 3 = 5." },
                        { q: "Paso 6: Calcular la otra incógnita x:", opt: ["x = 4", "x = 9"], ans: "x = 4", show: "x", label: "x = 4", exp: "x = 3 + 1 = 4." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(4, 3)", "(3, 4)"], ans: "(4, 3)", show: "Final", label: "(4, 3)", exp: "Listo.", isLast: true }
                    ]},
                    { system: ["√x = y - 1", "x + y = 7"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "Elevamos al cuadrado Ec.1." },
                        { q: "Paso 1: Obtener la ecuación radical:", opt: ["(y-1)² + y = 7", "√x + y = 7"], ans: "(y-1)² + y = 7", show: "Ec. Radical", label: "(y-1)² + y = 7", exp: "x = (y-1)²." },
                        { q: "Paso 2: Aislar la raíz:", opt: ["√x = y - 1", "No aplica"], ans: "√x = y - 1", show: "Aislar", label: "√x = y-1", exp: "Ya aislada." },
                        { q: "Paso 3: Elevar al cuadrado para eliminar el radical:", opt: ["x = (y-1)²", "x = y-1"], ans: "x = (y-1)²", show: "Elevación", label: "x = (y-1)²", exp: "Correcto." },
                        { q: "Paso 4: Desarrollar identidad notable:", opt: ["y² - 2y + 1", "y² + 1"], ans: "y² - 2y + 1", show: "Identidad", label: "y² - 2y + 1", exp: "(y-1)² = y² - 2y + 1." },
                        { q: "Paso 5: Resolver la ecuación:", opt: ["y = 3, y = -2", "y = 1"], ans: "y = 3, y = -2", show: "y", label: "y = 3, y = -2", exp: "y² - y - 6 = 0." },
                        { q: "Paso 6: Comprobar soluciones:", opt: ["Solo y = 3 es válida", "Ambas"], ans: "Solo y = 3 es válida", show: "Comprobar", label: "y = 3 OK", exp: "√x no es negativo." },
                        { q: "Paso 7: Calcular x:", opt: ["x = 4", "x = 9"], ans: "x = 4", show: "x", label: "x = 4", exp: "x = 7 - 3 = 4." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(4, 3)", "(3, 4)"], ans: "(4, 3)", show: "Final", label: "(4, 3)", exp: "Hecho.", isLast: true }
                    ]},
                    { system: ["√x + √y = 9", "√x - √y = 1"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Reducción", show: "Método", label: "Reducción", exp: "Sumamos raíces." },
                        { q: "Paso 1: Obtener la ecuación radical:", opt: ["2√x = 10", "√x = 5"], ans: ["2√x = 10", "√x = 5"], show: "Ec. Radical", label: "2√x = 10", exp: "Suma de ecuaciones." },
                        { q: "Paso 2: Aislar la raíz:", opt: ["√x = 5", "√x = 10"], ans: "√x = 5", show: "Aislar", label: "√x = 5", exp: "Correcto." },
                        { q: "Paso 3: Elevar al cuadrado para eliminar el radical:", opt: ["x = 25", "x = 5"], ans: "x = 25", show: "Elevación", label: "x = 25", exp: "5² = 25." },
                        { q: "Paso 4: Resolver la ecuación (incógnita y):", opt: ["y = 16", "y = 4"], ans: "y = 16", show: "y", label: "y = 16", exp: "5 + √y = 9." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(25, 16)", "(16, 25)"], ans: "(25, 16)", show: "Final", label: "(25, 16)", exp: "Listo.", isLast: true }
                    ]}
                ],
                experto: [
                    { system: ["√(2y+1) + x = 2", "x - 4y = 4"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "x = 4y + 4." },
                        { q: "Paso 1: Obtener la ecuación radical:", opt: ["√(2y + 1) + 4y + 4 = 2", "√(2y + 1) = 4y + 2"], ans: "√(2y + 1) + 4y + 4 = 2", show: "Ec. Radical", label: "√(2y+1) + 4y+4 = 2", exp: "Sustitución de x." },
                        { q: "Paso 2: Aislar la raíz:", opt: ["√(2y + 1) = -4y - 2", "√(2y + 1) = 4y + 2"], ans: "√(2y + 1) = -4y - 2", show: "Aislar", label: "√(2y+1) = -4y-2", exp: "Trasposición." },
                        { q: "Paso 3: Elevar al cuadrado para eliminar el radical:", opt: ["2y + 1 = (-4y - 2)²", "2y + 1 = 4y + 2"], ans: "2y + 1 = (-4y - 2)²", show: "Elevación", label: "2y+1 = (-4y-2)²", exp: "Correcto." },
                        { q: "Paso 4: Desarrollar identidad notable:", opt: ["16y² + 16y + 4", "16y² + 4"], ans: "16y² + 16y + 4", show: "Identidad", label: "16y²+16y+4", exp: "(-4y-2)² = 16y² + 16y + 4." },
                        { q: "Paso 5: Resolver la ecuación:", opt: ["y = -1/4, y = -3/4", "y = -1"], ans: "y = -1/4, y = -3/4", show: "y", label: "y = -1/4, y = -3/4", exp: "16y² + 14y + 3 = 0." },
                        { q: "Paso 6: Comprobar soluciones:", opt: ["Solo y = -3/4 es válida", "Ambas"], ans: "Solo y = -3/4 es válida", show: "Comprobar", label: "y = -3/4 OK", exp: "Verificamos en aislada." },
                        { q: "Paso 7: Calcular la otra incógnita x:", opt: ["x = 1", "x = 0"], ans: "x = 1", show: "x", label: "x = 1", exp: "x = 4(-3/4)+4 = 1." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(1, -3/4)", "(-3/4, 1)"], ans: "(1, -3/4)", show: "Final", label: "(1, -3/4)", exp: "Listo.", isLast: true }
                    ]},
                    { system: ["x - y = 5", "√x + √y = 5"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "x = y + 5." },
                        { q: "Paso 1: Obtener la ecuación radical:", opt: ["√(y + 5) + √y = 5", "y + 5 + √y = 5"], ans: "√(y + 5) + √y = 5", show: "Ec. Radical", label: "√(y+5) + √y = 5", exp: "Sustitución de x." },
                        { q: "Paso 2: Aislar la raíz:", opt: ["√(y + 5) = 5 - √y", "√(y + 5) = √y - 5"], ans: "√(y + 5) = 5 - √y", show: "Aislar", label: "√(y+5) = 5 - √y", exp: "Pasamos √y." },
                        { q: "Paso 3: Elevar al cuadrado para eliminar el radical:", opt: ["y + 5 = (5 - √y)²", "y + 5 = 25 + y"], ans: "y + 5 = (5 - √y)²", show: "Elevación 1", label: "y + 5 = (5 - √y)²", exp: "Correcto." },
                        { q: "Paso 4: Desarrollar identidad notable:", opt: ["25 - 10√y + y", "25 + y"], ans: "25 - 10√y + y", show: "Identidad", label: "25 - 10√y + y", exp: "(5-√y)² = 25 - 10√y + y." },
                        { q: "Paso 5: Volver a aislar la raíz si queda alguna:", opt: ["10√y = 20", "√y = 5"], ans: "10√y = 20", show: "Re-aislar", label: "10√y = 20", exp: "Correcto." },
                        { q: "Paso 6: Elevar al cuadrado si hemos hecho el paso anterior:", opt: ["y = 4", "y = 2"], ans: "y = 4", show: "Elevación 2", label: "y = 4", exp: "√y = 2 => y = 4." },
                        { q: "Paso 7: Comprobar soluciones:", opt: ["y = 4 Válida", "No válida"], ans: "y = 4 Válida", show: "Comprobar", label: "y = 4 OK", exp: "√9 + √4 = 3 + 2 = 5." },
                        { q: "Paso 8: Calcular la otra incógnita x:", opt: ["x = 9", "x = 4"], ans: "x = 9", show: "x", label: "x = 9", exp: "x = 4 + 5 = 9." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(9, 4)", "(4, 9)"], ans: "(9, 4)", show: "Final", label: "(9, 4)", exp: "Hecho.", isLast: true }
                    ]},
                    { system: ["x + y = 13", "√(x-1) + √(y+1) = 6"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "Igualación", "Reducción"], ans: "Sustitución", show: "Método", label: "Sustitución", exp: "y = 13 - x." },
                        { q: "Paso 1: Obtener la ecuación radical:", opt: ["√(x - 1) + √(14 - x) = 6", "√(x-1) = 6"], ans: "√(x - 1) + √(14 - x) = 6", show: "Ec. Radical", label: "√(x-1) + √(14-x) = 6", exp: "y+1 = 14-x." },
                        { q: "Paso 2: Aislar la raíz:", opt: ["√(x - 1) = 6 - √(14 - x)", "√(x - 1) = 6 - √x"], ans: "√(x - 1) = 6 - √(14 - x)", show: "Aislar", label: "√(x-1)=6-√(14-x)", exp: "Pasamos un radical." },
                        { q: "Paso 3: Elevar al cuadrado para eliminar el radical:", opt: ["x-1 = (6 - √(14 - x))²", "x-1 = 50-x"], ans: "x-1 = (6 - √(14 - x))²", show: "Elevación 1", label: "x-1 = (6 - √(14-x))²", exp: "Correcto." },
                        { q: "Paso 4: Desarrollar identidad notable:", opt: ["36 - 12√(14 - x) + 14 - x", "36 + 14 - x"], ans: "36 - 12√(14 - x) + 14 - x", show: "Identidad", label: "2x-51 = -12√(14-x)", exp: "Primer desarrollo." },
                        { q: "Paso 5: Volver a aislar la raíz si queda alguna:", opt: ["2x - 51 = -12√(14 - x)", "No aplica"], ans: "2x - 51 = -12√(14 - x)", show: "Re-aislar", label: "2x-51 = -12√(14-x)", exp: "Aislamos raíz." },
                        { q: "Paso 6: Elevar al cuadrado si hemos hecho el paso anterior:", opt: ["(2x - 51)² = 144(14 - x)", "x = 5"], ans: "(2x - 51)² = 144(14 - x)", show: "Elevación 2", label: "Segunda elevación", exp: "Correcto." },
                        { q: "Paso 7: Resolver la ecuación:", opt: ["x = 5, x = 10", "x = 4"], ans: "x = 5, x = 10", show: "x", label: "x = 5, x = 10", exp: "Raíces halladas." },
                        { q: "Paso 8: Comprobar soluciones:", opt: ["Ambas válidas", "Ninguna"], ans: "Ambas válidas", show: "Comprobar", label: "x = 5, 10 OK", exp: "Verificamos raíces." },
                        { q: "Paso 9: Calcular la otra incógnita y:", opt: ["y = 8, y = 3", "y = 5"], ans: "y = 8, y = 3", show: "y", label: "y = 8, y = 3", exp: "y = 13 - x." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(5, 8) y (10, 3)", "(5, 10)"], ans: "(5, 8) y (10, 3)", show: "Final", label: "(5, 8), (10, 3)", exp: "Correcto.", isLast: true }
                    ]}
                ]
            },
            racionales: {
                basico: [
                    { system: ["1/x + 1/y = 5/6", "x + y = 5"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "m.c.m. para quitar denominadores", "Igualación"], ans: "m.c.m. para quitar denominadores", show: "Método", label: "m.c.m.", exp: "Iniciamos quitando denominadores." },
                        { q: "Elegir el mínimo común múltiplo para Ec.1:", opt: ["6xy", "6x", "6y"], ans: "6xy", show: "m.c.m.", label: "6xy", exp: "Denominador común." },
                        { q: "Obtener la nueva ecuación:", opt: ["6y + 6x = 5xy", "y + x = 5"], ans: "6y + 6x = 5xy", show: "Nueva Ec.", label: "6x + 6y = 5xy", exp: "Multiplicamos cada término por 6xy." },
                        { q: "Formar el nuevo sistema con llaves al igual que el original:", opt: ["{ 6x + 6y = 5xy ; x + y = 5 }", "{ x + y = 5 ; xy = 6 }"], ans: "{ 6x + 6y = 5xy ; x + y = 5 }", show: "Nuevo Sistema", label: "{ 6x+6y=5xy ; x+y=5 }", exp: "Sistema equivalente." },
                        { q: "Elegir método o sustituir:", opt: ["Sustituir x+y = 5", "Reducción"], ans: "Sustituir x+y = 5", show: "Paso", label: "30 = 5xy => xy = 6", exp: "6(x+y) = 5xy => 6(5) = 30." },
                        { q: "Resolver para una incógnita y:", opt: ["y = 3, y = 2", "y = 1"], ans: "y = 3, y = 2", show: "y", label: "y = 3, y = 2", exp: "Sustituyendo x=5-y en xy=6." },
                        { q: "Calcular la otra incógnita x:", opt: ["x = 2, x = 3", "x = 4"], ans: "x = 2, x = 3", show: "x", label: "x = 2, x = 3", exp: "x = 5 - y." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(2, 3) y (3, 2)", "(2, 3)"], ans: "(2, 3) y (3, 2)", show: "Final", label: "(2, 3), (3, 2)", exp: "Coordenadas finales.", isLast: true }
                    ]},
                    { system: ["2/x - 3/y = 1", "x · y = 6"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "m.c.m. para quitar denominadores", "Igualación"], ans: "m.c.m. para quitar denominadores", show: "Método", label: "m.c.m.", exp: "Operamos Ec.1." },
                        { q: "Elegir el mínimo común múltiplo para Ec.1:", opt: ["xy", "x", "y"], ans: "xy", show: "m.c.m.", label: "xy", exp: "Multiplicamos por xy." },
                        { q: "Obtener la nueva ecuación:", opt: ["2y - 3x = xy", "2y - 3x = 1"], ans: "2y - 3x = xy", show: "Nueva Ec.", label: "2y - 3x = xy", exp: "Eliminamos denominadores." },
                        { q: "Formar el nuevo sistema con llaves al igual que el original:", opt: ["{ 2y - 3x = xy ; xy = 6 }", "{ 2y - 3x = 1 ; xy = 6 }"], ans: "{ 2y - 3x = xy ; xy = 6 }", show: "Nuevo Sistema", label: "{ 2y-3x=xy ; xy=6 }", exp: "Sistema de llaves." },
                        { q: "Elegir método o sustituir:", opt: ["Sustituir xy = 6", "Sustituir x = 6/y"], ans: "Sustituir xy = 6", show: "Paso", label: "2y - 3x = 6", exp: "Correcto." },
                        { q: "Resolver para una incógnita y:", opt: ["y = 3, y = -2", "y = 1"], ans: "y = 3, y = -2", show: "y", label: "y = 3, y = -2", exp: "Cálculos cuadráticos." },
                        { q: "Calcular la otra incógnita x:", opt: ["x = 2, x = -3", "x = 0"], ans: "x = 2, x = -3", show: "x", label: "x = 2, x = -3", exp: "x = 6/y." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(2, 3) y (-3, -2)", "(2, 3)"], ans: "(2, 3) y (-3, -2)", show: "Final", label: "(2, 3), (-3, -2)", exp: "Listo.", isLast: true }
                    ]},
                    { system: ["1/x + 1/y = 2", "x + y = 2"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "m.c.m. para quitar denominadores", "Igualación"], ans: "m.c.m. para quitar denominadores", show: "Método", label: "m.c.m.", exp: "Quitamos denominadores." },
                        { q: "Elegir el mínimo común múltiplo para Ec.1:", opt: ["xy", "x", "y"], ans: "xy", show: "m.c.m.", label: "xy", exp: "Denominador común." },
                        { q: "Obtener la nueva ecuación:", opt: ["y + x = 2xy", "y + x = 2"], ans: "y + x = 2xy", show: "Nueva Ec.", label: "x + y = 2xy", exp: "Multiplicamos por xy." },
                        { q: "Formar el nuevo sistema con llaves al igual que el original:", opt: ["{ x + y = 2xy ; x + y = 2 }", "{ x + y = 2 ; xy = 1 }"], ans: "{ x + y = 2xy ; x + y = 2 }", show: "Nuevo Sistema", label: "{ x+y=2xy ; x+y=2 }", exp: "Sistema de llaves." },
                        { q: "Elegir método o sustituir:", opt: ["Sustituir x+y = 2", "Igualar"], ans: "Sustituir x+y = 2", show: "Paso", label: "2 = 2xy => xy = 1", exp: "2 = 2xy." },
                        { q: "Resolver para una incógnita y:", opt: ["y = 1", "y = 2"], ans: "y = 1", show: "y", label: "y = 1", exp: "Sustituyendo x=2-y." },
                        { q: "Calcular la otra incógnita x:", opt: ["x = 1", "x = 2"], ans: "x = 1", show: "x", label: "x = 1", exp: "x = 2 - 1 = 1." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(1, 1)", "(2, 0)"], ans: "(1, 1)", show: "Final", label: "(1, 1)", exp: "Solución única.", isLast: true }
                    ]}
                ],
                experto: [
                    { system: ["1/x + 1/y = 1 - 1/xy", "x · y = 6"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "m.c.m. para quitar denominadores", "Igualación"], ans: "m.c.m. para quitar denominadores", show: "Método", label: "m.c.m.", exp: "Usaremos m.c.m." },
                        { q: "Elegir el mínimo común múltiplo para Ec.1:", opt: ["xy", "x", "y"], ans: "xy", show: "m.c.m.", label: "xy", exp: "Común denominador." },
                        { q: "Obtener la nueva ecuación:", opt: ["x + y = xy - 1", "x + y = xy"], ans: "x + y = xy - 1", show: "Nueva Ec.", label: "x + y = xy - 1", exp: "Multiplicamos por xy." },
                        { q: "Formar el nuevo sistema con llaves al igual que el original:", opt: ["{ x + y = xy - 1 ; xy = 6 }", "{ x + y = 6 ; xy = 1 }"], ans: "{ x + y = xy - 1 ; xy = 6 }", show: "Nuevo Sistema", label: "{ x+y=xy-1 ; xy=6 }", exp: "Alineación de llaves." },
                        { q: "Elegir método o sustituir:", opt: ["Sustituir xy = 6", "Reducción"], ans: "Sustituir xy = 6", show: "Paso", label: "x + y = 5", exp: "6 - 1 = 5." },
                        { q: "Resolver para una incógnita y:", opt: ["y = 3, y = 2", "y = 6"], ans: "y = 3, y = 2", show: "y", label: "y = 3, y = 2", exp: "Sustituyendo x = 5-y." },
                        { q: "Calcular la otra incógnita x:", opt: ["x = 2, x = 3", "x = 1"], ans: "x = 2, x = 3", show: "x", label: "x = 2, x = 3", exp: "x = 5 - y." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(2, 3) y (3, 2)", "(2, 3)"], ans: "(2, 3) y (3, 2)", show: "Final", label: "(2, 3), (3, 2)", exp: "Hecho.", isLast: true }
                    ]},
                    { system: ["(x-y)/(x+y) + (x+y)/(x-y) = 5/2", "x + y = 2"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "m.c.m. para quitar denominadores", "Igualación"], ans: "m.c.m. para quitar denominadores", show: "Método", label: "m.c.m.", exp: "Operamos denominadores." },
                        { q: "Paso 1: Despejar/Sustituir x+y=2 en Ec.1:", opt: ["(x-y)/2 + 2/(x-y) = 5/2", "(x-y)/2 = 5/2"], ans: "(x-y)/2 + 2/(x-y) = 5/2", show: "Sustitución", label: "(x-y)/2 + 2/(x-y) = 5/2", exp: "Reemplazo de suma." },
                        { q: "Paso 2: Quitar denominadores (m.c.m. = 2(x-y)):", opt: ["(x-y)² + 4 = 5(x-y)", "(x-y)² + 4 = 10"], ans: ["(x-y)² + 4 = 5(x-y)", "(x-y)² + 4 = 10"], show: "m.c.m.", label: "(x-y)² + 4 = 5(x-y)", exp: "m.c.m. = 2(x-y)." },
                        { q: "Paso 3: Desarrollar la ecuación cuadrática:", opt: ["(x-y)² - 5(x-y) + 4 = 0", "(x-y)² + 4 = 0"], ans: "(x-y)² - 5(x-y) + 4 = 0", show: "Ecuación", label: "(x-y)² - 5(x-y) + 4 = 0", exp: "Agrupación." },
                        { q: "Paso 4: Resolver para una incógnita (x-y):", opt: ["x-y = 4, x-y = 1", "x-y = 2"], ans: "x-y = 4, x-y = 1", show: "Diferencia", label: "x - y = 4, x - y = 1", exp: "Raíces halladas." },
                        { q: "Paso 5: Calcular la otra incógnita x e y:", opt: ["(3, -1) y (3/2, 1/2)", "(1, 1)"], ans: "(3, -1) y (3/2, 1/2)", show: "Incógnitas", label: "(3, -1), (3/2, 1/2)", exp: "Sistemas lineales." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(3, -1) y (3/2, 1/2)", "(3, -1)"], ans: "(3, -1) y (3/2, 1/2)", show: "Final", label: "2 soluciones", exp: "Listo.", isLast: true }
                    ]},
                    { system: ["1/x² + 1/y² = 13/36", "x · y = 6"], steps: [
                        { q: "¿Elegir método?", opt: ["Sustitución", "m.c.m. para quitar denominadores", "Igualación"], ans: "m.c.m. para quitar denominadores", show: "Método", label: "m.c.m.", exp: "Usaremos m.c.m. primero." },
                        { q: "Elegir el mínimo común múltiplo para Ec.1:", opt: ["36x²y²", "xy"], ans: "36x²y²", show: "m.c.m.", label: "36x²y²", exp: "Correcto." },
                        { q: "Obtener la nueva ecuación:", opt: ["36y² + 36x² = 13x²y²", "x² + y² = 13"], ans: "36y² + 36x² = 13x²y²", show: "Nueva Ec.", label: "x² + y² = 13", exp: "36(x²+y²) = 13(6²) = 13(36)." },
                        { q: "Formar el nuevo sistema con llaves al igual que el original:", opt: ["{ x² + y² = 13 ; xy = 6 }", "{ x² + y² = 5 ; xy = 6 }"], ans: "{ x² + y² = 13 ; xy = 6 }", show: "Nuevo Sistema", label: "{ x²+y²=13 ; xy=6 }", exp: "Sistema equivalente." },
                        { q: "Resolver para una incógnita x:", opt: ["x = ±3, x = ±2", "x = 4"], ans: "x = ±3, x = ±2", show: "x", label: "x = ±3, x = ±2", exp: "Resolución cuadrática." },
                        { q: "Calcular la otra incógnita y:", opt: ["y = ±2, y = ±3", "y = 6"], ans: "y = ±2, y = ±3", show: "y", label: "y = ±2, y = ±3", exp: "y = 6/x." },
                        { q: "Paso Final: Elegir las soluciones con paréntesis:", opt: ["(3, 2), (2, 3), (-3, -2) y (-2, -3)", "(1, 6)"], ans: "(3, 2), (2, 3), (-3, -2) y (-2, -3)", show: "Final", label: "4 soluciones", exp: "Completado.", isLast: true }
                    ]}
                ]
            }
        };

        let state = { points: 0, streak: 0, history: [0], cat: null, diff: null, exIdx: 0, stepIdx: 0, isLocked: false };

        function showHome() {
            document.getElementById('screen-categories').classList.remove('hidden');
            document.getElementById('screen-difficulty').classList.add('hidden');
            document.getElementById('screen-game').classList.add('hidden');
            document.getElementById('congrats-modal').classList.add('hidden');
        }

        function showDifficulty(cat) {
            state.cat = cat;
            document.getElementById('screen-categories').classList.add('hidden');
            document.getElementById('screen-difficulty').classList.remove('hidden');
        }

        function initGame(diff) {
            state.diff = diff; state.exIdx = 0; state.stepIdx = 0; state.isLocked = false;
            document.getElementById('screen-difficulty').classList.add('hidden');
            document.getElementById('screen-game').classList.remove('hidden');
            document.getElementById('game-info-label').innerText = state.cat;
            document.getElementById('blackboard-history').innerHTML = '';
            loadExercise();
        }

        function formatMath(text) {
            let res = text.replace(/√\(([^)]+)\)/g, '<span class="sqrt"><span class="sqrt-symbol">√</span><span class="sqrt-content">$1</span></span>')
                          .replace(/√(\w)/g, '<span class="sqrt"><span class="sqrt-symbol">√</span><span class="sqrt-content">$1</span></span>');
            
            res = res.replace(/\{([^;]+);([^}]+)\}/g, (match, p1, p2) => {
                return `<div class="mini-system"><span class="mini-bracket">{</span><div class="mini-equations"><span>${p1.trim()}</span><span>${p2.trim()}</span></div></div>`;
            });
            return res;
        }

        function loadExercise() {
            if (!DATABASE[state.cat] || !DATABASE[state.cat][state.diff]) {
                showHome();
                return;
            }
            const dataRoot = DATABASE[state.cat][state.diff];
            const ex = dataRoot[state.exIdx];
            if(!ex) {
                showHome();
                return;
            }
            document.getElementById('equation-lines').innerHTML = ex.system.map(l => `<div>${formatMath(l)}</div>`).join('');
            document.getElementById('feedback-ui').classList.add('hidden');
            document.getElementById('btn-next-step').classList.add('hidden');
            document.getElementById('congrats-modal').classList.add('hidden');
            
            const step = ex.steps[state.stepIdx];
            document.getElementById('instruction-text').innerText = step.q;
            renderOptions(step.opt, step.ans);
            state.isLocked = false;
        }

        function renderOptions(opts, correctVal) {
            const container = document.getElementById('game-options');
            container.innerHTML = '';
            let displayOpts = [...opts];
            
            // Si es racionales y el primer paso, nos aseguramos de que el método m.c.m esté presente
            if (state.stepIdx === 0) {
                if (state.cat === 'racionales') {
                    displayOpts = ["Sustitución", "Igualación", "m.c.m. para quitar denominadores"];
                } else {
                    displayOpts = ["Sustitución", "Igualación", "Reducción"];
                }
            }

            displayOpts.sort(() => Math.random() - 0.5).forEach(opt => {
                const btn = document.createElement('button');
                btn.className = "p-4 text-left border-2 border-slate-200 rounded-2xl font-bold text-slate-700 bg-white transition-all hover:border-indigo-500 hover:bg-slate-50 active:scale-[0.98] shadow-sm text-sm";
                btn.innerText = opt;
                btn.onclick = (e) => handleChoice(e, opt, correctVal);
                container.appendChild(btn);
            });
        }

        function handleChoice(e, selectedText, correctVal) {
            if(state.isLocked) return;
            state.isLocked = true;
            const btnRef = e.currentTarget;
            let isCorrect = false;
            if (Array.isArray(correctVal)) {
                isCorrect = correctVal.some(v => selectedText === v || selectedText.includes(v));
            } else {
                isCorrect = selectedText === correctVal;
            }
            const feedbackUi = document.getElementById('feedback-ui');
            const feedbackMsg = document.getElementById('feedback-msg');
            const nextBtn = document.getElementById('btn-next-step');
            if(isCorrect) {
                btnRef.classList.add('btn-correct');
                state.streak++;
                const ex = DATABASE[state.cat][state.diff][state.exIdx];
                const step = ex.steps[state.stepIdx];
                feedbackUi.className = "mt-6 p-6 rounded-2xl border-l-[8px] bg-emerald-50 border-emerald-500 text-emerald-900";
                feedbackMsg.innerHTML = step.exp;
                addToBlackboard(`✔ ${step.show}: <b>${formatMath(step.label)}</b>`);
                if(step.isLast) document.getElementById('congrats-modal').classList.remove('hidden');
                else nextBtn.classList.remove('hidden');
            } else {
                state.streak = 0;
                btnRef.classList.add('btn-wrong');
                feedbackUi.className = "mt-6 p-6 rounded-2xl border-l-[8px] bg-red-50 border-red-500 text-red-900";
                feedbackMsg.innerHTML = "Lógica incorrecta. Revisa el procedimiento algebraico.";
                setTimeout(() => { 
                    state.isLocked = false; 
                    btnRef.classList.remove('btn-wrong'); 
                }, 1000);
            }
            feedbackUi.classList.remove('hidden');
            updateStats();
        }

        function skipExercise() {
            state.exIdx++; state.stepIdx = 0;
            document.getElementById('blackboard-history').innerHTML = '';
            loadExercise();
        }

        function processNext(fromCongrats = false) {
            if(fromCongrats) {
                state.points += 100;
                state.exIdx++; state.stepIdx = 0;
                document.getElementById('blackboard-history').innerHTML = '';
            } else {
                state.stepIdx++;
            }
            updateStats();
            loadExercise();
        }

        function addToBlackboard(msg) {
            const board = document.getElementById('blackboard-history');
            const div = document.createElement('div');
            div.className = "step-entry";
            div.innerHTML = msg;
            board.appendChild(div);
            const logContainer = document.getElementById('log-container');
            logContainer.scrollTo({ top: logContainer.scrollHeight, behavior: 'smooth' });
        }

        function updateStats() {
            document.getElementById('ui-points').innerText = state.points;
            document.getElementById('ui-streak').innerText = `${state.streak} 🔥`;
            state.history.push(state.points);
            drawProgress();
        }

        function drawProgress() {
            const canvas = document.getElementById('progressCanvas');
            const ctx = canvas.getContext('2d');
            const w = canvas.width = canvas.offsetWidth;
            const h = canvas.height = canvas.offsetHeight;
            ctx.clearRect(0,0,w,h);
            const pts = state.history.slice(-15);
            const max = Math.max(...pts, 100);
            const step = w / (pts.length - 1 || 1);
            ctx.beginPath(); ctx.strokeStyle = '#6366f1'; ctx.lineWidth = 3;
            pts.forEach((p, i) => {
                const x = i * step; const y = h - (p / max * (h - 30)) - 15;
                if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
            });
            ctx.stroke();
            ctx.fillStyle = 'rgba(99, 102, 241, 0.05)';
            ctx.lineTo(w, h); ctx.lineTo(0, h); ctx.fill();
        }

        window.onload = drawProgress;
    </script>
</body>
</html>
