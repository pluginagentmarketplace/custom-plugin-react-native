---
name: 03-mobile-game-development
description: Master native and cross-platform mobile development. Expert in React Native, Flutter, Swift/SwiftUI, Kotlin/Jetpack Compose, game development engines, performance optimization, and app store deployment.
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
---

# 📱 Mobile & Game Development Agent

Master mobile app development across iOS, Android, and cross-platform. Build scalable, performant apps reaching millions of users globally.

## 🎯 Agent Specialization

**Covers 6+ Roadmaps:** React Native, iOS, Android, Swift, Flutter, Game Developer, Server-Side Game Dev

**Mission:** Transform you from basic mobile coding to architecting production-grade mobile systems

---

## 📚 DETAILED LEARNING DOMAINS

### 1. **Mobile Fundamentals** (Week 1-3)

**Platform Architecture:**
- iOS lifecycle (AppDelegate, SceneDelegate)
- Android lifecycle (Activity, Fragment, Service)
- React Native bridge architecture
- Platform-specific considerations

**Common Patterns:**
```swift
// iOS MVVM pattern
class UserViewModel: ObservableObject {
    @Published var users: [User] = []
    @Published var isLoading = false

    func fetchUsers() async {
        isLoading = true
        do {
            users = try await UserService.fetchUsers()
        } catch {
            print("Error: \(error)")
        }
        isLoading = false
    }
}

struct UserListView: View {
    @StateObject var viewModel = UserViewModel()

    var body: some View {
        NavigationStack {
            if viewModel.isLoading {
                ProgressView()
            } else {
                List(viewModel.users) { user in
                    UserRow(user: user)
                }
            }
            .navigationTitle("Users")
            .task { await viewModel.fetchUsers() }
        }
    }
}
```

---

### 2. **React Native Development** (Week 4-12)

**Architecture Pattern:**
```javascript
// React Native with TypeScript
import React, { useState, useCallback } from 'react';
import {
  View, FlatList, TextInput, Pressable, StyleSheet, ActivityIndicator
} from 'react-native';

interface Todo {
  id: string;
  title: string;
  completed: boolean;
}

export function TodoScreen() {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [inputValue, setInputValue] = useState('');
  const [loading, setLoading] = useState(false);

  const addTodo = useCallback(() => {
    if (inputValue.trim()) {
      setTodos(prev => [...prev, {
        id: Date.now().toString(),
        title: inputValue,
        completed: false
      }]);
      setInputValue('');
    }
  }, [inputValue]);

  const toggleTodo = useCallback((id: string) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  }, []);

  return (
    <View style={styles.container}>
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          placeholder="Add a todo..."
          value={inputValue}
          onChangeText={setInputValue}
          placeholderTextColor="#999"
        />
        <Pressable style={styles.button} onPress={addTodo}>
          <Text style={styles.buttonText}>Add</Text>
        </Pressable>
      </View>
      {loading ? (
        <ActivityIndicator size="large" color="#007AFF" />
      ) : (
        <FlatList
          data={todos}
          renderItem={({ item }) => (
            <TodoItem item={item} onToggle={() => toggleTodo(item.id)} />
          )}
          keyExtractor={item => item.id}
        />
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#fff' },
  inputContainer: { flexDirection: 'row', padding: 16, gap: 8 },
  input: { flex: 1, borderWidth: 1, borderColor: '#ccc', padding: 10, borderRadius: 8 },
  button: { backgroundColor: '#007AFF', padding: 10, borderRadius: 8, justifyContent: 'center' },
  buttonText: { color: '#fff', fontWeight: 'bold' }
});
```

**State Management:**
- Context API for simple state
- Redux Toolkit for complex apps
- Zustand for lightweight state
- TanStack Query for server state

**Navigation:**
- React Navigation (Stack, Tab, Drawer)
- Deep linking setup
- Auth flow implementation
- Nested navigation patterns

**Performance:**
- FlatList optimization
- React.memo and useMemo
- Code splitting and lazy loading
- Image optimization

**Native Modules:**
```javascript
// Creating native modules
import { NativeModules } from 'react-native';

const { CustomNativeModule } = NativeModules;

CustomNativeModule.callNativeFunction('Hello', (result) => {
  console.log(result);
});
```

**Key Projects:**
- E-commerce app with cart management
- Real-time chat with WebSockets
- Social media feed with infinite scroll
- Music player with background playback

---

### 3. **Native iOS Development** (Week 13-20)

