# Architecture

- test
    - sub 

- Target interfaces
    - Target (Storage + TagResolver)
    - GraphTarget (Target + PredecessorFinder)
    - ReadonlyTarget
    - ReadonlyGraphTarget

- Storage Interfaces (CAS)
    - Fetch
    - Exist
    - Push

- TagResolver Interfaces
    - Resolve
    - Tag

- Content Stores
    - File
    - OCI
    - Memory
    - Repository

- Top-level methods
    - Copy
    - Extended Copy
    - Pack, Resolve, etc. (omit)

```mermaid

classDiagram

TagResolver --> Tagger : embedded
TagResolver --> Resolver

ReadonlyStorage --> Fetcher
ReadonlyStorage --> Pusher
Storage --> ReadonlyStorage
Storage --> Pusher

ReadonlyGraphStorage --> ReadonlyStorage
ReadonlyGraphStorage --> PredecessorFinder
GraphStorage --> Storage
GraphStorage --> PredecessorFinder

ReadonlyTarget --> ReadonlyStorage
ReadonlyTarget --> Resolver
ReadonlyGraphTarget --> ReadonlyGraphStorage
ReadonlyGraphTarget --> Resolver
Target --> Storage
Target --> TagResolver
GraphTarget --> GraphStorage
GraphTarget --> TagResolver

Memory ..|> GraphTarget
OCI ..|> GraphTarget
File ..|> GraphTarget
Repository ..|> GraphTarget


class Fetcher{
    <<interface>>
    Fetch()
}

class Pusher{
    <<interface>>
    Push()
}

class Tagger{
    <<interface>>
    Tag()
}

class Resolver{
    <<interface>>
    Resolve()
}

class TagResolver{
    <<interface>>
    Tagger
    Resolver
}

class PredecessorFinder{
    <<interface>>
    Predecessors()
}

class ReadonlyStorage{
    <<interface>>
    Fetcher
    Pusher
    Exists()
}

class ReadonlyGraphStorage{
    <<interface>>
    ReadonlyStorage
    PredecessorFinder
}

class Storage{
    <<interface>>
    ReadonlyStorage
    Pusher
}

class GraphStorage{
    <<interface>>
    Storage
    PredecessorFinder
}

class ReadonlyTarget{
    <<interface>>
    ReadonlyStorage
    TagResolver
}

class ReadonlyGraphTarget{
    <<interface>>
    ReadonlyGraphStorage
    TagResolver
}

class Target{
    <<interface>>
    Storage
    Resolver
}

class GraphTarget{
    <<interface>>
    GraphStorage
    TagResolver
}

class Memory
class OCI
class File
class Repository

```