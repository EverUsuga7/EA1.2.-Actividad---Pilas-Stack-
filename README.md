# EA1.2.-Actividad---Pilas-Stack-
simulador de deshacer/rehacer (Undo/Redo)
import java.util.Scanner;

 * Clase que implementa una Pila de forma manual usando nodos
 */
class Pila {
    // Clase interna para los nodos
    private class Nodo {
        String dato;
        Nodo siguiente;
        
        Nodo(String dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
    
    private Nodo cima; // Elemento superior de la pila
    private int tamaño; // Cantidad de elementos
    
     * Constructor - pila vacía
     */
    public Pila() {
        this.cima = null;
        this.tamaño = 0;
    }
   
     * Agrega un elemento a la pila
     */
    public void push(String dato) {
        Nodo nuevo = new Nodo(dato);
        nuevo.siguiente = cima;
        cima = nuevo;
        tamaño++;
    }
    
     * Elimina y devuelve el elemento superior
     */
    public String pop() {
        if (estaVacia()) {
            return null;
        }
        String dato = cima.dato;
        cima = cima.siguiente;
        tamaño--;
        return dato;
    }
    
     * Devuelve el elemento superior sin eliminarlo
     */
    public String peek() {
        if (estaVacia()) {
            return null;
        }
        return cima.dato;
    }
    
     * Verifica si la pila está vacía
     */
    public boolean estaVacia() {
        return cima == null;
    }
    
     * Obtiene el tamaño de la pila
     */
    public int getTamaño() {
        return tamaño;
    }
    
     * Vacía la pila
     */
    public void vaciar() {
        cima = null;
        tamaño = 0;
    }
}

 * Clase principal del editor de texto
 */
public class EditorTexto {
    // Pilas para las operaciones
    private Pila pilaPrincipal;   // Guarda los cambios realizados
    private Pila pilaSecundaria;  // Guarda los cambios deshechos
    
    // Texto actual
    private StringBuilder textoActual;
    
    // Para entrada de datos
    private Scanner scanner;
    
     * Constructor
     */
    public EditorTexto() {
        pilaPrincipal = new Pila();
        pilaSecundaria = new Pila();
        textoActual = new StringBuilder();
        scanner = new Scanner(System.in);
    }
    
     * Agrega texto al contenido actual
     */
    public void escribir(String texto) {
        // Guardar estado actual antes de cambiar
        pilaPrincipal.push(textoActual.toString());
        
        // Agregar nuevo texto
        if (textoActual.length() > 0) {
            textoActual.append("\n");
        }
        textoActual.append(texto);
        
        // Limpiar pila de rehacer
        pilaSecundaria.vaciar();
        
        System.out.println("Texto agregado");
    }
    
     * Deshace la última acción
     */
    public void deshacer() {
        if (pilaPrincipal.estaVacia()) {
            System.out.println("No hay acciones para deshacer");
            return;
        }
        
        // Guardar estado actual para poder rehacer
        pilaSecundaria.push(textoActual.toString());
        
        // Restaurar estado anterior
        textoActual = new StringBuilder(pilaPrincipal.pop());
        
        System.out.println("Deshacer realizado");
    }
    
     * Rehace la última acción deshecha
     */
    public void rehacer() {
        if (pilaSecundaria.estaVacia()) {
            System.out.println("No hay acciones para rehacer");
            return;
        }
        
        // Guardar estado actual
        pilaPrincipal.push(textoActual.toString());
        
        // Restaurar estado deshecho
        textoActual = new StringBuilder(pilaSecundaria.pop());
        
        System.out.println("Rehacer realizado");
    }
    
     * Muestra el texto actual
     */
    public void mostrarTexto() {
        System.out.println("\nTEXTO ACTUAL:");
        System.out.println("==================");
        if (textoActual.length() == 0) {
            System.out.println("[Vacío]");
        } else {
            System.out.println(textoActual);
        }
        System.out.println("==================\n");
    }
    
     * Muestra el menú de opciones
     */
    public void mostrarMenu() {
        System.out.println("\nEDITOR DE TEXTO");
        System.out.println("1. Escribir texto");
        System.out.println("2. Deshacer");
        System.out.println("3. Rehacer");
        System.out.println("4. Mostrar texto");
        System.out.println("5. Salir");
        System.out.print("Elija una opción: ");
    }
    
     * Ejecuta el programa
     */
    public void iniciar() {
        int opcion;
        
        System.out.println("BIENVENIDO AL EDITOR DE TEXTO");
        
        do {
            mostrarMenu();
            
            try {
                opcion = Integer.parseInt(scanner.nextLine());
                
                switch (opcion) {
                    case 1:
                        System.out.print("Escriba el texto: ");
                        String texto = scanner.nextLine();
                        escribir(texto);
                        mostrarTexto();
                        break;
                        
                    case 2:
                        deshacer();
                        mostrarTexto();
                        break;
                        
                    case 3:
                        rehacer();
                        mostrarTexto();
                        break;
                        
                    case 4:
                        mostrarTexto();
                        break;
                        
                    case 5:
                        System.out.println("¡Hasta luego!");
                        break;
                        
                    default:
                        System.out.println("Opción no válida");
                }
                
                // Mostrar estado de las pilas
                System.out.println("Estado: Principal=" + 
                    pilaPrincipal.getTamaño() + " | Secundaria=" + 
                    pilaSecundaria.getTamaño());
                
            } catch (NumberFormatException e) {
                System.out.println("Error: Ingrese un número válido");
                opcion = 0;
            }
            
        } while (opcion != 5);
        
        scanner.close();
    }
    
     * Método principal
     */
    public static void main(String[] args) {
        EditorTexto editor = new EditorTexto();
        editor.iniciar();
    }
}



 BIENVENIDO AL EDITOR DE TEXTO

 EDITOR DE TEXTO
1.  Escribir texto
2.  Deshacer
3.  Rehacer
4.  Mostrar texto
5.  Salir
 Elija una opción: 1
Escriba el texto: Hola mundo
 Texto agregado

 TEXTO ACTUAL:
==================
Hola mundo
==================

 Estado: Principal=1 | Secundaria=0
