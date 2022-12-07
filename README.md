# Documentacion_Script_Misiones
Documentacion del script de misiones que estoy desarrollando para HispanoRP


## Documentación de Misiones
Esta guía buscará documentar todos los posibles parámetros que una misión puede tener..
Se recomienda revisar  **la documentacion de NPCs primero**.


#### Lista de parametros

|Parámetro padre| Parámetro hijo            | Tipo        |  | Description                | 
|:-| :--------| :-------    |:-----| :------------------------- | 
| `key`**(\*)** | | `texto`     | | Nombre interno de la misión. No puede repetirse. Ej: **Roggers_vehicle_delivery01** |
| `description`**(\*)** | | `texto`     | | Texto que mostrará al activarse la misión. ![Logo](https://i.imgur.com/adOIDNe.png) |
| `timer` | | `segundos`     | | Definirá el tiempo que durará la mision. Si se acaba, fallará. |
| `type`**(\*)** | | `texto`     | | Tipo de misión. |
|
| `rewardsWhenActive`**(\*)** |  |   | | Recompensas que dará al activarse la mision  |
|  | `item` | `nombre_de_item` \| `xp` \| `money`     | | Nombre Interno de Item (ej. **water**)  |
|                            | `amount` | `numero`      | | Monto a dar del **item** especificado|
|
| `transport` |`vehicle`| `vehicleModel`|  |  Vehículo a transportar |  
|             |`from` | `vec(x,y,z,h)`  |  |  Punto de aparición del vehículo |  
|             |`to`   | `vec(x,y,z,h)` |  |  Vehículo a transportar |  
|             | `timerOutsideVehicle`| `seconds`|  |  Segundos que el jugador podrá abandonar el vehiculo. La misión fallará si llega a 0 |  
|  | | | | |
| `waypoint`  |`coords`| `vehicleModel`|  |  Vehículo a transportar |  
|             |`msgAtArrival` | `vec(x,y,z,h)`  |  |  Punto de aparición del vehículo |
|             |`completedAtArrival`|`true/false`|| |La misión se completa automaticamente al llegar al punto de destino  
|             |`enemys`   | `PENDIENTE` |  |  Enemigos que aparecerán en la zona de destino |  
|  | | | | |
| `npc`  || |  | | NPC el cual que te dará esta misión. De esta manera el sistema detectará si tienes misión cuando hablas con el NPC |  
|        |`name`| |  |  Nombre interno del NPC (**`key`**) |  
|        |`dialog`| |   | Dialogo que abrirá para presentarte la misión (debe estar definido en los dialogos del mismo npc) |  


## Ejemplo de una misión
### Mision #1
Misión que trata de llevar un **`sultan3`** del puerto de Los Santos a un Garage en la ciudad.

#### Detalle de Mision:
```lua
{
	key = "Delivery_Thief_Vehicle_01",
	description = "Entrega el ~b~vehículo~s~ en el ~y~punto de entrega~s~ antes de que se acabe el tiempo.",
	type = "transport",
	transport = {
		vehicle = "sultan3",
		from = vec(-1311.46, -600.84, 26.98, 85.72),
		to = vec(826.27, -156.32, 26.75, 75.40),
		timerOutsideVehicle = 60, 
	},
	timer = 300,
	npc = {
		name = "NPC_Robberies",
		dialog = "Robbery_01"
	},
	completedAtArrive = true,
	rewardsWhenActive = {
		{ item = "medikit", amount = 1 },
	},
	rewards = {
		{ item = "money", value = "10000" },
		{ item = "xp", value = "50" },
	},
},
```


#### Detalle de NPC:



```lua
["NPC_Robberies"] = {
	npc = 'mp_m_waremech_01',
	name = 'Mike',
	subtitle = 'Empleado de Rogers',
	coords = vec(-612.23, -1609.33, 26.90, 350.10),
	scenario = "WORLD_HUMAN_CLIPBOARD_FACILITY", -- https://bit.ly/3ui4V3N
	camOffset = vector3(0.0, 0.0, 0.0),
	camRotation = vector3(0.0, 0.0, 0.0),
	interactionRange = 2.5, -- Distancia de interaccion
	noMissions = { "¿Buscas algo? Pues no encontrarás nada..", "No tengo nada para tí, vete.", "¿Hmm? Fuera de aquí." },
	dialogs = { 
		["Robbery_01"] = {
			dialog = 'Eh, tu.. Tengo un vehiculo para tí.. ¿Puedes entregarlo en un garage? Te pagaré bien..',
			options = {
				{ text = 'Vale', type = 'dialog', value = 'more_info' },
				{ text = 'No, gracias', type = 'dialog', value = 'deny' },
			},
		},
		["deny"] = {
			dialog = 'Entonces no vuelvas.',
			options = {
				{ text = 'Cerrar', type = 'close' },
			},
		},
		["Robbery_01_1"] = {
			dialog = 'Bien, tengo un Sultán de exportación que debo entregar lo antes posible a un cliente. No lo dañes! O no te pagaré nada',
			options = {
				{ text = 'Bien, vamos allá.', type = 'mission', value = 'Delivery_Thief_Vehicle_01' },
				{ text = 'Olvídalo', type = 'dialog', value = 'deny' }
			},

		},

		["Delivery_Thief_Vehicle_01"] = {
			dialog = "¿Que mierda haces? Ve a entregar el puto vehículo",
			options = {
				{ text = 'Vale, vale.. Estoy en ello', type = 'close' },
				{ text = 'Tengo problemas.. ¿Podrías repetirme las instrucciones?', type = 'mission', value = "Delivery_Thief_Vehicle_01" },
			},
		},
		},

	},
	jobs = { -- Jobs que pueden interactuar con el npc

	},
},
```

# Explicación NPC
Para comprender mejor la estructura del NPC, tomemos uno de los dialogos y analicemoslo.

```lua
["Robbery_01"] = { 
	dialog = 'Eh, tu.. Tengo un vehiculo para tí.. ¿Puedes entregarlo en un garage? Te pagaré bien..',
	options = {
		{ text = 'Vale', type = 'dialog', value = 'more_info' },
		{ text = 'No, gracias', type = 'dialog', value = 'deny' },
		{ text = 'OPCION ESPECIAL ', type = 'dialog', value = 'more_info', requiredItems = { { item = "medikit", amount = 1, removeOnInteract = true }	} },
	},
},
```

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

