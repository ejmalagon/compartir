<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Compartir</title>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>
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
    <p id="status" style="color: red;"></p>

    <script>
        const firebaseConfig = {
            apiKey: "TU_API_KEY",
            authDomain: "TU_AUTH_DOMAIN",
            databaseURL: "TU_DATABASE_URL",
            projectId: "TU_PROJECT_ID",
            storageBucket: "TU_STORAGE_BUCKET",
            messagingSenderId: "TU_MESSAGING_SENDER_ID",
            appId: "TU_APP_ID"
        };
        
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        const estudiantes = ["Jorgita", "Leidis", "Lau", "MAJO", "Cristian", "Karen", "Jeferson", "Marito", "Cordero", "Angella", "Yuyis", "Ducu", "Gamboas", "Sofi", "Lucho", "Daniel", "Mafe", "Deilis", "Nico", "Aaron", "Aletsa", "Sharol", "Juancho", "Liz", "Julian", "Rayfel", "Viloria"];

        function cargarLista() {
            const lista = document.getElementById("lista");
            lista.innerHTML = "";

            estudiantes.forEach(nombre => {
                const fila = document.createElement("tr");
                fila.innerHTML = `<td>${nombre}</td><td><input type="text" id="${nombre}" disabled></td>`;
                lista.appendChild(fila);
                cargarDato(nombre);
            });
        }

        function cargarDato(nombre) {
            db.ref("listaCompartir/" + nombre).on("value", snapshot => {
                const valor = snapshot.val();
                const input = document.getElementById(nombre);
                if (valor) {
                    input.value = valor;
                } else {
                    input.disabled = false;
                    input.addEventListener("change", () => guardarDato(nombre, input.value));
                }
            }, error => {
                document.getElementById("status").innerText = "Error al conectar con la base de datos.";
            });
        }

        function guardarDato(nombre, valor) {
            if (valor.trim() === "") {
                alert("Por favor, ingresa un valor válido.");
                return;
            }
            db.ref("listaCompartir/" + nombre).set(valor)
                .then(() => {
                    document.getElementById(nombre).disabled = true;
                    alert("Dato guardado");
                })
                .catch(error => {
                    alert("Error al guardar: " + error.message);
                });
        }

        cargarLista();
    </script>
</body>
</html>
