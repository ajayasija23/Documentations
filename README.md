# 📱 Android MVVM App

A modern Android application built using MVVM architecture, Kotlin, Jetpack components, and clean modular practices.

---

## 🚀 Features
- MVVM Architecture
- Jetpack Navigation
- Hilt Dependency Injection
- Retrofit & OkHttp for Networking
- Room Database with Kotlin Flow
- LiveData / StateFlow for UI updates
- Modular and scalable project structure

---

## 🏗️ Project Structure
```
- app/              # Application module (UI layer)
- data/             # Data module (API, DB, repository impl)
  - api/
  - db/
  - repository/
- domain/           # Domain models and interfaces
  - model/
- presentation/     # ViewModels and UI components
  - ui/
  - viewmodel/
```

---

## 🧰 Tech Stack
| Component    | Tech                     |
|--------------|--------------------------|
| Language     | Kotlin                   |
| Architecture | MVVM                     |
| DI           | Hilt                     |
| DB           | Room                     |
| Networking   | Retrofit, OkHttp         |
| UI           | XML / Jetpack Compose    |
| Testing      | JUnit, Mockito, Espresso |

---

## 🔧 Setup Instructions
1. Clone the repository
```bash
git clone git@github.com:Webcomsystem/jokerz-kotlin.git
```

2. Open in Android Studio

3. Sync Gradle and build the project

4. Run the app on an emulator or device

---

## ✅ Best Practices Followed
- Separation of concerns (ViewModel handles UI logic)
- Single source of truth for data (Repository pattern)
- Reactive data flows using LiveData/Flow
- Centralized error handling

### Sample Use

## Remote Service
```
interface RemoteService {

    @POST("user/sentotpNew")
    suspend fun sentOtp(
        @Body request: HashMap<String, Any>
    ): Response<BaseResponse>
}
```

## Repository
``` 
class Repository @Inject constructor(private val remoteService: RemoteService) {

    suspend fun sentOtp(request: HashMap<String, Any>): Flow<ScreenState<BaseResponse>> {
        return flow {
            try {
                emit(ScreenState.Loading())
                val response = remoteService.sentOtp(request)
                if (response.isSuccessful) {
                    emit(ScreenState.Data(response.body()!!))
                } else if (response.isSuccessful && response.body()?.status == false) {
                    emit(
                        ScreenState.Error(
                            response.body()?.message.orEmpty(),
                            Gson().toJson(response.body())
                        )
                    )
                } else {
                    emit(
                        ScreenState.Error(
                            response.message(),
                            response.errorBody()?.string().orEmpty()
                        )
                    )
                }
                emit(ScreenState.Idle())
            } catch (e: Exception) {
                Log.d("jokerz_log_error", e.message.orEmpty())
                if (e is UnknownHostException) {
                    emit(ScreenState.Error("No internet", "internet"))
                }

            }
        }.flowOn(Dispatchers.IO)
    }
}
```

## ViewModel
```
@Suppress("DEPRECATION")
@HiltViewModel
class AuthViewModel @Inject constructor(private val repository: Repository):ViewModel() {

    private var baseResponse = MutableStateFlow<ScreenState<BaseResponse>>(ScreenState.Idle())
    var baseLiveData = baseResponse.asLiveData()
    fun sentOtp(request: HashMap<String, Any>) {
        viewModelScope.launch {
            repository.sentOtp(request).collectLatest {
                baseResponse.value = it
            }
        }
    }
}
```

### Fragment or Activity

Create View Model Instance

```
private val viewModel: AuthViewModel by viewModels()
```

Call Api

```
 viewModel.sentOtp(
 hashMapOf(
     "email" to binding.emailEdit.text.toString().trim()
 )
)
```

Observe Data

```
viewModel.baseLiveData.observe(viewLifecycleOwner){
    when(it){
        is ScreenState.Data -> {
            dismissLoader()
            val action= EmailFragmentDirections.actionNavEmailToNavOtp(binding.emailEdit.text.toString().trim(),isRegistered)
            findNavController().navigate(action)
        }
        is ScreenState.Error -> {
            dismissLoader()
        }
        is ScreenState.Idle -> {

        }
        is ScreenState.Loading -> {
            showLoader()
        }
    }
}
```

# Android Project Naming Conventions

This document outlines the standard naming conventions used across the Android application to ensure consistency, readability, and maintainability.