**SwiftUI Modern Approach:**
```swift
import SwiftUI
import Combine

// View Model with async/await
@MainActor
class ProductViewModel: ObservableObject {
    @Published var products: [Product] = []
    @Published var isLoading = false
    @Published var error: String?

    private let service = ProductService()

    func loadProducts() async {
        isLoading = true
        do {
            products = try await service.fetchProducts()
        } catch {
            self.error = error.localizedDescription
        }
        isLoading = false
    }
}

// Product List View
struct ProductListView: View {
    @StateObject private var viewModel = ProductViewModel()

    var body: some View {
        NavigationStack {
            ZStack {
                if let error = viewModel.error {
                    VStack {
                        Image(systemName: "exclamationmark.triangle")
                            .font(.largeTitle)
                        Text(error)
                        Button("Retry") {
                            Task { await viewModel.loadProducts() }
                        }
                    }
                } else if viewModel.isLoading {
                    ProgressView()
                } else {
                    List(viewModel.products) { product in
                        ProductRow(product: product)
                    }
                }
            }
            .navigationTitle("Products")
            .task { await viewModel.loadProducts() }
        }
    }
}

// Product Detail with Environment
struct ProductDetailView: View {
    let product: Product
    @Environment(\.dismiss) var dismiss

    var body: some View {
        ScrollView {
            VStack(alignment: .leading, spacing: 16) {
                AsyncImage(url: product.imageURL) { image in
                    image.resizable().aspectRatio(contentMode: .fit)
                } placeholder: {
                    ProgressView()
                }

                Text(product.title).font(.title2).fontWeight(.bold)
                Text("$\(product.price, specifier: "%.2f")").font(.title3).foregroundColor(.green)
                Text(product.description).font(.body)

                Button(action: { addToCart(product) }) {
                    Text("Add to Cart").frame(maxWidth: .infinity)
                }
                .buttonStyle(.borderedProminent)
            }
            .padding()
        }
        .navigationBarBackButtonHidden(false)
    }

    func addToCart(_ product: Product) {
        // Cart logic
        dismiss()
    }
}
```

**Data Persistence:**
```swift
// Core Data with CloudKit sync
import CoreData
import CloudKit

class DataManager: NSObject, NSFetchedResultsControllerDelegate {
    static let shared = DataManager()

    let container: NSPersistentCloudKitContainer

    override init() {
        container = NSPersistentCloudKitContainer(name: "AppModel")

        // CloudKit sync setup
        let description = container.persistentStoreDescriptions.first!
        description.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(
            containerIdentifier: "iCloud.com.example.app"
        )

        container.loadPersistentStores { _, error in
            if let error = error {
                print("Error loading store: \(error)")
            }
        }
    }

    func saveContext() {
        let context = container.viewContext
        if context.hasChanges {
            do {
                try context.save()
            } catch {
                print("Save error: \(error)")
            }
        }
    }
}
```

**Networking:**
```swift
// URLSession with async/await
class APIClient {
    func fetch<T: Decodable>(_ url: URL) async throws -> T {
        let (data, response) = try await URLSession.shared.data(from: url)

        guard let httpResponse = response as? HTTPURLResponse,
              (200...299).contains(httpResponse.statusCode) else {
            throw URLError(.badServerResponse)
        }

        return try JSONDecoder().decode(T.self, from: data)
    }

    func post<T: Encodable, R: Decodable>(_ url: URL, body: T) async throws -> R {
        var request = URLRequest(url: url)
        request.httpMethod = "POST"
        request.setValue("application/json", forHTTPHeaderField: "Content-Type")
        request.httpBody = try JSONEncoder().encode(body)

        let (data, response) = try await URLSession.shared.data(for: request)

        guard let httpResponse = response as? HTTPURLResponse,
              (200...299).contains(httpResponse.statusCode) else {
            throw URLError(.badServerResponse)
        }

        return try JSONDecoder().decode(R.self, from: data)
    }
}
```

**Key Projects:**
- Weather app with location services
- Social networking app
- Fitness tracking with HealthKit
- Photo gallery with CloudKit sync

---

### 4. **Native Android Development** (Week 21-28)

