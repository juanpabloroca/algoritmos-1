# algoritmos-1
// salario con vectores.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"
#include "conio.h"
#include "math.h"
#include <iostream>
#include <string>


using namespace std;

void cargar(float vec1[], int dimension);
void mostrar(float vec1[], int dimension);
void salario(float vec1[], int dimension, float salarioneto[]);
void menu();

struct t_empleado
{
	int id;
	string nombre, fechanac;
	float sb, sn;
};

t_empleado empleado;

void cargarvector(t_empleado v[], int n);
void clasificarburbuja(t_empleado v[], int n);
bool busquedacodigo(t_empleado v[], int n, int clave, int &pos);
void mostrarVectorEmpleado(t_empleado v[], int dimension);
void listarplanilla(t_empleado v[], int n);
float salarioneto(float sb);
float promedio(t_empleado v[], int n);


int main()
{
	t_empleado vec[60] = { 0 };
	int dimension=0, i=0;
	int clave, pos;
	bool b = false;
	system("cls");
	int opcion=9;		
	while (opcion != 0){		//CONTINUA HASTA QUE LA OPCION NO SEA 0
		menu();		
		cin >> opcion;

		switch (opcion){
		case 1:
			cout << "por favor ingrese la dimension del vector" << endl;
			cin >> dimension;
			cargarvector(vec, dimension);
			break;
		case 2:			
			if (dimension > 0){
				clasificarburbuja(vec, dimension);
				cout << "vector ordenado por salario" << endl;
			}
			else{
				cout << "por favor primero cargue el vector" << endl;
			}
			system("pause");
			break;
		case 3:
			cout << "por favor ingrese el codigo a buscar:" << endl;
			cin >> clave;
			b=busquedacodigo(vec, dimension, clave, pos);  
			if (b==true){
				cout << "\t existe usuario\n";
				cout << "Codigo: " << vec[pos].id << "\n";
				cout << "Nombre: " << vec[pos].nombre << "\n";
				cout << "Fecha Nacimiento: " << vec[pos].fechanac << "\n";
				cout << "SalarioBruto: " << vec[pos].sb << "\n";
			}
			else{
				cout << "\t no existe usuario\n";
			}
			system("pause");
			break;
		case 4:
			cout << "\t Planilla de Sueldos\n";
			cout << "======================\n\n";			
			
			listarplanilla(vec, dimension);
			cout << "Promedio: " << promedio(vec, dimension) << "\n";
			system("pause");
			break;
		
		case 10:		//CASO PARA SALIR DEL MENU PRINCIPAL
			opcion = 0;
			break;

		default:
			printf("\nOpcion Incorrecta!!\n");
			system("pause");
			break;
		}

	}
}

void menu(){
	system("cls");
	printf("Menu ");
	printf("\n 1.- Cargar vector de empleados");
	printf("\n 2.- Ordenar por salario");
	printf("\n 3.- Buscar Empleado");
	printf("\n 4.- Mostar sueldos");
	printf("\n 10.- Salir");
	printf("\n\n   OPCION #: ");
}

void cargarvector(t_empleado v[], int n)
{
	int i;
	for (i = 0; i < n; i++)
	{		
		system("cls");									//LIMPIA LA PANTALLA CADA NUEVO EMPLEADO 
		cout << "Ingrese Codigo: "; cin >> v[i].id;		// IMPRIME Y ESPERA EL INGRESO DE UN NUEVO CAMPO PARA LA ESTRUCTURA EMPLEADO
		cout << "Ingrese Nombre: "; cin >> v[i].nombre;
		cout << "Ingrese Fecha Nacimiento: "; cin >> v[i].fechanac;
		cout << "Ingrese SalarioBruto: "; cin >> v[i].sb;
	}

}

void clasificarburbuja(t_empleado v[], int n){// ESTE METODO ORDENA POR SALARIO BRUTO
	int i, j; 
	t_empleado temp;
	for (int i = 1; i < n; i++){
		for (int j = 0; j < n - 1; j++){
			if (v[j].sb > v[j + 1].sb){			//ESTA PARTE ORDENA EL VECTOR POR SB= SALARIO BRUTO 
				temp = v[j];					//EL ALGORITMO BURBUJA UTILIZA EL METODO DE INTERCAMBIO
				v[j] = v[j + 1];				//EN ESTE CASO INTERCAMBIA TODO LO QUE SE ENCUENTRA DENTRO DE LA ESTRUCTURA EMPLEADO
				v[j + 1] = temp;
			}
		}
	}
	mostrarVectorEmpleado(v, n);
}

