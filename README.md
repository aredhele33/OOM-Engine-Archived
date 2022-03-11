# OOM-Engine

<p align="center">
  <img src="https://github.com/Aredhele/tree/master/Press/Floppy_Bug_Gif_1.gif" />
</p>

#### This is an archived project containing only binaries.

OOM-Engine is a small 2 weeks game engine used to create Floppy-bug, a game created for the Floppy Gam Jam with.
The concept is to create a game that fits into 1.44 MB (basically an HD Floppy), binaries and assets included.
It's quite small and we didn't mean to cheat by downloading heavy binaries at runtime so we created a custom game engine.

The engine was completely written by myself during my studies and the game was created with some friends :

| Name            | Work                              |
|-----------------|-----------------------------------|
| Aurélien Demey  | Sound design                      |
| Darenn Keller   | Game design, Gameplay programming |
| Thomas Garcia   | Game art                          |
| Vincent Calisto | Engine, Gameplay programming      |

The engine, written in C++17, is as light as one 440 KB executable file, with everything embedded.
The game won the design and the quality prices and the second place in gameplay :

<p align="center">
  <img src="https://github.com/Aredhele/tree/master/Press/Floppy_Jam_1.png" width="729" height="181" />
</p>

Third party libraries :

| Name    | Used for            |
|---------|---------------------|
| Glew    | OpenGL loader       |
| Glfw    | Graphic context     |
| Glm     | Maths               |
| DirectX | Direct Sound, Audio |
| qu3e    | OOB physics         |
| ogg     | Audio decoding      |
| vorbis  | Audio decoding      |

The project was compiled in 32 bits using Microsoft Visual Studio 2017 and CMake.

**Game scene example preview**

<p align="center">
  <img src="https://github.com/Aredhele/tree/master/Press/OOM-Engine-Overview.gif" />
</p>

### Features

#### General
* Full entity-component pattern
* Game objects
* A complete SDK

#### Rendering
* Post-processing
    * Post-processing stack
    * Vignette
    * Depth of field
    * Fog
    * FXAA

* Renderer
    * MeshRenderer
    * TextRenderer
    * SpriteRenderer

* Lighting
    * Directional light
    * Point light

* Debug
    * Gizmos (Box, Cone, lights etc.)
    * Lines
    * Rays

#### Audio
* DirectX (Direct Sound)
    * Wav file loading
    * 3D Audio listener
    * 3D Audio source
    * Support of WAV files
    * Support of OGG files

### Physics
* Integration of qu3e
    * Box collider
    * Rigid body
    * Ray cast

## Usage
### Minimal example

```cpp
#include "SDK/SDK.hpp"

int main(int argc, char** argv)
{
    Sdk::Debug::EnableGizmo(EGizmo::Axis);
    Sdk::Debug::EnableGizmo(EGizmo::Grid);
    Sdk::Debug::EnableGizmo(EGizmo::Transform);
    Sdk::Debug::EnableGizmo(EGizmo::AllColliders);

    Sdk::GameObject::CreateFreeCamera();
    Sdk::GameObject::CreateDirectionalLight();

    // Import a mesh
    Sdk::Import::ImportMesh("Resources/Mesh/Mesh_Cube.obj");

    // Create the ground
    auto* p_ground_go   = Sdk::GameObject::CreateGameObjectFromMesh("Cube");
    auto* p_ground_body = p_ground_go->AddComponent<CRigidBody>();
    auto* p_ground_box  = p_ground_go->AddComponent<CBoxCollider>();

    // The ground is a static body
    p_ground_body->SetBodyType(CRigidBody::EBodyType::Static);

    return 0;
}
```

## Demo scenes examples

### Physics demo

<p align="center">
  <img src="https://github.com/Aredhele/tree/master/Press/OOM-Engine-Physics.gif" />
</p>

