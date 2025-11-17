# Adapter Pattern - Weight Conversion System

**Type**: Structural Design Pattern

## Definition

**Adapter Pattern**: Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

**Purpose**: Bridge the gap between incompatible interfaces by wrapping an existing class with a new interface.

**Problem**: 
- Client expects weight in **kilograms (metric)**
- Existing system returns weight in **pounds (imperial)**
- Cannot modify the existing Imperial weighing machine

**Solution**: 
Create an adapter that converts pounds to kilograms, allowing client to work with the Imperial machine through a metric interface.

---

## UML Diagrams

### 1. Complete Class Diagram

```mermaid
classDiagram
    %% ========== CLIENT ==========
    class Client {
        +main(String[] args)$
    }
    
    %% ========== TARGET INTERFACE ==========
    class WeighingMachineAdapter {
        <<interface>>
        +getWeightInKg(): double*
    }
    
    %% ========== ADAPTEE INTERFACE ==========
    class ImperialWeighingMachine {
        <<interface>>
        +getWeightInPounds(): double*
    }
    
    %% ========== ADAPTEE IMPLEMENTATION ==========
    class ImperialWeighingMachineImpl {
        -double weightInPounds
        +ImperialWeighingMachineImpl(weightInPounds: double)
        +getWeightInPounds(): double
    }
    
    %% ========== ADAPTER ==========
    class WeightMachineAdapterImpl {
        -ImperialWeighingMachine imperialMachine
        +WeightMachineAdapterImpl(imperialMachine: ImperialWeighingMachine)
        +getWeightInKg(): double
    }
    
    %% ========== RELATIONSHIPS ==========
    Client ..> WeighingMachineAdapter : uses
    Client ..> ImperialWeighingMachineImpl : creates
    
    WeighingMachineAdapter <|.. WeightMachineAdapterImpl : implements
    ImperialWeighingMachine <|.. ImperialWeighingMachineImpl : implements
    
    WeightMachineAdapterImpl o-- ImperialWeighingMachine : adapts
    
    note for Client "Client expects\nweight in KG"
    note for WeighingMachineAdapter "Target Interface\nProvides metric units"
    note for ImperialWeighingMachine "Adaptee Interface\nExisting system"
    note for WeightMachineAdapterImpl "Adapter converts\npounds to kg\nusing 0.453592 factor"
```

---

### 2. Adapter Pattern Structure (Generic)

```mermaid
classDiagram
    class Client {
        +operation()
    }
    
    class Target {
        <<interface>>
        +request()*
    }
    
    class Adapter {
        -Adaptee adaptee
        +request()
    }
    
    class Adaptee {
        +specificRequest()
    }
    
    Client ..> Target : uses
    Target <|.. Adapter : implements
    Adapter o-- Adaptee : adapts
    
    note for Target "Interface expected\nby client"
    note for Adaptee "Existing incompatible\ninterface"
    note for Adapter "Converts Target calls\nto Adaptee calls"
```

---

### 3. Sequence Diagram - Weight Conversion Flow

```mermaid
sequenceDiagram
    participant Client
    participant Adapter as WeightMachineAdapterImpl
    participant Adaptee as ImperialWeighingMachineImpl
    
    Note over Client,Adaptee: Weight Conversion Process
    
    Client->>Adaptee: new ImperialWeighingMachineImpl(25.0)
    activate Adaptee
    Adaptee->>Adaptee: weightInPounds = 25.0
    Adaptee-->>Client: imperialMachine
    deactivate Adaptee
    
    Client->>Adapter: new WeightMachineAdapterImpl(imperialMachine)
    activate Adapter
    Adapter->>Adapter: this.imperialMachine = imperialMachine
    Adapter-->>Client: adapter
    deactivate Adapter
    
    Client->>Adapter: getWeightInKg()
    activate Adapter
    
    Adapter->>Adaptee: getWeightInPounds()
    activate Adaptee
    Adaptee-->>Adapter: 25.0 pounds
    deactivate Adaptee
    
    Adapter->>Adapter: convert: 25.0 * 0.453592
    Note over Adapter: Conversion: 11.3398 kg
    
    Adapter-->>Client: 11.3398 kg
    deactivate Adapter
    
    Client->>Client: Display result
```

---

### 4. Component Interaction Diagram

```mermaid
flowchart LR
    Client[Client<br/>Expects KG]
    
    Target[Target Interface<br/>WeighingMachineAdapter<br/>getWeightInKg]
    
    Adapter[Adapter<br/>WeightMachineAdapterImpl<br/>Conversion Logic]
    
    Adaptee[Adaptee<br/>ImperialWeighingMachine<br/>getWeightInPounds]
    
    Client -->|Uses| Target
    Target -.->|Implemented by| Adapter
    Adapter -->|Wraps| Adaptee
    Adaptee -->|Returns| Pounds[25 pounds]
    Adapter -->|Converts| KG[11.34 kg]
    KG -->|Returns to| Client
    
    style Client fill:#e3f2fd
    style Target fill:#fff3e0
    style Adapter fill:#c8e6c9
    style Adaptee fill:#ffccbc
    style Pounds fill:#ffebee
    style KG fill:#e8f5e9
```

---

### 5. Pattern Participants Diagram

```mermaid
graph TB
    subgraph "Adapter Pattern Participants"
        Client[Client<br/>- Uses Target interface<br/>- Unaware of Adaptee]
        Target[Target Interface<br/>- Defines domain-specific interface<br/>- getWeightInKg]
        Adapter[Adapter<br/>- Implements Target interface<br/>- Wraps Adaptee<br/>- Performs conversion]
        Adaptee[Adaptee<br/>- Existing interface<br/>- getWeightInPounds<br/>- Cannot be modified]
    end
    
    Client -->|depends on| Target
    Adapter -->|implements| Target
    Adapter -->|contains| Adaptee
    
    style Client fill:#4CAF50,color:#fff
    style Target fill:#2196F3,color:#fff
    style Adapter fill:#FF9800,color:#fff
    style Adaptee fill:#F44336,color:#fff
```

---

### 6. Object Diagram - Runtime Instance

```mermaid
graph LR
    subgraph "Client Layer"
        CL[client: Client]
    end
    
    subgraph "Adapter Instance"
        AD["adapter: WeightMachineAdapterImpl<br/>imperialMachine reference"]
    end
    
    subgraph "Adaptee Instance"
        IM["imperialMachine: ImperialWeighingMachineImpl<br/>weightInPounds = 25.0"]
    end
    
    CL -->|uses| AD
    CL -.->|creates| IM
    AD -->|has-a| IM
    
    style CL fill:#e1f5ff
    style AD fill:#ffe1f5
    style IM fill:#fff4e1
```

---

## Pattern Components

| Component | Role | Description |
|-----------|------|-------------|
| **Client** | User of Target interface | Expects weight in kilograms |
| **Target** | WeighingMachineAdapter | Interface client expects (metric) |
| **Adapter** | WeightMachineAdapterImpl | Converts pounds to kilograms |
| **Adaptee** | ImperialWeighingMachine | Existing system (imperial) |

---

## Key Relationships

| Relationship | Type | Description |
|--------------|------|-------------|
| Client → Target | Dependency | Client uses target interface |
| Adapter → Target | Implementation | Adapter implements target |
| Adapter → Adaptee | Composition | Adapter wraps adaptee |

---

## Conversion Formula

```
Weight in Kilograms = Weight in Pounds × 0.453592

Example:
25 pounds × 0.453592 = 11.3398 kilograms
```
