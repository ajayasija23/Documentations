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
