# Agenda de turnos 馃摉

## Objetivos 馃搵
Desarrollar un sistema, que permita la administraci贸n general de un consultorio (de cara a los administradores): Prestaciones, Profesionales, Pacientes, etc., como as铆 tambi茅n, permitir a los pacientes, realizar reserva sobre turnos ofrecidos.
Utilizar Visual Studio 2019 preferentemente y crear una aplicaci贸n utilizando ASP.NET MVC Core (versi贸n a definir por el docente 3.1 o 6.0).

<hr />

## Enunciado 馃摙
La idea principal de este trabajo pr谩ctico, es que Uds. se comporten como un equipo de desarrollo.
Este documento, les acerca, un equivalente al resultado de una primera entrevista entre el cliente y alguien del equipo, el cual relev贸 e identific贸 la informaci贸n aqu铆 contenida. 
A partir de este momento, deber谩n comprender lo que se est谩 requiriendo y construir dicha aplicaci贸n, 

Deben recopilar todas las dudas que tengan y evacuarlas con su nexo (el docente) de cara al cliente. De esta manera, 茅l nos ayudar谩 a conseguir la informaci贸n ya un poco m谩s procesada. 
Es importante destacar, que este proceso, no debe esperar a ser en clase; es importante, que junten algunas consultas, sea de 铆ndole funcional o t茅cnicas, en lugar de cada consulta enviarla de forma independiente.

Las consultas que sean realizadas por correo deben seguir el siguiente formato:

Subject: [NT1-<CURSO LETRA>-GRP-<GRUPO NUMERO>] <Proyecto XXX> | Informativo o Consulta

Body: 

1.<xxxxxxxx>

2.< xxxxxxxx>


# Ejemplo
**Subject:** [NT1-A-GRP-5] Agenda de Turnos | Consulta

**Body:**

1.La relaci贸n del paciente con Turno es 1:1 o 1:N?

2.Est谩 bien que encaremos la validaci贸n del turno activo, con una propiedad booleana en el Turno?

<hr />

