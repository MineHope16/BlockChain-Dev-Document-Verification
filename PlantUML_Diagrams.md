# PlantUML Code for Blockchain Document Verification System

## 1. System Architecture Diagram

```plantuml
@startuml SystemArchitecture
!define RECTANGLE class
!theme plain

package "Frontend Layer" {
    [Web Application] as WebApp
    [MetaMask Integration] as MetaMask
    [User Interface] as UI
}

package "Blockchain Layer" {
    [Smart Contract] as Contract
    [Ethereum Network] as Ethereum
    [Web3.js Library] as Web3
}

package "Storage Layer" {
    [IPFS Network] as IPFS
    [Infura Gateway] as Infura
    [Document Storage] as DocStorage
}

package "Security Layer" {
    [Encryption Module] as Encryption
    [Hash Generator] as HashGen
    [Digital Signatures] as Signatures
}

WebApp --> MetaMask
WebApp --> Web3
Web3 --> Contract
Contract --> Ethereum
WebApp --> Infura
Infura --> IPFS
WebApp --> Encryption
Encryption --> HashGen
HashGen --> Signatures

@enduml
```

## 2. Class Diagram - Smart Contract Structure

```plantuml
@startuml ContractClassDiagram
!theme plain

class Verification {
    +address owner
    +uint16 count_Exporters
    +uint16 count_hashes

    +constructor()
    +add_Exporter(address _add, string _info)
    +delete_Exporter(address _add)
    +alter_Exporter(address _add, string _newInfo)
    +changeOwner(address _newOwner)
    +addDocHash(bytes32 hash, string _ipfs)
    +findDocHash(bytes32 _hash) : (uint, uint, string, string)
    +deleteHash(bytes32 _hash)
    +getExporterInfo(address _add) : string
}

class Record {
    +uint blockNumber
    +uint minetime
    +string info
    +string ipfs_hash
}

class Exporter_Record {
    +uint blockNumber
    +string info
}

Verification --> Record : "docHashes mapping"
Verification --> Exporter_Record : "Exporters mapping"

note right of Verification
    Modifiers:
    - onlyOwner()
    - validAddress()
    - authorised_Exporter()
    - canAddHash()
end note

@enduml
```

## 3. Sequence Diagram - Document Upload Process

```plantuml
@startuml DocumentUpload
!theme plain

actor User
participant "Web App" as WebApp
participant MetaMask
participant "IPFS\nInfura" as IPFS
participant "Smart Contract" as Contract
participant "Ethereum\nBlockchain" as Blockchain

User -> WebApp : Select Document
WebApp -> WebApp : Validate File Type & Size
WebApp -> WebApp : Encrypt Document
WebApp -> IPFS : Upload Encrypted Document
IPFS --> WebApp : Return IPFS Hash
WebApp -> WebApp : Generate Document Hash (SHA-256)
WebApp -> MetaMask : Request Transaction Approval
MetaMask -> User : Show Transaction Details
User --> MetaMask : Approve Transaction
MetaMask -> Contract : Call addDocHash()
Contract -> Contract : Validate Exporter Permission
Contract -> Contract : Store Hash & IPFS Reference
Contract -> Blockchain : Write to Blockchain
Blockchain --> Contract : Transaction Receipt
Contract --> WebApp : Event: addHash()
WebApp -> WebApp : Generate QR Code
WebApp --> User : Display Success + QR Code

note right of WebApp
    QR Code contains:
    - Document Hash
    - IPFS Hash
    - Timestamp
    - Exporter Info
end note

@enduml
```

## 4. Sequence Diagram - Document Verification Process

