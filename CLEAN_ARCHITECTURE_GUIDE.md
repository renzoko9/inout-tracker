# Clean Architecture Simplificada - INOUT

Guía completa de arquitectura para el frontend móvil de INOUT usando React Native + Expo.

---

## 📁 Estructura Completa del Proyecto

```
inout-mobile/
├── app/                                    # Expo Router (UI Routing)
│   ├── (auth)/
│   │   ├── login.tsx
│   │   ├── register.tsx
│   │   └── _layout.tsx
│   │
│   ├── (tabs)/
│   │   ├── index.tsx                       # Dashboard
│   │   ├── accounts.tsx
│   │   ├── transactions.tsx
│   │   └── _layout.tsx
│   │
│   └── _layout.tsx
│
├── src/
│   │
│   ├── domain/                             # 🟢 CAPA 1: DOMAIN (Núcleo del negocio)
│   │   │
│   │   ├── entities/                       # Modelos de dominio
│   │   │   ├── Account.ts
│   │   │   ├── Transaction.ts
│   │   │   ├── Category.ts
│   │   │   └── User.ts
│   │   │
│   │   ├── repositories/                   # Contratos (interfaces)
│   │   │   ├── IAccountRepository.ts
│   │   │   ├── ITransactionRepository.ts
│   │   │   ├── ICategoryRepository.ts
│   │   │   └── IAuthRepository.ts
│   │   │
│   │   ├── use-cases/                      # Casos de uso (lógica de negocio)
│   │   │   │
│   │   │   ├── account/
│   │   │   │   ├── CreateAccount.ts
│   │   │   │   ├── GetAccounts.ts
│   │   │   │   ├── GetAccountById.ts
│   │   │   │   ├── UpdateAccount.ts
│   │   │   │   └── DeleteAccount.ts
│   │   │   │
│   │   │   ├── transaction/
│   │   │   │   ├── CreateTransaction.ts
│   │   │   │   ├── GetTransactions.ts
│   │   │   │   ├── UpdateTransaction.ts
│   │   │   │   ├── DeleteTransaction.ts
│   │   │   │   └── CalculateBalance.ts      # Lógica de negocio
│   │   │   │
│   │   │   ├── category/
│   │   │   │   ├── GetCategories.ts
│   │   │   │   ├── CreateCategory.ts
│   │   │   │   └── GetGlobalCategories.ts
│   │   │   │
│   │   │   └── auth/
│   │   │       ├── Login.ts
│   │   │       ├── Register.ts
│   │   │       ├── Logout.ts
│   │   │       └── RefreshToken.ts
│   │   │
│   │   └── value-objects/                  # Objetos de valor (opcional)
│   │       ├── Money.ts
│   │       ├── Email.ts
│   │       └── TransactionDate.ts
│   │
│   │
│   ├── data/                               # 🔵 CAPA 2: DATA (Infraestructura)
│   │   │
│   │   ├── repositories/                   # Implementaciones concretas
│   │   │   ├── AccountRepository.ts
│   │   │   ├── TransactionRepository.ts
│   │   │   ├── CategoryRepository.ts
│   │   │   └── AuthRepository.ts
│   │   │
│   │   ├── api/                            # Cliente HTTP
│   │   │   ├── client.ts                   # Axios config + interceptors
│   │   │   ├── endpoints.ts                # URLs endpoints
│   │   │   └── dto/                        # DTOs de API
│   │   │       ├── AccountDTO.ts
│   │   │       ├── TransactionDTO.ts
│   │   │       └── ResponseDTO.ts
│   │   │
│   │   ├── mappers/                        # DTO → Entity conversion
│   │   │   ├── AccountMapper.ts
│   │   │   ├── TransactionMapper.ts
│   │   │   └── CategoryMapper.ts
│   │   │
│   │   └── storage/                        # Persistencia local
│   │       ├── SecureStorage.ts            # Tokens
│   │       └── AsyncStorage.ts             # Cache
│   │
│   │
│   ├── presentation/                       # 🟡 CAPA 3: PRESENTATION (UI)
│   │   │
│   │   ├── components/                     # Componentes UI
│   │   │   │
│   │   │   ├── ui/                         # Design System
│   │   │   │   ├── Button/
│   │   │   │   │   ├── Button.tsx
│   │   │   │   │   ├── Button.styles.ts
│   │   │   │   │   └── index.ts
│   │   │   │   │
│   │   │   │   ├── Card/
│   │   │   │   ├── Input/
│   │   │   │   ├── Badge/
│   │   │   │   └── index.ts
│   │   │   │
│   │   │   ├── cards/                      # Cards específicos
│   │   │   │   ├── BalanceCard.tsx
│   │   │   │   ├── AccountCard.tsx
│   │   │   │   └── TransactionCard.tsx
│   │   │   │
│   │   │   ├── forms/                      # Formularios
│   │   │   │   ├── AccountForm.tsx
│   │   │   │   └── TransactionForm.tsx
│   │   │   │
│   │   │   └── charts/                     # Gráficas
│   │   │       ├── PieChart.tsx
│   │   │       └── BarChart.tsx
│   │   │
│   │   ├── hooks/                          # Custom hooks (conectan UI con domain)
│   │   │   ├── useAuth.ts
│   │   │   ├── useAccounts.ts
│   │   │   ├── useTransactions.ts
│   │   │   ├── useCategories.ts
│   │   │   └── useBalance.ts
│   │   │
│   │   ├── store/                          # State management (Zustand)
│   │   │   ├── authStore.ts
│   │   │   ├── accountsStore.ts
│   │   │   ├── transactionsStore.ts
│   │   │   └── uiStore.ts
│   │   │
│   │   └── theme/                          # Design system
│   │       ├── colors.ts
│   │       ├── spacing.ts
│   │       ├── typography.ts
│   │       └── index.ts
│   │
│   │
│   └── shared/                             # 🟣 SHARED (Compartido entre capas)
│       │
│       ├── types/                          # Types globales
│       │   ├── api.types.ts
│       │   ├── navigation.types.ts
│       │   └── common.types.ts
│       │
│       ├── utils/                          # Utilidades
│       │   ├── formatters.ts               # Formateo de montos/fechas
│       │   ├── validators.ts               # Validaciones
│       │   └── helpers.ts
│       │
│       ├── constants/                      # Constantes
│       │   ├── config.ts
│       │   └── endpoints.ts
│       │
│       └── errors/                         # Manejo de errores
│           ├── AppError.ts
│           └── ErrorHandler.ts
│
├── .env
├── .env.example
├── app.json
├── package.json
└── tsconfig.json
```

