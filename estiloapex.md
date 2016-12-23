# Guía de estilo Apex #

<!-- MarkdownTOC depth=0 autolink=true autoanchor=true bracket=round -->

- [Introducción](#intro)
  - [Objetivos](#goals)
  - [Fuentes](#sources)
- [Consideraciones básicas](#basics)
  - [Caracteres especiales](#special-characters)
    - [Espacio en blanco](#whitespace)
    - [Secuencias de escape especiales](#special-escape-sequences)
    - [Otros caracteres No-ASCII](#other-non-ascii-characters)
- [Estructura](#structure)
  - [Sangrías](#indentation)
  - [Nuevas líneas y espacios](#new-lines-and-spaces)
  - [Llaves] (#braces)
  - [Preferimos declaraciones explícitas](#prefer-explicit-declarations)
  - [Capitalization](#capitalization)
  - [Example](#example)
  - [`@isTest`](#istest)  
  - [Test.startTest() and Test.stopTest()](#teststarttest-and-teststoptest)  
- [SOQL](#soql)
- [Apex-Specific SObject Constructor Syntax](#apex-specific-sobject-constructor-syntax)
- [Nomenclatura](#naming-conventions)
  - [Clases](#class)
  - [Trigger y Scheduler](#trigger-and-scheduler)
  - [Métodos](#methods)
  - [Clases de prueba](#test-classes)
- [Buenas prácticas](#best-practices)
  - [Se seco](#dry)
  - [No te tragues excepciones](#exceptions)
  - [Datos de prueba](#test-data)
  - [Enlaces](#links)

<!-- /MarkdownTOC -->

<a name="intro"></a>
## Introducción

<a name="goals"></a>
### Objetivos
El principal objetivo de esta guía es proporcionar una serie de reglas que seguir durante el desarrollo de código Apex.  Cada desarrollador puede tener una serie de ideas distintas sobre que hace el código mas legible o bonito, pero si el equipo sigue las mismas reglas conseguimos:

1. Que sea mas facil para todos los desarrolladores entender el código.
2. La mezcla de código se centre en la lógica y no en la estética.

Este documento está abierto a discusión y puede ser modificado.

<a name="sources"></a>
### Fuentes

La inspiración para este documento han sido las guías de estilo Java de [Google](http://google-styleguide.googlecode.com/svn/trunk/javaguide.html), [Twitter](https://github.com/twitter/commons/blob/master/src/java/com/twitter/common/styleguide.md) y la de [Estilo Apex de Polaris Project](https://github.com/PolarisProject/salesforceStyleGuide)

Son lectura recomendada para comprender algunas de las decisiones de este documento y los beneficios de mantenerlo:
-1. [Clean Code](https://books.google.es/books/about/Clean_Code.html?id=hjEFCAAAQBAJ)
-2. [Code Complete 2](https://books.google.es/books?id=LpVCAwAAQBAJ)


<a name="basics"></a>
## Consideraciones básicas
<a name="special-characters"></a>
### Special characters
<a name="whitespace"></a>
#### Espacio en blanco
Los únicos espacios en blanco permitidos son los de espacio (0x20) y tabulación horizontal (0x09).  Las lineas no han de terminar con espacios (`/ +$/` no debe encontrar nada en el archivo).  Todas las clases deberían terminar en una linea nueva.

<a name="special-escape-sequences"></a>
#### Secuencias de escape especiales
Para los caracteres que tienen secuencia de escape (`\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'` y `\\`), se usará ese código y no el octal (e.g. `\012`) ni el Unicode (e.g. `\u000a`).

<a name="other-non-ascii-characters"></a>
#### Otros caracteres No-ASCII
Para el resto de caracteres no-ASCII, se usará el caracter Unicode (p.ej. `∞`) o su secuencia de escape Unicode (p.ej. `\u221e`), dependiendo de cual haga el código mas facil de entender.

  > Consejo: Cuando se usan caracteres de escape Unicode un comentario explicativo suele venir bien.

<a name="structure"></a>
## Estructura

El orden de los miembros de una clase puede tener un gran efecto sobre la facilidad para entenderla, pero no hay una sola forma correcta de hacerlo. Distintas clases pueden ordenarse de manera distinta.

Lo importante es que el orden dentro de la clase sea lógico, un orden que el mantenedor pueda explicar si es necesario. Por ejemplo, no añadir los nuevos métodos al final de la clase, que daría un orden cronológico, pero sinjustificación lógica.

<a name="indentation"></a>
### Sangrías
La sangría (indentation) de todos los bloques de código será la tabulación. Siempre usaremos este caracter y no 2, ni 4 espacios en blanco, para mantener la consistencia en toda la base de código.

Las tabulaciones suelen traer mas problemas que beneficios, pero entendemos que en código Apex, con la limitación existente en el número de caracteres, son justificables.

<a name="new-lines-and-spaces"></a>
### Nuevas lineas y espacios

Las lineas deberan tener menos de cien columnas.

Usa el interlineado vertical como resulte apropiado. No dudes en separar los bloques de código.

Es preferible comentar en una linea independiente, no a continuación del código.

Las llaves de apertura deberían tener un espacio delante, no una nueva linea. 

Los `else`s y `else if`s no llevan una nueva linea delante. Tampoco los `catch`es ni los `while`s.

Los parentesis de los `if`, `while`, `do`, `catch`, etc., deberían estar precedidos y seguidos de un espacio.

En las definiciones de los métodos debería no haber espacio antes del parentesis de apertura y un espaico tras el de cierre.

En las llamadas y definiciones de métodos, no debería haber espacios entre el nombre y el parentesis de apertura.

Los operadores binarios (`+`, `||`, `=`, `>=`, ...) deberían llevar un espacio antes y otro después.  Los unitarios (`!`, `-`) deberían ir junto a sus parámetros.  Los dos puntos de un `for each` loop (e.g., `for (Contact cnt : contacts) {`) debería llevar un espacio antes y otro después.  Las comas no llevan espacio delante y llevan uno detrás.

La declaración de getters y setters sin lógica seguira el formato de una linea (`{ get; set; }`).

<a name="braces"></a>
### Llaves
Estilo egipcio, con la llave de apertura en la linea actual. [Más] (https://google.github.io/styleguide/javaguide.html#s4.1.2-blocks-k-r-style)

<a name="prefer-explicit-declarations"></a>
### Preferimos declaraciones explicitas
Especificar siempre:

* Modificadores `global`/`public`/`private` - preferir `private`, y si se puede, `static`.
* `with sharing`/`without sharing`.
* `this` al llamar a métodos locales dar valor a miembros/propiedades locales.


<a name="capitalization"></a>
### Mayúsculas y su uso

Seguimos el standard Java con dos excepciones.  Esto quiere decir que los `for`, `if`, etc. irán en minúsculas, las constantes serán `MAYUSCULAS_CON_GUION_BAJO`, las clases son `UpperCamelCase`, y los métodos, parámetros y variables locales son todas declaradas `lowerCamelCase`.

Las excepciones son:

Los métodos y clases nativos de Apex se referenciarán como en la documentacón oficial de Salesforce, a excepción del SObject, que escribiremos así a pesar de en la documentación aparecer también como sObject.

De cualquier manera, al referenciar un metadato de Salesforce (SObject, SObjectField, FieldSet, Action, Class, Page, etc.), utilizaremos las mayúsculas de su declaración, aún cuando no coincidan con las de esta guía.


<a name="example"></a>
### Ejemplo

```java
public class MyClass {

	private Contact internallyUsedContact { get; set; }

	public Integer calculatedInteger {
		get {
			return 5;
		}
		set {
			this.internallyUsedContact = [SELECT Id
										  FROM Contact
                                          WHERE Number_of_Peanuts__c > :value
										  LIMIT 1];
		}
	}

	private Id contactId {
		get;
		set {
			System.debug('Why do this?');
			this.contactId = value;
		}
	}

	public void foo(Integer bar) {
		if (bar == 3) {
			// Diane often asks when bar is 3.
			System.debug(this.debugCode(bar) + ' - hi there!');
			return;
		} else if (bar > 7) {
			List<Integer> wasteOfSpace = new List<Integer>();
			do {
				wasteOfSpace.add(this.calculatedInteger);
			} while (wasteOfSpace.size() < 5);
		} else {
			try {
				upsert v;
			} catch (Exception ex) {
				handleException(ex);
			}
		}

		for (Integer i : wasteOfSpace) {
			System.debug('Here\'s an integer! ' + i);
		}
	}

}
```

<a name="istest"></a>
### `@isTest`
En un método de prueba usar el atributo `@isTest` en vez de el modificador `testmethod`.

Los métodos de prueba deberían ir en su propia clase y nunca inlined en el código que prueban.  La nomenclatura de la clase de pruebas por defecto será <NombreDeLaClaseQuePrueba>Test.

<a name="teststarttest-and-teststoptest"></a>
## Test.startTest() and Test.stopTest()
Al escribir casos de prueba utilizar siempre `Test.startTest();` y `Test.stopTest();`. No utilices sangría para el código entre esas llamadas, pero separa estas y el bloque con espacio vertical.


<a name="soql"></a>
## SOQL

En general el SOQL se deberian declarar inline donde se utilice.  En los casos en los que sea necesario realizar declaración dinámica (FieldSets) se intentarán aplicar las mismas reglas.

Las palabras clave SOQL (`SELECT`, `WHERE`, `TODAY`, ...) deberían ir en `MAYUSCULAS`.  Los objetos, campos y variables deberían referenciarse de la forma en la que se han declarado.  Cada clausula de la sentencia SOQL debería ir en su propia linea para facilitar la identificación de cambios.  Esto quiere decir que cada `SELECT`, `FROM`, `WHERE`, `AND`, `OR`, `GROUP BY`, `HAVING`, `ROLL UP`, `ORDER BY`, etc., debería empezar una linea nueva, con las excepción del primer `SELECT`.  La linea debería empezar en la misma columna que su `SELECT` relevante.

La listas largas de campos deberían ordenarse lógicamente, partirse para ser conformes con la limitación del ancho de página y alinearse con el primer campo de la lista.  Seleccionar siempre `Id` como primer campo.

Ejemplo (en contexto):

```java
String typeToSelect = 'abcde';
List<Contact> cnts = [SELECT Id, FirstName, LastName, Phone, Email,
                             MailingCity, MailingState,
                             (SELECT Id, ActivityDate, Origin, Type,
                                     WhatId, What.Name, RecordTypeId
                              FROM ActivityHistories
                              WHERE Type = :typeToSelect)
                      FROM Contact
                      WHERE CreatedDate >= TODAY];
```

<a name="apex-specific-sobject-constructor-syntax"></a>
## Sintaxis específica del SObject

Cuando se crea un nuevo SObject, es preferible utilizar la sintaxis específica de Apex segun la cual todos los campos se pueden inicializar desde el constructor.  Al utilizar esta, cambiamos de linea para cada propiedad para hacer mas sencilla la comparación y versionado.

Ejemplo:

```java
Contact c = new Contact(RecordTypeId = CONTACT_RECORDTYPE_ID,
                        FirstName = firstName,
                        LastName = surname,
                        MailingCountry = DEFAULT_COUNTRY
                       );
```


<a name="naming-conventions"></a>
## Nomenclatura

<a name="class"></a>
### Clases
Nombra una clase por lo que hace.  

Un controlador tendrá Controller como final de su nombre.
Una extensión de controlador se llamará <sObjectDescripcion>ControllerExt.
Un Scheduler o Batch mostrarán esta información al final del nombre de la clase <descripcion>Batch o <descripcion>Scheduler.
Agrupa las funciones usadas en multiples sitios en clases de Utils (p.ej. LogUtils, StringUtils)


<a name="trigger-and-scheduler"></a>
### Trigger y Scheduler
Según la envergadura del proyecto, considerar utilizar frameworks para los Triggers y el Scheduler.

- [Framework Trigger] (https://code.google.com/archive/p/apex-trigger-architecture-framework/)
- [Framework Scheduler] (https://th3silverlining.com/2014/02/02/salesforce-universal-batch-scheduling-class/)

NUNCA utilizar mas de un Trigger por objeto y SIEMPRE sacar la lógica del Trigger como mínimo a un <sObjectTrigger>Handler.

<a name="methods"></a>
### Métodos
Los métodos siempre serán verbos.  Los getters y setters no tendrán efectos secundarios y empezarán por `get` o `set`.

<a name="test-classes"></a>
### Clases de prueba
Las clases de prueba se llamarán `ClaseAProbarTest`.  Si no es un test unitario y comprueba un caso mas grande, el nombre será `FuncionalidadAProbarTest`.

<a name="best-practices"></a>
## Buenas prácticas

<a name="dry"></a>
### Se seco
[DRY](http://wiki.c2.com/?DontRepeatYourself): Del acrónimo inglés para `No te repitas`. 

- Extrae constantes cuando sea necesario.
- Centraliza la lógica duplicada.

<a name="exceptions"></a>
### No te tragues excepciones
Un bloque catch vacio no suele ser buena idea.

<a name="test-data"></a>
### Datos de prueba
Genera tus propios datos de prueba, no te fies de datos preexistentes en la organización.

<a name="Enlaces"></a>
### Enlaces
Recomendaciones de Salesforce, a seguir estrictamente -> [Apex Code Best Practices](https://developer.salesforce.com/page/Apex_Code_Best_Practices)
Sobre todo:
- Bulkifica.
- No hagas consultas en bucles.
- No metas IDs a piñón.


