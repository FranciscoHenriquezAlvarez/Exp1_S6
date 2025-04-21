/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package exp1_s6_francisco_henriquez;

import java.util.ArrayList;
import java.util.Scanner;

/**
 *
 * @author usuario
 */
public class Exp1_s6_francisco_henriquez {

    // Declaracion de precios
    static final double precioVIP = 30000;
    static final double precioPlateaBaja = 15000;
    static final double precioPlateaAlta = 18000;
    static final double precioPalcos = 13000;
    static final double IVA = 0.19;

    // Declaracion variables globales
    static int totalEntradasVendidas = 0;
    static double ingresosTotales = 0;
    static int cantidadEstudiantes = 0;

    // Declaracion de lista que almacena todas las entradas vendidas y reservas
    static ArrayList<Entrada> entradasVendidas = new ArrayList<>();
    static ArrayList<Reserva> reservas = new ArrayList<>();
    static int contadorEntradas = 1;
    static int contadorReservas = 1;
    
    // Variables para estados temporales
    static int tiempoReserva = 5; // Simulado, no en tiempo real
    static String nombreTeatro = "Teatro Moro";
    
    // Metodo principal que incia el sistema
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        char[][] teatro = inicializarTeatro();
        boolean continuarSistema = true;

        while (continuarSistema) {
            int opcionMenu = mostrarMenu(sc);

            switch (opcionMenu) {
                case 1:
                    reservarEntrada(teatro, sc);
                    continuarSistema = deseaOtraOperacion(sc);
                    break;
                case 2:
                    procesarCompra(teatro, sc);
                    continuarSistema = deseaOtraOperacion(sc);
                    break;
                case 3:
                    modificarVenta(teatro, sc);
                    continuarSistema = deseaOtraOperacion(sc);
                    break;
                case 4:
                    imprimirBoletaPorNumero(sc);
                    continuarSistema = deseaOtraOperacion(sc);
                    break;
                case 5:
                    mostrarResumenFinal();
                    System.out.println("\nGracias por visitar el " + nombreTeatro + ". Hasta pronto!.");
                    continuarSistema = false;
                    break;
            }
        }
        sc.close();
    }
    
    // Metodo auxiliar - reserva entradas solicitadas por el cliente.
    static void reservarEntrada(char[][] teatro, Scanner sc) {
        System.out.println("\n-------------------------------------------------------------");
        System.out.println("                    Sistema de Reservas                     ");
        System.out.println("-------------------------------------------------------------");
        mostrarPlanoTeatro(teatro);

        int[] seleccion = seleccionarAsiento(teatro, sc);
        int filaSeleccionada = seleccion[0];
        int asientoSeleccionado = seleccion[1];
        
        if (filaSeleccionada == -1 || asientoSeleccionado == -1) {
            System.out.println("\nOperacion cancelada.");
            return;
        }

        if (teatro[filaSeleccionada][asientoSeleccionado] == 'O') {
            teatro[filaSeleccionada][asientoSeleccionado] = 'R';
            double precioBase = obtenerPrecioEntrada(filaSeleccionada);
            String tipoEntrada = obtenerTipoEntrada(filaSeleccionada);
            Reserva nuevaReserva = new Reserva(contadorReservas++, tipoEntrada, filaSeleccionada, asientoSeleccionado, precioBase);
            reservas.add(nuevaReserva);
            System.out.println("\n-------------------------------------------------------------");
            System.out.println("             Reserva realizada exitosamente                    ");
            System.out.println("-------------------------------------------------------------");
            System.out.println("\nNumero de reserva: " + nuevaReserva.numeroReserva);
            System.out.println("Tipo de entrada reservada: " + tipoEntrada);
            System.out.println("Fila seleccionada: " + filaSeleccionada + " - Asiento seleccionado: " + asientoSeleccionado);
            System.out.println("Muchas gracias por su Preferencia!");
            mostrarPlanoTeatro(teatro);
            
        } else {
            System.out.println("\nEl asiento seleccionado no esta disponible para reserva.");
        }
    }
    
    // Metodo auxiliar - gestiona la compra de entradas, ya sea desde una reserva o de forma directa.
    static void procesarCompra(char[][] teatro, Scanner sc) {
        System.out.println("\n-------------------------------------------------------------");
        System.out.println("                    Sistema de Compras                      ");
        System.out.println("-------------------------------------------------------------");
        mostrarPlanoTeatro(teatro);

        int tipoCompra = validarEntradaUsuarioConOpcionCancelar(sc, "\nSeleccione tipo de compra: 1. Desde reserva / 2. Compra directa", 1, 2);
        if (tipoCompra == -1) {
            System.out.println("\nOperacion cancelada.");
            return;
        }

        switch (tipoCompra) {
            case 1:
                int numeroReserva = validarEntradaUsuarioConOpcionCancelar(sc, "\nIngrese el numero de reserva a comprar", 1, (contadorReservas-1));
                if (numeroReserva == -1) {
                    System.out.println("\nOperacion cancelada.");
                    return;
                }

                Reserva reservaEncontrada = null;
                for (int i = 0; i< reservas.size();i++){
                    Reserva r = reservas.get(i);
                    if (r.numeroReserva == numeroReserva) {
                        reservaEncontrada = r;
                        break;
                    }
                }

                if (reservaEncontrada != null) {
                    teatro[reservaEncontrada.filaSeleccionada][reservaEncontrada.asientoSeleccionado] = 'X'; // <- Punto de depuracion
                    procesarDatosCompra(teatro, sc, reservaEncontrada.filaSeleccionada, reservaEncontrada.asientoSeleccionado, reservaEncontrada.tipoEntrada, 
                                        reservaEncontrada.precioBase, reservaEncontrada);
                } else {
                    System.out.println("\nReserva no encontrada.");
                }
                break;

            case 2:
                int[] seleccion = seleccionarAsiento(teatro, sc);
                if (seleccion[0] == -1 || seleccion[1] == -1) {
                    System.out.println("\nOperacion cancelada.");
                    return;
                }
                int filaSeleccionada = seleccion[0];
                int asientoSeleccionado = seleccion[1];

                teatro[filaSeleccionada][asientoSeleccionado] = 'X';
                double precioBase = obtenerPrecioEntrada(filaSeleccionada);
                String tipoEntrada = obtenerTipoEntrada(filaSeleccionada);

                procesarDatosCompra(teatro, sc, filaSeleccionada, asientoSeleccionado, tipoEntrada, precioBase, null);
                break;
        }
    }
    
    // Metodo auxiliar - calcula descuentos, total a pagar, valor neto, valor iva y guarda la entrada vendida. También imprime el resumen y la boleta.
    static void procesarDatosCompra(char[][] teatro, Scanner sc, int filaSeleccionada, int asientoSeleccionado, String tipoEntrada, double precioBase, Reserva reserva) {
        int edad = validarEntradaUsuario(sc, "Ingrese su edad: ", 1, 120);
        int opcionEstudiante = validarEntradaUsuario(sc, "¿Es estudiante? (1. Si / 2. No): ", 1, 2);

        boolean esEstudiante = (opcionEstudiante == 1);
        String tarifa = determinarTipoTarifa(esEstudiante);
        boolean esAdultoMayor = edad >= 60;
        double descuento = calcularDescuento(precioBase, esEstudiante, esAdultoMayor);
        double totalPagar = precioBase - descuento;
        double valorNeto = totalPagar / (1 + IVA);
        double valorIVA = valorNeto * IVA;
        
        imprimirResumen(tipoEntrada, filaSeleccionada, asientoSeleccionado, precioBase, descuento, 
                                        totalPagar, valorNeto, valorIVA, "Compra"); // <- Punto de depuracion

        if (validarEntradaUsuario(sc, "\n¿Desea confirmar la compra? (1.Si / 2.No): ", 1, 2) == 1) {
            Entrada nuevaEntrada = new Entrada(contadorEntradas++, tipoEntrada, filaSeleccionada, asientoSeleccionado, totalPagar, esEstudiante, esAdultoMayor, tarifa);
            entradasVendidas.add(nuevaEntrada); // <- Punto de depuracion
            totalEntradasVendidas++;
            ingresosTotales += totalPagar;
            if (esEstudiante) {
                cantidadEstudiantes++;
            }
            imprimirBoleta(tipoEntrada, tarifa, nuevaEntrada.numeroEntrada, filaSeleccionada, asientoSeleccionado,precioBase, 
                    descuento, totalPagar, valorNeto, valorIVA, "Pago realizado con exito."); // <- Punto de depuracion
            
        } else {
            System.out.println("\nCompra cancelada.");
            teatro[filaSeleccionada][asientoSeleccionado] = 'O';
            reservas.remove(reserva);
            contadorReservas--;
            System.out.println("\nReserva eliminada.");
        }  
    }
    
    // Metodo auxiliar - Permite cambiar la ubicacion de una entrada ya comprada.
    static void modificarVenta(char[][] teatro, Scanner sc) {
        System.out.println("\n-------------------------------------------------------------");
        System.out.println("                  Sistema de Modificacion de Ventas          ");
        System.out.println("-------------------------------------------------------------");
        mostrarPlanoTeatro(teatro);

        int numeroEntrada = validarEntradaUsuarioConOpcionCancelar(sc, "\nIngrese el numero de entrada a modificar", 1, contadorEntradas);
        if (numeroEntrada == -1) {
            System.out.println("\nOperacion cancelada.");
            return;
        }

        Entrada entradaEncontrada = null;
        for (int i = 0; i < entradasVendidas.size(); i++){
            Entrada e = entradasVendidas.get(i);
            if (e.numeroEntrada == numeroEntrada) {
                entradaEncontrada = e;
                break;
            }
        }

        if (entradaEncontrada != null) {
            teatro[entradaEncontrada.filaSeleccionada][entradaEncontrada.asientoSeleccionado] = 'O';
            System.out.println("Seleccione nueva ubicacion:");
            int[] nuevaSeleccion = seleccionarAsiento(teatro, sc);
            if (nuevaSeleccion[0] == -1 || nuevaSeleccion[1] == -1) {
                System.out.println("\nOperacion cancelada.");
                return;
            }

            String TipoEntrada = obtenerTipoEntrada(nuevaSeleccion[0]);
            int filaSeleccionada = nuevaSeleccion[0];
            int asientoSeleccionado = nuevaSeleccion[1];
            double precioBase = obtenerPrecioEntrada(nuevaSeleccion[0]);
            double totalPagar = precioBase;
            double valorNeto = obtenerPrecioEntrada(nuevaSeleccion[0]) / (1 + IVA);
            double valorIVA = valorNeto * IVA;
            
            System.out.println("\nBoleta anterior anulada!. Monto abonado directo a su tarjeta.");
            imprimirResumen(TipoEntrada, filaSeleccionada, asientoSeleccionado, precioBase, 0, 
                    totalPagar, valorNeto, 
                    valorIVA, "Modificacion");

            if (validarEntradaUsuario(sc, "\n¿Desea confirmar la modificacion? (1.Si / 2.No): ", 1, 2) == 1) {
                ingresosTotales -= entradaEncontrada.totalPagar;
                
                entradaEncontrada.tipoEntrada = TipoEntrada;
                entradaEncontrada.filaSeleccionada = nuevaSeleccion[0];
                entradaEncontrada.asientoSeleccionado = nuevaSeleccion[1];
                entradaEncontrada.totalPagar = totalPagar;
                
                ingresosTotales += entradaEncontrada.totalPagar;
                
                teatro[nuevaSeleccion[0]][nuevaSeleccion[1]] = 'X';
                System.out.println("\nModificacion realizada exitosamente.");
                
                imprimirBoleta(entradaEncontrada.tipoEntrada, entradaEncontrada.tarifa, entradaEncontrada.numeroEntrada, filaSeleccionada, asientoSeleccionado,
                    precioBase, 0, totalPagar, valorNeto, valorIVA, "Pago realizado con exito.");    
                
            } else {
                teatro[entradaEncontrada.filaSeleccionada][entradaEncontrada.asientoSeleccionado] = 'X';
                System.out.println("\nModificacion cancelada.");
            }
        } else {
            System.out.println("\nEntrada no encontrada.");
        }
    }

    // Metodo auxiliar - reimprime la boleta de ventas segun el numero de boleto vendido
    static void imprimirBoletaPorNumero(Scanner sc) {
        System.out.println("\n-------------------------------------------------------------");
        System.out.println("               Sistema de Reimpresion de Boletas        ");
        System.out.println("-------------------------------------------------------------");

        int numeroEntrada = validarEntradaUsuarioConOpcionCancelar(sc, "\nIngrese el numero de entrada ", 1, contadorEntradas);
        if (numeroEntrada == -1) {
            System.out.println("\nOperacion cancelada.");
            return;
        }

        Entrada entradaEncontrada = null; 
        for (int i = 0; i < entradasVendidas.size(); i++){
            Entrada e = entradasVendidas.get(i);
            if (e.numeroEntrada == numeroEntrada) {
                entradaEncontrada = e;
                break;
            }
        }

        if (entradaEncontrada != null) {
            double valorNeto = entradaEncontrada.totalPagar / (1 + IVA);
            double valorIVA = valorNeto * IVA;
            double descuento = 0;
            double precioBase = obtenerPrecioEntrada(entradaEncontrada.filaSeleccionada);
            imprimirBoleta(entradaEncontrada.tipoEntrada, entradaEncontrada.tarifa, entradaEncontrada.numeroEntrada, entradaEncontrada.filaSeleccionada, 
                    entradaEncontrada.asientoSeleccionado, precioBase, descuento, entradaEncontrada.totalPagar, valorNeto, valorIVA,
                    "Reimpresion realizada con exito.");
            
        } else {
            System.out.println("\nEntrada no encontrada.");
        }
    }

    // Metodo auxiliar - muestra el menú principal con las opciones disponibles.
    static int mostrarMenu(Scanner sc) {
        System.out.println("\n-------------------------------------------------------------");
        System.out.println("                  Bienvenido al " + nombreTeatro + "                  ");
        System.out.println("-------------------------------------------------------------");
        System.out.println("\n1. Reservar entrada");
        System.out.println("2. Comprar entrada");
        System.out.println("3. Modificar venta");
        System.out.println("4. Imprimir boleta");
        System.out.println("5. Salir");
        return validarEntradaUsuario(sc, "\nSeleccione una opcion: ", 1, 5);
    }

    // Metodo auxiliar - inicializa el arreglo para el plano: 7 filas, 5 asientos, libres.
    static char[][] inicializarTeatro() {
        char[][] teatro = new char[7][5];
        for (int i = 0; i < teatro.length; i++) {
            for (int c = 0; c < teatro[i].length; c++) {
                // Asignar O para asiento libre
                teatro[i][c] = 'O';
            }
        }
        return teatro;
    }

    // Metodo auxiliar - imprimir plano del teatro.
    static void mostrarPlanoTeatro(char[][] teatro) {
        System.out.println("\nPLANO DEL TEATRO (O = libre, R= reservado, X = ocupado):\n");
        System.out.println("                        Asiento");
        System.out.println("                       0 1 2 3 4");
        for (int fila = 0; fila < teatro.length; fila++) {
            String zonaNombre = "";
            if (fila <= 0) zonaNombre = "Fila " + fila + " - VIP        ";       // VIP 
            else if (fila <= 3) zonaNombre = "Fila " + fila + " - Palco      ";  // Palco 
            else if (fila <= 5) zonaNombre = "Fila " + fila + " - Platea Baja";  // Platea Baja 
            else zonaNombre = "Fila " + fila  + " - Platea Alta"; // Plantea Alta 

            System.out.print(zonaNombre + " | ");
            for (int asiento = 0; asiento < teatro[fila].length; asiento++) {
                System.out.print(teatro[fila][asiento] + " ");
            }
            System.out.println();
        }
    }
    
    // Metodo auxiliar - permite al usuario elegir un asiento disponible en el teatro.
    static int[] seleccionarAsiento(char[][] teatro, Scanner sc) {
        int filaSeleccionada = -1, asientoSeleccionado = -1;
        boolean asientoLibre = false;

        while (!asientoLibre) {
            filaSeleccionada = validarEntradaUsuarioConOpcionCancelar(sc, "\nIngrese fila (0-6)", 0, 6); // <- Punto de depuracion
            if (filaSeleccionada == -1) {
                return new int[] {-1, -1};
            }

            asientoSeleccionado = validarEntradaUsuarioConOpcionCancelar(sc, "Ingrese asiento (0-4)", 0, 4); // <- Punto de depuracion
            if (asientoSeleccionado == -1) {
                return new int[] {-1, -1};
            }

            if (teatro[filaSeleccionada][asientoSeleccionado] == 'O') {
                asientoLibre = true; // <- Punto de depuracion
            } else {
                System.out.println("Asiento ocupado. Intente con otro.");
            }
        }
        return new int[] {filaSeleccionada, asientoSeleccionado};
    }

    // Metodo auxiliar - determina el precio de la entrada segun fila seleccionada.
    static double obtenerPrecioEntrada(int filaSeleccionada) {
        switch (filaSeleccionada) {
            case 0: return precioVIP;
            case 1: case 2: case 3: return precioPalcos;
            case 4: case 5: return precioPlateaBaja;
            case 6: return precioPlateaAlta;
            default: return 0;
        }
    }

    // Metodo auxiliar - determina el tipo de entrada segun fila seleccionada.
    static String obtenerTipoEntrada(int filaSeleccionada) {
        switch (filaSeleccionada) {
            case 0: return "VIP";
            case 1: case 2: case 3: return "Palco";
            case 4: case 5: return "Platea Baja";
            case 6: return "Platea Alta";
            default: return "";
        }
    }
    
    // Metodo auxiliar - determina el tipo de tarifa si es estudiante o publico general.
    static String determinarTipoTarifa (boolean esEstudiante){
        if (esEstudiante){
        return "Estudiante";
        } else{
        return "Publico General"; 
        }     
    }    
    
    // Metodo auxiliar - determina el descuento a aplicar segun si es usuario es estudiante y/o adulto mayor.
    static double calcularDescuento(double precioBase, boolean estudiante, boolean adultoMayor) {
        if (estudiante && adultoMayor) {
            System.out.println("\n¡Felicitaciones!. Se aplicara un 25% de descuento por ser adulto mayor y estudiante. ¡Tu puedes alcanzar tu meta!");
            return precioBase * 0.25;
        }
        else if (adultoMayor) {
        System.out.println("\nSe aplicara un 15% de descuento por tercera edad.");           
            return precioBase * 0.15;
        }
        else if (estudiante) {
            System.out.println("\nSe aplicara un 10% de descuento por ser estudiante.");           
            return precioBase * 0.10;
        }
        else return 0;
    }

    // Metodo auxiliar - solicita un valor dentro de un rango, permitiendo cancelar con 'c'.
    static int validarEntradaUsuarioConOpcionCancelar(Scanner sc, String mensaje, int min, int max) {
        int valor = -1;
        boolean valido = false;
        if (min == 1 && max == 0){
        max = 1;
        }
        while (!valido) {
            System.out.print(mensaje + " o 'c' para cancelar: ");
            String entrada = sc.nextLine().trim();
            if (entrada.equalsIgnoreCase("c")) {
                return -1; // Cancelar operación
            }
            try {
                valor = Integer.parseInt(entrada);
                if (valor >= min && valor <= max){ 
                    valido = true;
                } else {
                    System.err.println("Entrada invalida. Intente nuevamente.");
                }
            } catch (NumberFormatException  e) {
                System.err.println("Entrada invalida. Intente nuevamente.");
            }
        }
        return valor;
    }
    
    // Metodo auxiliar - solicita un valor dentro de un rango, sin opcion de cancelar.
    static int validarEntradaUsuario(Scanner sc, String mensaje, int min, int max) {
        int valor = -1;
        boolean valido = false;
        while (!valido) {
            System.out.print(mensaje);
            String entrada = sc.nextLine().trim();
            try {
                valor = Integer.parseInt(entrada);
                if (valor >= min && valor <= max){ 
                    valido = true;
                } else {
                    System.err.println("Entrada invalida. Intente nuevamente.");
                }
            } catch (NumberFormatException  e) {
                System.err.println("Entrada invalida. Intente nuevamente.");
            }
        }
        return valor;
    }
    
    // Metodo auxiliar - permite imprimir un resumen del tipo de operacion. 
    static void imprimirResumen(String tipoEntrada, int filaSeleccionada, int asientoSeleccionado, double precioBase, double descuento, 
                                        double totalPagar, double valorNeto, double valorIVA, String tipoOperacion){
        System.out.println("\n-------------------------------------------------------------");
        System.out.println("                    Resumen de " + tipoOperacion);
        System.out.println("-------------------------------------------------------------");
        System.out.println("\nTipo de entrada: " + tipoEntrada);
        System.out.printf("Ubicacion: Fila %d, Asiento %d\n", filaSeleccionada, asientoSeleccionado);
        System.out.printf("Precio base: $%.0f\n", precioBase);
        System.out.printf("Descuento aplicado: $%.0f\n", descuento);
        System.out.printf("Total a pagar: $%.0f\n", totalPagar);
        System.out.printf("Valor neto (sin IVA): $%.0f\n", valorNeto);
        System.out.printf("IVA (19%%): $%.0f\n", valorIVA);
        System.out.println("-------------------------------------------------------------");     
    }

    // Metodo auxiliar - permite confirma la realizacion de otra operacion. 
    static boolean deseaOtraOperacion(Scanner sc) {
        int opcion = validarEntradaUsuario(sc, "\n¿Desea realizar otra operacion? (1. Si / 2. No): ", 1, 2);
        if (opcion == 2) {
            System.out.println("\nGracias por visitar el " + nombreTeatro + ". Hasta pronto!.");
            return false;
        }
        return opcion == 1;
    }

    // Metodo auxiliar - imprimir resumen final - termino de ejecucion del programa. 
    static void mostrarResumenFinal() {
        System.out.println("\n-----------------------------------------------------------");
        System.out.println("                       Resumen Final");
        System.out.println("-----------------------------------------------------------");
        System.out.println("\nEntradas vendidas: " + totalEntradasVendidas);
        System.out.printf("Total recaudado: $%.0f\n", ingresosTotales);
        System.out.println("Cantidad de estudiantes: " + cantidadEstudiantes);
        System.out.println("\n-----------------------------------------------------------");
    }

    // Metodo auxiliar - imprimir boleta de compra.
    static void imprimirBoleta(String tipoEntrada, String tarifa, int numeroEntrada, int filaSeleccionada, int asientoSeleccionado,
                                double precioBase, double descuento, double totalPagar,
                                double valorNeto, double valorIVA, String glosa) {
    System.out.println("\n"+glosa+"\n");
    System.out.println("-------------------------------------------------------------");
    System.out.println("                         "+nombreTeatro+"                            ");
    System.out.println("-------------------------------------------------------------");
    System.out.println("                      Boleta de Venta                          ");
    System.out.println("-------------------------------------------------------------");
    System.out.println("");
    System.out.println("Tipo de entrada: " + tipoEntrada);
    System.out.println("Tarifa: " + tarifa);
    System.out.println("Numero de entrada: " + numeroEntrada);
    System.out.printf("Ubicacion: Fila %d, Asiento %d\n", filaSeleccionada, asientoSeleccionado);
    System.out.printf("Precio base tarifa: $%.0f\n", precioBase);
    System.out.printf("Descuento aplicado: $%.0f\n", descuento);
    System.out.printf("Total a pagar: $%.0f\n", totalPagar);
    System.out.printf("Valor neto (sin IVA): $%.0f\n", valorNeto);
    System.out.printf("IVA incluido (19%%): $%.0f\n", valorIVA);
    System.out.println("");
    System.out.println("-------------------------------------------------------------");
    System.out.println("         Gracias por su compra, disfrute la funcion          ");
    System.out.println("-------------------------------------------------------------");
    }
}

