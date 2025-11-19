---
name: mobile-technologies
description: Build native and cross-platform mobile applications using React Native, Flutter, Swift, and Kotlin. Master mobile UI patterns, platform-specific APIs, and app distribution.
---

# Mobile Technologies Skill

## Quick Start

Mobile development creates applications for iOS, Android, and cross-platform solutions. Here's how to get started:

### React Native Cross-Platform
```javascript
import React, { useState } from 'react';
import { View, Text, TextInput, TouchableOpacity, StyleSheet } from 'react-native';

export default function App() {
  const [name, setName] = useState('');
  const [users, setUsers] = useState([]);

  const addUser = () => {
    if (name.trim()) {
      setUsers([...users, { id: Date.now(), name }]);
      setName('');
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>User List</Text>
      <TextInput
        style={styles.input}
        placeholder="Enter name"
        value={name}
        onChangeText={setName}
      />
      <TouchableOpacity style={styles.button} onPress={addUser}>
        <Text style={styles.buttonText}>Add User</Text>
      </TouchableOpacity>
      {users.map(user => (
        <Text key={user.id} style={styles.item}>{user.name}</Text>
      ))}
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20, backgroundColor: '#fff' },
  title: { fontSize: 24, fontWeight: 'bold', marginBottom: 20 },
  input: { borderWidth: 1, borderColor: '#ccc', padding: 10, marginBottom: 10 },
  button: { backgroundColor: '#007AFF', padding: 10, borderRadius: 5 },
  buttonText: { color: '#fff', textAlign: 'center', fontWeight: 'bold' },
  item: { fontSize: 16, padding: 10, borderBottomWidth: 1, borderBottomColor: '#eee' }
});
```

### Flutter with Dart
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'User List',
      home: UserListScreen(),
    );
  }
}

class UserListScreen extends StatefulWidget {
  @override
  State<UserListScreen> createState() => _UserListScreenState();
}

class _UserListScreenState extends State<UserListScreen> {
  final List<String> users = [];
  final TextEditingController _controller = TextEditingController();

  void _addUser() {
    if (_controller.text.isNotEmpty) {
      setState(() {
        users.add(_controller.text);
        _controller.clear();
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('User List')),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: TextField(
              controller: _controller,
              decoration: InputDecoration(
                hintText: 'Enter name',
                border: OutlineInputBorder(),
              ),
            ),
          ),
          ElevatedButton(
            onPressed: _addUser,
            child: const Text('Add User'),
          ),
          Expanded(
            child: ListView.builder(
              itemCount: users.length,
              itemBuilder: (context, index) {
                return ListTile(title: Text(users[index]));
              },
            ),
          ),
        ],
      ),
    );
  }
}
```

### Swift/SwiftUI (iOS Native)
```swift
import SwiftUI

struct ContentView: View {
    @State private var name = ""
    @State private var users: [String] = []

    var body: some View {
        NavigationStack {
            VStack {
                TextField("Enter name", text: $name)
                    .textFieldStyle(.roundedBorder)
                    .padding()

                Button("Add User") {
                    if !name.isEmpty {
                        users.append(name)
                        name = ""
                    }
                }
                .buttonStyle(.borderedProminent)

                List {
                    ForEach(users, id: \.self) { user in
                        Text(user)
                    }
                }
            }
            .navigationTitle("User List")
        }
    }
}

#Preview {
    ContentView()
}
```

### Kotlin for Android
```kotlin
class MainActivity : AppCompatActivity() {
    private val users = mutableListOf<String>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val editText: EditText = findViewById(R.id.editText)
        val button: Button = findViewById(R.id.addButton)
        val listView: ListView = findViewById(R.id.userList)

        button.setOnClickListener {
            val name = editText.text.toString()
            if (name.isNotEmpty()) {
                users.add(name)
                listView.adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, users)
                editText.text.clear()
            }
        }
    }
}
```

## Key Areas

### 1. React Native
- Component architecture
- Navigation (React Navigation)
- State management (Redux, Zustand)
- Native modules bridge
- Platform-specific code
- Performance optimization
- Testing and debugging

### 2. Flutter
- Widget composition
- State management (Provider, Riverpod)
- Navigation and routing
- Platform channels
- Asset management
- Animation frameworks
- Testing strategies

### 3. Native iOS (Swift)
- SwiftUI framework
- View lifecycle
- State and data binding
- Network requests
- Local storage (Core Data)
- Push notifications
- App extensions

### 4. Native Android (Kotlin)
- Jetpack Compose
- Activity and Fragment lifecycle
- ViewModel and LiveData
- Room database
- WorkManager for background tasks
- Foreground services

## Advanced Concepts

### Platform Channels (React Native)
```javascript
// JavaScript side
import { NativeModules } from 'react-native';

const { CustomModule } = NativeModules;

CustomModule.getValue()
  .then(value => console.log(value))
  .catch(error => console.error(error));
```

### State Management (Flutter)
```dart
class UserNotifier extends StateNotifier<List<String>> {
  UserNotifier() : super([]);

  void addUser(String name) {
    state = [...state, name];
  }
}

// Usage with Riverpod
final userProvider = StateNotifierProvider<UserNotifier, List<String>>(
  (ref) => UserNotifier(),
);
```

## Resources

- **React Native Docs**: https://reactnative.dev/
- **Flutter Documentation**: https://flutter.dev/docs
- **Swift Language Guide**: https://swift.org/
- **Android Developers**: https://developer.android.com/
- **Expo Documentation**: https://docs.expo.dev/

## Real-World Projects

1. **TODO App**: Core mobile development patterns
2. **Weather App**: API integration and location services
3. **Social Feed**: Complex list rendering and state management
4. **E-commerce**: Navigation, payments, and backend integration
5. **Chat App**: Real-time messaging and notifications

---

**Next Steps:** Choose your platform (React Native for cross-platform or native for specific OS), master navigation, and practice state management.
