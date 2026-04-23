¡Hola! Entiendo perfectamente el objetivo. Vamos a construir un sistema **CRUD (Create, Read, Update, Delete)** para gestionar empleados de "Burger King", utilizando **Flutter** y **Firebase**, pero bajo la arquitectura de orquestación de **Antigravity**.

Como "Software Architect", he diseñado esta práctica guiada para que los estudiantes no solo programen, sino que entiendan cómo los **Agentes** y **Roles** dentro de un flujo de trabajo automatizan la lógica de negocio.

---

## 1. Metodología Antigravity: Agentes y Flujo de Trabajo

En la metodología de **Antigravity**, dividimos la lógica en entidades autónomas. Para este CRUD, definiremos lo siguiente:

### Roles y Agentes
* **Agente de Persistencia (Firebase Agent):** Su rol es comunicarse exclusivamente con Firestore. Sus *skills* son: conectar, mapear datos y ejecutar transacciones.
* **Agente de Interfaz (UI Agent):** Su rol es la presentación. Sus *skills* son: validación de formularios, renderizado de listas y manejo de temas visuales.
* **Agente de Validación (Business Logic):** Verifica que los datos del empleado (edad, salario) sean coherentes antes de enviarlos a la nube.

---

## 2. Configuración Inicial y Estructura

### Carpeta del Proyecto
Ejecuta en tu terminal:
```bash
mkdir xflutterdiaz0562
cd xflutterdiaz0562
flutter create crudburgerking
cd crudburgerking
```

### Estructura de Carpetas (Clean Architecture)
```text
crudburgerking/
├── lib/
│   ├── agents/          # Lógica de Antigravity (Agentes y Roles)
│   ├── models/          # Modelo de Empleado
│   ├── services/        # Configuración de Firebase
│   ├── ui/              # Widgets y Pantallas (Colores BK: Rojo, Amarillo, Café)
│   └── main.dart        # Punto de entrada
├── pubspec.yaml
└── ...
```

---

## 3. Integración de Librerías (pubspec.yaml)

Para que el proyecto funcione, necesitamos el núcleo de Firebase y la base de datos Firestore. Modifica tu archivo `pubspec.yaml` en la sección de `dependencies`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  # Librerías necesarias para Firebase
  firebase_core: ^3.0.0
  cloud_firestore: ^5.0.0
  # Antigravity (Simulado mediante lógica de Agentes en Flutter)
  cupertino_icons: ^1.0.6
```
*Nota: Después de guardar, ejecuta `flutter pub get` en la terminal.*

---

## 4. Configuración de Base de Datos (Firebase Console)

1.  Ve a [Firebase Console](https://console.firebase.google.com/).
2.  Crea un proyecto llamado **BK-CRUD**.
3.  En el menú lateral, ve a **Firestore Database**.
4.  Haz clic en **Crear base de datos**.
5.  Crea una colección llamada `empleados`.
6.  Registra tu app (Android/iOS) siguiendo el asistente y descarga el archivo `google-services.json`.

---

## 5. Implementación del Código Funcional

### El Modelo: `lib/models/employee_model.dart`
```dart
class Employee {
  String id;
  String nombre;
  int edad;
  double salario;

  Employee({required this.id, required this.nombre, required this.edad, required this.salario});

  // Convertir de Firebase a Objeto
  factory Employee.fromFirestore(Map<String, dynamic> data, String id) {
    return Employee(
      id: id,
      nombre: data['nombre'] ?? '',
      edad: data['edad'] ?? 0,
      salario: (data['salario'] ?? 0).toDouble(),
    );
  }

  // Convertir de Objeto a Firebase
  Map<String, dynamic> toFirestore() {
    return {
      'nombre': nombre,
      'edad': edad,
      'salario': salario,
    };
  }
}
```

### El Agente de Persistencia: `lib/agents/firebase_agent.dart`
Este agente tiene el *skill* de CRUD.

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/employee_model.dart';

class FirebaseAgent {
  final CollectionReference _collection = 
      FirebaseFirestore.instance.collection('empleados');

  // Skill: Crear
  Future<void> createEmployee(String nombre, int edad, double salario) async {
    await _collection.add({'nombre': nombre, 'edad': edad, 'salario': salario});
  }

  // Skill: Leer (Stream)
  Stream<List<Employee>> get employeesStream {
    return _collection.snapshots().map((snapshot) {
      return snapshot.docs.map((doc) => 
          Employee.fromFirestore(doc.data() as Map<String, dynamic>, doc.id)).toList();
    });
  }

  // Skill: Actualizar
  Future<void> updateEmployee(String id, String nombre, int edad, double salario) async {
    await _collection.doc(id).update({'nombre': nombre, 'edad': edad, 'salario': salario});
  }

  // Skill: Borrar
  Future<void> deleteEmployee(String id) async {
    await _collection.doc(id).delete();
  }
}
```

