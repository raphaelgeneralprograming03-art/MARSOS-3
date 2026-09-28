
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA MARSOS + SROS + SAMS/MHD - DIGITAL TWIN 3D</title>
    <style>
        :root {
            --red-metallic: #8b0000;
            --gold-anodized: #d4af37;
            --titanium-silver: #a8b2c1;
            --hud-blue: #00f0ff;
            --reactor-cyan: #00ffff;
            --bg-dark: #030712;
            --panel-bg: rgba(7, 16, 34, 0.88);
            --border-hud: rgba(0, 240, 255, 0.4);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, Roboto, sans-serif;
            user-select: none;
        }

        body {
            background-color: var(--bg-dark);
            color: #d1d5db;
            height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        header {
            height: 50px;
            background: linear-gradient(90deg, rgba(139,0,0,0.4) 0%, rgba(3,7,18,0.9) 50%, rgba(212,175,55,0.3) 100%);
            border-bottom: 1px solid var(--border-hud);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 20px;
            z-index: 10;
        }

        header h1 {
            font-size: 0.95rem;
            color: var(--hud-blue);
            letter-spacing: 2px;
            text-shadow: 0 0 10px var(--hud-blue);
        }

        .sys-badges {
            display: flex;
            gap: 10px;
            font-size: 0.7rem;
        }

        .badge {
            padding: 3px 8px;
            border-radius: 3px;
            border: 1px solid var(--border-hud);
            background: rgba(0, 240, 255, 0.1);
            color: #fff;
        }

        .app-container {
            display: grid;
            grid-template-columns: 320px 1fr 340px;
            height: calc(100vh - 50px);
            position: relative;
        }

        .panel {
            background: var(--panel-bg);
            border-right: 1px solid var(--border-hud);
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            overflow-y: auto;
            z-index: 5;
            backdrop-filter: blur(10px);
        }

        .panel-right {
            border-right: none;
            border-left: 1px solid var(--border-hud);
        }

        .panel-title {
            font-size: 0.8rem;
            color: var(--gold-anodized);
            border-bottom: 1px solid var(--border-hud);
            padding-bottom: 4px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 6px;
        }

        button {
            background: rgba(0, 240, 255, 0.05);
            border: 1px solid var(--border-hud);
            color: var(--hud-blue);
            padding: 8px 5px;
            font-size: 0.68rem;
            font-weight: bold;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.2s;
            text-transform: uppercase;
        }

        button:hover, button.active {
            background: var(--hud-blue);
            color: #000;
            box-shadow: 0 0 10px var(--hud-blue);
        }

        button.btn-mode {
            border-color: var(--gold-anodized);
            color: var(--gold-anodized);
        }

        button.btn-mode:hover, button.btn-mode.active {
            background: var(--gold-anodized);
            color: #000;
            box-shadow: 0 0 10px var(--gold-anodized);
        }

        .calc-box {
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(212, 175, 55, 0.3);
            border-radius: 4px;
            padding: 8px;
            font-size: 0.72rem;
        }

        .calc-box label {
            display: block;
            color: var(--titanium-silver);
            margin-bottom: 3px;
        }

        .calc-box input[type="range"] {
            width: 100%;
            margin-bottom: 6px;
            accent-color: var(--hud-blue);
        }

        .calc-output {
            color: var(--reactor-cyan);
            font-family: monospace;
            font-weight: bold;
            display: flex;
            justify-content: space-between;
        }

        #viewport {
            width: 100%;
            height: 100%;
            position: relative;
        }

        .viewport-hud {
            position: absolute;
            top: 15px;
            left: 15px;
            pointer-events: none;
            font-family: monospace;
            font-size: 0.75rem;
            color: var(--hud-blue);
            line-height: 1.4;
            background: rgba(3, 7, 18, 0.6);
            padding: 10px;
            border-radius: 4px;
            border: 1px solid var(--border-hud);
        }

        .func-toggle {
            display: flex;
            align-items: center;
            justify-content: space-between;
            font-size: 0.72rem;
            background: rgba(255, 255, 255, 0.03);
            padding: 5px 8px;
            border-radius: 3px;
            margin-bottom: 4px;
            border: 1px solid rgba(255,255,255,0.05);
        }

        .switch {
            position: relative;
            display: inline-block;
            width: 32px;
            height: 16px;
        }

        .switch input { opacity: 0; width: 0; height: 0; }

        .slider {
            position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0;
            background-color: #333; transition: .3s; border-radius: 16px;
        }

        .slider:before {
            position: absolute; content: ""; height: 10px; width: 10px; left: 3px; bottom: 3px;
            background-color: white; transition: .3s; border-radius: 50%;
        }

        input:checked + .slider { background-color: var(--hud-blue); }
        input:checked + .slider:before { transform: translateX(16px); }

        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: rgba(0,0,0,0.2); }
        ::-webkit-scrollbar-thumb { background: var(--border-hud); border-radius: 3px; }
    </style>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