---

## 💡 Flujo de Datos (Ejemplo: Crear Cuenta)

```
[UI Screen] → [Hook] → [Use Case] → [Repository Interface] → [Repository Impl] → [API Client] → [Backend]
                ↓          ↓              ↓                        ↓                   ↓
            Zustand    Business       Contract            Implementation         HTTP Request
            Store      Logic
```

**Explicación del flujo:**
1. Usuario hace clic en "Crear Cuenta" (UI Screen)
2. Screen llama al hook `useAccounts.createAccount()`
3. Hook ejecuta el use case `CreateAccountUseCase.execute()`
4. Use case valida datos y llama a `IAccountRepository.create()`
5. Repository implementación (`AccountRepository`) hace request HTTP
6. API Client (Axios) envía request al backend
7. Response vuelve por el mismo camino (invertido)
8. Mapper convierte DTO a Entity
9. Entity se guarda en Zustand store
10. UI se actualiza automáticamente

---

## 📝 Ejemplos de Código

### 1. DOMAIN LAYER

#### `src/domain/entities/Account.ts`

```typescript
// Entidad de dominio (modelo puro)
export class Account {
  constructor(
    public readonly id: number,
    public name: string,
    public balance: number,
    public description?: string,
    public readonly createdAt?: Date,
  ) {}

  // Métodos de negocio (opcional)
  public updateBalance(amount: number): void {
    this.balance += amount;
  }

  public canBeDeleted(): boolean {
    // Regla de negocio: solo se puede eliminar si balance es 0
    return this.balance === 0;
  }

  public get formattedBalance(): string {
    return `$${this.balance.toFixed(2)}`;
  }
}
```

#### `src/domain/repositories/IAccountRepository.ts`

```typescript
// Contrato (interface) - NO implementación
import { Account } from '../entities/Account';

export interface IAccountRepository {
  create(name: string, balance?: number, description?: string): Promise<Account>;
  getAll(): Promise<Account[]>;
  getById(id: number): Promise<Account>;
  update(id: number, data: Partial<Account>): Promise<Account>;
  delete(id: number): Promise<void>;
}
```

