# Unreal 취업 기준 핵심 설명 정리

이 문서는 이득우 언리얼 파트2 강의를 따라 만든 `ArenaBattle` 프로젝트를 기준으로, 취업 준비 단계에서 최소한 말로 설명할 수 있어야 하는 Unreal Engine 핵심 개념을 정리한 것이다.

목표는 코드를 외우는 것이 아니라 다음 질문에 답할 수 있게 되는 것이다.

- 누가 이 객체를 만드는가?
- 누가 이 함수를 호출하는가?
- 진짜 상태값은 어디에 저장되는가?
- C++은 어떤 Blueprint 또는 asset과 연결되는가?
- 이 함수 다음에는 어디로 흐르는가?

---

## 1. GameMode, PlayerController, Pawn/Character

`GameMode`는 게임 규칙을 관리한다. 어떤 Pawn을 기본으로 spawn할지, 어떤 PlayerController를 쓸지, 점수와 클리어 조건을 어떻게 판단할지 같은 "게임 룰"을 담당한다.

이 프로젝트에서는 `AABGameMode`가 다음을 관리한다.

- `DefaultPawnClass`
- `PlayerControllerClass`
- `ClearScore`
- `CurrentScore`
- `bIsCleared`

`PlayerController`는 플레이어의 의도를 게임에 전달하는 객체다. 입력 소유자이고, Pawn을 possess하며, UI나 SaveGame 같은 플레이어 단위 흐름을 중계하기도 한다.

이 프로젝트의 `AABPlayerController`는 다음을 처리한다.

- retry count 저장
- 점수 변경 Blueprint event 호출
- game clear Blueprint event 호출
- game over Blueprint event 호출

`Pawn`은 조종 가능한 월드 객체이고, `Character`는 Pawn 중에서도 Capsule, SkeletalMesh, CharacterMovement가 기본으로 붙은 캐릭터 특화 클래스다.

이 프로젝트의 `AABCharacterPlayer`, `AABCharacterNonPlayer`는 실제로 월드에서 움직이고 싸우는 몸이다.

면접식 한 문장:

> GameMode는 게임 규칙, PlayerController는 플레이어의 입력과 UI 중계, Character는 월드 안에서 움직이는 실제 조작 대상입니다.

---

## 2. Constructor, PostInitializeComponents, BeginPlay

생성자는 Unreal이 객체를 만들 때 호출된다. 여기서는 component 생성, 기본 collision 설정, 기본 asset reference 연결 같은 일을 한다.

단, 생성자에서는 아직 world runtime 상황이나 controller possession을 확신하기 어렵다.

`PostInitializeComponents`는 Actor의 component들이 초기화된 뒤 호출된다. component끼리 delegate를 연결하거나, 이미 만들어진 component 참조를 바탕으로 초기 binding을 하기 좋다.

이 프로젝트에서는 `AABCharacterBase::PostInitializeComponents`에서 다음 delegate를 연결한다.

```cpp
Stat->OnHpZero.AddUObject(this, &AABCharacterBase::SetDead);
Stat->OnStatChanged.AddUObject(this, &AABCharacterBase::ApplyStat);
```

`BeginPlay`는 Actor가 실제 game world에서 play를 시작할 때 호출된다. Controller, World, Subsystem, runtime object를 사용하는 초기화에 적합하다.

이 프로젝트에서는 `AABCharacterPlayer::BeginPlay`에서 PlayerController를 얻고 입력을 enable한 뒤 조작 모드를 적용한다.

면접식 한 문장:

> 생성자는 기본 subobject와 asset 연결, PostInitializeComponents는 component 간 연결, BeginPlay는 실제 world runtime 초기화에 적합합니다.

---

## 3. CreateDefaultSubobject와 Component 소유권

`CreateDefaultSubobject`는 Actor가 기본 component를 생성할 때 사용한다. 이렇게 만든 component는 Actor의 subobject가 되고, lifetime도 Actor와 함께 간다.

직접 `delete`하지 않는다.

이 프로젝트에서는 `AABCharacterBase`가 다음 component를 생성한다.

- `Stat`
- `HpBar`
- `Weapon`

`AABCharacterPlayer`는 다음 component를 생성한다.

- `CameraBoom`
- `FollowCamera`

즉, 캐릭터가 이 component들을 소유한다.

중요한 구분은 "포인터를 가지고 있다"와 "소유한다"가 다르다는 점이다.

예를 들어 AnimInstance가 Movement pointer를 cache해도 Movement를 소유하는 것은 Character다.

면접식 한 문장:

> CreateDefaultSubobject로 만든 component는 Actor가 소유하는 기본 subobject이고, Actor lifetime과 함께 관리됩니다.

---

## 4. Enhanced Input 흐름

Enhanced Input은 입력을 코드에 직접 박지 않고 asset 중심으로 관리한다.

`InputAction`은 "공격", "점프", "이동" 같은 행동 단위다.

