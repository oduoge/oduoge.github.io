---
title: 'FPS C++ '
date: 2019-05-07 22:58:34
tags:
- UE4
categories: 
- Game
toc: true
---

记录`UE4`中如何使用`C++`控制`Actor`

<!----more ---->

使用<code>UGameplayStatics</code>需要引入头文件<code>Kismet/GameplayStatics.h</code>

<!-----more ---------------->
```c++
#include "Kismet/GameplayStatics.h"


void AFPSCppProjectile::OnHit(
    UPrimitiveComponent* HitComp,
    AActor* OtherActor,
    UPrimitiveComponent* OtherComp,
    FVector NormalImpulse,
    const FHitResult& Hit)
{
        if ((OtherActor != NULL) && (OtherActor != this) && (OtherComp != NULL) && OtherComp->IsSimulatingPhysics())
        {
            OtherComp->AddImpulseAtLocation(GetVelocity() * 100.0f, GetActorLocation());
            Destroy();
        }

        UGameplayStatics::SpawnEmitterAtLocation(GetWorld(), ExplosionEffect, GetActorLocation());
}
```

## 创建组件

一个<code>Actor</code>中的添加多个组件时需要指定一个根组件。

```c++
// .h
UPROPERTY(VisibleAnywhere, Category = "Components")
class UStaticMeshComponent* MeshComp;

UPROPERTY(VisibleAnywhere, Category = "Components")
class USphereComponent* SphereComp;

```

```c++
// .cpp

MeshComp = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("MeshComp"));
RootComponent = MeshComp;

SphereComp = CreateDefaultSubobject<USphereComponent>(TEXT("SphereComp"));
SphereComp->SetupAttachment(MeshComp);
```

头文件中不要添加额外的包，其会加长编译的时间，可通过声明的方式解决编译时的报错

### 设置碰撞

```c++
// 无碰撞
MeshComp->SetCollisionEnabled(ECollisionEnabled::NoCollision); 

// 可查询，查询包括了追踪轨迹线或重叠事件等
SphereComp->SetCollisionEnabled(ECollisionEnabled::QueryOnly);

// 响应通道 忽略
SphereComp->SetCollisionResponseToAllChannels(ECR_Ignore);

// 响应 Pawn通道 发生重叠
SphereComp->SetCollisionResponseToChannel(ECC_Pawn, ECR_Overlap);

```


### 碰撞时生成粒子效果

```c++
// .h 
protected:
    // 新建函数
    void PlayEffects();
```


```c++
// .cpp
#include "Kismet/GameplayStatics.h"

AFPSObjectiveActor::PlayEffects()
{
    UGameplayStatics::SpawnEmitterAtLocation()
}

```

### 重叠时调用

```c++
public:
    virtual void NotifyActorBeginOverlap(AActor* OtherActor) override;

```

```c++
// .cpp
// 一种是调用 OnComponentBeginOverlap
// 另一种是重写NotifyActorBeginOverlap方法
void AFPSObjectiveActor::NotifyActorBeginOverlap(AActor* OtherActor)
{
	Super::NotifyActorBeginOverlap(OtherActor);
	PlayEffects();
}

```

## <code>errors</code>

### 不能将 <code>"UStaticMeshComponent *"</code> 类型的值分配到 <code>"USceneComponent *"</code> 类型的实体

原因是未识别<code>UStaticMeshComponent</code>类型，导入其头文件即可: <code>include "Components/StaticMeshComponent.h"</code>

### <code>unrealheadertool failed for target ue4editor</code>

用<code>VS</code>编译时出现上述错误，不明所以，改用<code>Unreal Editor</code>打开项目，通过<code>Editor</code>编译，报错，信息指出是代码拼写出错，修正后解决。若<code>VS</code>提供的调试信息不够明确，可尝试换个方式。