**¿Por qué interfaces?**
- El domain NO depende de implementaciones concretas
- Podemos cambiar de API REST a GraphQL sin tocar el domain
- Fácil hacer tests con mocks

#### `src/domain/use-cases/account/CreateAccount.ts`

```typescript
// Caso de uso (lógica de negocio)
import { Account } from '../../entities/Account';
import { IAccountRepository } from '../../repositories/IAccountRepository';

export class CreateAccountUseCase {
  constructor(private accountRepository: IAccountRepository) {}

  async execute(
    name: string,
    balance?: number,
    description?: string
  ): Promise<Account> {
    // Validaciones de negocio
    if (!name || name.trim().length === 0) {
      throw new Error('El nombre de la cuenta es requerido');
    }

    if (balance && balance < 0) {
      throw new Error('El balance no puede ser negativo');
    }

    // Delega la implementación al repository
    return await this.accountRepository.create(name, balance, description);
  }
}
```

**Ventajas de Use Cases:**
- Encapsulan lógica de negocio
- Reutilizables (mismo use case para móvil y web)
- Testeables sin UI

#### `src/domain/use-cases/account/GetAccounts.ts`

```typescript
import { Account } from '../../entities/Account';
import { IAccountRepository } from '../../repositories/IAccountRepository';

export class GetAccountsUseCase {
  constructor(private accountRepository: IAccountRepository) {}

  async execute(): Promise<Account[]> {
    const accounts = await this.accountRepository.getAll();

    // Lógica de negocio: ordenar por balance descendente
    return accounts.sort((a, b) => b.balance - a.balance);
  }
}
```

---

### 2. DATA LAYER

#### `src/data/api/dto/AccountDTO.ts`

```typescript
// DTO que viene del backend
export interface AccountResponseDTO {
  responseType: number;
  title: string;
  message: string;
  data?: {
    id: number;
    name: string;
    balance: string; // Viene como string del backend
    description?: string;
    createdAt: string;
  };
}

export interface AccountListResponseDTO {
  responseType: number;
  title: string;
  message: string;
  data?: Array<{
    id: number;
    name: string;
    balance: string;
    description?: string;
    createdAt: string;
  }>;
}
```

**¿Por qué DTOs separados?**
- El backend puede cambiar su formato
- Domain entities no se acoplan al backend
- Validación y transformación en un solo lugar

#### `src/data/mappers/AccountMapper.ts`

```typescript
// Convierte DTO → Entity
import { Account } from '../../domain/entities/Account';
import { AccountResponseDTO } from '../api/dto/AccountDTO';

export class AccountMapper {
  static toDomain(dto: AccountResponseDTO['data']): Account {
    if (!dto) throw new Error('Invalid DTO');

    return new Account(
      dto.id,
      dto.name,
      parseFloat(dto.balance), // Convierte string a number
      dto.description,
      new Date(dto.createdAt)
    );
  }

  static toDomainList(dtoList: Array<AccountResponseDTO['data']>): Account[] {
    return dtoList
      .filter(dto => dto !== null)
      .map(dto => this.toDomain(dto));
  }

  // Para crear/actualizar (Entity → DTO)
  static toAPI(account: Partial<Account>) {
    return {
      name: account.name,
      balance: account.balance,
      description: account.description,
    };
  }
}
```

**Responsabilidades del Mapper:**
- Convertir tipos (string → number, string → Date)
- Manejar campos opcionales
- Transformar estructuras diferentes

#### `src/data/api/client.ts`

```typescript
import axios from 'axios';
import { SecureStorage } from '../storage/SecureStorage';

const API_URL = process.env.EXPO_PUBLIC_API_URL || 'http://localhost:4800/api/v1';

export const apiClient = axios.create({
  baseURL: API_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Interceptor para agregar token
apiClient.interceptors.request.use(
  async (config) => {
    const token = await SecureStorage.getAccessToken();
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Interceptor para manejar errores y refresh token
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;

    // Si es 401 y no hemos intentado refresh
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;

      try {
        const refreshToken = await SecureStorage.getRefreshToken();
        const { data } = await axios.post(`${API_URL}/auth/refresh`, {
          refreshToken,
        });

        await SecureStorage.setAccessToken(data.data.access);
        await SecureStorage.setRefreshToken(data.data.refresh);

        originalRequest.headers.Authorization = `Bearer ${data.data.access}`;
        return apiClient(originalRequest);
      } catch (refreshError) {
        // Refresh falló, logout
        await SecureStorage.clear();
        // Navegar a login
        return Promise.reject(refreshError);
      }
    }

    return Promise.reject(error);
  }
);
```