</head>
<body>

    <header>
        <h1>ARMADURA NANOTECNOLÓGICA - MARK LXXXV</h1>
        <div class="sys-badges">
            <div class="badge">M.A.R.S.O.S.: <span style="color:#4ade80">ONLINE</span></div>
            <div class="badge">SROS: <span style="color:#4ade80">SYNC</span></div>
            <div class="badge">MHD: <span style="color:#4ade80" id="badge-mhd">1.21 GW</span></div>
        </div>
    </header>

    <div class="app-container">
        <!-- PAINEL ESQUERDO -->
        <aside class="panel">
            <div class="panel-title">1. Ângulos de Câmera</div>
            <div class="btn-grid">
                <button class="btn-cam active" onclick="setCameraView('FRONTAL', this)">Frontal</button>
                <button class="btn-cam" onclick="setCameraView('POSTERIOR', this)">Posterior</button>
                <button class="btn-cam" onclick="setCameraView('LATERAL_DIR', this)">Lat. Direita</button>
                <button class="btn-cam" onclick="setCameraView('LATERAL_ESQ', this)">Lat. Esquerda</button>
                <button class="btn-cam" onclick="setCameraView('SUPERIOR', this)">Superior</button>
                <button class="btn-cam" onclick="setCameraView('INFERIOR', this)">Inferior</button>
            </div>

            <div class="panel-title">2. Visão Estrutural (SROS)</div>
            <div class="btn-grid">
                <button id="btn-vis-ext" class="btn-mode active" onclick="setVisMode('EXTERNA')">Externa</button>
                <button id="btn-vis-int" class="btn-mode" onclick="setVisMode('INTERNA')">Interna (Raio-X)</button>
                <button id="btn-vis-hib" class="btn-mode" onclick="setVisMode('HIBRIDA')" style="grid-column: span 2">Híbrida (Transparente)</button>
            </div>

            <div class="panel-title">3. Calculadoras da Tríade</div>
            
            <div class="calc-box">
                <label>Potência Reator MHD (GW):</label>
                <input type="range" id="input-mhd" min="0.1" max="2.5" step="0.1" value="1.21" oninput="executarCalculos()">
                <div class="calc-output"><span>Campo Escudo:</span><span id="out-escudo">121.0 Tesla</span></div>
                <div class="calc-output"><span>Empuxo Plasma:</span><span id="out-empuxo">84.7 kN</span></div>
            </div>

            <div class="calc-box">
                <label>CO₂ Respiratório (L/min):</label>
                <input type="range" id="input-co2" min="0.2" max="2.0" step="0.1" value="0.8" oninput="executarCalculos()">
                <div class="calc-output"><span>Taxa Nanotubos:</span><span id="out-reparo">4.8 mm²/s</span></div>
            </div>

            <div class="calc-box">
                <label>Impacto Cinético (kN):</label>
                <input type="range" id="input-impacto" min="0" max="500" step="10" value="150" oninput="executarCalculos()">
                <div class="calc-output"><span>Absorção S.A.M.S.:</span><span id="out-sams">98.5%</span></div>
                <div class="calc-output"><span>Carga Óssea:</span><span id="out-ossos">0.0 MPa</span></div>
            </div>
        </aside>

        <!-- VIEWPORT THREE.JS -->
        <main id="viewport">
            <div class="viewport-hud">
                DIGITAL TWIN - ARMADURA MARK LXXXV<br>
                ANGULO: <span id="hud-angulo">FRONTAL</span><br>
                CAMADA: <span id="hud-camada">EXTERNA</span><br>
                TRIPLE-CORE: MHD + SROS + M.A.R.S.O.S.
            </div>
        </main>

        <!-- PAINEL DIREITO -->
        <aside class="panel panel-right">
            <div class="panel-title">4. Simulação de Subsistemas</div>

            <div class="calc-box">
                <label style="color:var(--gold-anodized); font-weight:bold;">CAPACETE & CÚPULA</label>
                <div class="func-toggle"><span>HUD Holográfico SROS</span><label class="switch"><input type="checkbox" id="sw-hud" checked onchange="toggleFuncao('hud')"><span class="slider"></span></label></div>
                <div class="func-toggle"><span>Protetor Ocular LCD</span><label class="switch"><input type="checkbox" id="sw-lcd" onchange="toggleFuncao('lcd')"><span class="slider"></span></label></div>
            </div>

            <div class="calc-box">
                <label style="color:var(--gold-anodized); font-weight:bold;">CHASSI & TORSO</label>
                <div class="func-toggle"><span>Reator Arc & Escudo MHD</span><label class="switch"><input type="checkbox" id="sw-mhd" checked onchange="toggleFuncao('mhd')"><span class="slider"></span></label></div>
                <div class="func-toggle"><span>Reciclador CO₂ Carbono</span><label class="switch"><input type="checkbox" id="sw-co2" checked onchange="toggleFuncao('co2')"><span class="slider"></span></label></div>
            </div>

            <div class="calc-box">
                <label style="color:var(--gold-anodized); font-weight:bold;">BRAÇOS & PERNAS</label>
                <div class="func-toggle"><span>Placas S.A.M.S. Cinéticas</span><label class="switch"><input type="checkbox" id="sw-sams" checked onchange="toggleFuncao('sams')"><span class="slider"></span></label></div>
                <div class="func-toggle"><span>Propulsão a Plasma</span><label class="switch"><input type="checkbox" id="sw-prop" checked onchange="toggleFuncao('prop')"><span class="slider"></span></label></div>
            </div>
        </aside>
    </div>

    <script>
        const container = document.getElementById('viewport');
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x030712);

        const camera = new THREE.PerspectiveCamera(45, container.clientWidth / container.clientHeight, 0.1, 1000);
        camera.position.set(0, 0.5, 9);

        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(container.clientWidth, container.clientHeight);
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.toneMapping = THREE.ACESFilmicToneMapping;
        renderer.toneMappingExposure = 1.2;
        container.appendChild(renderer.domElement);

        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;

        // Luzes
        scene.add(new THREE.AmbientLight(0xffffff, 0.7));
        const dirLight1 = new THREE.DirectionalLight(0xffffff, 1.2);
        dirLight1.position.set(5, 10, 7);
        scene.add(dirLight1);

        const dirLight2 = new THREE.DirectionalLight(0x00f0ff, 0.8);
        dirLight2.position.set(-5, -5, -5);
        scene.add(dirLight2);

        const pointLightCore = new THREE.PointLight(0x00ffff, 3.0, 4);
        pointLightCore.position.set(0, 0.9, 0.5);
        scene.add(pointLightCore);

        // Materiais Realistas
        const matRed = new THREE.MeshStandardMaterial({ color: 0x8b0000, metalness: 0.85, roughness: 0.25 });
        const matGold = new THREE.MeshStandardMaterial({ color: 0xd4af37, metalness: 0.9, roughness: 0.2 });
        const matSilver = new THREE.MeshStandardMaterial({ color: 0xa8b2c1, metalness: 0.95, roughness: 0.15 });
        const matCyan = new THREE.MeshBasicMaterial({ color: 0x00ffff });
        const matGlowInt = new THREE.MeshBasicMaterial({ color: 0x00f0ff, wireframe: true });
        const matCO2 = new THREE.MeshBasicMaterial({ color: 0x4ade80, wireframe: true });
        const matPropulsion = new THREE.MeshBasicMaterial({ color: 0x00f0ff, transparent: true, opacity: 0.8 });

        // Grupos
        const armorGroup = new THREE.Group();
        const externalGroup = new THREE.Group();
        const internalGroup = new THREE.Group();
        const fxGroup = new THREE.Group();
        const propulsionFXGroup = new THREE.Group();
        const samsGroup = new THREE.Group();
        const co2Group = new THREE.Group();

        armorGroup.add(externalGroup);
        armorGroup.add(internalGroup);
        armorGroup.add(fxGroup);
        armorGroup.add(propulsionFXGroup);
        scene.add(armorGroup);

        // --- CONSTRUÇÃO DETALHADA DA ARMADURA ---

        // Capacete & Rosto
        const helmet = new THREE.Mesh(new THREE.SphereGeometry(0.38, 32, 16), matRed);
        helmet.position.set(0, 1.85, 0); helmet.scale.set(0.9, 1.1, 1.0);
        externalGroup.add(helmet);

        const face = new THREE.Mesh(new THREE.ConeGeometry(0.3, 0.45, 4), matGold);
        face.position.set(0, 1.82, 0.22); face.rotation.x = -Math.PI / 4;
        externalGroup.add(face);

        const eyes = new THREE.Mesh(new THREE.BoxGeometry(0.26, 0.04, 0.1), matCyan);
        eyes.position.set(0, 1.9, 0.35);
        externalGroup.add(eyes);

        // Torso / Peitoral
        const chest = new THREE.Mesh(new THREE.CylinderGeometry(0.7, 0.45, 1.2, 8), matRed);
        chest.position.set(0, 0.85, 0);
        externalGroup.add(chest);

        // Reator Arc
        const coreMesh = new THREE.Mesh(new THREE.OctahedronGeometry(0.18, 2), matCyan);
        coreMesh.position.set(0, 0.95, 0.42);
        externalGroup.add(coreMesh);

        // Ombros, Braços e Mãos
        [-0.85, 0.85].forEach(x => {
            const isLeft = x < 0;
            const shoulder = new THREE.Mesh(new THREE.SphereGeometry(0.32, 16, 16), matGold);
            shoulder.position.set(x, 1.25, 0); externalGroup.add(shoulder);

            const arm = new THREE.Mesh(new THREE.CylinderGeometry(0.15, 0.12, 0.7), matRed);
            arm.position.set(x, 0.7, 0); externalGroup.add(arm);

            const hand = new THREE.Mesh(new THREE.SphereGeometry(0.12, 12, 12), matSilver);
            hand.position.set(x, 0.25, 0); externalGroup.add(hand);

            // Efeito Propulsor das Mãos
            const pHand = new THREE.Mesh(new THREE.ConeGeometry(0.08, 0.4, 16), matPropulsion);
            pHand.position.set(x, 0.05, 0);
            pHand.rotation.x = Math.PI;
            propulsionFXGroup.add(pHand);
        });

        // Cintura e Pernas
        const waist = new THREE.Mesh(new THREE.CylinderGeometry(0.45, 0.4, 0.3, 8), matGold);
        waist.position.set(0, 0.1, 0);
        externalGroup.add(waist);

        [-0.35, 0.35].forEach(x => {
            const thigh = new THREE.Mesh(new THREE.CylinderGeometry(0.2, 0.15, 0.8), matRed);
            thigh.position.set(x, -0.45, 0); externalGroup.add(thigh);

            const calf = new THREE.Mesh(new THREE.CylinderGeometry(0.15, 0.12, 0.9), matSilver);
            calf.position.set(x, -1.3, 0); externalGroup.add(calf);

            const foot = new THREE.Mesh(new THREE.BoxGeometry(0.22, 0.12, 0.4), matRed);
            foot.position.set(x, -1.8, 0.1); externalGroup.add(foot);

            // Efeito Propulsor dos Pés
            const pFoot = new THREE.Mesh(new THREE.ConeGeometry(0.1, 0.7, 16), matPropulsion);
            pFoot.position.set(x, -2.2, 0);
            pFoot.rotation.x = Math.PI;
            propulsionFXGroup.add(pFoot);
        });

        // --- PLACAS S.A.M.S. (Cinéticas Externa) ---
        [-0.5, 0.5].forEach(x => {
            const samsPlate = new THREE.Mesh(new THREE.BoxGeometry(0.25, 0.4, 0.08), matSilver);
            samsPlate.position.set(x, 0.85, 0.38);
            samsGroup.add(samsPlate);
        });
        externalGroup.add(samsGroup);

        // --- ESTRUTURA INTERNA (RAIO-X / SROS) ---
        const spine = new THREE.Mesh(new THREE.CylinderGeometry(0.06, 0.06, 2.6), matGlowInt);
        spine.position.set(0, 0.2, -0.05);
        internalGroup.add(spine);

        const innerHead = new THREE.Mesh(new THREE.SphereGeometry(0.28, 16, 16), matGlowInt);
        innerHead.position.set(0, 1.85, 0);
        internalGroup.add(innerHead);

        const innerChest = new THREE.Mesh(new THREE.BoxGeometry(0.6, 0.8, 0.4), matGlowInt);
        innerChest.position.set(0, 0.85, 0);
        internalGroup.add(innerChest);

        // Reciclador CO2 Carbono Interno
        const co2Pipes = new THREE.Mesh(new THREE.TorusGeometry(0.28, 0.03, 8, 24), matCO2);
        co2Pipes.position.set(0, 0.8, 0.1);
        co2Pipes.rotation.x = Math.PI / 2;
        co2Group.add(co2Pipes);
        internalGroup.add(co2Group);

        internalGroup.visible = false; // Inicia na visão Externa

        // Escudo MHD
        const shieldMesh = new THREE.Mesh(
            new THREE.SphereGeometry(2.5, 32, 16),
            new THREE.MeshBasicMaterial({ color: 0x00f0ff, transparent: true, opacity: 0.15, wireframe: true })
        );
        fxGroup.add(shieldMesh);

        // 1. Algoritmo de Câmera e Visão
        function setCameraView(vista, btnElement) {
            document.getElementById('hud-angulo').innerText = vista;
            if(btnElement) {
                document.querySelectorAll('.btn-cam').forEach(b => b.classList.remove('active'));
                btnElement.classList.add('active');
            }

            const targetPos = new THREE.Vector3();
            switch (vista) {
                case 'FRONTAL': targetPos.set(0, 0.5, 9); break;
                case 'POSTERIOR': targetPos.set(0, 0.5, -9); break;
                case 'LATERAL_DIR': targetPos.set(9, 0.5, 0); break;
                case 'LATERAL_ESQ': targetPos.set(-9, 0.5, 0); break;
                case 'SUPERIOR': targetPos.set(0, 9.5, 0.1); break;
                case 'INFERIOR': targetPos.set(0, -8.5, 0.1); break;
            }
            camera.position.copy(targetPos);
            controls.target.set(0, 0, 0);
            controls.update();
        }

        function setVisMode(modo) {
            document.getElementById('hud-camada').innerText = modo;
            document.querySelectorAll('.btn-mode').forEach(b => b.classList.remove('active'));

            if (modo === 'EXTERNA') {
                document.getElementById('btn-vis-ext').classList.add('active');
                externalGroup.visible = true;
                internalGroup.visible = false;
                setArmourOpacity(1.0, false);
            } else if (modo === 'INTERNA') {
                document.getElementById('btn-vis-int').classList.add('active');
                externalGroup.visible = false;
                internalGroup.visible = true;
            } else if (modo === 'HIBRIDA') {
                document.getElementById('btn-vis-hib').classList.add('active');
                externalGroup.visible = true;
                internalGroup.visible = true;
                setArmourOpacity(0.35, true);
            }
        }

        function setArmourOpacity(opacity, transparent) {
            externalGroup.traverse((child) => {
                if (child.isMesh) {
                    child.material.transparent = transparent;
                    child.material.opacity = opacity;
                    child.material.needsUpdate = true; // CORREÇÃO CRÍTICA DO THREE.JS
                }
            });
        }

        // 2 & 3. Algoritmos das Calculadoras
        function executarCalculos() {
            const gw = parseFloat(document.getElementById('input-mhd').value);
            const co2 = parseFloat(document.getElementById('input-co2').value);
            const impacto = parseFloat(document.getElementById('input-impacto').value);

            document.getElementById('badge-mhd').innerText = `${gw.toFixed(2)} GW`;

            // Algoritmo MHD
            document.getElementById('out-escudo').innerText = `${(gw * 100).toFixed(1)} Tesla`;
            document.getElementById('out-empuxo').innerText = `${(gw * 70).toFixed(1)} kN`;
            shieldMesh.scale.setScalar(1.0 + (gw * 0.08));

            // Algoritmo Bio / SAMS
            document.getElementById('out-reparo').innerText = `${(co2 * 6).toFixed(1)} mm²/s`;
            const absorcao = Math.max(70.0, 99.5 - (impacto * 0.02));
            const estresse = impacto > 350 ? ((impacto - 350) * 0.1).toFixed(1) : 0.0;

            document.getElementById('out-sams').innerText = `${absorcao.toFixed(1)}%`;
            document.getElementById('out-ossos').innerText = `${estresse} MPa`;

            // Reação visual ao impacto nas placas SAMS
            samsGroup.children.forEach(plate => {
                const shift = (impacto / 500) * 0.08;
                plate.position.z = 0.38 - shift;
            });
        }

        // 4. Alternância Dinâmica das Funções
        function toggleFuncao(func) {
            if (func === 'hud') {
                eyes.material.color.setHex(document.getElementById('sw-hud').checked ? 0x00ffff : 0x222222);
            }
            if (func === 'lcd') {
                face.material.color.setHex(document.getElementById('sw-lcd').checked ? 0x111111 : 0xd4af37);
            }
            if (func === 'mhd') {
                const checked = document.getElementById('sw-mhd').checked;
                shieldMesh.visible = checked;
                coreMesh.visible = checked;
            }
            if (func === 'co2') {
                co2Group.visible = document.getElementById('sw-co2').checked;
            }
            if (func === 'sams') {
                samsGroup.visible = document.getElementById('sw-sams').checked;
            }
            if (func === 'prop') {
                const checked = document.getElementById('sw-prop').checked;
                pointLightCore.intensity = checked ? 3.0 : 0.2;
                propulsionFXGroup.visible = checked;
            }
        }

        // Loop principal 60 FPS
        function animate() {
            requestAnimationFrame(animate);

            // Animação contínua dos subsistemas
            coreMesh.rotation.z += 0.02;
            coreMesh.rotation.y += 0.01;

            if (shieldMesh.visible) {
                shieldMesh.rotation.y += 0.005;
            }

            if (co2Group.visible) {
                co2Pipes.rotation.z += 0.03;
            }

            // Animação de pulsação suave no empuxo de plasma
            if (propulsionFXGroup.visible) {
                const scale = 1 + Math.sin(Date.now() * 0.01) * 0.1;
                propulsionFXGroup.children.forEach(p => p.scale.set(scale, scale, scale));
            }

            controls.update();
            renderer.render(scene, camera);
        }

        window.addEventListener('resize', () => {
            camera.aspect = container.clientWidth / container.clientHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(container.clientWidth, container.clientHeight);
        });

        // Inicialização
        executarCalculos();
        animate();
    </script>
</body>
</html>
