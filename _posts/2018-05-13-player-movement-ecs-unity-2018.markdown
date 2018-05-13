---
layout: post
title: "Player Movement System in Unity 2018 - Using ECS"
caterogies:
  - programming
---

**Firstly:** I'm truly sorry. I'm far away behind the schedule with the [AI Decision System](http://www.vhasselmann.me/programming/2018/03/06/ai-programming-series.html){:target="_blank"}. I have been busy with work priorities and other personal life stuff. I will be back in track with the series as soon as possible, but as long as I do not have enough time to do something cool with good practical examples, I decided to do a spin-off post about Unity's Entity Component System.

> Unity is introducing a new high performance multithreaded system, that will make it possible for your game to fully utilise the multicore processors available today without heavy programming headache. (check [more information about Unity's ECS](https://unity3d.com/unity/features/job-system-ECS){:target="_blank"})

> The [repository](https://github.com/vichasselmann/jobsystem-playermovement){:target="_blank"} for this article's sample is already available.

> In order to get this ECS working with your Unity 2018.1, you will need to replace project's manifest.json file with the following code:

{% highlight JavaScript %}
{
        "dependencies":{
                "com.unity.entities":"0.0.11"
        },
        "testables": [
        "com.unity.collections",
        "com.unity.entities",
        "com.unity.jobs"
        ],
        "registry": "https://staging-packages.unity.com"
}
{% endhighlight %}

# Practical Sample

This new ECS is pretty good to create awesome components for your game. :)

<img src="https://media.giphy.com/media/Yjv7rpgyuNTRGgs5Vg/giphy.gif">

# Components.cs

{% highlight C# %}
public struct PlayerInput : IComponentData
{
    public float Horizontal;
    public float Vertical;
}
{% endhighlight %}

The struct above will define our Player Input data which will be used by our Player Input System's Job to update **horizontal** and **vertical** values.

# Breaking down Player Input System's Job

This class represents our entire Player Input System; Firstly, the data needed to store user's input values is defined, then we create **Execute** method which will be responsible to update our values. Then, we override **OnUpdate** method and create a new instance for our job and return its execution scheduled.

> Note that when inheriting IJobProcessComponentData < PlayerInput > we get a PlayerInput reference and work with its reference when updating the values.

{% highlight C# %}
using Unity.Entities;
using Unity.Jobs;
using UnityEngine;

public class PlayerInputSystem : JobComponentSystem
{
    public struct PlayerInputJob : IJobProcessComponentData<PlayerInput>
    {
        public bool Left;
        public bool Right;
        public bool Up;
        public bool Down;

        public bool UpLeft;
        public bool UpRight;
        public bool DownLeft;
        public bool DownRight;

        public void Execute(ref PlayerInput input)
        {
            input.Horizontal = Left || UpLeft || DownLeft ? -1f : Right || UpRight || DownRight ? 1f : 0f;
            input.Vertical = Down || DownLeft || DownRight ? -1f : Up || UpRight || UpLeft ? 1f : 0f;
        }
    }

    protected override JobHandle OnUpdate(JobHandle inputDeps)
    {
        var job = new PlayerInputJob
        {

            Left = Input.GetKeyDown(KeyCode.A),
            Right = Input.GetKeyDown(KeyCode.D),
            Up = Input.GetKeyDown(KeyCode.W),
            Down = Input.GetKeyDown(KeyCode.S),

            UpLeft = Input.GetKeyDown(KeyCode.Q),
            UpRight = Input.GetKeyDown(KeyCode.E),
            DownLeft = Input.GetKeyDown(KeyCode.Less),
            DownRight = Input.GetKeyDown(KeyCode.X),
        };

        return job.Schedule(this, 1, inputDeps);
    }
}
{% endhighlight %}

This system is simple and really easy to understand. Now we can move forward and have our **Player Movement System** to consume the data from **Player Input** and update its own position.

> Note that for each JobComponentSystem we have to schedule and return the job in order to get it working properly.

# Player Movement System

{% highlight C# %}
public class PlayerMovementSystem : JobComponentSystem
{
    public struct PlayerInputJob : IJobProcessComponentData<Position, PlayerInput>
    {
        public void Execute(ref Position position, ref PlayerInput playerInput)
        {
            position.Value.x += playerInput.Horizontal;
            position.Value.y += playerInput.Vertical;
        }
    }

    protected override JobHandle OnUpdate(JobHandle inputDeps)
    {
        var job = new PlayerInputJob { };

        return job.Schedule(this, 1, inputDeps);
    }
}
{% endhighlight %}

Last but not least, our **Controller** will be responsible to create an Player Archetype containing the needed components and be handled by an Entity Manager.

{% highlight C# %}
using Unity.Entities;
using Unity.Rendering;
using Unity.Transforms;
using UnityEngine;

public class Controller : MonoBehaviour
{
    public Mesh PlayerMesh;
    public Material PlayerMaterial;

    [RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.AfterSceneLoad)]
    private void Start()
    {
        var entityManager = World.Active.GetOrCreateManager<EntityManager>();

        var playerArchetype = entityManager.CreateArchetype
        (
            typeof(TransformMatrix),
            typeof(Position),
            typeof(MeshInstanceRenderer),
            typeof(PlayerInput)
        );

        var player = entityManager.CreateEntity(playerArchetype);
        entityManager.SetSharedComponentData
        (
            player,
            new MeshInstanceRenderer
            {
                mesh = PlayerMesh,
                material = PlayerMaterial
            }
        );
    }
}
{% endhighlight %}
