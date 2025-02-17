<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guion Técnico para Producciones Audiovisuales</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            background-color: #f3f4f6;
            color: #333;
        }
        .container {
            max-width: 1000px;
            margin: 20px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            margin-bottom: 20px;
            color: #1f2937;
        }
        form {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        label {
            font-weight: bold;
        }
        input, select, textarea {
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            resize: vertical;
        }
        .storyboard-upload {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        button {
            padding: 12px;
            background-color: #3b82f6;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #2563eb;
        }
        .preview {
            margin-top: 20px;
        }
        .preview-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }
        .preview-table th, .preview-table td {
            border: 1px solid #ccc;
            padding: 10px;
            text-align: center;
        }
        .preview-table th {
            background-color: #e5e7eb;
        }
        .storyboard-img {
            max-width: 100px;
            max-height: 100px;
        }
        @media (max-width: 600px) {
            .container {
                padding: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Guion Técnico de Producción Audiovisual</h1>
        <form id="scriptForm">
            <label>Escena:</label>
            <input type="text" id="scene" required>

            <label>Duración:</label>
            <input type="text" id="duration" required>

            <label>Descripción:</label>
            <textarea id="description" rows="3" required></textarea>

            <label>Tipo de Plano:</label>
            <select id="shotType" required>
                <option value="Gran Plano General">Gran Plano General</option>
                <option value="Plano General">Plano General</option>
                <option value="Plano Medio">Plano Medio</option>
                <option value="Primer Plano">Primer Plano</option>
                <option value="Plano Detalle">Plano Detalle</option>
            </select>

            <label>Ángulo de Plano:</label>
            <select id="angle" required>
                <option value="Normal">Normal</option>
                <option value="Picado">Picado</option>
                <option value="Contrapicado">Contrapicado</option>
                <option value="Cenital">Cenital</option>
                <option value="Nadir">Nadir</option>
            </select>

            <label>Sonido:</label>
            <input type="text" id="sound" required>

            <label>Storyboard:</label>
            <input type="file" id="storyboard" accept="image/*">

            <button type="button" onclick="addScene()">Agregar Escena</button>
            <button type="button" onclick="generatePDF()">Exportar a PDF</button>
        </form>

        <div class="preview" id="preview">
            <h2>Vista Previa</h2>
            <table class="preview-table">
                <thead>
                    <tr>
                        <th>Escena</th>
                        <th>Duración</th>
                        <th>Descripción</th>
                        <th>Tipo de Plano</th>
                        <th>Ángulo</th>
                        <th>Sonido</th>
                        <th>Storyboard</th>
                    </tr>
                </thead>
                <tbody id="previewBody"></tbody>
            </table>
        </div>
    </div>

    <script>
        const scenes = [];

        function addScene() {
            const scene = document.getElementById('scene').value;
            const duration = document.getElementById('duration').value;
            const description = document.getElementById('description').value;
            const shotType = document.getElementById('shotType').value;
            const angle = document.getElementById('angle').value;
            const sound = document.getElementById('sound').value;
            const storyboardInput = document.getElementById('storyboard');
            let storyboardURL = '';

            if (storyboardInput.files.length > 0) {
                const file = storyboardInput.files[0];
                storyboardURL = URL.createObjectURL(file);
            }

            const newScene = {
                scene, duration, description, shotType, angle, sound, storyboardURL
            };

            scenes.push(newScene);
            updatePreview();

            document.getElementById('scriptForm').reset();
        }

        function updatePreview() {
            const tbody = document.getElementById('previewBody');
            tbody.innerHTML = '';
            scenes.forEach(s => {
                const row = `<tr>
                    <td>${s.scene}</td>
                    <td>${s.duration}</td>
                    <td>${s.description}</td>
                    <td>${s.shotType}</td>
                    <td>${s.angle}</td>
                    <td>${s.sound}</td>
                    <td><img src="${s.storyboardURL}" class="storyboard-img" alt="Storyboard"></td>
                </tr>`;
                tbody.innerHTML += row;
            });
        }

        function generatePDF() {
            const element = document.getElementById('preview');
            html2pdf(element, {
                margin: 10,
                filename: 'guion_tecnico.pdf',
                image: { type: 'jpeg', quality: 0.98 },
                html2canvas: { scale: 2 },
                jsPDF: { unit: 'mm', format: 'a4', orientation: 'landscape' }
            });
        }
    </script>
</body>
</html>
