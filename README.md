<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WR7 SERVERSIDE - ADM PANEL</title>
    <style>
        :root { --neon: #00ff88; --bg: #0a0a0a; --panel: #151515; }
        * { box-sizing: border-box; transition: 0.4s ease; }
        body { background: var(--bg); color: white; font-family: 'Segoe UI', sans-serif; margin: 0; display: flex; overflow: hidden; }
        
        /* Sidebar Retrátil */
        #sidebar { width: 250px; background: var(--panel); height: 100vh; border-right: 2px solid var(--neon); padding: 20px; position: relative; flex-shrink: 0; }
        #sidebar.closed { width: 0; padding: 0; overflow: hidden; border: none; }
        
        .toggle-btn { position: absolute; right: -40px; top: 20px; background: var(--panel); border: 2px solid var(--neon); color: var(--neon); padding: 10px; cursor: pointer; border-radius: 0 8px 8px 0; z-index: 1000; }
        
        .logo { font-size: 24px; font-weight: bold; color: var(--neon); text-shadow: 0 0 10px var(--neon); margin-bottom: 30px; white-space: nowrap; }
        .tab-btn { display: block; width: 100%; padding: 12px; margin: 10px 0; background: #222; border: none; color: #aaa; border-radius: 8px; cursor: pointer; text-align: left; }
        .tab-btn:hover, .active { background: var(--neon); color: black; font-weight: bold; box-shadow: 0 0 15px var(--neon); }

        /* Conteúdo */
        #main { flex-grow: 1; padding: 30px; overflow-y: auto; background: linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 100%); }
        .card { background: rgba(25, 25, 25, 0.9); padding: 20px; border-radius: 12px; border: 1px solid #333; margin-bottom: 20px; }
        
        /* ScriptBlox List */
        #results { margin-top: 20px; display: grid; gap: 10px; }
        .script-card { background: #222; padding: 15px; border-radius: 8px; border-left: 5px solid var(--neon); display: flex; justify-content: space-between; }
        
        input, textarea { width: 100%; padding: 12px; background: #000; color: #0f0; border: 1px solid var(--neon); border-radius: 8px; margin-bottom: 10px; }
        .status { padding: 5px 10px; border-radius: 20px; font-size: 12px; font-weight: bold; }
        .off { background: #550000; color: #ff5555; }
    </style>
</head>
<body>

<button class="toggle-btn" onclick="toggleSidebar()">☰</button>

<div id="sidebar">
    <div class="logo">WR7 WEB</div>
    <p style="font-size: 12px;">ADM: <b>yhago636</b></p>
    <button class="tab-btn active" onclick="show('dash')">📊 DASHBOARD</button>
    <button class="tab-btn" onclick="show('exec')">💻 EXECUTOR</button>
    <button class="tab-btn" onclick="show('sblox')">🔍 SCRIPTBLOX</button>
    <button class="tab-btn" onclick="show('players')">😈 PLAYERS TOOL</button>
    <button class="tab-btn" onclick="show('cfg')">⚙️ CONFIGURAÇÕES</button>
</div>

<div id="main">
    <div id="dash" class="tab">
        <div class="card">
            <h2>Status da Conexão</h2>
            <p>Antena Roblox: <span class="status off">DESCONECTADO</span></p>
            <p>Jogo: <b>Nenhum (Aguardando Antena...)</b></p>
        </div>
    </div>

    <div id="sblox" class="tab" style="display:none;">
        <div class="card">
            <h2>ScriptBlox Finder</h2>
            <input type="text" id="sInput" placeholder="Pesquisar Script (Ex: King Legacy)...">
            <button onclick="fetchScripts()" class="tab-btn" style="width:100px; display:inline;">BUSCAR</button>
            <div id="results"></div>
        </div>
    </div>

    <div id="players" class="tab" style="display:none;">
        <div class="card">
            <h2>Players Tool (ServerSide)</h2>
            <div class="script-card">
                <span>Player_Amigo</span>
                <div>
                    <button onclick="cmd('fling')" style="background:red; color:white;">FLING</button>
                    <button onclick="cmd('jail')" style="background:orange;">JAIL</button>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    function toggleSidebar() { document.getElementById('sidebar').classList.toggle('closed'); }

    function show(id) {
        document.querySelectorAll('.tab').forEach(t => t.style.display = 'none');
        document.getElementById(id).style.display = 'block';
    }

    async function fetchScripts() {
        const query = document.getElementById('sInput').value;
        const res = await fetch(`https://scriptblox.com/api/script/search?q=${query}&mode=free`);
        const data = await res.json();
        let html = '';
        data.result.scripts.forEach(s => {
            html += `<div class="script-card"><span>${s.title}</span><button onclick="alert('Script Copiado!')">COPY</button></div>`;
        });
        document.getElementById('results').innerHTML = html;
    }

    function cmd(c) { alert("Comando " + c + " enviado via WR7 Antena!"); }
</script>
</body>
</html>