```plantuml
@startuml DocumentVerification
!theme plain

actor User
participant "Web App" as WebApp
participant "Smart Contract" as Contract
participant "IPFS\nInfura" as IPFS
participant "Ethereum\nBlockchain" as Blockchain

User -> WebApp : Upload Document/Scan QR Code

alt Upload Document
    WebApp -> WebApp : Calculate Document Hash
else Scan QR Code
    WebApp -> WebApp : Extract Hash from QR
end

WebApp -> Contract : Call findDocHash()
Contract -> Blockchain : Query Document Hash
Blockchain --> Contract : Return Record Data

alt Hash Found
    Contract --> WebApp : Return (blockNumber, timestamp, info, ipfs_hash)
    WebApp -> WebApp : Compare Calculated vs Stored Hash

    alt Hashes Match
        WebApp -> IPFS : Retrieve Original Document (Optional)
        IPFS --> WebApp : Return Document Data
        WebApp --> User : ✅ Document Verified\nTimestamp: [timestamp]\nExporter: [info]
    else Hashes Don't Match
        WebApp --> User : ❌ Document Modified/Corrupted
    end

else Hash Not Found
    Contract --> WebApp : Return Empty Record
    WebApp --> User : ❌ Document Not Registered
end

@enduml
```

## 5. Activity Diagram - Admin Workflow

```plantuml
@startuml AdminWorkflow
!theme plain

|Admin|
start
:Connect MetaMask Wallet;
:Verify Owner Address;

if (Is Owner?) then (yes)
    :Access Admin Panel;

    repeat
        :Choose Admin Action;

        switch (Action Type?)
        case (Add Exporter)
            :Enter Exporter Address;
            :Enter Exporter Info;
            :Call add_Exporter();
            :Wait for Transaction;
            :Confirm Addition;
        case (Remove Exporter)
            :Select Exporter Address;
            :Call delete_Exporter();
            :Wait for Transaction;
            :Confirm Removal;
        case (Update Exporter)
            :Select Exporter Address;
            :Enter New Info;
            :Call alter_Exporter();
            :Wait for Transaction;
            :Confirm Update;
        case (Transfer Ownership)
            :Enter New Owner Address;
            :Call changeOwner();
            :Wait for Transaction;
            :Confirm Transfer;
        case (View Statistics)
            :Display count_Exporters;
            :Display count_hashes;
            :Show Recent Transactions;
        endswitch

    repeat while (Continue Managing?) is (yes)

else (no)
    :Show Access Denied;
endif

:Disconnect Wallet;
stop

@enduml
```

## 6. Component Diagram - System Components

```plantuml
@startuml ComponentDiagram
!theme plain

package "User Interface" {
    [index.html] as IndexPage
    [upload.html] as UploadPage
    [verify.html] as VerifyPage
    [admin.html] as AdminPage
    [about.html] as AboutPage
}

package "JavaScript Modules" {
    [App.js] as AppJS
    [script.js] as ScriptJS
    [Encryption.js] as EncryptionJS
}

package "CSS Styles" {
    [main.css] as MainCSS
    [bootstrap.min.css] as BootstrapCSS
    [darkmode.css] as DarkModeCSS
}

package "External Libraries" {
    [Web3.js] as Web3
    [QRCode.js] as QRCode
    [AOS.js] as AOS
}

package "Blockchain" {
    [Verification.sol] as SmartContract
    [MetaMask] as Wallet
}

package "Storage" {
    [IPFS Network] as IPFSNet
    [Infura Gateway] as InfuraAPI
}

IndexPage --> AppJS
UploadPage --> AppJS
VerifyPage --> AppJS
AdminPage --> AppJS

AppJS --> Web3
AppJS --> QRCode
AppJS --> EncryptionJS
AppJS --> InfuraAPI

Web3 --> SmartContract
Web3 --> Wallet

AppJS --> IPFSNet
InfuraAPI --> IPFSNet

IndexPage --> MainCSS
IndexPage --> BootstrapCSS
IndexPage --> DarkModeCSS
IndexPage --> AOS

@enduml
```

## 7. Use Case Diagram

```plantuml
@startuml UseCaseDiagram
!theme plain

left to right direction

actor "System Admin" as Admin
actor "Document Exporter" as Exporter
actor "Document Verifier" as Verifier
actor "General User" as User

rectangle "Blockchain Document Verification System" {
    usecase "Manage Exporters" as UC1
    usecase "Upload Documents" as UC2
    usecase "Verify Documents" as UC3
    usecase "Generate QR Codes" as UC4
    usecase "View Document History" as UC5
    usecase "Delete Documents" as UC6
    usecase "Transfer Ownership" as UC7
    usecase "Browse System Info" as UC8
    usecase "Connect Wallet" as UC9
    usecase "Encrypt Documents" as UC10
}

Admin --> UC1
Admin --> UC7
Admin --> UC5

Exporter --> UC2
Exporter --> UC4
Exporter --> UC6
Exporter --> UC5
Exporter --> UC10

Verifier --> UC3
Verifier --> UC5

User --> UC8
User --> UC9

UC2 ..> UC10 : includes
UC2 ..> UC4 : includes
UC3 ..> UC9 : includes
UC2 ..> UC9 : includes
UC6 ..> UC9 : includes

@enduml
```