**Características del API Client:**
- Configuración centralizada
- Auto-refresh de tokens
- Manejo de errores global
- Headers comunes

#### `src/data/repositories/AccountRepository.ts`

```typescript
// Implementación concreta del contrato
import { Account } from '../../domain/entities/Account';
import { IAccountRepository } from '../../domain/repositories/IAccountRepository';
import { apiClient } from '../api/client';
import { AccountMapper } from '../mappers/AccountMapper';
import { AccountResponseDTO, AccountListResponseDTO } from '../api/dto/AccountDTO';

export class AccountRepository implements IAccountRepository {
  async create(name: string, balance?: number, description?: string): Promise<Account> {
    const response = await apiClient.post<AccountResponseDTO>('/accounts', {
      name,
      balance,
      description,
    });

    return AccountMapper.toDomain(response.data.data);
  }

  async getAll(): Promise<Account[]> {
    const response = await apiClient.get<AccountListResponseDTO>('/accounts');

    if (!response.data.data) return [];

    return AccountMapper.toDomainList(response.data.data);
  }

  async getById(id: number): Promise<Account> {
    const response = await apiClient.get<AccountResponseDTO>(`/accounts/${id}`);
    return AccountMapper.toDomain(response.data.data);
  }

  async update(id: number, data: Partial<Account>): Promise<Account> {
    const response = await apiClient.patch<AccountResponseDTO>(
      `/accounts/${id}`,
      AccountMapper.toAPI(data)
    );
    return AccountMapper.toDomain(response.data.data);
  }

  async delete(id: number): Promise<void> {
    await apiClient.delete(`/accounts/${id}`);
  }
}
```

**Repository implementa:**
- Todas las operaciones CRUD
- Convierte responses a Entities con Mappers
- Maneja errores HTTP

---

### 3. PRESENTATION LAYER

#### `src/presentation/hooks/useAccounts.ts`

```typescript
// Hook que conecta UI con domain
import { useState, useEffect } from 'react';
import { Account } from '../../domain/entities/Account';
import { GetAccountsUseCase } from '../../domain/use-cases/account/GetAccounts';
import { CreateAccountUseCase } from '../../domain/use-cases/account/CreateAccount';
import { DeleteAccountUseCase } from '../../domain/use-cases/account/DeleteAccount';
import { AccountRepository } from '../../data/repositories/AccountRepository';

// Singleton del repository (o usar DI container)
const accountRepository = new AccountRepository();

export const useAccounts = () => {
  const [accounts, setAccounts] = useState<Account[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  // Use cases
  const getAccountsUseCase = new GetAccountsUseCase(accountRepository);
  const createAccountUseCase = new CreateAccountUseCase(accountRepository);
  const deleteAccountUseCase = new DeleteAccountUseCase(accountRepository);

  const fetchAccounts = async () => {
    try {
      setLoading(true);
      setError(null);
      const data = await getAccountsUseCase.execute();
      setAccounts(data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Error al cargar cuentas');
    } finally {
      setLoading(false);
    }
  };

  const createAccount = async (
    name: string,
    balance?: number,
    description?: string
  ) => {
    try {
      setLoading(true);
      setError(null);
      const newAccount = await createAccountUseCase.execute(name, balance, description);
      setAccounts(prev => [...prev, newAccount]);
      return newAccount;
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Error al crear cuenta');
      throw err;
    } finally {
      setLoading(false);
    }
  };

  const deleteAccount = async (id: number) => {
    try {
      setLoading(true);
      await deleteAccountUseCase.execute(id);
      setAccounts(prev => prev.filter(acc => acc.id !== id));
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Error al eliminar cuenta');
      throw err;
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchAccounts();
  }, []);

  return {
    accounts,
    loading,
    error,
    refetch: fetchAccounts,
    createAccount,
    deleteAccount,
  };
};
```

**Hook responsibilities:**
- Estado local (loading, error, data)
- Ejecutar use cases
- Proveer API simple a la UI

#### `app/(tabs)/accounts.tsx` (Pantalla UI)