```cpp
void LoadDemoScenePhysics()
{
    // Enable all gizmo
    // [...]
    
    Sdk::GameObject::CreateDirectionalLight();
    Sdk::Import::ImportMesh("Resources/Mesh/Mesh_Cube.obj");

    auto* p_camera = Sdk::GameObject::CreateFreeCamera();

    auto* p_ground_go_1 = Sdk::GameObject::CreateGameObjectFromMesh("Cube");
    auto* p_ground_go_2 = Sdk::GameObject::CreateGameObjectFromMesh("Cube");
    auto* p_ground_go_3 = Sdk::GameObject::CreateGameObjectFromMesh("Cube");
    auto* p_ground_go_4 = Sdk::GameObject::CreateGameObjectFromMesh("Cube");

    // Static
    p_ground_go_1->GetTransform().SetPosition( 0.8f, 0.0f,  2.0f);
    p_ground_go_2->GetTransform().SetPosition( 2.8f, 0.0f,  5.0f);
    p_ground_go_3->GetTransform().SetPosition(-0.8f, 0.0f, 10.0f);
    p_ground_go_4->GetTransform().SetPosition(-1.8f, 0.0f,  2.0f);

    p_ground_go_1->AddComponent<CBoxCollider>();  
    p_ground_go_2->AddComponent<CBoxCollider>();
    p_ground_go_3->AddComponent<CBoxCollider>();
    p_ground_go_4->AddComponent<CBoxCollider>();

    auto* p_ground_body_1 = p_ground_go_1->AddComponent<CRigidBody>();
    auto* p_ground_body_2 = p_ground_go_2->AddComponent<CRigidBody>();
    auto* p_ground_body_3 = p_ground_go_3->AddComponent<CRigidBody>();
    auto* p_ground_body_4 = p_ground_go_4->AddComponent<CRigidBody>();

    // Static body
    p_ground_body_1->SetBodyType(CRigidBody::EBodyType::Static);
    p_ground_body_2->SetBodyType(CRigidBody::EBodyType::Static);
    p_ground_body_3->SetBodyType(CRigidBody::EBodyType::Static);
    p_ground_body_4->SetBodyType(CRigidBody::EBodyType::Static);

    p_ground_body_2->SetOrientationY(55.0f);
    p_ground_body_4->SetOrientationY(-50.0f);
    
    // Controller
    auto* p_controller = Sdk::GameObject::CreateEmpty();
    p_controller->AddComponent<S_Spawner>();
}
```

### Audio demo

<p align="center">
  <img src="https://github.com/Aredhele/tree/master/Press/OOM-Engine-Audio.gif" />
</p>

```cpp
void LoadDemoSceneAudio()
{
    // Enable all gizmo
    // [...]
    
    auto* p_camera = Sdk::GameObject::CreateCamera();
    p_camera->AddComponent<S_CameraRotator>();

    // Loads 2 sounds from a file
    buffer_1.LoadFromFile("Resources/Music/music_floppy_demo_1.ogg");
    buffer_2.LoadFromFile("Resources/Music/music_floppy_demo_2.ogg");

    // Creates 2 game objets with audio source attatched
    auto* p_source_1_go = Sdk::GameObject::CreateAudioSource3D();
    auto* p_source_2_go = Sdk::GameObject::CreateAudioSource3D();

    p_source_1_go->GetTransform().SetPosition(-8.0f, 0.0f, 0.0f);
    p_source_2_go->GetTransform().SetPosition( 8.0f, 0.0f, 0.0f);

    // Gets the audio components
    auto* p_source_1 = p_source_1_go->GetComponent<CAudioSource3D>();
    auto* p_source_2 = p_source_2_go->GetComponent<CAudioSource3D>();

    // Gives the audio buffer resource to the source
    p_source_1->SetAudioBuffer(&buffer_1);
    p_source_2->SetAudioBuffer(&buffer_2);

    p_source_1->SetMinDistance(1.0f);
    p_source_1->SetMaxDistance(5.0f);
    p_source_1->Play();

    p_source_2->SetMinDistance(1.0f);
    p_source_2->SetMaxDistance(5.0f);
    p_source_2->Play();
}
```