`InputMappingContext`는 어떤 키나 축이 어떤 InputAction을 발생시키는지 정의한다.

이 프로젝트에서는 `AABCharacterPlayer`가 다음 `InputAction` asset을 가지고 있다.

- `IA_Attack`
- `IA_Jump`
- `IA_ShoulderMove`
- `IA_ShoulderLook`
- `IA_QuaterMove`
- `IA_ChangeControl`

`SetupPlayerInputComponent`에서 이 action들을 C++ 함수에 binding한다.

`SetCharacterControl`에서는 `EnhancedInputLocalPlayerSubsystem`에 MappingContext를 등록해서 Shoulder/Quarter 조작 방식을 바꾼다.

입력 흐름:

```text
InputMappingContext
-> InputAction 발생
-> EnhancedInputComponent::BindAction
-> C++ 함수 호출
```

공격 입력 예시:

```text
IA_Attack
-> AABCharacterPlayer::Attack
-> AABCharacterBase::ProcessComboCommand
```

면접식 한 문장:

> Enhanced Input은 InputAction과 MappingContext asset으로 입력을 정의하고, C++에서는 BindAction으로 실제 gameplay 함수와 연결합니다.

---

## 5. AnimMontage와 AnimNotify 공격 판정

공격 버튼을 눌렀다고 바로 데미지를 주면 animation과 판정이 어긋날 수 있다.

그래서 액션 게임에서는 공격 animation의 특정 frame에 타격 판정을 붙이는 경우가 많다.

이 프로젝트에서는 공격 입력이 들어오면 `ComboActionMontage`를 재생한다.

그리고 montage 안의 특정 frame에 `AnimNotify_AttackHitCheck`가 있다.

이 notify가 실행되면 mesh owner를 `IABAnimationAttackInterface`로 cast하고 `AttackHitCheck()`를 호출한다.

그 다음 `AttackHitCheck`에서 전방 sphere sweep을 하고, 맞은 Actor에게 `TakeDamage`를 호출한다.

공격 판정 흐름:

```text
Attack input
-> Montage_Play
-> AnimNotify
-> AttackHitCheck
-> Sweep
-> TakeDamage
```

면접식 한 문장:

> 공격 판정 타이밍을 코드 tick이 아니라 AnimMontage의 AnimNotify에 두면, animation의 타격 순간과 gameplay damage를 맞출 수 있습니다.

---

## 6. StatComponent가 HP의 Source Of Truth인 이유

HP, 공격력, 이동 속도 같은 값은 여기저기 흩어져 있으면 위험하다.

UI도 HP를 알고, Character도 HP를 알고, Animation도 HP를 알면 어느 값이 진짜인지 헷갈린다.

이 프로젝트에서는 `UABCharacterStatComponent`가 다음 값을 가지고 있다.

- `CurrentHp`
- `BaseStat`
- `ModifierStat`
- `CurrentLevel`

그래서 HP의 진짜 값은 UI가 아니라 StatComponent에 있다.

UI의 ProgressBar percent는 표시용 복사본이다.

피해 흐름:

```text
TakeDamage
-> Stat->ApplyDamage
-> SetHp
-> OnHpChanged broadcast
-> UI update
```

HP 0 흐름:

```text
ApplyDamage
-> OnHpZero broadcast
-> Character::SetDead
```

면접식 한 문장:

> StatComponent가 HP의 source of truth이고, UI나 Movement는 그 값을 표시하거나 반영하는 파생 상태입니다.

---

## 7. UI가 Delegate로 갱신되는 흐름

UI가 매 frame HP를 polling하면 비효율적이고 구조도 지저분해진다.

대신 HP가 바뀌는 순간 StatComponent가 delegate로 알려주면 UI는 그때만 갱신하면 된다.

이 프로젝트에서는 `StatComponent`에 다음 delegate가 있다.

- `OnHpChanged`
- `OnStatChanged`
- `OnHpZero`

`AABCharacterBase::SetupCharacterWidget`이나 `AABCharacterPlayer::SetupHUDWidget`에서 widget 함수를 delegate에 연결한다.

즉, UI가 직접 StatComponent를 계속 찾는 게 아니라 처음 한 번 연결해두고 이후에는 이벤트를 받는다.

면접식 한 문장:

> Delegate를 쓰면 gameplay state를 가진 component와 UI 표시 로직을 느슨하게 연결할 수 있고, 상태가 바뀐 순간에만 UI를 갱신할 수 있습니다.

---

## 8. Interface를 쓰는 이유

Interface는 "상대의 구체 클래스는 모르지만 이 기능은 호출하고 싶다"는 상황에서 쓴다.

이 프로젝트의 좋은 예시는 `AnimNotify_AttackHitCheck`다.

Notify는 owner가 `AABCharacterBase`인지, player인지, NPC인지 몰라도 된다.

그냥 `IABAnimationAttackInterface`를 구현했다면 `AttackHitCheck()`를 호출한다.

아이템도 마찬가지다.

