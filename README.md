<!DOCTYPE html>
<html>
<body>
  <form id="miFormulario">
    <input type="text" name="nombre" id="nombre" placeholder="Nombre" required><br>
    <input type="number" name="dni" id="dni" placeholder="DNI" required><br>
    <input type="file" id="archivo" accept="application/pdf" required><br>
    <button type="button" onclick="enviar()">Enviar</button>
  </form>

  <script>
    function enviar() {
      const fileInput = document.getElementById('archivo');
      const file = fileInput.files[0];
      const reader = new FileReader();

      reader.onload = function(e) {
        const url = "https://script.google.com/macros/s/AKfycbwNCsWgOIrZ0sHc-R6niD8uAR46iI4UD1QkNDhRv9_yKZ7Jd0n3Sl9yML3rkyKeIqUD/exec"; // <-- Pega aquí tu URL de la implementación
        const data = {
          nombre: document.getElementById('nombre').value,
          dni: document.getElementById('dni').value,
          archivo: e.target.result.split(',')[1], // El contenido base64
          tipo: file.type,
          nombreArchivo: file.name
        };

        fetch(url, {
          method: 'POST',
          mode: 'no-cors', // Necesario para evitar bloqueos CORS
          body: JSON.stringify(data)
        }).then(() => alert("Enviado correctamente"));
      };
      reader.readAsDataURL(file);
    }
  </script>
</body>
</html>
