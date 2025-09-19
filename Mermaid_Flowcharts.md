# Mermaid/Gamma Flowchart Code for Blockchain Document Verification

## 1. System Architecture Flowchart

```mermaid
graph TD
    A[User] --> B[Web Interface]
    B --> C{User Role}

    C -->|Admin| D[Admin Panel]
    C -->|Exporter| E[Document Upload]
    C -->|Verifier| F[Document Verification]

    D --> G[Add/Remove Exporters]
    D --> H[Manage Permissions]
    G --> I[Smart Contract]
    H --> I

    E --> J[Select Document]
    J --> K[Encrypt Document]
    K --> L[Upload to IPFS]
    L --> M[Generate Hash]
    M --> N[Store Hash on Blockchain]
    N --> O[Generate QR Code]

    F --> P[Upload Document/Scan QR]
    P --> Q[Calculate Hash]
    Q --> R[Query Blockchain]
    R --> S{Hash Match?}
    S -->|Yes| T[Document Verified ✅]
    S -->|No| U[Document Invalid ❌]

    I --> V[Ethereum Blockchain]
    L --> W[IPFS Network]

    style A fill:#e1f5fe
    style V fill:#f3e5f5
    style W fill:#e8f5e8
    style T fill:#c8e6c9
    style U fill:#ffcdd2
```

## 2. Document Upload Process Flow

```mermaid
sequenceDiagram
    participant User
    participant WebApp
    participant MetaMask
    participant IPFS
    participant Blockchain

    User->>WebApp: Select Document
    WebApp->>WebApp: Validate File
    WebApp->>WebApp: Encrypt Document
    WebApp->>IPFS: Upload Encrypted Document
    IPFS-->>WebApp: Return IPFS Hash
    WebApp->>WebApp: Generate Document Hash
    WebApp->>MetaMask: Request Transaction
    MetaMask->>User: Confirm Transaction
    User->>MetaMask: Approve
    MetaMask->>Blockchain: Send Transaction
    Blockchain-->>WebApp: Transaction Receipt
    WebApp->>WebApp: Generate QR Code
    WebApp->>User: Display Success + QR
```

## 3. Document Verification Process Flow

```mermaid
sequenceDiagram
    participant User
    participant WebApp
    participant Blockchain
    participant IPFS

    User->>WebApp: Upload Document/Scan QR
    WebApp->>WebApp: Calculate Document Hash
    WebApp->>Blockchain: Query Document Hash
    Blockchain-->>WebApp: Return Stored Data

    alt Hash Found
        WebApp->>WebApp: Compare Hashes
        alt Hashes Match
            WebApp->>IPFS: Retrieve Original (Optional)
            IPFS-->>WebApp: Return Document
            WebApp->>User: Document Verified ✅
        else Hashes Don't Match
            WebApp->>User: Document Tampered ❌
        end
    else Hash Not Found
        WebApp->>User: Document Not Registered ❌
    end
```

## 4. Smart Contract Function Flow

```mermaid
graph TD
    A[Smart Contract: Verification.sol] --> B[Owner Functions]
    A --> C[Exporter Functions]
    A --> D[Document Functions]

    B --> B1[add_Exporter]
    B --> B2[delete_Exporter]
    B --> B3[alter_Exporter]
    B --> B4[changeOwner]

    C --> C1[canAddHash modifier]
    C --> C2[authorised_Exporter modifier]

    D --> D1[addDocHash]
    D --> D2[findDocHash]
    D --> D3[deleteHash]

    B1 --> E[Update Exporter Mapping]
    B2 --> E
    B3 --> E
    D1 --> F[Update Document Mapping]
    D3 --> F

    E --> G[Blockchain State Update]
    F --> G

    style A fill:#e3f2fd
    style B fill:#fff3e0
    style C fill:#f1f8e9
    style D fill:#fce4ec
```

## 5. User Role-Based Access Flow

```mermaid
graph TD
    A[User Connects Wallet] --> B{Check Address}

    B -->|Owner Address| C[Admin Access]
    B -->|Authorized Exporter| D[Exporter Access]
    B -->|Other Address| E[Verifier Access]

    C --> F[Manage Exporters]
    C --> G[View All Documents]
    C --> H[System Administration]

    D --> I[Upload Documents]
    D --> J[Generate QRs]
    D --> K[Delete Own Documents]

    E --> L[Verify Documents]
    E --> M[View Public Info]

    F --> N[Smart Contract Call]
    I --> N
    K --> N
    L --> O[Blockchain Query]

    style C fill:#ffebee
    style D fill:#e8f5e8
    style E fill:#e3f2fd
```

## 6. Data Flow Architecture

```mermaid
graph LR
    A[Frontend Web App] --> B[MetaMask Wallet]
    A --> C[IPFS Gateway]
    A --> D[Smart Contract]

    B --> E[User Authentication]
    B --> F[Transaction Signing]

    C --> G[Document Storage]
    C --> H[Document Retrieval]

    D --> I[Hash Storage]
    D --> J[Access Control]
    D --> K[Event Logging]

    E --> L[Ethereum Network]
    F --> L
    I --> L
    J --> L
    K --> L

    G --> M[IPFS Network]
    H --> M

    style A fill:#e1f5fe
    style L fill:#f3e5f5
    style M fill:#e8f5e8
```

## 7. Security Layer Flow

```mermaid
graph TD
    A[Document Input] --> B[Input Validation]
    B --> C[File Type Check]
    C --> D[Size Limit Check]
    D --> E[Malware Scan]
    E --> F[Encryption Process]
    F --> G[Hash Generation]
    G --> H[Digital Signature]
    H --> I[Blockchain Storage]

    J[Verification Request] --> K[Hash Calculation]
    K --> L[Blockchain Query]
    L --> M[Hash Comparison]
    M --> N[Signature Verification]
    N --> O[Access Control Check]
    O --> P[Result Output]

    style F fill:#ffebee
    style G fill:#ffebee
    style H fill:#ffebee
    style N fill:#e8f5e8
    style O fill:#e8f5e8
```

## 8. System Component Integration

```mermaid
graph TB
    subgraph "Frontend Layer"
        A[HTML/CSS/JS]
        B[Bootstrap UI]
        C[Web3.js]
    end

    subgraph "Blockchain Layer"
        D[MetaMask]
        E[Smart Contract]
        F[Ethereum Network]
    end

    subgraph "Storage Layer"
        G[IPFS Network]
        H[Infura Gateway]
    end

    subgraph "Security Layer"
        I[Encryption]
        J[Hash Functions]
        K[Digital Signatures]
    end

    A --> D
    C --> E
    D --> F
    E --> F
    A --> H
    H --> G
    A --> I
    I --> J
    J --> K

    style A fill:#e3f2fd
    style E fill:#f3e5f5
    style G fill:#e8f5e8
    style I fill:#fff3e0
```

## Usage Instructions:

1. Copy any of the above Mermaid code blocks
2. Paste them into:

   - Mermaid Live Editor (https://mermaid.live/)
   - GitHub README files
   - GitLab documentation
   - Notion pages
   - Any Mermaid-supported platform

3. The diagrams will render automatically as flowcharts
4. You can customize colors, styling, and layout as needed