---

## 🧱 Project Structure

| Component | Convention | Example |
|----------|------------|---------|
| Package Name | `lowercase.without.underscores` | `com.example.myapp` |
| Folder Names | `lowercase` | `network`, `repository`, `ui` |

---

## 🧑‍💻 Class Naming

| Type | Convention | Example |
|------|------------|---------|
| Activity | PascalCase + `Activity` suffix | `MainActivity`, `LoginActivity` |
| Fragment | PascalCase + `Fragment` suffix | `HomeFragment`, `ProfileFragment` |
| ViewModel | PascalCase + `ViewModel` suffix | `MainViewModel` |
| Adapter | PascalCase + `Adapter` suffix | `UserAdapter` |
| Data Class / Model | PascalCase | `User`, `Post`, `LoginRequest` |
| Utility Class | PascalCase + `Utils`/`Helper` suffix | `DateUtils`, `ImageHelper` |

---

## 🧾 Variable Naming

| Scope | Convention | Example |
|-------|------------|---------|
| Local Variable | camelCase | `userName`, `isLoggedIn` |
| Constant (companion or top-level) | UPPER_SNAKE_CASE | `API_KEY`, `MAX_LENGTH` |
| Global Variable | camelCase (prefer to avoid) | `currentUser` |

---

## 🧬 Function Naming

| Type | Convention | Example |
|------|------------|---------|
| Regular Function | camelCase | `fetchUserData()`, `showDialog()` |
| Callback Listener | camelCase + suffix | `onItemClicked()`, `onLoginSuccess()` |

---

## 🧱 Resource Naming

### XML Layout Files

| Type | Convention | Example |
|------|------------|---------|
| Activity | `activity_<name>.xml` | `activity_main.xml` |
| Fragment | `fragment_<name>.xml` | `fragment_home.xml` |
| RecyclerView Item | `item_<name>.xml` | `item_message.xml` |
| Dialog | `dialog_<name>.xml` | `dialog_confirm.xml` |

---

### Drawables

| Type | Convention | Example |
|------|------------|---------|
| Icon | `ic_<description>.xml/png` | `ic_user_profile.xml` |
| Background | `bg_<description>.xml` | `bg_rounded_button.xml` |
| Selector | `selector_<description>.xml` | `selector_button.xml` |

---

### IDs

| Type | Convention | Example |
|------|------------|---------|
| Views | `type_description` | `btnLogin`, `tvTitle`, `rvMessages` |

---

### Strings

| Type | Convention | Example |
|------|------------|---------|
| General Text | `label_<description>` | `label_username`, `label_password` |
| Messages | `msg_<description>` | `msg_login_failed` |
| Buttons | `btn_<description>` | `btn_submit`, `btn_cancel` |

---

## 🔗 API / Network Layer

| Type | Convention | Example |
|------|------------|---------|
| API Interface | PascalCase + `Api` | `UserApi`, `AuthApi` |
| DTO / Request | PascalCase + `Request`/`Response` | `LoginRequest`, `SignupResponse` |

---

## 🗃️ Database (Room)

| Type | Convention | Example |
|------|------------|---------|
| Entity | PascalCase | `UserEntity`, `MessageEntity` |
| DAO | PascalCase + `Dao` | `UserDao`, `MessageDao` |
| Table Name | snake_case | `user_table`, `chat_table` |
| Column Name | snake_case | `first_name`, `created_at` |

---

## 🧪 Test Naming

| Type | Convention | Example |
|------|------------|---------|
| Unit Test File | `<ClassName>Test` | `MainViewModelTest` |
| UI Test File | `<ClassName>Test` | `LoginActivityTest` |
| Test Function | `should_<doSomething>_when_<condition>` | `shouldShowError_whenLoginFails()` |

---

## 🏁 Kotlin Specific

| Type | Convention | Example |
|------|------------|---------|
| Extension Functions | camelCase, scoped | `fun View.hideKeyboard()` |
| Sealed Classes | PascalCase | `UiState`, `ResultState` |

---

## ✅ Summary

- Use **camelCase** for variables and functions.
- Use **PascalCase** for classes and interfaces.
- Use **UPPER_SNAKE_CASE** for constants.
- Keep **resources lowercase with underscores**.
- Be consistent across modules and developers.

---

> Following these conventions will improve code readability and teamwork across the project.

