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
1.	Electroválvula: El riego se activará mediante una electroválvula del fabricante por Hunter. Para el sistema se ha utilizado una de corriente continua que requiere un voltaje mínimo de apertura y operación de 6 VDC, y un voltaje máximo recomendado de 9 VDC para su correcto funcionamiento. Posee una resistencia nominal de la bobina de 4.8 ohmios. La presión máxima de operación que puede soportar es de 13.79 bar (1379 kPa). La electroválvula está equipada con cables de 45 cm de longitud, en colores negro y rojo, con un grosor de 0.8 mm² cada uno. La mayoría de las electroválvulas disponibles en el mercado funcionan con un voltaje estándar de 24 voltios corriente alterna, que no es compatible con el sistema de energía de las estaciones INIA, por lo que debe asegurarse tener la válvula solenoide correcta.
2.	Relé: El relé actuará como un interruptor controlado por el registrador CR1000 para suministrar el voltaje adecuado a la electroválvula. Se seleccionará un relé que pueda manejar un voltaje de 9 voltios en corriente continua, y que pueda ser controlado por un voltaje de 5 volt y con todas las características necesarias para que sea compatible con una señal digital emitida por el datalogger. Para este propósito se utilizó el relé de estado sólido SSR-100DD fabricado por NCElec. Este modelo de estado sólido opera bajo un esquema de control DC-DC (corriente continua – continua), con un voltaje de funcionamiento que varía entre 5 y 60 V CC y un voltaje de control de 3 a 32 V CC. Para aplicaciones que requieran de un alto consumo de energía, lo que nos es nuestro caso, se debe suministrar un sistema de enfriamiento con un radiador o ventiladores de ser necesario. 
3.	Registrador CR1000X Campbell Scientific: Este es el componente central del sistema de control y adquisición de datos. El registrador CR1000 debe ser capaz de generar señales de control y suministrar energía al relé, para controlar la electroválvula. Todo esto en paralelo con la adquisición, registro y transmisión de los datos de la estación. Para esto se debe agregar las líneas de código al programa del CR1000 que permitan activar o desactivar una salida digital de 5 volts, en base a criterios relacionados con el riego. Para esto se puede realizar una estimación básica de evapotranspiración e incorporar un sensor de humedad de suelo.
4.	Fuente de alimentación: Se requiere incorporar una fuente de alimentación que proporcione un voltaje de 12 voltios para el registrador CR1000. Asegurando que la fuente de alimentación tenga la capacidad de suministrar la corriente necesaria para las funciones estándar de las estaciones INIA y la nueva función encargada de controlar el riego.
5.	Convertidor de voltaje: Actualmente una estación estándar INIA, funciona con tensiones de 12 volt, esto permite alimentar el registrador, varios sensores y el modem celular encargado de la trasmisión. Pero la válvula seleccionada soporta un voltaje de corriente continua de 9 volt como máximo. Para estos efectos se utilizará el convertidor DC 24V/12V a 9V 3A 27W que regula un voltaje de entrada de 12V a 32V y proporciona una salida de 9V con una corriente de 3A. Tiene una eficiencia de conversión de hasta el 95% y cuenta con protecciones contra inversión de entrada y sobrecarga.
6.	Sensor de humedad del suelo (opcional): Se puede incorporar un sensor de humedad de suelo que permita activar el sistema de riego automáticamente cuando el suelo esté seco. Como sensor de humedad de suelo para esta aplicación se utilizará El TEROS 10 es un sensor de humedad del suelo fabricado por METER, caracterizado por su robustez y durabilidad. Este sensor opera a una frecuencia de 70 MHz, lo que minimiza los efectos de la salinidad y las texturas del suelo, proporcionando mediciones precisas. Está construido con un cuerpo de epoxi que puede soportar condiciones ambientales adversas, y está diseñado para ser confiable y eficaz en la recopilación de datos por hasta 10 años en una variedad de suelos, desde áridos hasta muy húmedos.

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