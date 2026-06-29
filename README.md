<!DOCTYPE html>
<html lang="es">
<head>
  <base target="_top">
  <title>Formulario de Pedidos de Títulos</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .campo-archivo { transition: all 0.3s ease; }
    .uppercase-input { text-transform: uppercase; }
  </style>
</head>
<body class="bg-gray-100 p-4 md:p-8 flex items-center justify-center min-h-screen">

  <div class="bg-white rounded-2xl shadow-xl p-6 w-full max-w-xl">
    <h1 class="text-2xl font-bold text-center text-gray-800 mb-6">Formulario de Pedidos</h1>
    
    <form id="requestForm" class="space-y-5">
      <div class="grid grid-cols-2 gap-4">
        <div>
          <label class="block text-xs font-bold text-gray-600">DNI (8 DÍGITOS)</label>
          <input type="text" id="dni" required pattern="\d{8}" maxlength="8" 
                 oninput="this.value = this.value.replace(/[^0-9]/g, '')"
                 class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none">
        </div>
        <div>
          <label class="block text-xs font-bold text-gray-600">SEDE</label>
          <select id="sede" required class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none">
            <option value="">Seleccionar...</option>
            <option value="Central">Central</option>
            <option value="Las Heras">Las Heras</option>
            <option value="Uspallata">Uspallata</option>
          </select>
        </div>
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div>
          <label class="block text-xs font-bold text-gray-600">APELLIDO</label>
          <input type="text" id="apellido" required oninput="this.value = this.value.toUpperCase()"
                 class="uppercase-input w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none">
        </div>
        <div>
          <label class="block text-xs font-bold text-gray-600">NOMBRE</label>
          <input type="text" id="nombre" required oninput="this.value = this.value.toUpperCase()"
                 class="uppercase-input w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none">
        </div>
      </div>

      <div>
        <label class="block text-xs font-bold text-gray-600">CARRERA</label>
        <select id="carrera" required class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none">
          <option value="">Seleccionar...</option>
          <option value="Turismo">Turismo</option>
          <option value="Administración de Empresas">Administración de Empresas</option>
          <option value="Gestión de Marketing">Gestión de Marketing</option>
        </select>
      </div>

      <div class="bg-blue-50 p-4 rounded-xl border border-blue-200 space-y-3">
        <h2 class="text-blue-800 font-bold text-sm mb-2">DOCUMENTACIÓN (Solo PDF)</h2>
        <div>
          <label class="block text-[10px] uppercase font-bold text-gray-600">1° Partida de Nacimiento</label>
          <input type="file" id="file_partida" accept="application/pdf" required class="w-full text-sm">
        </div>
        <div>
          <label class="block text-[10px] uppercase font-bold text-gray-600">2° Copia DNI</label>
          <input type="file" id="file_dni" accept="application/pdf" required class="w-full text-sm">
        </div>
        <div>
          <label class="block text-[10px] uppercase font-bold text-gray-600">3° CUIL</label>
          <input type="file" id="file_cuil" accept="application/pdf" required class="w-full text-sm">
        </div>
        <div>
          <label class="block text-[10px] uppercase font-bold text-gray-600">4° Analítico</label>
          <input type="file" id="file_analitico" accept="application/pdf" required class="w-full text-sm">
        </div>
      </div>

      <button type="submit" id="submitBtn" class="w-full bg-blue-600 text-white font-bold py-3 rounded-xl hover:bg-blue-700 transition">
        ENVIAR PEDIDO
      </button>
    </form>
    <div id="message-container" class="mt-4 text-center text-sm font-medium"></div>
  </div>

  <script>
    // PEGA AQUÍ TU URL DE IMPLEMENTACIÓN DE GOOGLE APPS SCRIPT
    const URL_WEB_APP = "https://script.google.com/macros/s/AKfycbzsSM5DqN0MVsB3TeqPuUvuk1PUXyQiVyJoPeWgaAROOR8ID6kjpzRIsiITkIJn7zzXWg/exec";

    document.getElementById('requestForm').addEventListener('submit', async function(e) {
      e.preventDefault();
      const btn = document.getElementById('submitBtn');
      const msg = document.getElementById('message-container');
      
      btn.disabled = true;
      btn.innerText = "Procesando...";
      msg.innerHTML = "Subiendo al servidor...";

      try {
        const config = [
          {id: 'file_partida', name: '1-Partida_Nacimiento'},
          {id: 'file_dni', name: '2-Copia_DNI'},
          {id: 'file_cuil', name: '3-CUIL'},
          {id: 'file_analitico', name: '4-Analitico'}
        ];

        const files = await Promise.all(config.map(async c => {
          const file = document.getElementById(c.id).files[0];
          return new Promise(res => {
            const reader = new FileReader();
            reader.onload = e => res({nombre: c.name, base64: e.target.result, tipo: file.type});
            reader.readAsDataURL(file);
          });
        }));

        const response = await fetch(URL_WEB_APP, {
          method: 'POST',
          mode: 'no-cors', // Mantenemos no-cors para evitar bloqueo, si falla, avísame.
          body: JSON.stringify({
            dni: document.getElementById('dni').value,
            apellido: document.getElementById('apellido').value,
            nombre: document.getElementById('nombre').value,
            sede: document.getElementById('sede').value,
            carrera: document.getElementById('carrera').value,
            listaArchivos: files
          })
        });

        msg.innerHTML = "<span class='text-green-600'>¡Enviado exitosamente!</span>";
        document.getElementById('requestForm').reset();
      } catch (err) {
        console.error("Error completo:", err);
        msg.innerHTML = "<span class='text-red-600'>Error al enviar. Revisa la consola (F12).</span>";
      } finally {
        btn.disabled = false;
        btn.innerText = "ENVIAR PEDIDO";
      }
    });
  </script>
</body>
</html>
