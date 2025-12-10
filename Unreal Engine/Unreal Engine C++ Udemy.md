# The Pawn Class
## Camera and Spring Arm
`USpringArmComponent*` 로 Spring Arm 추가
forward declaration이 필요하다.
`class USpringArmComponent` 를 헤더 파일에 추가해 준다.
같은 방식으로 Camera Component도 추가해 준다.
***
```cpp title:header
class UCameraComponent;
class USpringArmComponent;

//class declaration
private:
	UPROPERTY(VisibleAnywhere)  
	USpringArmComponent* SpringArm;
	
	UPROPERTY(VisibleAnywhere)  
	UCameraComponent* ViewCamera;
```
***
.cpp 에서 생성자 부분에 컴포넌트를 추가해 준다.
```cpp title:cpp
#include "GameFramework/SpringArmComponent.h"
#include "Camera/CameraComponent.h"

//constructor
SpringArm = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArm"));  
SpringArm->SetupAttachment(GetRootComponent());
SpringArm->TargetArmLength = 300.f;

ViewCamera = CreateDefaultSubobject<UCameraComponent>(TEXT("ViewCamera"));  
ViewCamera->SetupAttachment(SpringArm);
```
## Adding Controller Input
**Enhanced Input 4 YouTube Lecture**
```cpp title:header
//class
protected:
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Input)  
	UInputAction* LookAction;
	
	void Look(const FInputActionValue& Value);
```