### Proceso de ejecuci贸n en alto nivel 鈽戯笍
 - Crear un nuevo proyecto en [visual studio](https://visualstudio.microsoft.com/en/vs/).
 - Adicionar todos los modelos dentro de la carpeta Models cada uno en un archivo separado.
 - Especificar todas las restricciones y validaciones solicitadas a cada una de las entidades. [DataAnnotations](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations?view=netcore-3.1).
 - Crear las relaciones entre las entidades
 - Crear una carpeta Data que dentro tendr谩 al menos la clase que representar谩 el contexto de la base de datos DbContext. 
 - Crear el DbContext utilizando base de datos en memoria (con fines de testing inicial). [DbContext](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext?view=efcore-3.1), [Database In-Memory](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/?tabs=vs).
 - Agregar los DbSet para cada una de las entidades en el DbContext.
 - Crear el Scaffolding para permitir los CRUD de las entidades al menos solicitadas en el enunciado.
 - Aplicar las adecuaciones y validaciones necesarias en los controladores.  
 - Realizar un sistema de login con al menos los roles equivalentes a <Usuario Cliente> y <Usuario Administrador> (o con permisos elevados).
 - Si el proyecto lo requiere, generar el proceso de registraci贸n. 
 - Un administrador podr谩 realizar todas tareas que impliquen interacci贸n del lado del negocio (ABM "Alta-Baja-Modificaci贸n" de las entidades del sistema y configuraciones en caso de ser necesarias).
 - El <Usuario Cliente> s贸lo podr谩 tomar acci贸n en el sistema, en base al rol que tiene.
 - Realizar todos los ajustes necesarios en los modelos y/o funcionalidades.
 - Realizar los ajustes requeridos del lado de los permisos.
 - Todo lo referido a la presentaci贸n de la aplicai贸n (cuestiones visuales).
 
<hr />

## Entidades 馃搫

- Usuario
- Paciente
- Profesional
- Turno
- Prestacion
- Formulario

`Importante: Todas las entidades deben tener su identificador unico. Id o <ClassNameId>`

`
Las propiedades descriptas a continuaci贸n, son las minimas que deben tener las entidades. Uds. pueden agregar las que consideren necesarias.
De la misma manera Uds. deben definir los tipos de datos asociados a cada una de ellas, como as铆 tambi茅n las restricciones.
`

**Usuario**
```
- Nombre
- Email
- FechaAlta
- Password
```

**Paciente**
```
- Nombre
- Apellido
- DNI
- Telefono
- Direccion
- FechaAlta
- Email
- ObraSocial
- Turnos
```

**Profesional**
```
- Nombre
- Apellido
- Telefono
- Direccion
- FechaAlta
- Email
- Matricula
- Prestacion
- HoraInicio
- HoraFin
- Turnos
```

**Turno**
```
- Fecha
- Confirmado
- Activo
- FechaAlta
- Paciente
- Profesional
- DescripcionCancelacion
```


**Prestaci贸n**
```
- Nombre
- Descripcion
- Duracion
- Precio
- Profesionales
```

**Formulario**
```
- Fecha
- Email
- Nombre
- Apellido
- Leido
- Titulo
- Mensaje
- Usuario
```

**NOTA:** aqu铆 un link para refrescar el uso de los [Data annotations](https://www.c-sharpcorner.com/UploadFile/af66b7/data-annotations-for-mvc/).

<hr />

## Caracteristicas y Funcionalidades 鈱笍
`Todas las entidades, deben tener implementado su correspondiente ABM, a menos que sea implicito el no tener que soportar alguna de estas acciones.`

**Usuario**
- Los pacientes pueden auto registrarse.
- La autoregistraci贸n desde el sitio, es exclusiva pra los pacientes. Por lo cual, se le asignar谩 dicho rol.
- Los profesionales, deben ser agregados por un Administrador.
	- Al momento, del alta del profesional, se le definir谩 un username y password.
    - Tambi茅n se le asignar谩 a estas cuentas el rol de Profesional.

**Paciente**
- Un paciente puede sacar un turno Online
    - El proceso ser谩 en modo Wizard.
        - Selecciona la prestaci贸n
        - Selecciona un profesional que tenga dicha prestaci贸n
        - Seleccionar谩 un turno disponible dentro de la oferta.
            - La oferta estar谩 restringida desde el momento de la consulta hasta 7 dias posteriores.
            - No debe incluir desde luego turnos tomados.
            - Debe ser en base a la oferta del profesional seleccionado. Turnos no tomados y dentro del horario del profesional.
            - El paciente, solo puede tener un turno activo.
        - El paciente, podr谩 en todo momento, ver si tiene o no un turno solicitado.
            - Ver谩 el estado, si est谩 o no confirmado
            - Podr谩 cancelarlo, solo si es hasta 24hs. antes.
        - El paciente puede llenar un formulario de contacto, para enviar una consulta.
            - El formulario, puede ser enviado de forma anonima o no. Si el paciente est谩 logueado, cargar谩 automaticamente los datos de este. 
- Puede actualizar datos de contacto, como el telefono, direcci贸n, Obra Social. Pero no puede modificar su DNI, Nombre, Apellido, etc.

**Profesional**
- El profesional puede listar los turnos que tiene asignado en el futuro, para atender.
- El profesional, puede confirmar sus turnos. 
- Tambi茅n, puede ver un balance de los turnos que ya realiz贸 en el mes calendario. 
    - Visualizar谩 en este el valor que deber谩 percibir a fin de mes x valor hora.

**Administrador**
- Un Administrador, puede confirmar los turnos de cualquier profesional.
- Un Administrador, puede cancelar un turno en cualquier momento, agregando una descripci贸n del motivo.
- Puede cargar las prestaciones que brinda el centro.
- Dar de alta a profesionales, con su horario de atenci贸n, y todos los datos requeridos.

**Turno**
- El turno al crearse, debe estar en estado sin confirmar, y algun administrador debe confirmar el mismo.
- No se pueden superponer dos turnos del mismo profesional.

**Aplicaci贸n General**
- Informaci贸n institucional.
- Se deben mostrar las Prestaciones ofrecidas, con una descripci贸n de la misma, costos, duraci贸n tipica de la prestaci贸n, etc. 
- Se deben listar los profesionales que brindan su atenci贸n.
- Nadie, puede eliminar los pacientes del sistema. Esto debe estar restringido.
- Los accesos a las funcionalidades y/o capacidades, debe estar basada en los roles que tenga cada individuo.

`
Nota: El centro tiene consultorios ilimitados y cada consultorio est谩 preparado para soportar cualquier prestaci贸n, por lo cual esto no implica restricciones.
`