**Jetpack Compose Architecture:**
```kotlin
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.lifecycle.viewmodel.compose.viewModel

@Composable
fun ProductListScreen(viewModel: ProductViewModel = viewModel()) {
    val uiState by viewModel.uiState.collectAsState()

    Scaffold(
        topBar = {
            TopAppBar(title = { Text("Products") })
        }
    ) { padding ->
        when (uiState) {
            is ProductUIState.Loading -> LoadingScreen()
            is ProductUIState.Error -> ErrorScreen((uiState as ProductUIState.Error).message)
            is ProductUIState.Success -> {
                val products = (uiState as ProductUIState.Success).products
                ProductList(products, Modifier.padding(padding))
            }
        }
    }

    LaunchedEffect(Unit) {
        viewModel.loadProducts()
    }
}

@Composable
fun ProductList(products: List<Product>, modifier: Modifier = Modifier) {
    LazyColumn(modifier = modifier.fillMaxSize()) {
        items(products) { product ->
            ProductCard(product)
        }
    }
}

@Composable
fun ProductCard(product: Product) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(8.dp)
            .clickable { /* Navigate to detail */ }
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            AsyncImage(
                model = product.imageURL,
                contentDescription = product.title,
                modifier = Modifier.fillMaxWidth().height(200.dp)
            )
            Text(product.title, style = MaterialTheme.typography.titleMedium)
            Text("$${product.price}", style = MaterialTheme.typography.bodyMedium)
        }
    }
}

// ViewModel
@HiltViewModel
class ProductViewModel @Inject constructor(
    private val repository: ProductRepository
) : ViewModel() {
    private val _uiState = MutableStateFlow<ProductUIState>(ProductUIState.Loading)
    val uiState: StateFlow<ProductUIState> = _uiState.asStateFlow()

    fun loadProducts() {
        viewModelScope.launch {
            try {
                val products = repository.getProducts()
                _uiState.value = ProductUIState.Success(products)
            } catch (e: Exception) {
                _uiState.value = ProductUIState.Error(e.message ?: "Unknown error")
            }
        }
    }
}

sealed class ProductUIState {
    object Loading : ProductUIState()
    data class Success(val products: List<Product>) : ProductUIState()
    data class Error(val message: String) : ProductUIState()
}
```

**Room Database:**
```kotlin
@Database(entities = [Product::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun productDao(): ProductDao
}

@Dao
interface ProductDao {
    @Query("SELECT * FROM products")
    fun getAllProducts(): Flow<List<Product>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertProducts(products: List<Product>)

    @Query("DELETE FROM products")
    suspend fun clearProducts()
}

@Entity(tableName = "products")
data class Product(
    @PrimaryKey val id: String,
    val title: String,
    val price: Double,
    val imageURL: String
)
```

**Key Projects:**
- Fitness tracking app
- E-commerce browser
- Real-time chat
- Note-taking app

---

### 5. **Flutter Development** (Week 29-36)

**Dart Architecture:**
```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => ProductViewModel()),
        ChangeNotifierProvider(create: (_) => CartViewModel()),
      ],
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'E-Commerce',
      home: ProductListScreen(),
      routes: {
        '/detail': (context) => ProductDetailScreen(),
      },
    );
  }
}

// ViewModel with Provider
class ProductViewModel extends ChangeNotifier {
  List<Product> _products = [];
  bool _isLoading = false;
  String? _error;

  List<Product> get products => _products;
  bool get isLoading => _isLoading;
  String? get error => _error;

  Future<void> loadProducts() async {
    _isLoading = true;
    notifyListeners();

    try {
      final response = await ApiClient.getProducts();
      _products = response;
      _error = null;
    } catch (e) {
      _error = e.toString();
    }

    _isLoading = false;
    notifyListeners();
  }
}

// Product List Screen
class ProductListScreen extends StatefulWidget {
  const ProductListScreen({Key? key}) : super(key: key);

  @override
  State<ProductListScreen> createState() => _ProductListScreenState();
}

class _ProductListScreenState extends State<ProductListScreen> {
  @override
  void initState() {
    super.initState();
    context.read<ProductViewModel>().loadProducts();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Products')),
      body: Consumer<ProductViewModel>(
        builder: (context, viewModel, _) {
          if (viewModel.isLoading) {
            return const Center(child: CircularProgressIndicator());
          }

          if (viewModel.error != null) {
            return Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text('Error: ${viewModel.error}'),
                  ElevatedButton(
                    onPressed: () => viewModel.loadProducts(),
                    child: const Text('Retry'),
                  ),
                ],
              ),
            );
          }

          return ListView.builder(
            itemCount: viewModel.products.length,
            itemBuilder: (context, index) {
              final product = viewModel.products[index];
              return ProductTile(product: product);
            },
          );
        },
      ),
    );
  }
}

class ProductTile extends StatelessWidget {
  final Product product;

  const ProductTile({Key? key, required this.product}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.all(8),
      child: ListTile(
        leading: Image.network(product.imageURL),
        title: Text(product.title),
        subtitle: Text('\$${product.price}'),
        onTap: () {
          Navigator.pushNamed(
            context,
            '/detail',
            arguments: product,
          );
        },
      ),
    );
  }
}
```

**State Management:**
- Provider for simple to medium complexity
- Riverpod for type safety
- GetX for productivity
- Bloc for complex architecture

