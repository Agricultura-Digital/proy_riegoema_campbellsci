# Sistema de Control de Riego para Estación Agrometeorológica.

## Resumen Ejecutivo.
El presente informe ofrece una descripción del diseño y la implementación de un sistema de control de riego para ser incorporado en estaciones de la Red Agrometeorológica del Instituto de Investigaciones Agropecuarias (INIA) de Chile. Este sistema, ha sido integrado con los modelos de datalogger Campbell Sci CR300 y CR1000, además se utiliza un relé e inversor para adecuar la señal de control y suministrar el voltaje necesario a una válvula solenoide de riego. El sistema desarrollado permitirá, en las estaciones que se implemente, tener pasto alrededor de la estación durante todas las epocas del año, cumpliendo con el estándar para una estación de referencia.

## Introducción.
La mayoría de los modelos que se implementan a partir de los datos generados por una estación agrometeorológica, sobre todo los asociados a recomendaciones de riego, han sido calibrados, utilizando una estación de referencia, esto es, una estación en donde sus instrumentos son instalados a una altura aproximada a los 2 metros y la estación se encuentra emplazada sobre una superficie de pasto verde denso, como alfalfa, con una altura aproximada de 12 cm. En el caso de la Red INIA, poder contar con estaciones en referencia permitirá hacer estimaciones certeras de parámetros agrometeorológicos como la evapotranspiración.
Para poder llevar a cabo lo anterior es necesario, poder desarrollar un sistema que suministre riego durante las épocas del año, por este motivo y con el propósito de ahorrar costos, se ha ideado un sistema automático de control de válvula a partir de los datalogger utilizados en la estaciones de la Red INIA, esto permitirá implementar riegos periódicos, controlado centralmente.

## Descripción del Sistema.
El sistema de control de riego ha sido probado en laboratorio con los modelos de datalogger Campbell Sci CR300 y CR1000, estas unidades son actualmente los dispositivos de adquisición de datos utilizados por la Red INIA a nivel nacional.
El datalogger puede realizar el control a través de cualquier salida digital configurada para tales efectos, para las pruebas se utilizó la salida C1 del datalogger. Específicamente esta salida se utiliza para enviar una señal de control a un relé, que activa o desactiva la salida de un inversor de 24 voltios. El inversor se conecta directamente a la batería de 12 voltios que alimenta el datalogger. A su vez a la salida del inversor suministra el voltaje necesario a una válvula solenoide de riego. Esta configuración permite una gestión precisa del riego, adaptada a las necesidades específicas del cultivo de referencia, sobre todo, si en el control de la válvula de riego se incorporan criterios de humedades de suelos, cuyas lecturas pueden ser incorporadas.


## Componentes del Sistema.
1.	Electroválvula: https://mcielectronics.cl/shop/product/valvula-solenoide-25571/?srsltid=AfmBOopd-atXxPwGK7bXyIg65EwA0tXOQnndUbwleqOvvfPGXEq0hgQY
2.	
3.	Relé: https://www.yoinstalo.cl/producto/rele-solido/


## Ejemplos de codificación.

```basic
'CR1000X Series Datalogger
'The datalogger type listed on line 1 determines the default instruction set,
'compiler, and help files used for a program that uses the .DLD or .CRB program
'extension. These options can also be set using the Set Datalogger Type dialog box
'(CRBasic Editor|Tools|Set Datalogger Type).

'Date: 12/04/2024
'Program author: Ruben E. Ruiz.

'Declare Constants
'Example: Test basic control valve
'CONST PI = 3.141592654

'Declare Public Variables
'Example:
Public PTemp, Batt_volt
Public onIrrigation As Boolean

'Declare Private Variables
'Example:
'Dim Counter

'Define Data Tables
DataTable (Test,1,-1) 'Set table size to # of records, or -1 to autoallocate.
	DataInterval (0,15,Sec,10)
	Minimum (1,batt_volt,FP2,False,False)
	Sample (1,PTemp,FP2)
EndTable

'Define Subroutines
'Sub
	'EnterSub instructions here
'EndSub

'Main Program
BeginProg
  onIrrigation = False
	Scan (5,Sec,0,0)
		PanelTemp (PTemp,15000)
		Battery (Batt_volt)
		'Enter other measurement instructions
		'Call Output Tables
		'Example:
		CallTable Test
	
    'Control valve
		PortSet(C1,onIrrigation)
		'On/off Irrigation
    onIrrigation = NOT onIrrigation
    
	NextScan
EndProg
```