```typescript
import { View, Text, FlatList, TouchableOpacity } from 'react-native';
import { useAccounts } from '../../src/presentation/hooks/useAccounts';
import { AccountCard } from '../../src/presentation/components/cards/AccountCard';
import { Button } from '../../src/presentation/components/ui/Button';
import { useRouter } from 'expo-router';

export default function AccountsScreen() {
  const { accounts, loading, error, refetch } = useAccounts();
  const router = useRouter();

  if (loading) return <Text>Cargando...</Text>;
  if (error) return <Text>Error: {error}</Text>;

  return (
    <View className="flex-1 bg-background p-4">
      <Text className="text-2xl font-bold mb-4">Mis Cuentas</Text>

      <FlatList
        data={accounts}
        keyExtractor={(item) => item.id.toString()}
        renderItem={({ item }) => (
          <AccountCard
            account={item}
            onPress={() => router.push(`/account/${item.id}`)}
          />
        )}
        onRefresh={refetch}
        refreshing={loading}
      />

      <Button
        onPress={() => router.push('/account/create')}
        className="mt-4"
      >
        Crear Cuenta
      </Button>
    </View>
  );
}
```

**UI Screen es simple:**
- Usa hooks
- Renderiza componentes
- Maneja navegación
- NO tiene lógica de negocio

---

## 🔄 Ventajas de esta Arquitectura

### 1. Separación de Responsabilidades

```
Domain       → QUÉ hace la app (lógica de negocio)
Data         → CÓMO obtiene los datos (API, storage)
Presentation → CÓMO se ve (UI, componentes)
```

### 2. Testeable

```typescript
// Test del use case SIN necesidad de API real
describe('CreateAccountUseCase', () => {
  it('should create account with valid data', async () => {
    // Mock del repository
    const mockRepo: IAccountRepository = {
      create: jest.fn().mockResolvedValue(new Account(1, 'Test', 100)),
      getAll: jest.fn(),
      getById: jest.fn(),
      update: jest.fn(),
      delete: jest.fn(),
    };

    const useCase = new CreateAccountUseCase(mockRepo);
    const result = await useCase.execute('Test', 100);

    expect(result.name).toBe('Test');
    expect(mockRepo.create).toHaveBeenCalledWith('Test', 100, undefined);
  });

  it('should throw error with empty name', async () => {
    const mockRepo: IAccountRepository = {} as any;
    const useCase = new CreateAccountUseCase(mockRepo);

    await expect(useCase.execute('')).rejects.toThrow(
      'El nombre de la cuenta es requerido'
    );
  });

  it('should throw error with negative balance', async () => {
    const mockRepo: IAccountRepository = {} as any;
    const useCase = new CreateAccountUseCase(mockRepo);

    await expect(useCase.execute('Test', -100)).rejects.toThrow(
      'El balance no puede ser negativo'
    );
  });
});
```

### 3. Intercambiable

Si mañana quieres:
- Cambiar de Axios a Fetch → Solo cambias `apiClient`
- Cambiar de REST a GraphQL → Solo cambias `AccountRepository`
- Cambiar de Zustand a Redux → Solo cambias `store`

**El Domain NO se toca.**

### 4. Escalable

Agregar nuevas features es fácil:

**Ejemplo: Agregar Budgets (Presupuestos)**

1. Crear `Budget.ts` en `domain/entities`
2. Crear `IBudgetRepository.ts` en `domain/repositories`
3. Crear use cases en `domain/use-cases/budget/`
4. Implementar `BudgetRepository.ts` en `data/repositories`
5. Crear `useBudgets.ts` hook en `presentation/hooks`
6. Crear pantallas en `app/budget/`

**Todo sigue el mismo patrón.**

### 5. TypeScript First

Type-safety en todas las capas:
```typescript
Entity → Repository Interface → Repository Impl → DTO → API Response
  ✓           ✓                      ✓            ✓         ✓
```

---

## 🎯 Principios SOLID Aplicados

### Single Responsibility
Cada clase tiene UNA responsabilidad:
- `CreateAccountUseCase` → Solo crear cuentas
- `AccountRepository` → Solo comunicación con API
- `AccountMapper` → Solo conversión DTO ↔ Entity

### Open/Closed
Abierto a extensión, cerrado a modificación:
- Puedes agregar nuevos use cases sin modificar existentes
- Puedes agregar nuevos repositories sin tocar el domain

### Liskov Substitution
Puedes sustituir implementaciones:
```typescript
// Puedes intercambiar implementaciones
const repo1 = new HttpAccountRepository();
const repo2 = new GraphQLAccountRepository();
const repo3 = new MockAccountRepository(); // Para tests

// Todas implementan IAccountRepository
const useCase = new CreateAccountUseCase(repo1); // o repo2 o repo3
```

