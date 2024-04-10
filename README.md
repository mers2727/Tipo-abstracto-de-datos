public class Main {
    public static void main(String[] args) {
            Diccionario diccionario0 = new Diccionario();
        try {
            Diccionario diccionario1 = new Diccionario("cartera", "Objeto cuadrangular de pequeñas dimensiones hecho de piel u otro material, plegado por su mitad, que puede llevarse en el bolsillo y sirve para contener documentos, tarjetas, billetes de banco, etcétera.");
            ArrayList<String> palabra = new ArrayList<>();
            palabra.add("raton");
            palabra.add("pantalla");
            palabra.add("teclado");
            palabra.add("altavoz");
            ArrayList<String> definicion = new ArrayList<>();
            definicion.add("Mamífero roedor de pequeño tamaño, de hocico puntiagudo y cola larga, de pelaje corto. Usado en masculino referido a la especie.");
            definicion.add("Lámina que se sujeta delante o alrededor de un foco luminoso artificial, para que la luz no moleste a los ojos o para dirigirla hacia donde se quiera.");
            definicion.add("Conjunto de las teclas de un piano u otro instrumento musical.");
            definicion.add("Aparato electroacústico que transforma la corriente eléctrica en sonido.");

            Diccionario diccionario2 = new Diccionario(palabra, definicion);

            diccionario2.AñadirPalabra("vaso", "Recipiente de vidrio, metal u otra materia, por lo común de forma cilíndrica, que sirve para beber.");

            diccionario2.AñadirPalabraFecha("cable", "Cordón de alambres trenzados.", LocalDate.of(2003, 3, 12));

            System.out.println(diccionario2.obtenerTodasLasPalabras());
            System.out.println(diccionario2.obtenerDefinicion("vaso"));

            System.out.println(diccionario2.EliminarPalabra("pantalla"));

            System.out.println(diccionario2.actualizarPalabra("raton", "Mamífero roedor de pequeño tamaño, de hocico puntiagudo y cola larga, de pelaje corto. Usado en masculino referido a la especie.", LocalDate.of(2002, 5, 20)));



        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }


}


public class Diccionario {
    private ArrayList<Palabra> diccionario;


    public Diccionario() {
        this.diccionario = new ArrayList<>();

    }
    public Diccionario(String palabra,String definicion) throws Exception {
        this.diccionario = new ArrayList<>();
        Palabra nuevaPalabra=new Palabra(palabra, definicion);
        diccionario.add(nuevaPalabra);
    }

    public Diccionario(ArrayList<String> palabra, ArrayList<String> definicion ) throws Exception {
        if(palabra.size()!= definicion.size() || palabra.isEmpty() || definicion.isEmpty()){
            throw new Exception();
        }
        this.diccionario = new ArrayList<>();
        for (int i = 0; i < palabra.size(); i++) {

            Palabra nuevaPalabra=new Palabra(palabra.get(i), definicion.get(i));
            diccionario.add(nuevaPalabra);
        }
        assert diccionario.size()== palabra.size() && diccionario.size()== definicion.size();
    }
    public void AñadirPalabra(String palabra, String definicion) throws Exception {
        Palabra nuevaPalabra=new Palabra(palabra,definicion);
        diccionario.add(nuevaPalabra);
    }

    public void AñadirPalabraFecha(String palabra, String definicion, LocalDate fecha) throws Exception {
        Palabra nuevaPalabra = new Palabra(palabra, definicion,fecha);
        diccionario.add(nuevaPalabra);
    }


    public ArrayList<String> obtenerTodasLasPalabras() {
        ArrayList<String> todasLasPalabras = new ArrayList<>();
        for (Palabra palabra : diccionario) {
            todasLasPalabras.add(palabra.getPalabra());
        }
        return todasLasPalabras;
    }
    public String obtenerDefinicion(String palabra1) {
        for (Palabra palabra : diccionario) {
            if (palabra.getPalabra().equals(palabra1)) {
                return palabra.getDefinicion();
            }
        }
        return null;
    }

    public String EliminarPalabra(String palabra) throws Exception{
        if(Objects.equals(palabra, "")){
            throw new Exception();
        }
        for (int i = 0; i < diccionario.size() ; i++) {
            if(Objects.equals(diccionario.get(i).getPalabra(), palabra)){
                diccionario.remove(i);
                return palabra;
            }
        }
        return null;
    }

    public boolean actualizarPalabra(String palabra, String definicion, LocalDate fechaActual) throws Exception {
        if(Objects.equals(palabra, "") || Objects.equals(definicion, "")){
            throw new Exception();
        }
        for (Palabra value : diccionario) {
            if (Objects.equals(value.getPalabra(), palabra)) {
                value.cambiarFechayDefinicion(definicion, fechaActual);
                return true;
            }
        }
        return false;
    }

    public boolean ContienePalabra(String palabra) throws Exception {
        if(Objects.equals(palabra, "")){
            throw new Exception();
        }
        for (int i = 0; i < diccionario.size() ; i++) {
            if(Objects.equals(diccionario.get(i).getPalabra(), palabra)){
                return true;
            }
        }
        return false;
    }

    public int Npalabras(){
        return diccionario.size();
    }

    public Diccionario palabrasFechas(LocalDate fecha1, LocalDate fecha2) throws Exception {
        if(fecha1==null || fecha2==null || fecha2.isAfter(fecha1)){
            throw new Exception();
        }
        Diccionario diccionario_nuevo= new Diccionario();
        for (int i = 0; i <diccionario.size() ; i++) {
            if(diccionario.get(i).getFecha().isAfter(fecha1) && diccionario.get(i).getFecha().isBefore(fecha2)){
                diccionario_nuevo.AñadirPalabraFecha(diccionario.get(i).getPalabra(),diccionario.get(i).getDefinicion(),diccionario.get(i).getFecha());
            }
        }
        return diccionario_nuevo;

    }


}



public class Palabra {
    private String palabra;
    private String definicion;
    private LocalDate fecha;

    public String getPalabra() {
        return palabra;
    }

    public String getDefinicion() {
        return definicion;
    }

    public LocalDate getFecha() {
        return fecha;
    }

    public Palabra(String palabra, String definicion) throws Exception {
        if(Objects.equals(palabra, "") || Objects.equals(definicion, "")) {
            throw new Exception();
        }
        this.palabra = palabra;
        this.definicion = definicion;
        this.fecha = LocalDate.now();

    }
    public Palabra(String palabra, String definicion, LocalDate fecha) throws Exception {
        if(Objects.equals(palabra, "") || Objects.equals(definicion, "" ) || fecha == null) {
            throw new Exception();
        }
        this.palabra = palabra;
        this.definicion = definicion;
        this.fecha = fecha;
    }

    public void cambiarFechayDefinicion(String definicion, LocalDate fecha_actual) throws Exception {
        if(Objects.equals(definicion, "")){
            throw new Exception();
        }
        this.definicion=definicion;
        this.fecha=fecha_actual;
    }

}