```cpp title:cpp
// BeginPlay
if (APlayerController* PlayerController = Cast<APlayerController>(GetController()))  
{  
    if (UEnhancedInputLocalPlayerSubsystem* Subsystem = ULocalPlayer::GetSubsystem<UEnhancedInputLocalPlayerSubsystem>(PlayerController->GetLocalPlayer()))  
    {       
    Subsystem->AddMappingContext(BirdMappingContext, 0);  
    }
}

void ABird::Look(const FInputActionValue& Value)  
{  
    const FVector2D LookAxisValue = Value.Get<FVector2D>();  
    if (GetController())  
    {       AddControllerYawInput(LookAxisValue.X);  
       AddControllerPitchInput(LookAxisValue.Y);  
    }
}

//SetupPlayerInputComponent
if (UEnhancedInputComponent* EnhancedInputComponent = CastChecked<UEnhancedInputComponent>(PlayerInputComponent))  
{  
    EnhancedInputComponent->BindAction(LookAction, ETriggerEvent::Triggered, this, &ABird::Look);  
}
```
# The Character Class
## Character Inputs
캐릭터 이동 Move, 시점 이동 Look 함수를 만들었다.
```cpp title:header
#include "InputActionValue.h"

// forward declaration
class UInputMappingContext;
class UInputAction;


protected:
	UPROPERTY(EditAnywhere, Category = Input)
	UInputMappingContext *SlashContext; // Input Mapping Context Variable

	UPROPERTY(EditAnywhere, Category = Input)
	UInputAction *MovementAction; // Input Action Variable
	
	UPROPERTY(EditAnywhere, Category = Input)
	UInputAction *LookAction; // Input Action Variable
	
	void Move(const FInputActionValue& Value);
	void Look(const FInputActionValue &Value);
```
***
```cpp title:cpp
#include "Components/InputComponent.h"
#include "EnhancedInputComponent.h"
#include "EnhancedInputSubsystems.h"

// BeginPlay
	if (APlayerController *PlayerController = Cast<APlayerController>(GetController()))
	{
		if (UEnhancedInputLocalPlayerSubsystem *Subsystem = ULocalPlayer::GetSubsystem<UEnhancedInputLocalPlayerSubsystem>(PlayerController->GetLocalPlayer()))
		{
			Subsystem->AddMappingContext(SlashContext, 0);
		}
	}

void ASlashCharacter::Move(const FInputActionValue &Value)
{
	const FVector2D MovementVector = Value.Get<FVector2D>();

	const FVector Forward = GetActorForwardVector();
	AddMovementInput(Forward, MovementVector.Y);

	const FVector Right = GetActorRightVector();
	AddMovementInput(Right, MovementVector.X);
}

void ASlashCharacter::Look(const FInputActionValue &Value)
{
	const FVector2D LookAxisVector = Value.Get<FVector2D>();

	AddControllerPitchInput(LookAxisVector.Y);
	AddControllerYawInput(LookAxisVector.X);
}
```
## Character Camera and SpringArm
Character에 Spring Arm / Camera Component 추가
```cpp title:header
class UCameraComponent;
class USpringArmComponent;

private:
	UPROPERTY(VisibleAnywhere)
	USpringArmComponent *SpringArm;

	UPROPERTY(VisibleAnywhere)
	UCameraComponent *ViewCamera;
```
***
```cpp title:cpp
#include "GameFramework/SpringArmComponent.h"
#include "Camera/CameraComponent.h"

//initializer
bUseControllerRotationPitch = false;
bUseControllerRotationYaw = false;
bUseControllerRotationRoll = false;

SpringArm = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArm"));
SpringArm->SetupAttachment(GetRootComponent());
SpringArm->TargetArmLength = 300.f;

ViewCamera = CreateDefaultSubobject<UCameraComponent>(TEXT("ViewCamera"));
ViewCamera->SetupAttachment(SpringArm);
```
이제 카메라는 자유롭게 이동할 수 있지만, 문제는 컨트롤러가 바라보는 방향으로 캐릭터가 움직이지 않는다. 오직 캐릭터가 바라보는 방향으로만 이동한다.
## The Rotation Matrix
컨트롤러가 가리키는 방향으로 캐릭터를 움직이고 싶다.
그러려면 컨트롤러의 전방 벡터를 구할 방법을 찾아야 한다.
여기에는 회전변환행렬(Rotation Matrix) 을 사용한다.
## Controller Directions
컨트롤러의 방향에 맞게 움직여주려면, Move 의 코드를 수정한다.
```cpp
void ASlashCharacter::Move(const FInputActionValue &Value)
{
	const FVector2D MovementVector = Value.Get<FVector2D>();

	const FRotator Rotation = Controller->GetControlRotation();
	const FRotator YawRotation(0.f, Rotation.Yaw, 0.f);

	const FVector ForwardDirection = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::X);
	AddMovementInput(ForwardDirection, MovementVector.Y);
	const FVector RightDirection = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::Y);
	AddMovementInput(RightDirection, MovementVector.X);
}
```
***
캐릭터가 바라보는 방향이 움직이는 방향에 맞춰서 돌아가도록 변경해 준다.
```cpp
#include "GameFramework/CharacterMovementComponent.h"

// initializer
	GetCharacterMovement()->bOrientRotationToMovement = true;
	GetCharacterMovement()->RotationRate = FRotator(0.f, 400.f, 0.f);
```
## Hair and Eyebrows
Hair 컴포넌트를 장착시키기 위해, 먼저 Build.cs 파일에서 "HairStrandsCore" 모듈을 활성화해 준다.
```cpp title:header
class UGroomComponent;

private:
	UPROPERTY(VisibleAnywhere, Category = Hair)
	UGroomComponent* Hair;

	UPROPERTY(VisibleAnywhere, Category = Hair)
	UGroomComponent *Eyebrows;
```
***
```cpp title:cpp
#include "GroomComponent.h"

// initializer
	Hair = CreateDefaultSubobject<UGroomComponent>(TEXT("Hair"));
	Hair->SetupAttachment(GetMesh()); // 현재 캐릭터의 Mesh를 리턴하는 GetMesh()
	Hair->AttachmentName = FString("head");

	Eyebrows = CreateDefaultSubobject<UGroomComponent>(TEXT("Eyebrows"));
	Eyebrows->SetupAttachment(GetMesh());
	Eyebrows->AttachmentName = FString("head");
```
***
# The Animation Blueprint
## The Anim instance
AnimInstance를 수정하고 컴파일할 때는, Unreal Editor를 종료하고 진행한다.