// Clase Entrada representa cada entrada vendida
class Entrada {
    int numeroEntrada;
    String tipoEntrada;
    int filaSeleccionada;
    int asientoSeleccionado;
    double totalPagar;
    boolean estudiante;
    boolean adultoMayor;
    String tarifa;

    Entrada(int numeroEntrada, String tipoEntrada, int filaSeleccionada, int asientoSeleccionado, double totalPagar, boolean estudiante, boolean adultoMayor, String tarifa) {
        this.numeroEntrada = numeroEntrada;
        this.tipoEntrada = tipoEntrada;
        this.filaSeleccionada = filaSeleccionada;
        this.asientoSeleccionado = asientoSeleccionado;
        this.totalPagar = totalPagar;
        this.estudiante = estudiante;
        this.adultoMayor = adultoMayor;
        this.tarifa = tarifa;
    }
}

// Clase Reserva representa cada reserva
class Reserva {
    int numeroReserva;
    String tipoEntrada;
    int filaSeleccionada;
    int asientoSeleccionado;
    double precioBase;

    Reserva(int numeroReserva, String tipoEntrada, int filaSeleccionada, int asientoSeleccionado, double precioBase) {
        this.numeroReserva = numeroReserva;
        this.tipoEntrada = tipoEntrada;
        this.filaSeleccionada = filaSeleccionada;
        this.asientoSeleccionado = asientoSeleccionado;
        this.precioBase = precioBase;
    }
}