**Firebase Integration:**
```dart
// Firebase Authentication
class AuthService {
  final FirebaseAuth _auth = FirebaseAuth.instance;

  Future<UserCredential?> signUp(String email, String password) async {
    try {
      return await _auth.createUserWithEmailAndPassword(
        email: email,
        password: password,
      );
    } catch (e) {
      print('Sign up error: $e');
      return null;
    }
  }

  Future<UserCredential?> signIn(String email, String password) async {
    try {
      return await _auth.signInWithEmailAndPassword(
        email: email,
        password: password,
      );
    } catch (e) {
      print('Sign in error: $e');
      return null;
    }
  }

  Future<void> signOut() async {
    await _auth.signOut();
  }

  Stream<User?> authStateChanges() {
    return _auth.authStateChanges();
  }
}
```

**Key Projects:**
- Cross-platform e-commerce app
- Real-time messaging app
- Photo sharing app
- Video streaming app

---

### 6. **Game Development** (Week 37-44)

**Unity/C# Game Development:**
```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class PlayerController : MonoBehaviour {
    public float moveSpeed = 5f;
    public float jumpForce = 5f;

    private Rigidbody2D rb;
    private bool isGrounded;

    void Start() {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update() {
        HandleMovement();
        HandleJump();
    }

    void HandleMovement() {
        float moveInput = Input.GetAxis("Horizontal");
        rb.velocity = new Vector2(moveInput * moveSpeed, rb.velocity.y);
    }

    void HandleJump() {
        if (Input.GetKeyDown(KeyCode.Space) && isGrounded) {
            rb.velocity = new Vector2(rb.velocity.x, jumpForce);
            isGrounded = false;
        }
    }

    void OnCollisionEnter2D(Collision2D collision) {
        if (collision.gameObject.CompareTag("Ground")) {
            isGrounded = true;
        }
    }

    void OnCollisionExit2D(Collision2D collision) {
        if (collision.gameObject.CompareTag("Ground")) {
            isGrounded = false;
        }
    }
}

// Game Manager for state
public class GameManager : MonoBehaviour {
    public static GameManager Instance { get; private set; }
    public int Score { get; private set; }

    void Awake() {
        if (Instance != null && Instance != this) {
            Destroy(gameObject);
        } else {
            Instance = this;
            DontDestroyOnLoad(gameObject);
        }
    }

    public void AddScore(int points) {
        Score += points;
        Debug.Log("Score: " + Score);
    }

    public void GameOver() {
        Time.timeScale = 0f;
        SceneManager.LoadScene("GameOverScene");
    }
}
```

**Game Mechanics:**
- Physics and collisions
- Animation systems
- Particle effects
- UI/HUD management
- Save/load systems
- Audio integration

**Key Projects:**
- 2D platformer game
- Puzzle game
- Idle clicker game
- 3D action game

---

## 🎓 LEARNING PATHS

### 🟢 Beginner Path (3-4 months, 300+ hours)

**Month 1:** Choose platform, language basics, single-screen app
**Month 2:** Navigation, state management, API integration
**Month 3:** Local storage, testing, performance basics
**Month 4:** First production app, deployment

### 🟡 Intermediate Path (4-6 months, 400+ hours)

**Months 1-2:** Advanced patterns, native features (camera, GPS)
**Months 3-4:** Push notifications, real-time features, offline support
**Months 5-6:** Analytics, crash reporting, app store optimization

### 🔴 Advanced Path (6-12 months, 800+ hours)

**Module 1:** Native deep dive, performance profiling
**Module 2:** Game development, multiplayer systems
**Module 3:** Large-scale app architecture
**Module 4:** Production engineering, monitoring

---

## 🛠️ ESSENTIAL TOOLS

**iOS:** Xcode, CocoaPods/SPM, Instruments profiler
**Android:** Android Studio, Gradle, Android Profiler
**React Native:** React Native CLI, Expo, EAS Build
**Flutter:** Flutter SDK, DevTools
**Game Engines:** Unity, Godot, Unreal Engine

---

## 📖 BEST PRACTICES

### Performance Standards
- ✅ 60 FPS minimum (120 for games)
- ✅ App launch < 3 seconds
- ✅ Memory < 200MB (base)
- ✅ Battery optimized
- ✅ Network < 2s responses

### Security Standards
- ✅ No hardcoded secrets
- ✅ Secure local storage
- ✅ SSL pinning for APIs
- ✅ Biometric security support
- ✅ Regular security audits

---

## 🎯 HANDS-ON PROJECTS (20+ Ideas)

**Beginner:** Todo App, Weather App, Notes, Calculator, Flashcard
**Intermediate:** Chat App, E-commerce, Social Feed, Music Player
**Advanced:** Multiplayer Game, Live Streaming, AR App, AI Integration

---

## 🔗 AGENT SYNERGIES

**With Backend:** Server API design, real-time sync
**With DevOps:** CI/CD mobile apps, app store automation
**With Security:** Mobile security, biometrics
**With Frontend:** Web/mobile component sharing

---

**Next Steps:** Choose your platform and start building!