## 8. State Diagram - Document Lifecycle

```plantuml
@startuml DocumentState
!theme plain

[*] --> Created : Document Selected

Created --> Validated : File Type & Size Check
Validated --> Encrypted : Encryption Process
Encrypted --> Uploaded : IPFS Upload
Uploaded --> Hashed : Hash Generation
Hashed --> Pending : Blockchain Transaction
Pending --> Registered : Transaction Confirmed
Registered --> QRGenerated : QR Code Created

QRGenerated --> Verified : Verification Request
Verified --> Valid : Hash Match
Verified --> Invalid : Hash Mismatch
Verified --> NotFound : Hash Not Registered

Valid --> [*]
Invalid --> [*]
NotFound --> [*]

Registered --> Deleted : Delete Request
Deleted --> [*]

note right of Registered
    Document is immutably
    stored on blockchain
end note

note right of Verified
    Multiple verification
    attempts possible
end note

@enduml
```

## 9. Deployment Diagram

```plantuml
@startuml DeploymentDiagram
!theme plain

node "User Browser" {
    artifact "Web Application"
    artifact "MetaMask Extension"
}

cloud "IPFS Network" {
    node "IPFS Node 1"
    node "IPFS Node 2"
    node "IPFS Node N"
    database "Distributed Storage"
}

cloud "Ethereum Network" {
    node "Ethereum Node 1"
    node "Ethereum Node 2"
    node "Ethereum Node N"
    database "Blockchain"
}

node "Infura Gateway" {
    artifact "IPFS API"
    artifact "Ethereum API"
}

node "Web Server" {
    artifact "HTML/CSS/JS Files"
    artifact "Static Assets"
}

"Web Application" --> "MetaMask Extension"
"Web Application" --> "IPFS API"
"Web Application" --> "Ethereum API"
"IPFS API" --> "IPFS Node 1"
"Ethereum API" --> "Ethereum Node 1"

@enduml
```

## 10. Package Diagram - Code Organization

```plantuml
@startuml PackageDiagram
!theme plain

package "Root" {
    package "Contract" {
        class Verification.sol
    }

    package "js" {
        class App.js
        class script.js
        class Encryption.js
        class "External Libraries" {
            class aos.js
            class bootstrap.min.js
            class qrcode.min.js
            class purecounter.min.js
        }
    }

    package "css" {
        class main.css
        class bootstrap.min.css
        class darkmode.css
        class loader.css
        class aos.min.css
    }

    package "assets" {
        package "images" {
            class "*.jpg"
            class "*.png"
            class "*.svg"
        }
    }

    package "files" {
        class "Media Files"
        class "Icons"
        class "Animations"
    }

    package "HTML Pages" {
        class index.html
        class upload.html
        class verify.html
        class admin.html
        class about.html
    }
}

App.js --> Verification.sol : "Smart Contract Calls"
"HTML Pages" --> App.js : "JavaScript Integration"
"HTML Pages" --> main.css : "Styling"
"HTML Pages" --> "External Libraries" : "UI Components"

@enduml
```

## Usage Instructions:

1. **Copy any PlantUML code block**
2. **Paste into PlantUML editors:**

   - PlantUML Online Server (http://www.plantuml.com/plantuml)
   - VS Code PlantUML Extension
   - IntelliJ IDEA PlantUML plugin
   - Draw.io (supports PlantUML)

3. **Integration Options:**

   - GitHub/GitLab README files
   - Confluence pages
   - Documentation systems
   - Presentation slides

4. **Customization:**
   - Modify colors with `!theme` directive
   - Add/remove components as needed
   - Adjust layout and styling
   - Export as PNG, SVG, or PDF
