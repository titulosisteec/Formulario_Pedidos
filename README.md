<!DOCTYPE html>
<html>
<body>
  <form id="miFormulario">
    <input type="number" id="dni" placeholder="DNI" required><br>
    <input type="text" id="apellido" placeholder="APELLIDO" required><br>
    <input type="text" id="nombre" placeholder="NOMBRE" required><br>
    
    <select id="sede" required>
      <option value="">Selecciona una sede</option>
      <option value="Central">Central</option>
      <option value="Las Heras">Las Heras</option>
      <option value="Uspallata">Uspallata</option>
    </select><br>
    
    <select id="carrera" required>
      <option value="">Selecciona una carrera</option>
      <option value="Tecnicatura">Tecnicatura</option>
      <option value="Licenciatura">Licenciatura</option>
    </select><br>

    <button type="button" onclick="enviar()">Enviar Pedido</button>
  </form>

  <script>
    function enviar() {
      const url = "https://script.google.com/macros/s/AKfycbzt-jusq5N5SrU3s0TTK_AVdlW5cgpEQVNqB3chyeEoCAJunHed3eCtO0jitF8XtqMD/exec"; 
      const datos = {
        dni: document.getElementById('dni').value,
        apellido: document.getElementById('apellido').value.toUpperCase(),
        nombre: document.getElementById('nombre').value.toUpperCase(),
        sede: document.getElementById('sede').value,
        carrera: document.getElementById('carrera').value
      };

      fetch(url, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(datos)
      })
      .then(res => res.text())
      .then(res => alert("Respuesta: " + res));
    }
  </script>
</body>
</html>
