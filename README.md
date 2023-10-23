mport java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class Producto {
    String nombre;
    double precio;

    public Producto(String nombre, double precio) {
        this.nombre = nombre;
        this.precio = precio;
    }
}

class Venta {
    Producto producto;
    int cantidad;

    public Venta(Producto producto, int cantidad) {
        this.producto = producto;
        this.cantidad = cantidad;
    }

    public double getTotal() {
        return producto.precio * cantidad;
    }

    public String getTipoProducto() {
        return producto.nombre;
    }
}

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        Venta[] ventas = new Venta[100];
        int numVentas = 0;
        double totalVentas = 0;
        double totalDescuentoJubilados = 0;
        double[] totalPorTipoProducto = new double[5];

        Producto[] productos = new Producto[9];
        productos[0] = new Producto("Popcorn Chico", 1.25);
        productos[1] = new Producto("Popcorn Mediano", 2.00);
        productos[2] = new Producto("Popcorn Grande", 3.00);
        productos[3] = new Producto("Hotdog", 2.50);
        productos[4] = new Producto("Refresco Pequeño", 1.30);
        productos[5] = new Producto("Refresco Mediano", 2.00);
        productos[6] = new Producto("Refresco Grande", 2.75);
        productos[7] = new Producto("Agua", 1.50);
        productos[8] = new Producto("Chocolate", 1.75);

        // Imprimir el menú de productos
        System.out.println("Menú:");
        for (int i = 0; i < productos.length; i++) {
            System.out.println((i + 1) + ". " + productos[i].nombre + " - $" + productos[i].precio);
        }

        while (true) {
            System.out.println("Seleccione un producto (1-9) o 0 para finalizar:");
            int opcion = Integer.parseInt(reader.readLine());

            if (opcion == 0) {
                break;
            } else if (opcion >= 1 && opcion <= 9) {
                Producto producto = productos[opcion - 1];
                System.out.println("Cantidad:");
                int cantidad = Integer.parseInt(reader.readLine());

                ventas[numVentas] = new Venta(producto, cantidad);
                numVentas++;
                totalVentas += producto.precio * cantidad;

                if (opcion >= 1 && opcion <= 3) {
                    totalPorTipoProducto[0] += producto.precio * cantidad;
                } else if (opcion == 4) {
                    totalPorTipoProducto[1] += producto.precio * cantidad;
                } else if (opcion >= 5 && opcion <= 7) {
                    totalPorTipoProducto[2] += producto.precio * cantidad;
                } else if (opcion == 8) {
                    totalPorTipoProducto[3] += producto.precio * cantidad;
                } else if (opcion == 9) {
                    totalPorTipoProducto[4] += producto.precio * cantidad;
                }
            }
        }

        System.out.println("¿Es el cliente jubilado? (S/N):");
        String respuestaJubilado = reader.readLine();
        if (respuestaJubilado.equalsIgnoreCase("S")) {
            totalDescuentoJubilados = totalVentas * 0.20;
            totalVentas -= totalDescuentoJubilados;
        }

        System.out.println("Factura:");
        for (int i = 0; i < numVentas; i++) {
            Venta venta = ventas[i];
            System.out.println(venta.cantidad + ". " + venta.producto.nombre + " - $" + venta.getTotal());
        }

        // Consulta 1: Monto vendido y detalle recaudado por tipo de producto
        System.out.println("Consulta 1: Monto vendido y detalle recaudado por tipo de producto");
        String[] tiposProducto = {"Popcorn", "Hotdog", "Refrescos", "Agua", "Chocolate"};
        for (int i = 0; i < tiposProducto.length; i++) {
            String tipo = tiposProducto[i];
            double totalPorTipo = 0;
            for (int j = 0; j < numVentas; j++) {
                if (ventas[j].getTipoProducto().contains(tipo)) {
                    totalPorTipo += ventas[j].getTotal();
                }
            }
            System.out.println(tipo + " - $" + totalPorTipo);
        }

        // Consulta 2: Total recaudado y descuento a jubilados
        System.out.println("Consulta 2: Total recaudado y descuento a jubilados");
        System.out.println("Total de ventas: $" + totalVentas);
        System.out.println("Descuento a jubilados: $" + totalDescuentoJubilados);

        // Consulta 3: Porcentaje de aporte a las ventas por tipo de producto
        System.out.println("Consulta 3: Porcentaje de aporte a las ventas por tipo de producto");
        for (int i = 0; i < tiposProducto.length; i++) {
            String tipo = tiposProducto[i];
            double totalPorTipo = 0;
            for (int j = 0; j < numVentas; j++) {
                if (ventas[j].getTipoProducto().contains(tipo)) {
                    totalPorTipo += ventas[j].getTotal();
                }
            }
            double porcentaje = (totalPorTipo / totalVentas) * 100;
            System.out.println(tipo + " - " + porcentaje + "%");
        }
    }
}
