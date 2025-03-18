<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Compartir</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        table { width: 60%; margin: auto; border-collapse: collapse; }
        th, td { border: 1px solid black; padding: 10px; text-align: left; }
        input { width: 100%; padding: 5px; }
        button { margin-top: 10px; padding: 10px; cursor: pointer; }
    </style>
</head>
<body>
    <h2>Lista de Compartir</h2>
    <table>
        <thead>
            <tr>
                <th>Estudiante</th>
                <th>¿Qué va a llevar?</th>
            </tr>
        </thead>
        <tbody id="lista">
            <!-- Se llenará dinámicamente -->
        </tbody>
    </table>
    <button onclick="guardarDatos()">Guardar</button>
    <script>
        const estudiantes = ["Jorgita", "Leidis", "Lau", "MAJO", "Cristian", "Karen", "Jeferson", "Marito", "Cordero", "Angella", "Yuyis", "Ducu", "Gamboas", "Sofi", "Lucho", "Daniel", "Mafe", "Deilis", "Nico", "Aaron", "Aletsa", "Sharol", "Juancho", "Liz", "Julian", "Rayfel", "Viloria"];
        
        function cargarLista() {
            const lista = document.getElementById("lista");
            lista.innerHTML = "";
            estudiantes.forEach(nombre => {
                const fila = document.createElement("tr");
                fila.innerHTML = `<td>${nombre}</td><td><input type="text" id="${nombre}"></td>`;
                lista.appendChild(fila);
            });
        }
        
        function guardarDatos() {
            const datos = {};
            estudiantes.forEach(nombre => {
                datos[nombre] = document.getElementById(nombre).value;
            });
            localStorage.setItem("listaCompartir", JSON.stringify(datos));
            alert("Datos guardados");
        }
        
        function cargarDatosGuardados() {
            const datosGuardados = JSON.parse(localStorage.getItem("listaCompartir")) || {};
            estudiantes.forEach(nombre => {
                if (datosGuardados[nombre]) {
                    document.getElementById(nombre).value = datosGuardados[nombre];
                }
            });
        }
        
        cargarLista();
        window.onload = cargarDatosGuardados;
    </script>
</body>
</html>