AnimInstance를 상속받아서 우리만의 애니메이션 로직을 구현해 보자.
```cpp title:header
#pragma once
#include "CoreMinimal.h"
#include "Animation/AnimInstance.h"
#include "SlashAnimInstance.generated.h"

UCLASS()
class ARPG_API USlashAnimInstance : public UAnimInstance
{
	GENERATED_BODY()

public:
	virtual void NativeInitializeAnimation() override;
	virtual void NativeUpdateAnimation(float DeltaTime) override;

	UPROPERTY(BlueprintReadOnly)
	class ASlashCharacter* SlashCharacter;

	UPROPERTY(BlueprintReadOnly)
	class UCharacterMovementComponent* SlashCharacterMovement;

	UPROPERTY(BlueprintReadOnly, Category = Movement)
	float GroundSpeed;
};

```
***
```cpp title:cpp
#include "Characters/SlashAnimInstance.h"
#include "Characters/SlashCharacter.h"
#include "GameFramework/CharacterMovementComponent.h"
#include "Kismet/KismetMathLibrary.h"

void USlashAnimInstance::NativeInitializeAnimation()
{
	Super::NativeInitializeAnimation(); // override 했기 때문에 Super 호출

	SlashCharacter = Cast<ASlashCharacter>(TryGetPawnOwner());
	if (SlashCharacter)
	{
		// 캐릭터로부터 움직임 컴포넌트를 가져옴
		SlashCharacterMovement = SlashCharacter->GetCharacterMovement();
	}
	
}

void USlashAnimInstance::NativeUpdateAnimation(float DeltaTime)
{
	Super::NativeUpdateAnimation(DeltaTime);

	if (SlashCharacterMovement) 
	{
		// 캐릭터의 속도 벡터에서 XY(수평) 속도만 계산
		GroundSpeed = UKismetMathLibrary::VSizeXY(SlashCharacterMovement->Velocity);
	}
}

```
## Jumping
ACharacter class에 존재하는 Jump function을 override 한다.
```cpp
// header
class ARPG_API ASlashCharacter : public ACharacter
public:
	virtual void Jump() override;
protected:
	UPROPERTY(EditAnywhere, Category = Input)
	UInputAction* JumpAction;

// cpp
void ASlashCharacter::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
	if (UEnhancedInputComponent *EnhancedInputComponent = CastChecked<UEnhancedInputComponent>(PlayerInputComponent))
	{
		EnhancedInputComponent->BindAction(JumpAction, ETriggerEvent::Triggered, this, &ASlashCharacter::Jump);
	}
}
void ASlashCharacter::Jump()
{
	Super::Jump();
}
```
## Jump Animations
SlashAminInstance 에 IsFalling 변수를 추가해 준다.
```cpp
/// header
public:
	UPROPERTY(BlueprintReadOnly, Category = Movement)  
	bool IsFalling;
/// cpp
void USlashAnimInstance::NativeUpdateAnimation(float DeltaTime)  
{  
    Super::NativeUpdateAnimation(DeltaTime);  
  
    if (SlashCharacterMovement)   
    {  
       IsFalling = SlashCharacterMovement->IsFalling();  
    }
}
```
이후 Animation Blueprint에서 노드를 수정하고 추가하는 작업을 통해, 점프 및 착지 애니메이션을 구현하였다. 다만 착지 부분이 조금 어색한 감이 있다.
## Inverse Kinematics

# Collision and Overlaps
## Collision Presets
Collision Enabled 에는 4가지 옵션이 존재한다.
No Collision - 말 그대로 충돌 반응이 존재하지 않음
Query only - Query만을 위한 옵션
Physics Only - Physics simulation을 위한 옵션. Query는 적용되지 않는다.
Collision Enabled - 둘 다 적용
## Overlap Events
언리얼 엔진의 Overlap 시스템에 대해 학습해 보았다.
## Delegates
디자인 패턴 중에 The Observer Pattern이 있다.
옵저버 패턴이란, 객체들 간의 일대다 의존 관계를 정의하는 패턴이다.
한 객체의 상태가 변경되면, 그 객체에 의존하는 모든 객체들이 자동으로 알림을 받고 업데이트되는 방식이다.
실생활에서의 예를 들면, 유튜브가 있다.
유튜버가 Subject이고, 구독자들이 Observer이다.
유튜버가 새로운 영상을 올릴 때마다 구독자들은 자동으로 알림을 받게 된다.

