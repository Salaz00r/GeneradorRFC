# GeneradorRFC
Programa basico para Generacion de RFC

#include <iostream>
#include <string>
#include <cstdlib>
#include <ctime>

using namespace std;

class Empleado {
private:
    string nombre;
    string apellidoPaterno;
    string apellidoMaterno;
    string fechaNacimiento; // Formato: DD/MM/AAAA

public:
    // Método para capturar datos del empleado
    void capturarDatos() {
        cout << "Escribe el nombre: ";
        getline(cin, nombre);
        
        cout << "Escribe el apellido paterno: ";
        getline(cin, apellidoPaterno);
        
        cout << "Escribe el apellido materno: ";
        getline(cin, apellidoMaterno);
        
        cout << "Escribe la fecha de nacimiento (DD/MM/AAAA): ";
        getline(cin, fechaNacimiento);
    }

    // Método para obtener la primera vocal interna del apellido paterno
    char obtenerPrimeraVocalInterna(const string& apellido) {
        for (size_t i = 1; i < apellido.length(); ++i) {
            char c = tolower(apellido[i]);
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                return toupper(c);
            }
        }
        return 'X'; // En caso de no encontrar ninguna vocal interna
    }

    // Método para calcular el RFC
    string calcularRFC() {
        string rfc;

        // Primera letra del apellido paterno
        rfc += toupper(apellidoPaterno[0]);

        // Primera vocal interna del apellido paterno
        rfc += obtenerPrimeraVocalInterna(apellidoPaterno);

        // Inicial del apellido materno o 'X' si no hay apellido materno
        if (!apellidoMaterno.empty()) {
            rfc += toupper(apellidoMaterno[0]);
        } else {
            rfc += 'X';
        }

        // Inicial del primer nombre
        rfc += toupper(nombre[0]);

        // Los dos últimos dígitos del año de nacimiento
        rfc += fechaNacimiento.substr(8, 2);

        // Mes de nacimiento
        rfc += fechaNacimiento.substr(3, 2);

        // Día de nacimiento
        rfc += fechaNacimiento.substr(0, 2);

        return rfc;
    }

    // Método para generar homoclave
    string generarHomoclave() {
        string homoclave;
        const char caracteres[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        for (int i = 0; i < 3; ++i) {
            homoclave += caracteres[rand() % (sizeof(caracteres) - 1)];
        }
        return homoclave;
    }
};

int main() {
    srand(time(0)); // Inicializar la semilla para generación aleatoria

    Empleado empleado;
    empleado.capturarDatos();

    string rfc = empleado.calcularRFC();
    string homoclave = empleado.generarHomoclave();

    cout << "El RFC generado es: " << rfc << homoclave << endl;

    return 0;
