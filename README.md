<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>SIMIT Fleet Control | Dashboard Multas y Comparendos</title>
    <!-- Google Fonts & Font Awesome -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #f3f6fc;
            font-family: 'Inter', sans-serif;
            padding: 16px;
        }

        .dashboard-container {
            max-width: 1600px;
            margin: 0 auto;
        }

        /* Header responsive */
        .header {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            gap: 16px;
            margin-bottom: 24px;
        }

        .title h1 {
            font-size: clamp(1.4rem, 5vw, 1.8rem);
            font-weight: 700;
            color: #0b2b3b;
            display: flex;
            align-items: center;
            gap: 8px;
            flex-wrap: wrap;
        }

        .title p {
            font-size: 0.8rem;
            color: #4a627a;
            margin-top: 6px;
        }

        .upload-area {
            background: white;
            border-radius: 60px;
            padding: 6px 16px;
            display: flex;
            align-items: center;
            gap: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            flex-wrap: wrap;
        }

        .upload-label {
            background: #1e466e;
            color: white;
            padding: 8px 18px;
            border-radius: 40px;
            font-size: 0.8rem;
            font-weight: 500;
            cursor: pointer;
            transition: 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .upload-label:hover {
            background: #0e3352;
        }

        /* Cards grid responsivo */
        .cards-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 16px;
            margin-bottom: 24px;
        }

        .card {
            background: white;
            border-radius: 24px;
            padding: 16px 18px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.04);
            border-left: 4px solid;
            transition: transform 0.1s ease;
        }

        .card h4 {
            font-size: 0.75rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            color: #5a7d9e;
            margin-bottom: 10px;
        }

        .card .value {
            font-size: clamp(1.5rem, 6vw, 2rem);
            font-weight: 800;
            color: #1e2f3c;
        }

        /* Alertas inteligentes (flex responsivo) */
        .alert-banner {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 24px;
        }

        .alert-item {
            background: white;
            border-radius: 60px;
            padding: 10px 22px;
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 0.85rem;
            font-weight: 500;
            flex: 1 1 auto;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
        }

        .alert-red { background: #fff5f5; border-left: 4px solid #e53e3e; }
        .alert-orange { background: #fffaf0; border-left: 4px solid #ed8936; }
        .alert-green { background: #f0fff4; border-left: 4px solid #38a169; }

        /* Filtros: diseño mobile first */
        .filters-bar {
            background: white;
            border-radius: 28px;
            padding: 18px 20px;
            margin-bottom: 24px;
            display: flex;
            flex-wrap: wrap;
            gap: 14px;
            align-items: flex-end;
            box-shadow: 0 4px 12px rgba(0,0,0,0.03);
        }

        .filter-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
            flex: 1 1 140px;
            min-width: 120px;
        }

        .filter-group label {
            font-size: 0.7rem;
            font-weight: 700;
            color: #3b6e9f;
            text-transform: uppercase;
        }

        .filter-group input, .filter-group select {
            border: 1px solid #d4e2f0;
            border-radius: 40px;
            padding: 8px 14px;
            font-size: 0.85rem;
            background: white;
            outline: none;
            transition: 0.2s;
        }

        .filter-group input:focus, .filter-group select:focus {
            border-color: #2c7cb6;
        }

        .btn-reset {
            background: #eef2f9;
            border: none;
            border-radius: 40px;
            padding: 8px 22px;
            font-weight: 600;
            cursor: pointer;
            font-size: 0.8rem;
        }

        /* Tabla responsive - overflow horizontal en móvil */
        .table-wrapper {
            background: white;
            border-radius: 24px;
            overflow-x: auto;
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
            margin-bottom: 20px;
            -webkit-overflow-scrolling: touch;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.75rem;
            min-width: 780px;
        }

        th {
            text-align: left;
            padding: 14px 10px;
            background: #fafcff;
            color: #1a4766;
            font-weight: 700;
            border-bottom: 1px solid #e2edf7;
        }

        td {
            padding: 12px 10px;
            border-bottom: 1px solid #ecf3fa;
            vertical-align: middle;
        }

        .estado-badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 40px;
            font-size: 0.7rem;
            font-weight: 700;
            text-align: center;
        }

        .estado-pagado { background: #c6f6d5; color: #22543d; }
        .estado-pendiente { background: #fee2e2; color: #b91c1c; }
        .estado-vencido { background: #fecaca; color: #991b1b; }

        .alerta-vencimiento {
            font-size: 0.7rem;
            font-weight: 600;
            padding: 4px 10px;
            border-radius: 24px;
            display: inline-block;
            white-space: nowrap;
        }
        .alerta-roja { background: #fde5e5; color: #c53030; }
        .alerta-naranja { background: #fff0e0; color: #c2410c; }

        .doc-icons {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }
        .doc-btn {
            background: #f0f4fa;
            border: none;
            border-radius: 30px;
            padding: 5px 10px;
            font-size: 0.7rem;
            cursor: pointer;
            transition: 0.2s;
            font-weight: 500;
        }
        .doc-btn i {
            margin-right: 4px;
        }

        /* Modal responsivo */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0,0,0,0.75);
            align-items: center;
            justify-content: center;
            z-index: 1000;
            padding: 16px;
        }
        .modal-content {
            background: white;
            max-width: 90vw;
            width: 700px;
            max-height: 85vh;
            border-radius: 32px;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 18px 22px;
            border-bottom: 1px solid #eef2f8;
        }
        .modal-body {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            text-align: center;
        }
        .close-modal {
            background: none;
            border: none;
            font-size: 1.7rem;
            cursor: pointer;
            line-height: 1;
        }
        footer {
            text-align: center;
            font-size: 0.7rem;
            color: #7895b2;
            padding: 16px;
        }
        @media (max-width: 640px) {
            body {
                padding: 12px;
            }
            .card .value {
                font-size: 1.4rem;
            }
            .alert-item {
                font-size: 0.75rem;
                padding: 8px 16px;
            }
            .filter-group {
                min-width: 100%;
            }
            .btn-reset {
                width: 100%;
            }
        }
    </style>
</head>
<body>
<div class="dashboard-container">
    <div class="header">
        <div class="title">
            <h1><i class="fas fa-traffic-light"></i> SIMIT Fleet Control</h1>
            <p>Gestión de comparendos, multas y evidencias | Logística & Transporte</p>
        </div>
        <div class="upload-area">
            <span style="font-size:0.75rem;"><i class="fas fa-file-csv"></i> CSV</span>
            <input type="file" id="csvUpload" accept=".csv" style="display:none" />
            <label for="csvUpload" class="upload-label"><i class="fas fa-cloud-upload-alt"></i> Importar datos</label>
        </div>
    </div>

    <!-- KPI Cards -->
    <div class="cards-grid" id="kpiCards">
        <div class="card" style="border-left-color:#2c7da0;"><h4>📋 Total Registros</h4><div class="value" id="totalRegistros">0</div></div>
        <div class="card" style="border-left-color:#e63946;"><h4>⚠️ Multas Pendientes</h4><div class="value" id="pendientesCount">0</div></div>
        <div class="card" style="border-left-color:#2ecc71;"><h4>✅ Multas Pagadas</h4><div class="value" id="pagadasCount">0</div></div>
        <div class="card" style="border-left-color:#f39c12;"><h4>💰 Valor Pendiente</h4><div class="value" id="totalPendienteValor">$0</div></div>
    </div>

    <!-- Alertas inteligentes -->
    <div class="alert-banner" id="alertBanner"></div>

    <!-- Filtros dinámicos -->
    <div class="filters-bar">
        <div class="filter-group"><label>Conductor</label><input type="text" id="filterConductor" placeholder="Nombre"></div>
        <div class="filter-group"><label>Documento</label><input type="text" id="filterDocumento" placeholder="Cédula"></div>
        <div class="filter-group"><label>Placa</label><input type="text" id="filterPlaca" placeholder="Placa"></div>
        <div class="filter-group"><label>Estado</label><select id="filterEstado"><option value="todos">Todos</option><option value="Pendiente">Pendiente</option><option value="Pagado">Pagado</option><option value="Vencido">Vencido</option></select></div>
        <div class="filter-group"><label>Fecha desde</label><input type="date" id="fechaDesde"></div>
        <div class="filter-group"><label>Fecha hasta</label><input type="date" id="fechaHasta"></div>
        <div class="filter-group"><button class="btn-reset" id="resetFilters"><i class="fas fa-undo-alt"></i> Limpiar</button></div>
    </div>

    <!-- Tabla principal con scroll horizontal en móvil -->
    <div class="table-wrapper">
        <table id="tablaComparendos">
            <thead>
                <tr><th>Conductor</th><th>Documento</th><th>Placa</th><th>N° Comparendo</th><th>Fecha Comp.</th><th>Fecha Vcto.</th><th>Estado</th><th>Valor</th><th>Alerta</th><th>Documentos</th></tr>
            </thead>
            <tbody id="tablaBody"></tbody>
        </table>
    </div>
    <footer><i class="fas fa-mobile-alt"></i> Vista responsiva | Cargue CSV o use datos demo | Adjunte imagen/PDF y previsualice sin descargar</footer>
</div>

<!-- Modal previsualización documentos -->
<div id="previewModal" class="modal">
    <div class="modal-content">
        <div class="modal-header"><h3><i class="fas fa-file-alt"></i> Vista previa del documento</h3><button class="close-modal" id="closeModalBtn">&times;</button></div>
        <div class="modal-body" id="previewBody">Cargando previsualización...</div>
    </div>
</div>

<script>
    // DATASET GLOBAL
    let dataset = []; 

    // Helper moneda
    const formatCOP = (val) => new Intl.NumberFormat('es-CO', { style: 'currency', currency: 'COP', minimumFractionDigits: 0 }).format(val);

    // Determinar estado real considerando vencimiento automático
    function getEstadoReal(record) {
        let estadoOriginal = record.Estado?.trim();
        if (estadoOriginal === 'Pagado') return 'Pagado';
        const vencimiento = new Date(record.Fecha_Vencimiento);
        const hoy = new Date();
        hoy.setHours(0,0,0,0);
        if (vencimiento < hoy) return 'Vencido';
        return 'Pendiente';
    }

    // Alerta de vencimiento inteligente
    function getAlertaVencimiento(fechaVencimientoStr, estado) {
        if (estado === 'Pagado') return { texto: 'Pagado', clase: '' };
        const hoy = new Date(); hoy.setHours(0,0,0,0);
        const venc = new Date(fechaVencimientoStr);
        if (isNaN(venc)) return { texto: 'Fecha inválida', clase: '' };
        const diffDays = Math.ceil((venc - hoy) / (1000 * 3600 * 24));
        if (diffDays < 0) return { texto: 'VENCIDA', clase: 'alerta-roja' };
        if (diffDays <= 5) return { texto: `Vence en ${diffDays} días`, clase: 'alerta-naranja' };
        return { texto: `${diffDays} días rest.`, clase: '' };
    }

    // Actualizar KPIs y alertas globales
    function actualizarResumenGlobal() {
        const total = dataset.length;
        let pendientes = 0, pagadas = 0, sumaPendienteValor = 0;
        dataset.forEach(r => {
            const estado = getEstadoReal(r);
            if (estado === 'Pendiente') pendientes++;
            if (estado === 'Pagado') pagadas++;
            if (estado !== 'Pagado') sumaPendienteValor += (parseFloat(r.Valor) || 0);
        });
        document.getElementById('totalRegistros').innerText = total;
        document.getElementById('pendientesCount').innerText = pendientes;
        document.getElementById('pagadasCount').innerText = pagadas;
        document.getElementById('totalPendienteValor').innerHTML = formatCOP(sumaPendienteValor);

        // Alertas inteligentes dinámicas
        let proximasVencer = 0;
        dataset.forEach(r => {
            const estado = getEstadoReal(r);
            if (estado === 'Pendiente') {
                const venc = new Date(r.Fecha_Vencimiento);
                const hoy = new Date();
                const diff = Math.ceil((venc - hoy) / (1000*3600*24));
                if (diff >= 0 && diff <= 5) proximasVencer++;
            }
        });
        const alertDiv = document.getElementById('alertBanner');
        alertDiv.innerHTML = `
            <div class="alert-item alert-red"><i class="fas fa-exclamation-triangle"></i> <span>🔴 Multas pendientes: ${pendientes}</span></div>
            <div class="alert-item alert-orange"><i class="fas fa-hourglass-half"></i> <span>🟠 Próximas a vencer (<5d): ${proximasVencer}</span></div>
            <div class="alert-item alert-green"><i class="fas fa-check-circle"></i> <span>🟢 Pagadas: ${pagadas}</span></div>
        `;
        renderTabla();
    }

    // Render tabla con filtros (responsive y con estados)
    function renderTabla() {
        const filtroConductor = document.getElementById('filterConductor').value.toLowerCase();
        const filtroDocumento = document.getElementById('filterDocumento').value.toLowerCase();
        const filtroPlaca = document.getElementById('filterPlaca').value.toLowerCase();
        const filtroEstado = document.getElementById('filterEstado').value;
        const fechaDesde = document.getElementById('fechaDesde').value;
        const fechaHasta = document.getElementById('fechaHasta').value;

        let filtered = dataset.filter(reg => {
            const estadoReal = getEstadoReal(reg);
            if (filtroEstado !== 'todos' && estadoReal !== filtroEstado) return false;
            if (filtroConductor && !reg.Conductor?.toLowerCase().includes(filtroConductor)) return false;
            if (filtroDocumento && !reg.Documento?.toLowerCase().includes(filtroDocumento)) return false;
            if (filtroPlaca && !reg.Placa?.toLowerCase().includes(filtroPlaca)) return false;
            if (fechaDesde) {
                let fechaComp = new Date(reg.Fecha_Comparendo);
                if (fechaComp < new Date(fechaDesde)) return false;
            }
            if (fechaHasta) {
                let fechaComp = new Date(reg.Fecha_Comparendo);
                if (fechaComp > new Date(fechaHasta)) return false;
            }
            return true;
        });

        const tbody = document.getElementById('tablaBody');
        tbody.innerHTML = '';
        if (filtered.length === 0) {
            tbody.innerHTML = '<tr><td colspan="10" style="text-align:center; padding:32px;">📭 No hay comparendos con los filtros seleccionados</td></tr>';
            return;
        }

        filtered.forEach((rec, idx) => {
            const estadoReal = getEstadoReal(rec);
            let badgeClass = '';
            if (estadoReal === 'Pagado') badgeClass = 'estado-pagado';
            else if (estadoReal === 'Pendiente') badgeClass = 'estado-pendiente';
            else badgeClass = 'estado-vencido';
            
            const alertObj = getAlertaVencimiento(rec.Fecha_Vencimiento, estadoReal);
            const alertaHTML = estadoReal !== 'Pagado' ? `<span class="alerta-vencimiento ${alertObj.clase}">${alertObj.texto}</span>` : '<span class="alerta-vencimiento" style="background:#e0f2e0;">✔ Pagado</span>';
            const valorNum = parseFloat(rec.Valor) || 0;
            const docUrl = rec.Documento_URL || '';
            const tieneDoc = docUrl && docUrl.trim() !== '';
            // Índice global para referencia en carga local
            const globalIndex = dataset.findIndex(r => r === rec);
            const botonesDoc = `<div class="doc-icons">
                ${tieneDoc ? `<button class="doc-btn preview-doc" data-url="${escapeHtml(docUrl)}"><i class="fas fa-eye"></i> Previsualizar</button>` : '<span class="doc-btn" style="background:#e9ecef;"><i class="far fa-file"></i> Sin archivo</span>'}
                <button class="doc-btn subir-doc-local" data-idx="${globalIndex}"><i class="fas fa-upload"></i> Cargar</button>
            </div>`;
            const row = tbody.insertRow();
            row.insertCell(0).innerText = rec.Conductor || '—';
            row.insertCell(1).innerText = rec.Documento || '—';
            row.insertCell(2).innerText = rec.Placa || '—';
            row.insertCell(3).innerText = rec.N_Comparendo || '—';
            row.insertCell(4).innerText = rec.Fecha_Comparendo || '—';
            row.insertCell(5).innerText = rec.Fecha_Vencimiento || '—';
            row.insertCell(6).innerHTML = `<span class="estado-badge ${badgeClass}">${estadoReal}</span>`;
            row.insertCell(7).innerHTML = formatCOP(valorNum);
            row.insertCell(8).innerHTML = alertaHTML;
            row.insertCell(9).innerHTML = botonesDoc;
        });

        // Eventos previsualización y carga
        document.querySelectorAll('.preview-doc').forEach(btn => {
            btn.removeEventListener('click', previewHandler);
            btn.addEventListener('click', previewHandler);
        });
        document.querySelectorAll('.subir-doc-local').forEach(btn => {
            btn.removeEventListener('click', uploadDocHandler);
            btn.addEventListener('click', uploadDocHandler);
        });
    }

    // Escapar HTML para URLs
    function escapeHtml(str) {
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        }).replace(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g, function(c) {
            return c;
        });
    }

    function previewHandler(e) {
        const rawUrl = e.currentTarget.getAttribute('data-url');
        if (!rawUrl) return;
        const url = rawUrl;
        const modal = document.getElementById('previewModal');
        const previewBody = document.getElementById('previewBody');
        const lowerUrl = url.toLowerCase();
        if (lowerUrl.match(/\.(jpg|jpeg|png|gif|webp|svg)$/i) || url.startsWith('data:image')) {
            previewBody.innerHTML = `<img src="${url}" alt="documento evidencia" style="max-width:100%; max-height:55vh; border-radius:16px;">`;
        } else if (lowerUrl.includes('.pdf') || url.includes('pdf')) {
            previewBody.innerHTML = `<iframe src="${url}" width="100%" height="500px" style="border:none; border-radius:16px;"></iframe>`;
        } else {
            previewBody.innerHTML = `<div>Vista previa no automática. <a href="${url}" target="_blank" class="doc-btn">Abrir enlace externo</a></div>`;
        }
        modal.style.display = 'flex';
    }

    function uploadDocHandler(e) {
        const idx = parseInt(e.currentTarget.getAttribute('data-idx'));
        if (isNaN(idx) || idx < 0 || idx >= dataset.length) return;
        const inputFile = document.createElement('input');
        inputFile.type = 'file';
        inputFile.accept = 'image/*,application/pdf';
        inputFile.onchange = (ev) => {
            const file = ev.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (loadEv) => {
                    const dataURL = loadEv.target.result;
                    dataset[idx].Documento_URL = dataURL;
                    actualizarResumenGlobal(); // refresca vista
                };
                reader.readAsDataURL(file);
            }
        };
        inputFile.click();
    }

    // Validación y carga CSV
    function cargarCSV(csvText) {
        const lines = csvText.split(/\r?\n/);
        if (lines.length < 2) return false;
        let headers = lines[0].split(',').map(h => h.trim().replace(/^\uFEFF/, ''));
        const requiredCols = ['Conductor','Documento','Placa','N_Comparendo','Fecha_Comparendo','Fecha_Vencimiento','Estado','Valor'];
        const missing = requiredCols.filter(col => !headers.includes(col));
        if (missing.length) {
            alert(`Error en columnas del CSV. Faltan: ${missing.join(', ')}. Debe incluir: ${requiredCols.join(', ')} y opcional Documento_URL`);
            return false;
        }
        const nuevos = [];
        for (let i=1; i<lines.length; i++) {
            if (!lines[i].trim()) continue;
            const values = lines[i].split(',').map(v => v.trim());
            if (values.length < headers.length) continue;
            let obj = {};
            headers.forEach((h, idx) => { obj[h] = values[idx] || ''; });
            if (!obj.Fecha_Comparendo || !obj.Fecha_Vencimiento) continue;
            if (isNaN(parseFloat(obj.Valor))) obj.Valor = '0';
            obj.Documento_URL = obj.Documento_URL || '';
            nuevos.push(obj);
        }
        if (nuevos.length === 0) { alert("No se encontraron registros válidos en el CSV"); return false; }
        dataset = nuevos;
        actualizarResumenGlobal();
        return true;
    }

    // Data demo para experiencia inmediata
    function cargarDatosEjemplo() {
        const csvDemo = `Conductor,Documento,Placa,N_Comparendo,Fecha_Comparendo,Fecha_Vencimiento,Estado,Valor,Documento_URL
Carlos Mendoza,12345678,XYZ123,COMP-2025-001,2025-03-10,2025-04-05,Pendiente,580000,https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf
Laura Suárez,87654321,ABC987,COMP-2025-002,2025-03-15,2025-04-10,Pendiente,350000,
Andrés Herrera,56781234,TOY456,COMP-2025-003,2025-01-20,2025-02-15,Pagado,210000,https://samplelib.com/lib/preview/png/sample-maple.png
Mónica Pineda,11223344,SUZ789,COMP-2025-004,2025-03-25,2025-04-12,Pendiente,1250000,
Sebastián Rojas,99887766,MBZ321,COMP-2025-005,2025-02-28,2025-04-01,Vencido,890000,
Carolina Lozano,44332211,REN111,COMP-2025-006,2025-03-28,2025-04-02,Pendiente,450000,https://www.africau.edu/images/default/sample.pdf
Diego Vargas,55779933,FRD888,COMP-2025-007,2025-03-30,2025-04-18,Pendiente,310000,
`;
        cargarCSV(csvDemo);
    }

    // Inicialización y eventos
    document.getElementById('csvUpload').addEventListener('change', (e) => {
        const file = e.target.files[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = (ev) => {
            if (cargarCSV(ev.target.result)) {
                alert(`✅ Datos importados correctamente: ${dataset.length} comparendos`);
            } else {
                alert("⚠️ Error en el formato del CSV. Revise columnas requeridas.");
            }
        };
        reader.readAsText(file, 'UTF-8');
    });

    document.getElementById('resetFilters').addEventListener('click', () => {
        document.getElementById('filterConductor').value = '';
        document.getElementById('filterDocumento').value = '';
        document.getElementById('filterPlaca').value = '';
        document.getElementById('filterEstado').value = 'todos';
        document.getElementById('fechaDesde').value = '';
        document.getElementById('fechaHasta').value = '';
        renderTabla();
    });

    ['filterConductor','filterDocumento','filterPlaca','filterEstado','fechaDesde','fechaHasta'].forEach(id => {
        const el = document.getElementById(id);
        if (el) {
            el.addEventListener('input', () => renderTabla());
            el.addEventListener('change', () => renderTabla());
        }
    });

    // Modal close
    const modal = document.getElementById('previewModal');
    document.getElementById('closeModalBtn').onclick = () => modal.style.display = 'none';
    window.onclick = (e) => { if (e.target === modal) modal.style.display = 'none'; };

    // Carga inicial DEMO
    cargarDatosEjemplo();
</script>
</body>
</html>