언리얼 엔진에서는 Delegate로 옵저버 패턴을 구현한다.
delegate는 observer들의 list를 저장하는 클래스이다.
## On Component Begin Overlap
```cpp
// header file
class USphereComponent;
protected:
UFUNCTION()  
	void OnSphereOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult);
private:
	UPROPERTY(VisibleAnywhere)
	USphereComponent* SphereComponent;
// cpp file
#include "Components/SphereComponent.h"
AItem::AItem()
	SphereComponent = CreateDefaultSubobject<USphereComponent>(TEXT("SphereComponent"));  
	SphereComponent->SetupAttachment(GetRootComponent());
	
//BeginPlay
	SphereComponent->OnComponentBeginOverlap.AddDynamic(this, &AItem::OnSphereOverlap); // bind callback to a delegate

void AItem::OnSphereOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor,  
    UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)  
{  
    const FString OtherActorName = OtherActor->GetName();  
    if (GEngine)  
       GEngine->AddOnScreenDebugMessage(1, 30.f, FColor::Red, OtherActorName);  
}
```
BeginOverlap Event를 C++로 구현해 보았다. 
## On Component End Overlap
같은 방식으로 EndOverlap 도 구현해 본다.
```cpp
//header
protected:
	UFUNCTION()  
void OnSphereEndOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex);
//cpp
//beginplay
	SphereComponent->OnComponentEndOverlap.AddDynamic(this, &AItem::OnSphereEndOverlap);
	
void AItem::OnSphereEndOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor,  
    UPrimitiveComponent* OtherComp, int32 OtherBodyIndex)  
{  
    const FString OtherActorName = FString("Ending Overlap With: ") + OtherActor->GetName();  
    if (GEngine)  
       GEngine->AddOnScreenDebugMessage(1, 30.f, FColor::Blue, OtherActorName);  
}
```
# The Weapon Class
## The Weapon Class
Item 을 상속받는 Weapon 클래스를 C++로 생성한다.
```cpp
//header
class ARPG_API AWeapon : public AItem  
{  
    GENERATED_BODY()  
  
protected:  
    virtual void OnSphereOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult) override;  
    virtual void OnSphereEndOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor, UPrimitiveComponent* OtherComp, int32 OtherBodyIndex) override;  
};

//cpp
void AWeapon::OnSphereOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor,  
    UPrimitiveComponent* OtherComp, int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult)  
{  
    Super::OnSphereOverlap(OverlappedComponent, OtherActor, OtherComp, OtherBodyIndex, bFromSweep, SweepResult);  
}  
  
void AWeapon::OnSphereEndOverlap(UPrimitiveComponent* OverlappedComponent, AActor* OtherActor,  
    UPrimitiveComponent* OtherComp, int32 OtherBodyIndex)  
{  
    Super::OnSphereEndOverlap(OverlappedComponent, OtherActor, OtherComp, OtherBodyIndex);  
}
```
## Attaching the Sword
ASlashCharacter 클래스로 casting 해서, 성공하면 소켓에 칼을 장착한다.
```cpp
//On Overlap
	ASlashCharacter* SlashCharacter = Cast<ASlashCharacter>(OtherActor);
	if (SlashCharacter)
	{
		FAttachmentTransformRules TransformRules(EAttachmentRule::SnapToTarget, true);
		ItemMesh->AttachToComponent(SlashCharacter->GetMesh(), TransformRules, FName("RightHandSocket"));
	}
```
## Picking Up Items
아이템을 장착하는 과정은 크게 세 단계로 구성된다.
1. 아이템과의 오버랩 감지
2. 오버랩된 아이템의 참조를 저장
3. E키 입력 시 무기 장착
Item.cpp에서 오버랩된 액터가 SlashCharacter 인 경우 캐릭터의 OverlappingItem을 현재 아이템으로 설정하고, SlashCharacet가 오버랩된 아이템 참조를 저장.
이후 E 키 입력시 무기라면, 무기가 장착된다.
## Enum For Character State
```cpp
UENUM(BlueprintType)	// expose to Blueprint
enum class ECharacterState : uint8
{
	ECS_Unequipped UMETA(DisplayName = "Unequipped"),
	ECS_EquippedOneHandedWeapon UMETA(DisplayName = "Equipped One-Handed Weapon"),
	ECS_EquippedTwoHandedWeapon UMETA(DisplayName = "Equipped Two-Handed Weapon")
};
```
Enum 클래스를 만들고, 헤더 파일로 따로 생성했다.
이후 캐릭터 클래스에 include 해서 기본 state를 지정하고, 외부에서 state를 읽을 수 있도록 getter 함수를 만들었다.
Weapon을 장착했을 경우에 State가 EquippedOneHandedWeapon으로 변경된다.
또한 애니메이션 인스턴스에도 include 시켜, state에 따른 애니메이션이 발동될 수 있도록 했다.
## Switching Animation Poses, Equipped Animations
Enum 값에 맞춰 애니메이션이 변경될 수 있도록 하였다.
그런데 한 애니메이션 BP에 다 몰아넣으려니 너무 복잡하다
## Multiple Animation Blueprints
이를 해결하기 위해 여러 개의 Anim BP를 만들고, Linked Anim Graph로 이들을 연결해 준다.
이러면 훨씬 깔끔하게 작업이 가능하다. 
# Attacking
## Animation Montages
블루프린트로 Animation Montage를 사용하는 방법에 대해 가볍게 알아보았다.
## Playing Montages in C++
이제 Montage 를 C++로 구현해 보자.
캐릭터 헤더파일에 Montage를 지정할 수 있는 변수를 추가해 주었다.
이후 cpp 파일에서 Attack 시 어떤 행동을 할 지 구현하였다.
애니메이션 인스턴스를 가져와서 UAnimInstance 포인터로 저장하고,
AnimInstance와 AttackMontage가 모두 nullptr 가 아니라면
Montage를 실행한다.
FMath::RandRange로 코인 플립을 수행하여 공격을 할 때마다 랜덤한 애니메이션이 나가도록 구현했다.
## Attacking State, Resetting the Action State
마우스를 연속으로 누르면 공격 모션이 계속 초기화되는 버그가 있다.
이를 해결하기 위해, Notify를 사용한다.
Montage에 Attack end 라는 notify를 넣고, 그 이벤트에 맞춰 state를 변경하도록 한다.
```cpp
UENUM(BlueprintType)
enum class EActionState : uint8
{
	EAS_Unoccupied UMETA(DisplayName = "Unoccupied"),
	EAS_Attacking UMETA(DisplayName = "Attacking")
};
```
블루프린트 내에서 ActionState에 접근할 수 있도록 한다.
이제 공격 시 Unoccupied 상태 및 Unequipped 상태라면, 애니메이션이 재생된다.
## Item State
아이템이 위아래로 움직이다가, 캐릭터가 아이템을 장착하면 움직임이 종료되도록 하였다.
Item State를 추가하고, Weapon이 장착됐을 때 State를 equipped로 변경한다.
그리고 아이템에서는 state 기본값을 Hovering으로 두고 Hovering 상태일 때는 아이템이 움직이도록 한다.
## Sound Notifies and Meta Sounds
언리얼 5의 새로운 기능인 메타 사운드를 이용해 사운드를 조합하고 재생하는 방법을 학습하였다.
## Meta Sounds for Footsteps
발소리에 맞춰 메타사운드를 입혀주었다.
## Fixing Foot Placement
ik 작동 시에 발의 위치가 이상하게 변하는 것을 수정해 주었다.
## Putting the Sword Away ~ Attaching the Sword to the Back
검을 장착한 후에 검을 등쪽에 붙이고 해제하는 것을 작업하였다.
주로 enum을 이용해 작업을 수행하였는데, 검을 장착했을 때, 검을 등에 붙일 때는 검을 장착했다는 상태로 바꾸고 그 상태일 때는 움직일 수 없게 하였다. 그러니까 그것을 수행하는 동안은 움직임이 불가하다. 이처럼 enum으로 캐릭터의 특정 움직임 수행을 간편하게 구현할 수 있다.
## Equip and Unequip Sounds
무기를 처음에 e로 장착할 때, 소리가 나도록 하였다. 
그런데 무기를 장착하고 휘두른 뒤에 다시 e를 누르면, 갑자기 소리가 또 난다.
이는 무기의 충돌 판정이 장착된 뒤에도 유효하기 때문이다.
무기가 장착되고 나서, 충돌을 비활성화하여 해결하였다.
# Weapon Mechanics
## Collision Box
충돌 상자를 추가해 주었다. 이후 충돌을 감지하기 위해 블루프린트로 오버랩 시 디버그 구체를 소환한다.
## Tracing
Trace의 기본적인 것들에 대해 학습하였다. 블루프린트로 기본적인 trace를 구현해 보았다.
visibility 채널을 통해 trace를 진행하였다.
## Box Trace in C++
Box Trace 기능을 C++ 을 통하여 구현해 보았다.
Weapon 에 Box Collision 변수와 Scene Component 두개를 추가해 준 뒤, 생성자를 추가하여 생성자에 Box 의 이름과 Collision 세팅을 추가해 주었다. 이후 Overlap 함수에서 오버랩시 어떤 기능을 수행할 지 지정해 주었다.
## Disabling Weapon Box Collision
무기를 휘두를 때만 충돌 판정이 일어나도록 변경하기 위해 C++에서 코드를 수정하였다.
SetWeaponCollisionEnabled 함수를 만들고, 충돌 판정을 변경할수 있도록 만들었다.
BlueprintCallable 함수이기 때문에, Anim Notify를 지정해 주고 블루프린트에서 이 함수를 통해 충돌을 수정하였다.
## Enemies
Enemy 클래스를 만들고, 충돌 처리가 잘 일어날 수 있도록 원하는 대로 코딩하였다.
## Implementing Interfaces
HitInterface 에서 FVector& 를 받도록 하고, Enemy에서 HitInterface를 include 한다.
부모 클래스로 IHitInterface도 받은 뒤, override 한다.
GetHit 호출 시에는 DebugSphere 를 출력한다.
Weapon 클래스에서 BoxOverlap 발생 시에, GetHit가 호출되도록 하고 충돌 포인트를 전달해 준다.
## Playing the Hit React Montage
먼저 4방향에서 피격당하는 애니메이션을 전부 Montage에 집어넣고,
Montage를 블루프린트에서 지정할 수 있도록 UAnimMontage* 변수를 만들어 준다.
그리고 Montage를 재생할 함수를 만든다.
이전에 캐릭터 블루프린트에서 했던 것과 동일하게 Montage를 재생하는 함수를 만든다.
이후에 GetHit에서 그 함수를 호출한다.
## Dot Product
enemy의 forward vector와 Impact point를 이용해서 피격당했을 때 forward vector 기준으로 각도를 계산해서 enemy가 밀려날 방향을 결정할 수 있다.
Dot Product를 이용하여, 각도를 계산한다.
```cpp
const FVector Forward = GetActorForwardVector();    // returns normalized vector  
// Lower Impact Point to the Enemy's Actor Location Z  
const FVector ImpactLowered(ImpactPoint.X, ImpactPoint.Y, GetActorLocation().Z);  
const FVector ToHit = (ImpactLowered - GetActorLocation()).GetSafeNormal();  
  
// Forward * ToHit = |Forward| |ToHit| * cos(theta)  
// |Forward| = 1, |ToHit| = 1, so Forward * ToHit = cos(theta)  
const double CosTheta = FVector::DotProduct(Forward, ToHit);    // returns scalar  
// Take the inverse cosine (arc-cosine) of cos(theta) to get theta  
double Theta = FMath::Acos(CosTheta);  
// convert from radians to degrees  
Theta = FMath::RadiansToDegrees(Theta);
```
## Cross Product
Dot Product는 항상 양의 값을 리턴한다. 이는 Theta의 값이 항상 양수가 되기 때문에 어디서 enemy가 피격당하는지를 알 수가 없다.
Cross Product를 사용하면, 그 방향(위 또는 아래) 에 따라 Theta가 양의 값 또는 음의 값임을 알게 된다.
## Directional Hit Reactions
![[Pasted image 20250108215304.png]]
해당 그림에 맞게, Theta의 값에 따라 각자 다른 방향으로 적이 밀려나게 하였다.
```cpp
FName Section("FromBack");  
  
if (Theta >= -45.f && Theta <= 45.f)  
{  
    Section = FName("FromFront");  
}  
else if (Theta >= -135.f && Theta <= -45.f)  
{  
    Section = FName("FromLeft");  
}  
else if (Theta >= 45.f && Theta < 135.f)  
{  
    Section = FName("FromRight");  
}  
  
PlayHitReactMontage(Section);
```
## One Hit Per Swing
Weapon에서 BoxTrace가 일어났던 Actor를 배열에 저장해 둔다.
그리고 다음 Overlap이 발생할 때 그 Actor를 무시한다. (한 Action에 대해 두 번 이상의 Overlap 발생에 대한 BoxTrace 방지)
그리고 SlashCharacter에서, Notify로 Weapon Collision을 켜고 끌 때 배열에서 Actor를 제거해 준다.
# Breakable Actors
## Creating Fields with Weapons
무기로 공격했을 때, 타격 부위에 Field System이 발생되도록 한다.
C++로 CreateFields 이벤트를 만들어 주고, Impact Point를 인자로 넣어준다.
이후 블루프린트에서 Field System 을 구현해 준다.
## Breakable Actor
이제 C++로 "파괴 가능한 액터" 클래스를 만들어 본다.
UGeometryCollectionComponent를 추가해 주고, RootComponent로 지정한 뒤 GenerateOverlapEvents 를 true로 설정하고, 카메라에 영향을 주지 않도록 설정한다.
그리고 나서 추후 HitInterface를 이용해 액터가 파괴되었을 때 뭔가가 발생할 수 있도록 인터페이스를 추가해 주었다.
## Blueprint Native Event
BlueprintNativeEvent 함수는, BlueprintCallable 과 BlueprintImplementableEvent 를 조합했다고 보면 된다.
작동 방식 자체는 C++로 프로그래밍 하지만, 블루프린트로 덮어쓸 수 있다. 이 때 C++에서 코딩시에 이름 끝에 \_Implementation 이 붙는다.
# Treasure
## Spawning Actors from C++
이전에 Item의 자식 클래스인 Treasure 클래스를 C++로 만들어 주었다. 
블루프린트에서 Treasure 클래스를 지정해 줄 수 있도록 변수를 만들어 준다. 이때, TSubclassOf를 이용해 Treasure 클래스와 그 자식들만 선택 가능하도록 한다.
GetHit 가 발생했을 때, Actor의 위치에서 지정해준 Treasure가 스폰되도록 하였다.
## Different Types of Treasure
항아리를 파괴할 때 마다 매번 같은 보물이 나오는 것은 지겹다.
블루프린트로 BaseTreasure 를 만들어 주고, 자식 블루프린트로 여러 보물들을 만들어 준다. 그리고 각자 가치를 다르게 설정해 주어 게임에 랜덤성을 부여한다.
전에 만들었던 Treasure 포인터를, 포인터 배열로 만든다.
그리고 그 배열의 내용을 브루프린트에서 설정해 준다.
BreakableActor가 GetHit를 발생시킬 때, 랜덤한 Treasure가 스폰되도록 하였다.
## Niagara Components
간단한 나이아가라 이펙트를 만들었고, 이것을 treasure와 weapon에 적용해 본다.
item에 모두 적용시키기 위해, 부모 클래스인 item 클래스에 Niagara Component를 장착시켰다.
Treasure에는 잘 작동하는 모습이다.
그리고 Weapon에도 잘 작동한다.
문제는, Weapon을 캐릭터가 장착했을 때에도 나이아가라는 사라지지 않는다.
이를 해결하기 위해, Weapon.cpp에서 NiagaraComponent를 include 하여 Equip 하였을때 Deactivate 하였다.
# Combat
## Actor Components
Actor Component를 생성하였다.
Actor Component란 액터에 부착할 수 있는 기본적인 컴포넌트이다. 또한 개별적으로 BeginPlay와 Tick 기능이 있다.
Actor Component 클래스를 C++로 생성하고, Enemy에 장착하였다.
여기서는 체력 시스템을 추가해 주었다.
## Widget Components
체력 바를 추가한다.
먼저 블루프린트로 위젯을 만들고, 간단한 체력 바를 만들어 준다.
이후 C++로 위젯 클래스를 생성한 뒤, Enemy에 장착해 주었다.
이제 다시 에디터로 돌아가서 위젯 블루프린트를 지정해 주면, 적 위에 체력바가 뜬다.
주의할 점은, User Interfaces의 Space를 Screen으로 해야 화면이 돌아가도 체력바가 보인다는 점이다.
## Setting the Health Percent
위젯을 연결해줄 컴포넌트를 C++로 만들었다. 그리고 실제 Progress Bar를 컨트롤하는 HealthBar 를 이 컴포넌트에서 건드린다.
이 C++ 클래스에서 Percent를 입력받으면 체력바를 그 퍼센트로 바꿔주는 함수를 만들고, Enemy에서 이 컴포넌트를 장착한 다음 간단하게 체력을 변경할 수 있다.
## Damage
AActor::TakeDamage 와 UGameplayStatics::ApplyDamage 를 이용해 대미지를 입히는 시스템을 구축한다.
Enemy에서 TakeDamage를 override 하여 사용한다.
체력 변수를 다루어야 하니, Attribute에서 체력에 접근할 수 있는 함수를 만들어 준다.
대미지를 받은 만큼 체력을 감소시킨 뒤에 체력바를 세팅해 준다.
또한 Weapon에서는 ApplyDamage를 추가해 주었다.
그리고 Weapon의 Equip 함수에서 Owner와 Instigator를 장착한 캐릭터로 세팅해 주도록 변경해 주었다.
## Death Poses, Polishing Enemy Death
CharacterTypes 에서 생사 여부를 확인하고, 죽음 포즈를 결정짓는 Enum을 추가한다.
Enemy의 기본 상태는 Alive로 지정하고, 죽을 때 랜덤으로 포즈 5개 중 1개를 출력한다.
Enemy가 죽었을 때 캡슐 충돌을 해제해 주었다.
Enemy를 때리기 전에는 체력바가 보이지 않다가, 때리면 체력바가 보이도록 수정하였다.
또한 플레이어와 적 사이의 거리를 계산하여, 일정 범위에서 벗어나면 체력바가 보이지 않도록 하였고 적이 사망했을 때도 체력바가 사라지도록 하였다.
# Enemy Behavior
## Patrol Targets, Selecting Patrol Targets
순찰 타겟과 순찰 타겟 배열을 설정해 주고, 에디터에서 세팅해 주면 그곳으로 움직이도록 설정하였다.
## ~Enemy States
Patrol 할때 Delay를 범위 내 랜덤으로 걸어주고, 적에게 Enum State를 부여하였다.
이것을 이용해 다양하게 사용할 수 있다. 기본적으로는 정찰 상태이다가, 플레이어를 발견하면 Chasing 상태가 되어 플레이어를 추격한다.
이때 코드에서 cast를 사용하지 않고 Tag 시스템을 사용해 자원 낭비 없이 효율적으로 동작하도록 한다.
그리고 chasing이 한번만 발동하도록 하였다.
