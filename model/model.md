# Security Service

# Model

```plantuml
interface Lockable {
    +getType()
    +isLocked()
    +lock()
    +unlock()
}

class Window implements Lockable {
    -locked: boolean
}

class Door implements Lockable {
    -locked: boolean
}

class House {
    -name: string

    +House(name: string)
    +addWindow(window: Window)
    +addDoor(door: Door)
    +getDoors(): Doors[]
    +getWindows(): Windows[]
}

class Car {
    -name: string

    +Car(name: string)
    +addDoor(door: Door)
    +getDoors(): Doors[]
}

class SecurityService {
    -reportName: string

    +SecurityService(reportName: string)
    +addLockables(lockables: Lockable[])
    +checkSecurity(): string
}

Car -up-> "2..4" Door: -doors

House -up-> "1..*" Door: -doors
House -up-> "1..*" Window: -windows

SecurityService -up-> Lockable: -lockables
```
