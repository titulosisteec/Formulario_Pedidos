<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
</head>
<body>
    <form id="miFormulario">
        <input type="text" id="dni" placeholder="DNI"><br>
        <input type="text" id="apellido" placeholder="Apellido"><br>
        <input type="text" id="nombre" placeholder="Nombre"><br>
        <button type="button" id="btnEnviar">Enviar Datos</button>
    </form>

    <script>
        document.getElementById('btnEnviar').addEventListener('click', function() {
            alert("Botón clickeado"); // Si esto NO sale, el JS no carga.

            const url = "https://script.google.com/macros/s/AKfycbxP4STE34AuY6kYkT-9OltPOGRD6pQXyZQV6GjGb-91rtKsgxSeZgdGk8G0LzWhB0SD1w/exec";
            
            const datos = {
                dni: document.getElementById('dni').value,
                apellido: document.getElementById('apellido').value,
                nombre: document.getElementById('nombre').value
            };

            fetch(url, {
                method: 'POST',
                mode: 'no-cors', // Fundamental para saltar errores de origen
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(datos)
            })
            .then(() => alert("Petición enviada (puede que no haya retornado respuesta por no-cors)"))
            .catch(err => alert("Error de fetch: " + err));
        });
    </script>
</body>
</html>
