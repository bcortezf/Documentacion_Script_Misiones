# Documentación de Misiones
Esta guía buscará documentar todos los parámetros que una misión pueda tener.
De manera que tras comprender la lectura, puedas crear tus propias misiones.

## Índice
- [Documentación de NPC](#Documentación-de-NPC)
  - [Ejemplo de NPC](#Ejemplo-de-NPC)
- [Documentación de Misiones](#Documentación-de-Misiones)
  - [Índice](#Índice)
  - [Análisis de una misión](#Análisis-de-una-misión)
  - [Desglose de parámetros](#Desglose-de-parámetros)
  - [Tipos de Misiones](#Tipos-de-Misiones)
- [Links Utiles](#Links-Utiles)




# Documentación de NPC
Este apartado buscará entregar todos los parametros que un NPC pueda tener



### Ejemplo de NPC
<details> 
	<summary>Mostrar</summary>

```lua
["Jorge_Herido"] = { -- Identificador Interno del NPC
	-- Modelo del NPC. (https://forge.plebmasters.de/peds)
	npc = 'a_m_y_bevhills_01', 
	
	-- Nombre del NPC que se mostrará en el dialogo
	name = 'Joel', 

	-- (opcional) Subtitulo
	subtitle = 'Empleado de Rogers', 
	
	-- /coords de aparición del NPC
	coords = vec(-627.34, -1567.17, 25.00, 181.67), 
	
	-- Define si el ped estará reproduciendo un Scenario (https://bit.ly/3ui4V3N)
	scenario = "WORLD_HUMAN_STUPOR", 
	
	-- Parametros para arreglar la posición de la camara en caso de error
	camOffset = vector3(0.85, 0.0, 0.0), 
	
	-- Parametros para arreglar la rotacion de la camara en caso de error
	camRotation = vector3(-40.0, 0.0, 0.0),
	
	-- Distancia a la que debe estar el jugador para interactuar con el NPC
	interactionRange = 2.5, 
	
	-- Lista de mensajes que mostrará cuando el NPC no tenga ninguna misión disponible.
	noMissions = { "Agh.. estoy.. muriendo..", "Oh, mierda... *agh*" }, 
	
	-- Al interactuar con el NPC, abrira el dialogo especificado
	-- Esto hará que el NPC sea Informativo. (Ver Tipos de NPC más abajo)
	firstDialog = "greetings" 
	
	-- Aqui se definen los dialogos que el NPC tendrá.
	-- Este parametro es para los NPC Informativos y de Misiones.
	-- Ver "Explicacion de Dialogos" más abajo
	dialogs = { },
	
	-- Este parametro se utiliza solo cuando el NPC es Comerciante. No puede tener firstDialog ni dialogs.
	sell = { },
}
```
</details>

## Desglose de parámetros
- **`npc`**: Nombre del modelo del NPC. Puedes encontrar todos los NPC [en este sitio](https://wiki.rage.mp/index.php?title=Peds)
- **blipName**<sub><sup>(opcional)</sub></sup>: Nombre que aparecerá en el blip. Si no existe, será el mismo que `name`
- **`name`**: Nombre del NPC que se mostrará en el dialogo
- **`subtitle`**<sub><sup>(opcional)</sub></sup>: Subtitulo del NPC, generalmente es su ocupación o rol
- **`coords`**: `/coords` de aparición del NPC
- **`scenario`**<sub><sup>(opcional)</sub></sup>: Reproducirá un scenario en el NPC. [Ver Lista de Scenarios](#Links-Utiles)
- **`camOffset`**<sub><sup>(opcional)</sub></sup>: `vector3(x,y,z)`, Sirve para compensar la posicion de la cámara si esque está mal ajustada
- **`camRotation`**<sub><sup>(opcional)</sub></sup>: `vector3(x,y,z)`, Sirve para compensar la rotacion de la camara si esque está mal ajustada
- **`interactionRange`**<sub><sup>(opcional)</sub></sup>: Distancia a la que el jugador puede interactuar con el NPC, por defecto: 2.5
- **`noMissions`**<sub><sup>(opcional)</sub></sup>: Lista de textos que mostrará cuando el NPC no tenga ninguna misión.
- **`firstDialog`**<sub><sup>(opcional)</sub></sup>: Abrirá el dialogo especificado al interactuar con el NPC.

- <details><summary> 	
		
	#### `dialogs`	
	</summary>
	
	- <details><summary> Identificador_Unico </summary> 
	
		- **`dialog`**: Texto que se mostrará al abrir el dialogo. (Básicamente es lo que el NPC habla)
		- <details><summary> options </summary> 
	
			- **`text`**: Texto que se muestra en la opcion
			- **`type`**: Tipo de opción (Ver [Tipos de Opciones](#Tipos-de-opciones))
			- **`value`**: Valor del tipo especificado. (Si es dialog, ¿que dialogo?, si es mission, ¿que misión?)
			- **`type`**: Tipo de opción
		</details>
	</details>
</details>

	
- <details>
	<summary> 
	
	#### `sell` 
	</summary>
	
	- **`dialog`**: Dialogo que mostrará al abrir la interacción
	- <details><summary> items </summary>
	
	  - <details><summary> require: item que se requiere </summary>
	
	    - **`item`**: Puede ser `money`, `xp`, o el nombre del item
	    - **`weapon`**: Si es un arma, en lugar de `item`, se utiliza `weapon`
	    - **`amount`**: Cantidad a dar del item especificado
	    </details>
	  - <details><summary> rewards: item que se obtendrá </summary>

	    - **`item`**: Puede ser `money`, `xp`, o el nombre del item
	    - **`weapon`**: Si es un arma, en lugar de `item`, se utiliza `weapon`
	    - **`amount`**: Cantidad a dar del item especificado
	    </details>
	</details>
</details>



## Tipos de NPC
Hasta la fecha, existen 3 tipos de NPC.
- **Comerciantes**: Son NPCs que venderan, compraran o intercambiaran objetos con el jugador.
	- Un NPC es Comerciante cuando tiene el parametro `sell`
- **Informativos**: Son NPCs con dialogos y nada mas que eso. Sirve generalmente para informar sobre algo.
	- Un NPC es Informativo cuando tiene el parametro `firstDialog` y `dialogs`
- **Misiones**: Son NPCs que le darán misiones al jugador.
	- Un NPC es de Misiones cuando tiene el parametro `dialogs` y tiene misiones disponibles. Tambien puede utilizar el parametro `noMissions`

¿Se significa esto, finalmente?
Significa que si un NPC es Comerciante, no podrá utilizar parametros como `firstDialog` o `dialogs`.
Lo mismo para los otros tipos de NPC.

## Tipos de opciones
Este apartado busca informar los tipos de opciones que pueden existir en un dialogo.

- **`dialog`**: Desplegará el dialogo especificado en el parametro `value` (Se debe pasar el identificador interno del dialogo)
- **`mission`**: Comenzará la misión especificada en el parametro `value` (Se debe pasar el identificador interno de la misión)
- **`close`**: Cerrará el dialogo con el NPC
</details>

# Explicación Dialogos
Para comprender mejor la estructura del NPC, tomemos uno de los dialogos y analicemoslo.
<details>
	<summary>Mostrar ejemplo de diálogo</summary>

```lua
["Robbery_01"] = { 
	dialog = 'Eh, tu.. Tengo un vehiculo para tí.. ¿Puedes entregarlo en un garage? Te pagaré bien..',
	options = {
		{ text = 'Vale', type = 'dialog', value = 'more_info' },
		{ text = 'No, gracias', type = 'dialog', value = 'deny' },
		{ text = 'OPCION ESPECIAL ',
		  type = 'dialog', 
		  value = 'more_info',
	          requiredItems = {	{ item = "medikit", amount = 1, removeOnInteract = true}	}
		},
	},
},
```
</details>


Un `dialog` tiene:
- **`Identificador_Interno`**: Nombre único del dialogo. En este caso sería **`Robbery_01`**
- **`dialog`**: Texto que mostrará en el diálogo. (Lo que hablará el NPC) 
- **`options`**: Tipo de opciones que desplegará el diálogo. Cada opcion tiene 
  - **`text`**: El texto de la opción, Texto que verá el usuario.
  - **`type`**: Tipo de opción, puede ser:
    - **`dialog`**: Desplegará el dialogo especificado en el parametro **`value`** (Se debe pasar el **identificador interno** del dialogo)
    - **`mission`**: Comenzará la misión especificada en el parametro **`value`** (Se debe pasar el **identificador interno** de la misión)
    - **`close`**: Cerrará el dialogo con el NPC
  - **`value`**: Dependerá del parametro **`type`**.
  - **`requiredItems`**: Mostrará la opcion solo si cuentas con los items especificados. En el ejemplo especificado, se mostrará la opción **`OPCION ESPECIAL`** solo si el jugador tiene **1 Botiquín** en su inventario


Nota: Un dialogo puede tener muchas opciones, pero se verán limitadas por el espacio en pantalla. Lo máximo que he probado yo, son 6.




## Análisis de una misión
Para comprender la estructura de una misión, primero hay que ver un ejemplo detallado:


<details> 
	<summary> Mostrar Detalle de Misión</summary>
**Esta es una misión que spawnea un sultan3, el cual debe llevarse de un punto A a un punto B.**

```lua
{
	-- Nombre interno de la misión. Debe ser único y servira para enlazarla a una opción de dialogo (lo veremos más adelante)
	key = "Delivery_Thief_Vehicle_01",
	
	-- Mensaje que aparecerá al inicio de la misión. Generalmente notifica lo que el jugador debe hacer
	-- Ver Ejemplo (https://i.imgur.com/Le3AiMx.png)
	description = "Entrega el ~b~vehículo~s~ en el ~y~punto de entrega~s~ antes de que se acabe el tiempo.",

	-- Tipo de mision. Pueden ser: transport | waypoint
	type = "transport",

	-- Si el tipo de misión es transport, se agrega el parametro transport con sus parametros correspondientes
	-- En este caso, transport tiene estos 4
	transport = {
		vehicle = "sultan3", -- Modelo del vehiculo a spawnear
		from = vec(-1311.46, -600.84, 26.98, 85.72), -- /coords de spawn del vehiculo
		to = vec(826.27, -156.32, 26.75, 75.40),-- /coords de entrega del vehiculo
		timerOutsideVehicle = 60, -- Un contador opcional en Segundos que controla que el jugador no pase mucho tiempo fuera del vehiculo.
	},

	-- Tiempo límite en segundos para completar la misión. Si pasan los segundos especificados, la misión fallará.
	timer = 300,
	
	-- Cada misión necesita tener un NPCs asociado con el cual tomarás esa misión.
	npc = {
		name = "NPC_Robberies", -- Identificador_Unico del NPC asociado
		dialog = "Robbery_01" -- Identificador_Unico del dialogo que se abrirá para presentarte esta misión
	},
	-- Esto quiere decir, que para "tomar" esta misión, primero necesitas hablar con un NPC, ese NPC necesita un dialogo,
	-- y ese dialogo necesita una opción que al seleccionarla, te active la misión.
	-- (Lo veremos más adelante en la estructura del npc)

	-- Te dará los items especificados apenas comience la misión.
	-- En esta misión no tiene mucho sentido, pero en otras, se te podría dar un botiquín para ayudarte con la misión,
	-- un bate, o cualquier item que te ayude con la misión.
	rewardsWhenActive = {
		{ item = "medikit", amount = 1 },
	},

	-- Te dará los items especificados cuando completes la misión
	-- item puede ser <money> | <xp> | <o incluso el nombre de un item>
	rewards = {
		{ item = "water", value = "2" },
		{ item = "money", value = "10000" },
		{ item = "xp", value = "50" },
	},
},
```

</details>

## Desglose de parámetros
- **`key`**: Debe mantener un formato similar a los ejemplos. Y lo ideal es que mantenga el contexto de la mision, tanto del NPC que te da la mision, como de lo que se trata. Ejemplo: **Graveyard_Rob_Vehicle**: Este nombre indica que la misión se realiza en *Graveyard*, y se trata de un *robo de vehiculo*.
- **`description`**: Debe describir la misión de manera breve y concisa, que al leerse, se comprenda inmediatamente lo que se debe hacer. Ejemplo: **Roba el vehículo de Graveyard y llevalo al punto de entrega.**
- **`type`**: El tipo de misión. Por ahora solo hay 2. Al seleccionar un tipo, se debe agregar un parametro con el mismo nombre del tipo seleccionado. De esa manera se especifican las variables asociadas al tipo seleccionado.
- **`waypoint/transport`**: Como se mencionó anteriormente, se agrega un parametro y dentro de este, estarán definidas las distintas variables dependiendo del tipo de misión. (Ver [Tipos de Misiones](#Tipos-de-misionnes))

 

## Tipos de Misiones
Una misión tiene distintos tipos detallados a continuación:

	
<details><summary> transport </summary>

- **`vehicle`**: modelo del vehiculo a transportar
- **`from`**: `/coords` de aparición del vehículo
- **`to`**: `/coords` de entrega del vehículo
- **`timerOutsideVehicle`**: Cantidad de segundos que un jugador puede estar afuera de su vehiculo. Al llegar a 0, la misión fallará.
</details>

<details><summary> waypoint </summary>

- **`coords`**: `/coords` de destino
- **`msgAtArrival`**: Mensaje que aparecerá al llegar al destino
- **`timer`**: Tiempo límite en segundos para llegar al destino. Si llega a 0, termina la misión
- **`npc`**: Si este parametro existe, significa que el NPC especificado, te seguirá durante tu trayecto.
	-  **`name`**: Identificador_Unico del NPC
	- (Se agregarán más parametros)

</details>



# Links Utiles

### [Lista de Peds](https://forge.plebmasters.de/peds)
### [Lista de Objetos](https://forge.plebmasters.de/objects)
### [Lista de Scenarios](https://github.com/DurtyFree/gta-v-data-dumps/blob/master/scenariosCompact.json)