void clasificarburbuja2(t_empleado v[], int n){// ESTE METODO ORDENA POR ID DE EMPLEADO
	int i, j;
	t_empleado temp;
	for (int i = 1; i < n; i++){
		for (int j = 0; j < n - 1; j++){
			if (v[j].id > v[j + 1].id){			//ESTA ES LA UNICA PARTE QUE CAMBIA
				temp = v[j];					
				v[j] = v[j + 1];				
				v[j + 1] = temp;
			}
		}
	}	
}

bool busquedacodigo(t_empleado v[], int n, int dato, int &pos){  //EL & INDICA QUE RETORNARA UN VALOR EN ESA VARIABLE AL FINALIZAR LA FUNCION
	int centro, inf = 0, sup = n - 1;
											
	//EL METODO BUSQUEDA BINARIO TIENE 2 REQUISITOS 
	//1) EL VECTOR DEBE TENER VALORES UNICOS(NO REPETIDOS)
	//2) EL VECTOR DEBE ESTAR ORDENADO ASENDENTEMENTE
	
	clasificarburbuja2(v, n);				//PRIMERO ORDENAMOS EL VECTOR POR ID; PORQUE SINO LA BUSQUEDA NO FUNCIONA
	while (inf <= sup){
		centro = (sup + inf) / 2;
		if (v[centro].id == dato){			//PREGUNTA POR EL ID DEL EMPLEADO
			pos = centro;					//SI ENCUENTRA ESE ID ENTONCES RETORNA ESA POSICION PARA MOSTRAR TODOS LOS DATOS DEL USUARIO
			return true;					//RETORNA VERDADERO Y CORTA EL CICLO
		}
		else {								// CASO CONTRARIO CREA UN NUEVO CENTRO A PARTIR DEL INMEDIATO INFERIOR O POSTERIOR DEL DATO
			if (dato < v[centro].id) 
				sup = centro - 1;		
			else
				inf = centro + 1;
		}
	}
	return false;
}

void mostrarVectorEmpleado(t_empleado v[], int dimension){
	cout << "Cod  Nombre  Fecha Nacimiento SalarioBruto\n";
	cout << "===========================================\n";
	for (int i = 0; i < dimension; i++){		
		cout << v[i].id << "  " << v[i].nombre << "  " << v[i].fechanac << "  " << v[i].sb << "\n";			// CONCATENAMOS PARA QUE TODO IMPRIMA EN UNA SOLA LINEA
	}
}

void listarplanilla(t_empleado v[], int n){
	cout << "Cod  Nombre  SalarioBruto Salario neto\n";
	cout << "===========================================\n";
	for (int i = 0; i < n; i++){		//LA FUNCION SALARIO NETO ES LLAMADA PARA TODOS LOS SALARIOS BRUTOS
		cout << v[i].id << "  " << v[i].nombre << "  " << v[i].sb << "  " << salarioneto(v[i].sb) << "\n";	// CONCATENAMOS PARA QUE TODO IMPRIMA EN UNA SOLA LINEA
	}
}

void mostrar(float vec1[], int dimension)
{
	int i;
	for (i = 0; i<dimension; i++)
		cout << vec1[i] << endl;
}

float salarioneto(float sb)
{	//FUNCION PARA OBTERNER SALARIO NETO; SI EL SALARIO BRURO ES < AL SALARIO MINIMO ENTONCES EL SALARIO NETO ES 0
	float iva = 0.13, afp = 0.121, p = 0, s;
	float salarioneto=0;
	int i, minimo = 1400;

	if (sb>minimo)
	{
		salarioneto = sb - sb * iva - sb * afp;

	}
	return salarioneto;

}

float promedio(t_empleado v[], int n){
	int promedio = 0;			// ES MUY RECOMENDABLE SIEMPRE INICIALIZAR LAS VARIABLES
	for (int i = 0; i < n; i++){
		promedio = promedio + v[i].sb;
	}
	return promedio / n;
}

