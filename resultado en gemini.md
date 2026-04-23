¡Hola! Entiendo perfectamente el desafío. Vamos a construir el proyecto **PetcoCrud** con una arquitectura sólida, integrando Firebase y siguiendo tu estructura de carpetas específica.

Como desarrollador de software, he diseñado este plan de trabajo optimizado para estudiantes, utilizando un enfoque modular.

---

## 1. Estructura de Directorios y Preparación
Primero, organizamos el espacio de trabajo en tu sistema de archivos:

* **Carpeta raíz:** `xfluttercontreras0544`
* **Proyecto Flutter:** `crudpetco`

```bash
# Comandos para terminal
mkdir xfluttercontreras0544
cd xfluttercontreras0544
flutter create crudpetco
cd crudpetco
```

---

## 2. Configuración de Firebase y Dependencias
Para que el proyecto funcione, necesitamos comunicar Flutter con Google Firebase.

### Librerías Necesarias (pubspec.yaml)
Debes agregar estas dependencias en tu archivo `pubspec.yaml` bajo la sección `dependencies:`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^3.0.0      # Núcleo de Firebase
  cloud_firestore: ^5.0.0    # Base de datos NoSQL
  antigravity: ^1.0.0        # Inyección de dependencias / State Management
```

**Cómo modificarlo:**
1. Abre `pubspec.yaml`.
2. Identifica la línea `dependencies:`.
3. Pega las librerías alineadas correctamente (2 espacios).
4. Ejecuta `flutter pub get` en la terminal.

### Configuración en Consola Firebase
1. Ve a [Firebase Console](https://console.firebase.google.com/).
2. Crea un proyecto llamado **PetcoCrud**.
3. Registra tu app (Android/iOS) y descarga el archivo `google-services.json` (para Android, colócalo en `android/app/`).
4. En el menú lateral, ve a **Firestore Database**, dale a "Crear base de datos" y crea la colección llamada **animales**.

---

## 3. Metodología: Agentes, Roles y Flujo (Práctica Guiada)

Para fines educativos, dividiremos el código en "Agentes" de software (capas de responsabilidad):

| Agente | Rol | Skill (Habilidad) |
| :--- | :--- | :--- |
| **Data Agent** | Firebase Service | Comunicación directa con Firestore. |
| **Logic Agent** | Controller | Maneja el estado y la validación de datos. |
| **UI Agent** | View | Renderiza la interfaz con colores pastel rojo y azul. |

---

## 4. Implementación del Código (Funcional)

### Estructura de carpetas sugerida dentro de `lib/`:
```text
lib/
├── main.dart
├── services/
│   └── firebase_service.dart
├── controllers/
│   └── animal_controller.dart
└── widgets/
    └── home_page.dart
```

#### A. Inicialización (`main.dart`)
```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'widgets/home_page.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(); // Inicializa Firebase
  runApp(PetcoApp());
}

class PetcoApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Petco CRUD',
      theme: ThemeData(
        primaryColor: Color(0xFFFFB3BA), // Rojo Pastel
        scaffoldBackgroundColor: Color(0xFFBAE1FF), // Azul Pastel
      ),
      home: HomePage(),
    );
  }
}
```

#### B. Servicio de Datos (`services/firebase_service.dart`)
Este agente se encarga del **CRUD**.

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

class FirebaseService {
  final CollectionReference _db = FirebaseFirestore.instance.collection('animales');

  // CREATE
  Future<void> addAnimal(String especie, int edad, double precio) {
    return _db.add({
      'especie': especie,
      'edad_annos': edad,
      'precio': precio,
    });
  }

  // READ (Stream para cambios en tiempo real)
  Stream<QuerySnapshot> getAnimals() {
    return _db.snapshots();
  }

  // UPDATE
  Future<void> updateAnimal(String id, String especie, int edad, double precio) {
    return _db.doc(id).update({
      'especie': especie,
      'edad_annos': edad,
      'precio': precio,
    });
  }

  // DELETE
  Future<void> deleteAnimal(String id) {
    return _db.doc(id).delete();
  }
}
```

#### C. Interfaz de Usuario (`widgets/home_page.dart`)
Aquí aplicamos los colores pastel y la interacción.

```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';

class HomePage extends StatelessWidget {
  final FirebaseService _service = FirebaseService();
  final TextEditingController _especieCtrl = TextEditingController();
  final TextEditingController _edadCtrl = TextEditingController();
  final TextEditingController _precioCtrl = TextEditingController();

  void _showForm(BuildContext context, {String? id}) {
    showModalBottomSheet(
      context: context,
      backgroundColor: Color(0xFFBAE1FF),
      builder: (_) => Padding(
        padding: EdgeInsets.all(20),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(controller: _especieCtrl, decoration: InputDecoration(labelText: 'Especie')),
            TextField(controller: _edadCtrl, decoration: InputDecoration(labelText: 'Edad (años)'), keyboardType: TextInputType.number),
            TextField(controller: _precioCtrl, decoration: InputDecoration(labelText: 'Precio'), keyboardType: TextInputType.number),
            ElevatedButton(
              style: ElevatedButton.styleFrom(backgroundColor: Color(0xFFFFB3BA)),
              child: Text(id == null ? 'Crear' : 'Actualizar', style: TextStyle(color: Colors.black87)),
              onPressed: () async {
                if (id == null) {
                  await _service.addAnimal(_especieCtrl.text, int.parse(_edadCtrl.text), double.parse(_precioCtrl.text));
                } else {
                  await _service.updateAnimal(id, _especieCtrl.text, int.parse(_edadCtrl.text), double.parse(_precioCtrl.text));
                }
                Navigator.pop(context);
              },
            )
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Petco CRUD - Animales'), backgroundColor: Color(0xFFFFB3BA)),
      body: StreamBuilder(
        stream: _service.getAnimals(),
        builder: (context, AsyncSnapshot<dynamic> snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          return ListView.builder(
            itemCount: snapshot.data.docs.length,
            itemBuilder: (context, index) {
              var doc = snapshot.data.docs[index];
              return Card(
                color: Colors.white70,
                margin: EdgeInsets.all(8),
                child: ListTile(
                  title: Text(doc['especie'], style: TextStyle(fontWeight: FontWeight.bold)),
                  subtitle: Text('Edad: ${doc['edad_annos']} años - \$${doc['precio']}'),
                  trailing: Row(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      IconButton(icon: Icon(Icons.edit, color: Colors.blue), onPressed: () => _showForm(context, id: doc.id)),
                      IconButton(icon: Icon(Icons.delete, color: Colors.redAccent), onPressed: () => _service.deleteAnimal(doc.id)),
                    ],
                  ),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Color(0xFFFFB3BA),
        child: Icon(Icons.add),
        onPressed: () => _showForm(context),
      ),
    );
  }
}
```

---

## 5. Nota sobre Antigravity
**Antigravity** es una librería de utilidad para simplificar la navegación y el estado. En la práctica guiada para estudiantes, se recomienda usarla para "inyectar" el `FirebaseService` de modo que no tengas que crear una instancia nueva en cada pantalla, manteniendo el código limpio y profesional.

> **Tip para estudiantes:** Siempre verifiquen que las reglas de seguridad en la consola de Firebase permitan la lectura y escritura durante la fase de desarrollo (modo prueba).
