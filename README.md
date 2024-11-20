# trabajo_semestre2
informacion_cuota = [];
informacion = [];

// Función del "menú" donde están las opciones del simulador de crédito
function menu() {
    console.log("SIMULADOR DE GESTIÓN DE CRÉDITO");
    console.log("Elige la opción: ");
    console.log("1) Ingresar información para el crédito");
    console.log("2) Información de créditos guardados");
    console.log("3) Eliminar un usuario");
    console.log("4) Ordenar créditos por monto de cuota");
    console.log("5) Salir");
    let opcion = prompt("Opción: ");
    return opcion;
}

// Clase para registrar la información personal del usuario
class registro {
    constructor(nombre, identificacion, correo, estadoCivil) {
        this.nombre = nombre;
        this.identificacion = identificacion;
        this.correo = correo;
        this.estadoCivil = estadoCivil;
    }
}

// Función donde se introduce la información del usuario
function Datos_usuario(informacion) {
    console.log("Ingresando datos..");
    console.log("Por favor, complete la información del usuario.");
    let nombre = prompt("Nombre completo del usuario: ");
    let identificacion = prompt("Identificación Usuario: ");
    let numero = parseFloat(prompt("Número telefónico: "));
    let correo = prompt("Correo electrónico: ");
    let estadoCivil = prompt("Estado civil: ");
    let p = new registro(nombre, identificacion, correo, estadoCivil);
    informacion.push(p);
    console.log("Datos del usuario ingresados correctamente.");
    console.log("Registro actual", informacion);
}

// Función del cálculo del monto que se va a cotizar
function Calculo_pago_mensual(informacion_cuota) {
    console.log("Calculando el monto total del crédito...");
    let monto = parseFloat(prompt("Digite el monto a cotizar: "));
    if (isNaN(monto) || monto <= 0) {
        console.log("Monto no válido. Intenta nuevamente.");
        return;
    }
    let interes = 0.1633;
    let interes_total = monto * interes;
    let total = monto + interes_total;
    let cuotas = parseFloat(prompt("Digite cuántas cuotas va a tener el crédito: "));
    if (isNaN(cuotas) || cuotas < 2 || cuotas > 36) {
        console.log("Número de cuotas no válido. Debe ser entre 2 y 36.");
        return;
    }
    let saldoCuotas = total / cuotas;
    let saldoCuotas_aprox = saldoCuotas.toFixed(1);
    console.log("El total de su cuota será de: ", saldoCuotas_aprox);
    console.log("Interés total: ", interes_total);
    console.log("Su crédito consta de", cuotas, "cuotas, con un monto total de", saldoCuotas_aprox, "por cuota."); 
    let p1 = new registro_cuota(monto, cuotas, saldoCuotas_aprox, interes_total);
    informacion_cuota.push(p1);
    console.log("Registro de cuota: ", informacion_cuota);
}

// Función donde se guardan y se muestran los créditos cotizados por medio de un bucle
function mostrarCreditosGuardados(informacion_cuota) {
    if (informacion_cuota.length > 0) {
        console.log("Estos son los créditos que tienes guardados:");
        for (let i = 0; i < informacion_cuota.length; i++) {
            console.log(" ");
            console.log("Crédito #" + (i + 1));
            console.log("Monto del crédito: $" + informacion_cuota[i].saldoCuotas_aprox);
            console.log("Número de cuotas: " + informacion_cuota[i].cuotas);
            console.log("Interés total a pagar: $" + informacion_cuota[i].interes_total);
            console.log(" ");
        }
    } else {
        console.log("No tienes créditos guardados en este momento.");
    }
}

// Clase para el registro de la cuota de crédito
class registro_cuota {
    constructor(monto, cuotas, saldoCuotas_aprox, interes_total) {
        this.monto = monto;
        this.cuotas = cuotas;
        this.saldoCuotas_aprox = saldoCuotas_aprox;
        this.interes_total = interes_total;
    }
}

// Función para mostrar la información personal de los usuarios que se han ingresado
function mostrarLista(informacion) {
    if (informacion.length === 0) {
        console.log("La lista de usuarios está vacía.");
    } else {
        console.log("\nLista de usuarios:");
        informacion.forEach(e => {
            console.log(`Nombre: ${e.nombre}, Identificación: ${e.identificacion}, Correo: ${e.correo}, Estado Civil: ${e.estadoCivil}`);
        });
    }
}

// Función para eliminar un usuario por nombre
function eliminarUsuario(informacion) {
    let nombreEliminar = prompt("Ingrese el nombre del usuario que desea eliminar: ");
    const index = informacion.findIndex(usuario => usuario.nombre.toLowerCase() === nombreEliminar.toLowerCase());
    if (index !== -1) {
        informacion.splice(index, 1);
        console.log(`El usuario ${nombreEliminar} ha sido eliminado correctamente.`);
    } else {
        console.log("Usuario no encontrado.");
    }
}

// Método de burbuja para ordenar los créditos por monto de cuota (saldoCuotas_aprox)
function ordenarCreditos(informacion_cuota) {
    let n = informacion_cuota.length;
    for (let i = 0; i < n - 1; i++) {
        for (let j = 0; j < n - i - 1; j++) {
            if (parseFloat(informacion_cuota[j].saldoCuotas_aprox) > parseFloat(informacion_cuota[j + 1].saldoCuotas_aprox)) {
                let temp = informacion_cuota[j];
                informacion_cuota[j] = informacion_cuota[j + 1];
                informacion_cuota[j + 1] = temp;
            }
        }
    }
    console.log("Créditos ordenados por monto de cuota.");
}

// Bucle principal para ejecutar el simulador
while (true) {
    let opcion = menu();
    if (opcion == 1) { // Ingresar usuario y calcular crédito
        Datos_usuario(informacion);
        Calculo_pago_mensual(informacion_cuota);
    } else if (opcion == 2) { // Mostrar los créditos guardados y los usuarios
        mostrarCreditosGuardados(informacion_cuota);
        mostrarLista(informacion);
    } else if (opcion == 3) { // Eliminar un usuario
        eliminarUsuario(informacion);
    } else if (opcion == 4) { // Ordenar créditos
        ordenarCreditos(informacion_cuota);
        mostrarCreditosGuardados(informacion_cuota); // Mostrar créditos después de ordenar
    } else if (opcion == 5) { // Salir
        console.log("Saliendo del simulador...");
        break;
    } else {
        console.log("Opción incorrecta");
    }
    // Preguntar si desea continuar
    let continuar = prompt("¿Desea continuar? (si/no): ");
    if (continuar.toLowerCase() !== "si") {
        break;
    }
}

