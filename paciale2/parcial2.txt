1)
=> 	* Solamente hay 1 lugar
	* Su recorrido va de la parada 1 hasta la parada n
	* Pasando por las paradas intermedias 2,…,n-1
    * Tenemos m pasajeros esperando
    * Para cada pasajero i sabemos en que parada se quiere subir si , y en que parada se va a bajar bi (con 1<= si <=bi <= n)
    * Queremos trasladar la mayor cantidad de pasajeros posibles
    * Obtener el numero maximo de pasajeros trasladable en un unico recorrido	

a) Criterio de selección: se va a seleccionar el pasajero que se baje antes, o sea el pasajero con menor bi

b) type pasajero = tuple
                    nombre = string
                    sube = Nat
                    baja = Nat			     	
                  end tuple

c) La idea del algoritmo es recorrer un set de pasajeros y seleccionar al que tenga la bajada mas pronta, luego eliminar del set a los pasajeros que se quieren subir mientras tengo al otro pasajero encima, de manera que para cuando se baje, los otros pasajeros no seran elegibles porque ya pase su parada.

d)
fun recorrido (p: set of pasajero, n: Nat)ret res: Nat
	var p_aux: set of pasajero
	p_aux := copy_set(p)
	res:= 0
	var pasajero: pasajero
	var parada_actual: Nat
	parada_actual := 1
	do (¬is_empty_set(p_aux)) then
		pasajero := pasajero_min(p_aux)
		parada_actual:= pasajero.baja
		res:= res + 1
		elim(p_aux, pasajero)
		elim_pasajeros_anteriores(p_aux, parada_actual-1)
	od
	destroy_set(p_aux)
end fun

fun pasajero_min(P: set of pasajero)ret p: pasajero
	var p_aux: set of pasajero
	p_aux:= copy_set(P)
	var pasajero: pasajero
	var m: Nat
	m:= infinito
	do (¬is_empty_set(p_aux)) then
		pasajero:= get(p_aux)
		if (pasajero.baja < m) then 
			m:= pasajero.bajada
			p:= pasajero
		fi
		elim(p_aux, pasajero)
	od
	destroy_set(p_aux)
end fun

proc elim_pasajeros_anteriores(in/out p: set of pasajero, in parada: Nat)
	var p_aux: set of pasajero
	p_aux:= copy_set(p)
	var pasajero: pasajero
	do (¬is_empty_set(p_aux)) then
		pasajero:= get(p_aux)
		if (pasajero.subida <= parada) then
			elim(p, pasajero)
		fi
		elim(p_aux, pasajero)
	od
end proc

----------------------------------------------------------------------------------------------------------------

2)
=>	* Se proponen n medidas
	* Donde cada medida i∈{1..n} producira una mejora economica de mi puntos con mi > 0
	* Tambien se analizo para cada una el nivel de daño ecologico di que producira, donde di > 0
	* El puntaje que tendra cada medida i esta dado por la relacion mi/di
	* Se debe determinar cual es el maximo puntaje obtenible eligiendo K medidas, con K < n, de manera tal que la suma total del daño ecologico no sea mayor a C.

a)
=> la funcion recursiva:
	* n representara la cantidad de medidas disponibles a tomar
	* k representara el numero actual de medidas que se puedan tomar
	* c representara el maximo daño ecologico que se pueda generar tomando k medidas de n 
	* La llamada recursiva nos retornara el maximo puntaje que se pueda generar tomando k medidas de n medidas disponibles, tal que la sumatoria del daño de las medidas tomadas sea menor a c.

b)La llamada principal que resuelve el proble: medidas(n,k,c)

c)
				-----
				| 0 	si 	i = 0 v k = 0 	//no hay medidas para elegir, no hay medidas para tomar 
				|
medidas(n,k,c)=< medidas(n-1, k-1, c)	si 	di > c ^ i > 0 ^ k > 0	//si hay medidas por tomar y para elegir y
				|													  no se puede tomar la medida
				|max(medidas(n-1, k-1, c), mi/di + medidas(n-1, k-1, c-di))	si 	di > c ^ i > 0 ^ k > 0 	/
				|															//elegimos si tomamos la medida o no
				------

----------------------------------------------------------------------------------------------------------------

3)
a)
fun gunthonacci(n: Nat)ret res: Nat
	var tabla: array[0...n, 0...n] of nat
	for i:= 0 to n do
		for j:= 0 n do
			if  (i == 0 ^ j == 0) then
				tabla[i, j]:= 1
			else if (i == 0 ^ j == 1) then
				tabla[i, j]:= 1
			else if (i == 1 ^ j == 0) then
				tabla[i, j]:= 1
			else if (i == 0 ^ j > 1) then
				tabla[i, j]:= tabla[i, j-2] + tabla[i, j-1]
			else if (i > 1 ^ j == 0) then
				tabla[i, j]:= tabla[i-2, j] + tabla[i-1, j]
			else if (i > 0 ^ j > 0) then
				tabla[i, j]:= tabla[i, j-1] + tabla[i-1, j]
			fi
		od
	od
	res:= tabla[n,n]
end fun

b)
La tabla de valores será de dimensión nxn y se llena desde arriba hacia abajo, y de izquierda a derecha ya que los casos bases se encuentran en la primera fila y la primera columna, siempre mirando los casos anteriores en las filas superiores o columnas anteriores. El valor de retorno está en tabla[n,n].

----------------------------------------------------------------------------------------------------------------

4)
a) Las distintas maneras de recorrer T son de forma pre-order, pos-order (DFS) y (BFS).
Elegiria el método pre-order porque comienza desde la raíz, luego al subárbol izquierdo y finalmente al subárbol derecho, de este modo puedo contar cuántos nodos visitó hasta llegar a una hoja y me quedo con el mínimo

b) ¿?