### Interface Segregation
Interfaces específicas, no genéricas:
- `IAccountRepository` solo tiene métodos de Account
- No hay un mega-interface con TODO

### Dependency Inversion
Dependencias apuntan hacia abstracciones:
```typescript
// ❌ MAL: Use case depende de implementación concreta
class CreateAccountUseCase {
  constructor(private repo: AccountRepository) {} // Concreto
}

// ✅ BIEN: Use case depende de abstracción
class CreateAccountUseCase {
  constructor(private repo: IAccountRepository) {} // Interface
}
```

---

## 📦 Dependencias Recomendadas

```json
{
  "dependencies": {
    "expo": "~50.0.0",
    "expo-router": "~3.4.0",
    "react": "18.2.0",
    "react-native": "0.73.0",
    "nativewind": "^4.0.0",
    "zustand": "^4.5.0",
    "axios": "^1.6.0",
    "expo-secure-store": "~13.0.0",
    "react-hook-form": "^7.50.0",
    "zod": "^3.22.0"
  },
  "devDependencies": {
    "@types/react": "~18.2.0",
    "typescript": "^5.3.0",
    "jest": "^29.7.0",
    "@testing-library/react-native": "^12.4.0"
  }
}
```

---

## 🚀 Cómo Empezar

### 1. Crear proyecto Expo
```bash
npx create-expo-app@latest inout-mobile --template tabs
cd inout-mobile
```

### 2. Instalar dependencias
```bash
npm install axios zustand expo-secure-store zod react-hook-form
npm install -D @types/react typescript
```

### 3. Configurar TypeScript
```json
// tsconfig.json
{
  "extends": "expo/tsconfig.base",
  "compilerOptions": {
    "strict": true,
    "paths": {
      "@domain/*": ["./src/domain/*"],
      "@data/*": ["./src/data/*"],
      "@presentation/*": ["./src/presentation/*"],
      "@shared/*": ["./src/shared/*"]
    }
  }
}
```

### 4. Crear estructura de carpetas
```bash
mkdir -p src/domain/{entities,repositories,use-cases}
mkdir -p src/data/{repositories,api,mappers,storage}
mkdir -p src/presentation/{components,hooks,store,theme}
mkdir -p src/shared/{types,utils,constants,errors}
```

### 5. Configurar variables de entorno
```bash
# .env
EXPO_PUBLIC_API_URL=http://localhost:4800/api/v1
```

---

## 📚 Recursos Adicionales

### Lectura Recomendada
- [Clean Architecture - Robert C. Martin](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [SOLID Principles](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)
- [React Native Best Practices](https://reactnative.dev/docs/performance)

### Patrones Útiles
- **Repository Pattern**: Abstrae la capa de datos
- **Use Case Pattern**: Encapsula lógica de negocio
- **Mapper Pattern**: Convierte entre capas
- **Dependency Injection**: Inversión de dependencias

---

## ✅ Checklist de Implementación

Cuando implementes una nueva feature, sigue este checklist:

- [ ] Crear Entity en `domain/entities`
- [ ] Crear Repository Interface en `domain/repositories`
- [ ] Crear Use Cases en `domain/use-cases`
- [ ] Crear DTOs en `data/api/dto`
- [ ] Crear Mapper en `data/mappers`
- [ ] Implementar Repository en `data/repositories`
- [ ] Crear Hook en `presentation/hooks`
- [ ] Crear Componentes UI en `presentation/components`
- [ ] Crear Pantallas en `app/`
- [ ] Escribir Tests para Use Cases
- [ ] Documentar en este archivo

---

## 🎯 Conclusión

Esta arquitectura es **perfecta para INOUT** porque:

✅ **No es overkill**: Suficiente estructura sin complejidad extrema
✅ **Profesional**: Sigue principios SOLID y Clean Code
✅ **Mantenible**: Código organizado y predecible
✅ **Testeable**: Puedes hacer tests sin mock de API
✅ **Escalable**: Fácil agregar cuentas, presupuestos, reportes, etc.
✅ **TypeScript first**: Type-safety en todas las capas
✅ **Productivo**: No pierdes tiempo en boilerplate innecesario

---

**Última actualización:** Diciembre 2024
**Versión:** 1.0
**Autor:** Claude Code Assistant
