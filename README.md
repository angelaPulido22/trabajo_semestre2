# trabajo_semestre2
informacion_cuota = []; 
informacion = [];
//listas para guardar informacion de los datos personales del usuario e informacion de la cuota


// funcion de "menu" donde estan las opciones del simulador de credito
function menu() {
    console.log("SIMULADOR DE GESTIÓN DE CRÉDITO");
    console.log("Elige la opción: ");
    console.log("1) Ingresar información para el crédito");
    console.log("2) Información de créditos guardados");
    console.log("3) Eliminar un usuario");
    console.log("4) Eliminar una cuota");
    console.log("5) Ordenar créditos por monto de cuota");
    let opcion = prompt("Opción: ");
    return opcion;
}

// funcion donde se introduce la informacion del usuario
function Datos_usuario(informacion) {
    console.log("Ingresando datos..");
    console.log("Por favor, complete la información del usuario.");
    let nombre = prompt("Nombre completo del usuario: ");
    let identificacion = prompt("Identificación Usuario: ");
    let numero = parseFloat(prompt("Número telefónico: "));
    let correo = prompt("Correo electrónico: ");
    let estadoCivil = prompt("Estado civil: ");
    console.log("Datos del usuario ingresados correctamente.");
    informacion.push({nombre, identificacion, numero, correo, estadoCivil});
}

 //funcion del calculo del monto que se vaya a cotizar
function Calculo_pago_mensual(informacion_cuota) {
    console.log("Calculando el monto total del crédito...");
    let monto = parseFloat(prompt("Digite el monto a cotizar: "));
    let interes = 0.1633;
    let interes_total = monto * interes; 
    let total = monto + interes_total;
    let cuotas = parseFloat(prompt("Digite cuántas cuotas va a tener el crédito: "));    
    if (cuotas >= 2 && cuotas <= 36) {
        let saldoCuotas = total / cuotas;
        let saldoCuotas_aprox = saldoCuotas.toFixed(1);
        console.log("El total de su cuota será de: ", saldoCuotas_aprox);
        console.log("Interés total: ", interes_total);
        console.log("Su crédito consta de", cuotas, "cuotas, con un monto total de", saldoCuotas_aprox, "por cuota.");

}
 // funcion donde se guardan y se muestran los creditos cotizados por medio de un bucle 
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

//funcion para mostrar la informacion personal de los clientes que halla ingresado anteriormente
function mostrarLista(informacion) {
    if (informacion.length === 0) {
        console.log("La lista de usuarios está vacía.");
    } else {
        console.log("\nLista de usuarios:");
        informacion.forEach(e => {
            console.log(`Nombre: ${e.nombre}, Identificación: ${e.identificacion}, Número: ${e.numero}, Correo: ${e.correo}, Estado Civil: ${e.estadoCivil}`);});
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
                // Intercambiar los elementos
                let temp = informacion_cuota[j];
                informacion_cuota[j] = informacion_cuota[j + 1];
                informacion_cuota[j + 1] = temp;
            }
        }
    }
    console.log("Créditos ordenados por monto de cuota.");
}

while (true) {
    let opcion = menu();
    if (opcion == 1) { // Ingresar usuario y calcular crédito
        Datos_usuario(informacion);
        Calculo_pago_mensual(informacion_cuota);
    }
    else if (opcion == 2) { // Mostrar los créditos guardados y los usuarios
        mostrarCreditosGuardados(informacion_cuota);
        mostrarLista(informacion);
    }
    else if (opcion == 3) { // Eliminar un usuario
        eliminarUsuario(informacion);
    }
    else if (opcion == 5) { // Ordenar créditos
        ordenarCreditos(informacion_cuota);
        mostrarCreditosGuardados(informacion_cuota); // Mostrar créditos después de ordenar
    }
    else {
        console.log("Opción incorrecta");
    }
    // Para salir del bucle puedes agregar una opción de salida
    let continuar = prompt("¿Desea continuar? (si/no): ");
    if (continuar.toLowerCase() !== "si") {
        break;
    }
}