### La Interfaz (UI): `lib/ui/home_page.dart`
Utilizaremos los colores de Burger King: **Rojo (#D62300)**, **Amarillo (#FFB100)** y **Café (#502D16)**.

```dart
import 'package:flutter/material.dart';
import '../agents/firebase_agent.dart';
import '../models/employee_model.dart';

class HomePage extends StatelessWidget {
  final FirebaseAgent _agent = FirebaseAgent();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('BK Empleados - Admin'),
        backgroundColor: Color(0xFFD62300), // Rojo BK
      ),
      body: StreamBuilder<List<Employee>>(
        stream: _agent.employeesStream,
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          final employees = snapshot.data!;
          return ListView.builder(
            itemCount: employees.length,
            itemBuilder: (context, index) {
              final emp = employees[index];
              return Card(
                color: Color(0xFFFFB100).withOpacity(0.1), // Fondo amarillento
                margin: EdgeInsets.all(8),
                child: ListTile(
                  title: Text(emp.nombre, style: TextStyle(fontWeight: FontWeight.bold, color: Color(0xFF502D16))),
                  subtitle: Text('Edad: ${emp.edad} | Salario: \$${emp.salario}'),
                  trailing: Row(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      IconButton(icon: Icon(Icons.edit, color: Colors.blue), onPressed: () => _showForm(context, emp)),
                      IconButton(icon: Icon(Icons.delete, color: Colors.red), onPressed: () => _agent.deleteEmployee(emp.id)),
                    ],
                  ),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Color(0xFF502D16), // Café BK
        child: Icon(Icons.add, color: Colors.white),
        onPressed: () => _showForm(context, null),
      ),
    );
  }

  // Formulario Dinámico (Skill del UI Agent)
  void _showForm(BuildContext context, Employee? employee) {
    final nameCtrl = TextEditingController(text: employee?.nombre ?? '');
    final ageCtrl = TextEditingController(text: employee?.edad.toString() ?? '');
    final salaryCtrl = TextEditingController(text: employee?.salario.toString() ?? '');

    showModalBottomSheet(
      context: context,
      isScrollControlled: true,
      builder: (_) => Padding(
        padding: EdgeInsets.only(bottom: MediaQuery.of(context).viewInsets.bottom, top: 20, left: 20, right: 20),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(controller: nameCtrl, decoration: InputDecoration(labelText: 'Nombre')),
            TextField(controller: ageCtrl, decoration: InputDecoration(labelText: 'Edad'), keyboardType: TextInputType.number),
            TextField(controller: salaryCtrl, decoration: InputDecoration(labelText: 'Salario'), keyboardType: TextInputType.number),
            SizedBox(height: 20),
            ElevatedButton(
              style: ElevatedButton.styleFrom(backgroundColor: Color(0xFFD62300)),
              onPressed: () {
                if (employee == null) {
                  _agent.createEmployee(nameCtrl.text, int.parse(ageCtrl.text), double.parse(salaryCtrl.text));
                } else {
                  _agent.updateEmployee(employee.id, nameCtrl.text, int.parse(ageCtrl.text), double.parse(salaryCtrl.text));
                }
                Navigator.pop(context);
              },
              child: Text(employee == null ? 'Crear' : 'Actualizar', style: TextStyle(color: Colors.white)),
            )
          ],
        ),
      ),
    );
  }
}
```

### Punto de Entrada: `lib/main.dart`
```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'ui/home_page.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(); // Inicializa el flujo de Firebase
  runApp(BurgerKingApp());
}

class BurgerKingApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'BK CRUD Empleados',
      theme: ThemeData(primarySwatch: Colors.red),
      home: HomePage(),
    );
  }
}
```

---

## 6. Resumen de Flujo de Trabajo (Practice Guide)

1.  **Agente UI** detecta un `onPressed` en el botón "Añadir".
2.  **Agente de Validación** (dentro del formulario) captura los campos `Nombre`, `Edad` y `Salario`.
3.  **Agente de Persistencia (Antigravity)** recibe la orden y la envía a **Firestore**.
4.  **Firestore** emite un cambio en el `Stream`.
5.  **Agente UI** reacciona automáticamente refrescando la lista en pantalla con los nuevos colores aplicados.