`AABItemBox`는 겹친 Actor가 정확히 어떤 캐릭터 클래스인지 몰라도 `IABCharacterItemInterface`를 구현했다면 `TakeItem()`을 호출한다.

면접식 한 문장:

> Interface를 사용하면 호출자는 구체 클래스에 강하게 의존하지 않고, 필요한 기능 계약만 보고 호출할 수 있습니다.

---

## 9. DataAsset을 쓰는 이유

DataAsset은 gameplay 설정값이나 아이템 데이터처럼 "코드보다 데이터로 관리하는 게 좋은 값"을 asset으로 빼는 방식이다.

이 프로젝트에서는 `UABCharacterControlData`가 조작 방식별 카메라, 이동, InputMappingContext를 들고 있다.

그래서 Shoulder/Quarter 모드를 코드 if문 덩어리로 만들지 않고 DataAsset 교체로 처리한다.

`UABComboActionData`는 다음 콤보 정보를 들고 있다.

- combo section 이름 prefix
- 최대 콤보 수
- 입력 가능 frame

아이템 DataAsset 예시:

- `UABWeaponItemData`
- `UABPotionItemData`
- `UABScrollItemData`

면접식 한 문장:

> DataAsset을 쓰면 디자이너가 바꿀 수 있는 gameplay 데이터를 코드에서 분리할 수 있고, 같은 로직에 다른 데이터를 끼워 넣어 재사용할 수 있습니다.

---

## 10. Behavior Tree, Blackboard, Service, Decorator, Task

Behavior Tree는 AI의 의사결정 흐름이다.

"순찰한다", "플레이어를 찾는다", "공격 범위면 공격한다" 같은 흐름을 tree로 표현한다.

Blackboard는 AI가 공유하는 기억 공간이다.

이 프로젝트에서는 다음 값이 Blackboard key다.

- `HomePos`
- `PatrolPos`
- `Target`

Service는 주기적으로 실행되며 상태를 갱신한다.

`BTService_Detect`는 일정 간격으로 주변에 player가 있는지 검사하고, 찾으면 Blackboard의 `Target`에 저장한다.

Decorator는 조건 검사다.

`BTDecorator_AttackInRange`는 Blackboard의 Target과 NPC 사이 거리를 보고 공격 가능한지 판단한다.

Task는 실제 행동이다.

이 프로젝트의 task 예시:

- `BTTask_FindPatrolPos`: 순찰 위치 찾기
- `BTTask_TurnToTarget`: target 바라보기
- `BTTask_Attack`: NPC 공격 요청

AI 공격 흐름:

```text
Detect Service
-> Blackboard Target 저장
-> AttackInRange Decorator
-> BTTask_Attack
-> NPC AttackByAI
-> ProcessComboCommand
-> Montage 종료
-> FinishLatentTask
```

면접식 한 문장:

> Behavior Tree는 AI 흐름, Blackboard는 AI 기억, Service는 주기적 갱신, Decorator는 조건, Task는 실제 행동입니다.

---

## 프로젝트 핵심 전투 흐름 한 문장

이 문장은 반드시 자기 말로 설명할 수 있어야 한다.

> 이 프로젝트의 전투는 입력이나 AI가 공통으로 `ProcessComboCommand`를 호출하고, AnimMontage가 재생된 뒤 AnimNotify 시점에 `AttackHitCheck`가 실행됩니다. 데미지는 `TakeDamage`를 통해 StatComponent의 HP를 줄이고, HP 변화는 delegate로 UI와 사망 처리에 전달됩니다.

---

## 스스로 점검할 질문

각 개념을 공부한 뒤 아래 질문에 답해본다.

- 이 객체는 누가 생성하는가?
- 이 객체의 lifetime은 누가 관리하는가?
- 이 함수는 Unreal이 자동으로 호출하는가, 내가 직접 호출하는가, delegate/timer/notify/input이 호출하는가?
- 이 값은 권위 상태인가, cache인가, 파생 상태인가, UI 표시 상태인가?
- 이 C++ 코드는 어떤 Blueprint나 asset 경로와 연결되어 있는가?
- 이 함수가 끝난 뒤 다음 흐름은 어디로 넘어가는가?

---

## 취업 준비용 설명 체크리스트

아래 항목을 코드 없이 말로 설명할 수 있으면 이 프로젝트의 기본기는 상당히 잡힌 것이다.

- `GameMode`, `PlayerController`, `Pawn/Character` 역할 차이
- `Constructor`, `PostInitializeComponents`, `BeginPlay` 차이
- `CreateDefaultSubobject`로 만든 component의 소유권
- Enhanced Input의 `InputAction` / `MappingContext` 흐름
- AnimMontage와 AnimNotify로 공격 판정을 맞추는 이유
- StatComponent가 HP의 source of truth인 이유
- UI가 delegate로 갱신되는 흐름
- Interface를 쓰는 이유
- DataAsset을 쓰는 이유
- Behavior Tree, Blackboard, Service, Decorator, Task의 역할

